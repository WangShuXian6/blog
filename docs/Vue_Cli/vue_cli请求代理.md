#### vue-cli 请求代理

>使用 vue-cli-plugin-axios-ts

>https://www.npmjs.com/package/vue-cli-plugin-axios-ts

>https://github.com/axios/axios

>vue ui 安装 vue-cli-plugin-axios-ts 插件

***

>开发环境下跨域 

>https://cli.vuejs.org/zh/config/#css-loaderoptions

>https://github.com/chimurai/http-proxy-middleware#proxycontext-config

>项目根目录下新建 

>vue.config.js

```javascript
module.exports = {
  // baseUrl: process.env.NODE_ENV === 'production'
  //     ? '/wechat/'
  //     : '/',

  devServer: {
    proxy: {
      '/app': {
        target: 'http://taoke-admin.yueyuweb.cn/v1/app/',
        ws: true,
        changeOrigin: true,
        pathRewrite: {
          '^/app': ''
        }
      }
    }
  }
}
```
>Home..vue

```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>
  </div>
</template>

<script lang="ts">
  import {Component, Vue} from "vue-property-decorator";
  import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src

  @Component({
    components: {
      HelloWorld,
    },
  })
  export default class Home extends Vue {
    created() {
      console.log("home");
      console.log(this.$axios)
      this.$axios({
        method: "get",
        url: "/app/notices",
        headers: {
          nosign: 1,
          memberid: "3aecqG4PAW7sTaM8nemT5DEU41ZZn38cGLcbbJvX"
        }
      }).then((res) => {
        console.log("res---", res)
      }).catch((error) => {
        console.log("error---", error)
      })

    }
  }
</script>
```
***
