#### Taro

>一套基于 NodeJS 遵循 React 语法规范的多端统一开发框架

>https://github.com/NervJS/taro

>https://taro.js.org/

>https://nervjs.github.io/taro/

>https://github.com/NervJS/awesome-taro

>https://github.com/NervJS/awesome-taro. 多端统一开发框架 Taro 优秀学习资源汇总

<hr/>

>安装

```bash
/** Quick Start With NPM Or Yarn **/
$ sudo npm install -g @tarojs/cli
$ sudo yarn global add @tarojs/cli
```

>更新 Taro

>更新 taro-cli 工具：

```bash
# taro
$ taro update self
# npm 
npm i -g @tarojs/cli@latest 
# yarn 
yarn global add @tarojs/cli@latest
```

>更新项目中 Taro 相关的依赖，这个需要在你的项目下执行。

```bash
$ taro update project
```
***

>使用命令创建模板项目

```bash
taro init myApp
```

>NPM 5.2+ 也可在不全局安装的情况下使用 npx 创建模板项目：

```bash
npx @tarojs/cli init myApp
```

***
>目前 Taro 已经支持微信/百度/支付宝小程序、H5 以及 ReactNative 等端的代码转换，针对不同端的启动以及预览、打包方式并不一致。

***
#### 微信小程序
>微信开发者工具的项目设置：

>设置关闭 ES6 转 ES5 功能

>设置关闭上传代码时样式自动补全

>设置关闭代码压缩上传

>需要自行下载并打开微信开发者工具，然后选择项目根目录进行预览

>微信小程序编译预览及打包：

```bash
# npm script
$ npm run dev:weapp
$ npm run build:weapp
# 仅限全局安装
$ taro build --type weapp --watch
$ taro build --type weapp
# npx 用户也可以使用
$ npx taro build --type weapp --watch
$ npx taro build --type weapp
```

***
#### 百度小程序
>选择百度小程序模式，需要自行下载并打开百度开发者工具，然后在项目编译完后选择项目根目录下 dist 目录进行预览。

>百度小程序编译预览及打包：

```bash
# npm script
$ npm run dev:swan
$ npm run build:swan
# 仅限全局安装
$ taro build --type swan --watch
$ taro build --type swan
# npx 用户也可以使用
$ npx taro build --type swan --watch
$ npx taro build --type swan
```

***
#### 支付宝小程序

>选择支付宝小程序模式，需要自行下载并打开支付宝小程序开发者工具，然后在项目编译完后选择项目根目录下 dist 目录进行预览。

>支付宝小程序编译预览及打包：

```bash
# npm script
$ npm run dev:alipay
$ npm run build:alipay
# 仅限全局安装
$ taro build --type alipay --watch
$ taro build --type alipay
# npx 用户也可以使用
$ npx taro build --type alipay --watch
$ npx taro build --type alipay
```
***
#### H5
>H5 模式，无需特定的开发者工具，在执行完下述命令之后即可通过浏览器进行预览。

>H5 编译预览及打包：

```bash
# npm script
$ npm run dev:h5
# 仅限全局安装
$ taro build --type h5 --watch
# npx 用户也可以使用
$ npx taro build --type h5 --watch
```
***
#### React Native
>React Native 端运行需执行如下命令，React Native 端相关的运行说明请参见 React Native 教程。

```bash
# npm script
$ npm run dev:rn
# 仅限全局安装
$ taro build --type rn --watch
# npx 用户也可以使用
$ npx taro build --type rn --watch
```

***
#### Taro-UI

>npm install taro-ui

>或者使用自定义主题版本

>npm install taro-ui@next

>安装最新预览版

>npm install taro-ui@next

>Noticebar 默认换行

>禁止换行需要增加 css

```less
white-space:nowrap;
```

***
###### 环境判断
>process.env.TARO_ENV