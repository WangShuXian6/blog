#### NUXT SEO

#### nuxt.js中添加统计代码，添加百度统计，或者google的统计

##### 添加百度统计
>在 plugins 目录下创建 plugins/baiduGa.js 文件

>plugins/baiduGa.js

```javascript
export default ({app: {router}, store}) => {
  /* 每次路由变更时进行pv统计 */
  router.afterEach((to, from) => {
    /* 告诉增加一个PV */
    try {
      window._hmt = window._hmt || []
      window._hmt.push(['_trackPageview', to.fullPath])
    } catch (e) {
    }
  })
}
```
>然后，我们需要告诉 Nuxt.js 将该插件导入主应用中，在nuxt.config.js配置如下

>nuxt.config.js

```javascript
head: {
    script: [
     {src: 'https://hm.baidu.com/hm.js?****'}, /*引入百度统计的js*/
    ]
},
plugins: [
   '@/plugins/ga.js', /*百度统计*/
]
```

<hr>

##### 添加google统计 官方
>https://zh.nuxtjs.org/faq/google-analytics/

<hr>