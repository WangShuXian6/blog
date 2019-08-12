#### 安装
```javascript
npm install wepy-cli -g
wepy init standard my-project
```
#### 更新
```
wepy upgrade
```
初始化项目后的注意事项：
>关闭ES6转ES5,由WePY自动处理

>关闭样式自动补全,由WePY自动处理

>关闭代码压缩

#### 安装util
>否则可能报错 module "npm/lodash/_nodeUtil.js" is not defined

```
npm i util --save-dev
```
#### 安装wepy-compiler-typescript
>https://www.npmjs.com/package/wepy-compiler-typescript

```bash
npm install wepy-compiler-typescript --save-dev
```
```vue
<script lang="typescript" src ="./index.ts"></script>
```
#### 安装less autoprefix
```
npm install less-plugin-autoprefix --save-dev
```
>wepy.config.js

```javascript
const LessPluginAutoPrefix = require('less-plugin-autoprefix');

//...
//...

  compilers: {
    less: {
      compress: true,
      plugins: [new LessPluginAutoPrefix({browsers: ['Android >= 2.3', 'Chrome > 20', 'iOS >= 6']})]
    }

```
#### wepy-async-function
```
npm install wepy-async-function --save
```

>wepy.config.js

```javascript
babel: {
            "presets": [
                "env"
            ],
            "plugins": [
                "transform-export-extensions",
                "syntax-export-extensions"
            ]
        }
```
>app.wpy

```vue
import 'wepy-async-function'

export default class extends wepy.app {

    constructor () {
        super();
        this.use('promisify');
    }

}
```
```
wepy build --no-cache
```
>

#### promise-polyfill
```
npm install promise-polyfill --save
```
>app.wpy

```vue
import Promise from 'promise-polyfill'

export default class extends wepy.app {

    constructor () {
        super();
        this.use('promisify');
    }

}
```
>详细说明 https://github.com/Tencent/wepy/wiki/升级指南

#### wepy 文件压缩插件
>https://github.com/Tencent/wepy/tree/master/packages/wepy-plugin-filemin

>支持css，xml，json的压缩

>安装

>npm install wepy-plugin-filemin --save-dev

>配置wepy.config.js

```javascript
module.exports.plugins = {
    'filemin': {
        filter: /\.(json|wxml|xml)$/
    }
};
```

#### wepy生产文件去注释
>配置wepy.config.js

```javascript
module.exports.plugins = {
    uglifyjs: {
      filter: /\.js$/,
      config: {
        comments: false // 去注释
      }
    },
    imagemin: {
      filter: /\.(jpg|png|jpeg)$/,
      config: {
        jpg: {
          quality: 80
        },
        png: {
          quality: 80
        }
      }
    },
    'filemin': {
      filter: /\.(json|wxml|xml)$/
    }
  }
```

<hr>

#### 快速开始配置
>package.json

```JSON
{
  "name": "news",
  "version": "0.0.2",
  "description": "A WePY project",
  "main": "dist/app.js",
  "scripts": {
    "dev": "wepy build --watch",
    "build": "cross-env NODE_ENV=production wepy build --no-cache",
    "dev:web": "wepy build --output web",
    "clean": "find ./dist -maxdepth 1 -not -name 'project.config.json' -not -name 'dist' | xargs rm -rf",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "wepy": {
    "module-a": false,
    "./src/components/list": "./src/components/wepy-list.wpy"
  },
  "author": "王树贤 <airmusic@msn.com>",
  "license": "MIT",
  "dependencies": {
    "crypto-js": "^3.1.9-1",
    "promise-polyfill": "^8.0.0",
    "redux": "^3.7.2",
    "redux-actions": "^2.2.1",
    "redux-promise": "^0.5.3",
    "wepy-redux": "^1.5.3",
    "wepy": "^1.6.0",
    "wepy-async-function": "^1.4.4",
    "wepy-com-toast": "^1.0.2"
  },
  "devDependencies": {
    "babel-eslint": "^7.2.1",
    "babel-plugin-transform-class-properties": "^6.24.1",
    "babel-plugin-transform-decorators-legacy": "^1.3.4",
    "babel-plugin-transform-export-extensions": "^6.22.0",
    "babel-plugin-transform-object-rest-spread": "^6.26.0",
    "babel-preset-env": "^1.6.1",
    "cross-env": "^5.1.3",
    "eslint": "^3.18.0",
    "eslint-config-standard": "^7.1.0",
    "eslint-friendly-formatter": "^2.0.7",
    "eslint-plugin-html": "^2.0.1",
    "eslint-plugin-promise": "^3.5.0",
    "eslint-plugin-standard": "^2.0.1",
    "less-plugin-autoprefix": "^1.5.1",
    "util": "^0.11.0",
    "wepy-compiler-babel": "^1.5.1",
    "wepy-compiler-less": "^1.3.10",
    "wepy-compiler-typescript": "^1.5.9",
    "wepy-eslint": "^1.5.3",
    "wepy-plugin-imagemin": "^1.5.3",
    "wepy-plugin-uglifyjs": "^1.3.7"
  }
}

```
>wepy.config.js

