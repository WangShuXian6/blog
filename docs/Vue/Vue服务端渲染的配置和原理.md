#### 服务端渲染的配置和原理
![vue-server-render](https://user-images.githubusercontent.com/30850497/41214341-772217f2-6d7d-11e8-897e-d3f396f0a391.jpg)
>服务端渲染出首屏页面html文件，不含js.

>服务端将js插入渲染出的页面，返回客户端


>./build/webpack.config.server.js

```javascript
// ./build/webpack.config.server.js
// 服务端渲染配置

// 安装服务端渲染库 npm i vue-server-renderer -S
const VueServerPlugin = require('vue-server-render/server-plugin')

const path = require('path')
const ExtractPlugin = require('extract-text-webpack-plugin')
const webpack = require('webpack')
const merge = require('webpack-merge')
const baseConfig = require('./webpack.config.base')


// const defaultPluins = [
//     new webpack.DefinePlugin({
//         'process.env': {
//             NODE_ENV: '"development"'
//         }
//     })
// ]

const config = {
    target: 'node', // 服务端node环境
    entry: path.join(__dirname, '../client/server-entry.js'),
    devtool: 'source-map',
    output: {
        libraryTarget: 'commonjs2', // 模块类型 // module.exports=
        filename: 'server-entry.js', // 服务端无需缓存
        path: path.join(__dirname, '../server-build')
    },
    externals: Object.keys(require('../package.json').dependencies), // vue,vue-router,vuex // 排除服务端打包的文件，直接使用node_modules内的
    module: {
        // node环境没有css,不能将css插入DOM中。
        rules: [
            {
                test: /\.styl/,
                use: ExtractPlugin.extract({
                    fallback: 'vue-style-loader',
                    use: [
                        'css-loader',
                        {
                            loader: 'postcss-loader',
                            options: {
                                sourceMap: true,
                            }
                        },
                        'stylus-loader'
                    ]
                })
            },
        ]
    },
    plugins: [
        new ExtractPlugin('styles.[contentHash:8].css'),
        // new webpack.HotModuleReplacementPlugin(),
        // new webpack.NoEmitOnErrorsPlugin(),
        new webpack.DefinePlugin({
            'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV || 'development'),
            'process.env.VUE_ENV': '"server"', // vue-server-render 会用到
        }),
        // new VueServerPlugin(), // 服务端渲染,输出json文件 vue-ssr-server-bundle.json
        new VueServerPlugin({
            filename: 'vue-ssr-server-bundle'
        })
    ],
}

// dependencies 应用运行时环境 -S
// devDependencies 应用开发时工具环境，运行时不需要 -D

module.exports = config
```


>./build/webpack.config.client.js

```javascript
// ./build/webpack.config.client.js

const path = require('path')
const HTMLPlugin = require('html-webpack-plugin')
const webpack = require('webpack')
const merge = require('webpack-merge')
const ExtractPlugin = require('extract-text-webpack-plugin')
const baseConfig = require('./webpack.config.base')
const VueClientPlugin = require('vue-server-renderer/client-plugin')

const defaultPlugins = [
    new webpack.DefinePlugin({
        'process.env': {
            NODE_ENV: isDev ? '"development"' : '"production"'
        }
    }),
    new HTMLPlugin({
        template: path.join(__dirname, 'template.html')
    }),
    new VueClientPlugin()
]

plugins: defaultPluins.concat([
    new ExtractPlugin('styles.[contentHash:8].css'),
    new webpack.optimize, CommonChunkPlugin({
        name: 'vendor'
    }),
    new webpack.optimize.CommonsChunkPlugin({
        name: 'runtime'
    })
])
```


>./server/server.js

```javascript
// ./server/server.js
// npm i koa -S
// npm i koa-router -S 
// npm i axios -S 服务端也会使用
// npm i memory-fs -D 只在开发环境使用, 类似nodejs fs,扩展了fs.不会把文件写入磁盘，而是写入内存,节省时间,提高效率
// 只有 nodejs可以提供服务端渲染

const Koa = require('koa')

// npm i koa-send -S   // 帮助发送静态资源文件
const send = require('koa-send')

const path = require('path')

const pageRouter = require('./routers/dev-ssr')

const app = new Koa()

// 服务端渲染区分正式与开发环境
const isDev = process.env.NODE_ENV === 'development'

// 记录日志中间件
app.use(async (ctx, next) => {
    try {
        console.log(`request with path ${ctx.path}`)
        await next()
    } catch (error) {
        console.log(error)
        ctx.status = 500 // 服务器发生错误，返回500
        if (isDev) {
            // 开发环境 直接显示在页面上  提供给开发者的页面
            ctx.body = error.message
        } else {
            // 正式环境 提供给用户的优化后的页面
            ctx.body = 'please try again later'
        }
    }
})

// 处理favicon.ico资源
app.use(async (ctx, next) => {
    if (ctx.path === '/favicon.ico') {
        await send(ctx, '/favicon.ico', { root: path.join(__dirname, '../') })
    } else {
        await next()
    }
})

app.use(pageRouter.routes())
    .use(pageRouter.allowedMethods())

const HOST = process.env.HOST || '0.0.0.0'
const PORT = process.env.PORT || 3333

app.listen(PORT, HOST, () => {
    console.log(`server is listening on ${HOST}:${PORT}`)
})
```


>./nodemon.json

```JSON
// ./nodemon.json
// 有修改时自动重启服务
// npm i nodemon -D
{
    "restartable": "rs", // 输入rs命令重启服务
    "ignore": [
        // 忽略对某些文件修改操作的监听 // 配置文件
        ".git",
        "node_modules/**/node_modules",
        ".eslintrc",
        "client",
        "build/webpack.config.client.js",
        "public"
    ],
    "verbose": true,
    "env": {
        "NODE_ENV": "development"
    },
    "ext": "js json ejs", // 监听的文件 
}
```


>./server/routers/dev-ssr.js

```javascript
// ./server/routers/dev-ssr.js
// 处理开发环境的服务端渲染

// 旧版nodejs 不支持import,服务端不需babel编译，直接使用require
const Router = require('koa-router')
const axios = require('axios')
const MemoryFS = require('memory-fs')
const path = require('path')
const fs = require('fs')

const webpack = require('webpack')
const VueServerRenderer = require('vue-server-renderer') // 服务端渲染

const serverRender = require('./server-render')
const serverConfig = require('../../build/webpack.config.server')

const serverCompiler = webpack(serverConfig) //在nodejs中使用webpack编译文件,生成服务端渲染需要的bundle

const mfs = new MemoryFS()
serverCompiler.outputFileSystem = mfs // 指定输出目录在mfs

let bundle // 记录每次生成的打包文件

// 每次文件变化时
serverCompiler.watch({}, (error, states) => {
    // 处理错误
    if (error) throw error
    states = states.toJson()

    // 非打包错误，例如eslint错误
    states.errors.forEach(error => console.log(error))
    states.warnings.forEach(warn => console.log(warn))

    // 读取bundle
    const bundlePath = path.join(
        serverConfig.output.path,
        'vue-ssr-server-bundle.json'
    )

    // bundle=mfs.readFileSync(bundlePath,'utf-8') // 返回字符串
    bundle = JSON.parse(mfs.readFileSync(bundlePath, 'utf-8')) // 转成json
    console.log('new bundle')
})

const handleSSR = async (ctx) => {
    if (!bundle) {
        // 服务器刚刚启动，bundle尚未准备好
        ctx.body = 'wait!!!'
        return
    }

    // 获取webpack打包的js包，用于插入模版
    const clientManifestResp = await axios.get(
        'http://127.0.0.1:8000/public/vue-ssr-client-manifest.json'
    )

    // 实际需要的内容
    const clientManifest = clientManifestResp.data

    // 读取ejs模版文件
    const template = fs.readFileSync(
        path.join(__dirname, '../server.template.ejs'),
        'utf-8'
    )

    const renderer = VueServerRenderer.createBundleRenderer(bundle, {
        inject: false, //取消 vue默认注入
        clientManifest, // 自动生成带script标签的字符串
    })

    await serverRender(ctx, renderer, template)
}

const router = new Router()
router.get('*', handleSSR) // 所有请求都通过handleSSR处理

module.exports = router
```


>./server/server.template.ejs

```javascript
// ./server/server.template.ejs
// 帮助生成服务端渲染文件的模版
// npm i ejs -S

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>

    // <%= %> 会转义特殊字符
    <%- style %> // 不转译
</head>
<body>
    <div id="root">
        <%- appString %>
    </div>
    
    <%- scripts %>
</body>
</html>
```


>

```javascript
// ./server/routers/ssr.js
// 处理正式环境的服务端渲染
```


>./server/router/server-render.js

```javascript
// ./server/router/server-render.js
// 服务端渲染

const ejs = require('ejs')

module.exports = async (ctx, renderer, template) => {
    ctx.headers['Content-Type'] = 'text/html' // 服务端渲染最终返回html页面

    const context = { url: ctx.path } // 用于传入vue-server-renderer里

    try {
        const appString = await renderer.renderToString(context)

        // 渲染
        const html = ejs.render(template, {
            appString,
            style: context.renderStyles(), // 带style标签的完整字符串
            scripts: context.renderScripts(),
        })

        ctx.body = html // 返回完整页面
    } catch (error) {
        console.log('render error:', error)
        throw error
    }
}
```

>./clent/server-entry.js

```javascript
// ./clent/server-entry.js
// 服务端渲染入口文件

import createApp from './create-app'

export default context => {
    return new Promise((resolve, reject) => {
        const { app, router, store } = createApp()

        router.push(context.url) // 服务端环境主动跳转页面，加载组件

        router.onReady(() => {
            // router.onReady一般在服务端渲染用到
            // router.push()后，等到所有异步操作执行完成才会调用本回调函数
            // 开始获取数据

            // 根据 context.url 匹配到响应组件
            const matchedComponents = router.getMatchedComponents()
            if (!matchedComponents.length) {
                // 没有匹配到组件,返回错误，并结束
                return reject(new Error('no component matched'))
            }
            resolve(app)
        })
    })
}
```

>package.json

```JSON
// package.json
{
    "script": {
        "dev:client": "cross-env NODE_ENV=development webpack-dev-server --config build/webpack.config.client.js",
        // "dev:server": "cross-env NODE_ENV=development node server/server.js"
        "dev:server": "nodemon server/server.js", // 启动后，有修改自动重启服务
        "dev": "concurrently \"npm run dev:client\" \"npm run dev:server\" "
    }
}
// 开发时   npm run dev:server     ,npm run dev:client
// 访问 loaclhost:3333 服务端渲染
// npm i concurrently -D   // 一次启动两个服务,接受字符串参数
```

>./create-app.js

```javascript
// ./create-app.js

// 每次服务端渲染，会渲染一个新的app
// 如果单例渲染，会包含上次渲染的状态

import Vue from 'vue'
import VueRouter from 'vue-router'
import Vuex from 'vuex'

import App from './app.vue'
import createStore from './store/store'
import createRouter from './config/router'

import './assets/styles/global.styl'

Vue.use(VueRouter)
Vue.use(Vuex)

// 每次调用都返回一个新对象，防止内存溢出
export default () => {
    const router = createRouter()
    const store = createStore()

    const app = new Vue({
        router,
        store,
        render: h => h(App)
    })

    return { app, router, store }
}
```
<hr>