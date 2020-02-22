#### 使用vue-meta处理元信息
>npm i vue-meta -S

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

            // 服务端渲染的时候使用vue-meta的方式
            context.meta = app.$meta()

            resolve(app)
        })
    })
}
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

        // 从context获取meta
        const { title } = context.meta.inject()

        // 渲染
        const html = ejs.render(template, {
            appString,
            style: context.renderStyles(), // 带style标签的完整字符串
            scripts: context.renderScripts(),
            title: title.text(), // 带标签
        })

        ctx.body = html // 返回完整页面
    } catch (error) {
        console.log('render error:', error)
        throw error
    }
}
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
    // <title>Document</title>
    <%- title %>

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

>./client/client-entry.js

```javascript
// ./client/client-entry.js
// 服务端渲染的客户端入口

import createApp from './create-app'

const { app, router } = createApp()

// 路由就绪后渲染
router.onReady(() => {
    app.$mount('#root')
})
```

>App.vue

```vue
// App.vue
<template>
    <div>
        <p>{{ textA }}</p>
        <p>{{ textPlus }}</p>
    </div>
</template>

<script>
    // Actions,Mutations 帮助函数
    import {
        mapState,
        mapGetters,
        mapActions, // 对应异步操作
        mapMutations // 对应同步操作
    } from 'vuex'
    
    export default {
        metaInfo: {
            title: 'wang \'s App', // 进入页面时vue-meta会自动修改页面title值,下级组件的meta 会覆盖上级
        },
    
        methods: {
            ...mapActions(['updateCountAsync', 'a/add', 'textAction']), // b模块未声明命名空间，textAction无需写模块名
            ...mapMutations(['updateCount', 'a/updateText']), // 帮助函数声明模块化的 updateText mutation
            // Vuex 默认会将全部mutation,包括模块化的，放入全局mutations
            // 启用命名空间后，调用模块内的mutations --- a/updateText
        },
    
        mounted() {
            this['a/add']()
    
            this.textAction() // b模块未声明命名空间
        },
    
        computed: {
            // 模块后，带命名空间的调用
            // textA(){
            //     return this.$store.state.a.text
            // }
    
            // 使用帮助函数调用模块化的 state
            ...mapState({
                textA: state => state.a.text, // 必须使用方法返回
                textC: state => state.c.text, // 使用动态注册的模块c
            }),
    
            // 使用帮助函数调用模块化的 启用命名空间的 // 'a/textPlus'
            // 'a/textPlus' 无法在模版中直接使用
            // ...mapGetters(['fullName', 'a/textPlus'])
    
            // 使用帮助函数 重命名getters
            ...mapGetters({
                'fullName': 'fullName', // 全局的getter
                'textPlus': 'a/textPlus', // a模块启用命名空间的getter - 'a/textPlus' 重命名为 'textPlus',可在模版使用
            })
        }
    }
</script>
```