```javascript
const path = require('path');
var prod = process.env.NODE_ENV === 'production';
const LessPluginAutoPrefix = require('less-plugin-autoprefix');

module.exports = {
  wpyExt: '.wpy',
  eslint: true,
  cliLogs: !prod,
  build: {
    web: {
      htmlTemplate: path.join('src', 'index.template.html'),
      htmlOutput: path.join('web', 'index.html'),
      jsOutput: path.join('web', 'index.js')
    }
  },
  resolve: {
    alias: {
      counter: path.join(__dirname, 'src/components/counter'),
      '@': path.join(__dirname, 'src')
    },
    aliasFields: ['wepy', 'weapp'],
    modules: ['node_modules']
  },
  compilers: {
    less: {
      compress: true,
      plugins: [new LessPluginAutoPrefix({browsers: ['Android >= 2.3', 'Chrome > 20', 'iOS >= 6']})]
    },
    /*sass: {
      outputStyle: 'compressed'
    },*/
    babel: {
      sourceMap: true,
      presets: [
        'env'
      ],
      plugins: [
        'transform-class-properties',
        'transform-decorators-legacy',
        'transform-object-rest-spread',
        'transform-export-extensions',
        "syntax-export-extensions",
      ]
    }
  },
  plugins: {
  },
  appConfig: {
    noPromiseAPI: ['createSelectorQuery']
  }
}

if (prod) {

  // 压缩sass
  // module.exports.compilers['sass'] = {outputStyle: 'compressed'}

  // 压缩js
  module.exports.plugins = {
    uglifyjs: {
      filter: /\.js$/,
      config: {
      }
    },
    imagemin: {
      filter: /\.(jpg|png|jpeg)$/,
      config: {
        jpg: {
          quality: 80
        },
        png: {
          quality: 80
        }
      }
    }
  }
}

```
>app.wpy

```
<style lang="less">
  .container {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    box-sizing: border-box;
  }
</style>

<script>
  import wepy from 'wepy'
  import 'wepy-async-function'
  import Promise from 'promise-polyfill'

  import { setStore } from 'wepy-redux'
  import configStore from './store'

  const store = configStore()
  setStore(store)

  export default class extends wepy.app {
    config = {
      pages: [
        'pages/index'
      ],
      window: {
        backgroundTextStyle: 'light',
        navigationBarBackgroundColor: '#fff',
        navigationBarTitleText: 'WeChat',
        navigationBarTextStyle: 'black'
      }
    }

    globalData = {
      userInfo: null
    }

    constructor () {
      super()
      this.use('requestfix')
      this.use('promisify');
    }

    onLaunch() {

    }

    getUserInfo(cb) {
      const that = this
      if (this.globalData.userInfo) {
        return this.globalData.userInfo
      }
      wepy.getUserInfo({
        success (res) {
          that.globalData.userInfo = res.userInfo
          cb && cb(res.userInfo)
        }
      })
    }
  }
</script>

```

***

>wepy d.ts 类型申明

>https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/wepy/index.d.ts

```bash
npm install --save-dev @types/wepy
```