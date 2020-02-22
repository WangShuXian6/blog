#### Nuxt Vue.js 通用应用框架
>https://zh.nuxtjs.org

>https://github.com/nuxt/nuxt.js

>https://github.com/nuxt-community

##### 安装

```bash
npm install -g vue-cli

```

***
>生成生产静态文件需要首先开启 开发环境

***


指令 | 描述
-- | --
nuxt | 开启一个监听3000端口的服务器，同时提供hot-reloading功能
nuxt build | 构建整个应用，压缩合并JS和CSS文件（用于生产环境）
nuxt start | 开启一个生产模式的服务器（必须先运行nuxt build命令）
nuxt generate | 构建整个应用，并为每一个路由生成一个静态页面（用于静态服务器）


##### NUXT 架构
![nuxt](https://user-images.githubusercontent.com/30850497/48045770-9689ee80-e1cc-11e8-961a-6367d9a5d0f1.png)

***