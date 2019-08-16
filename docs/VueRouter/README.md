#### Vue-router

>npm i vue-router -S

#### routes.js 路由映射关系文件

```javascript
// routes.js 路由映射关系文件

import Todo from '../views/todo/todo.vue'
import Login from '../views/login/login.vue'

export default [
    {
        path: '/app',
        component: Todo,
        name: 'todo'
    },
    {
        path: '/login',
        component: Login,
        name: 'login'
    }
]
```
<hr>

##### router.js 路由设置文件

>不推荐

>每次调用会缓存路由文件，不被释放，在服务端渲染会累加缓存，内存溢出

```javascript
import Router from 'vue-router'
import routes from './routes'

// 全局唯一的路由() 不推荐
const router = new Router({
    routes
})

export default router
```
<hr>

##### router.js 路由设置文件 适用于服务端渲染的路由

>适用于服务端渲染的路由

```javascript
import Router from 'vue-router'
import routes from './routes'

// 适用于服务端渲染，防止内存溢出
// 每次 import 该router都将创建一个新的路由
export default () => {
    return new Router({
        routes
    })
}
```
<hr>

> index

>注入路由，每个组件都可获得路由实例

```javascript
// index.js
import Vue from 'vue'
import VueRouter from 'vue-router'
import App from './app.vue'

import './assets/styles/global.styl'
import createRouter from './config/router'

Vue.use(VueRouter)

const router = createRouter()

new Vue({
    router,
    render: (h) => h(App)
}).$mount('#root')
```
<hr>

##### Vue-router 配置

>路由跳转，redirect: '/app', // 跳转到 path '/app'

```javascript
// routes.js 路由映射关系文件

import Todo from '../views/todo/todo.vue'
import Login from '../views/login/login.vue'

export default [
    {
        path: '/',
        redirect: '/app', // 跳转到 path '/app'
    },
    {
        path: '/app',
        component: Todo,
        name: 'todo'
    },
    {
        path: '/login',
        component: Login,
        name: 'login'
    }
]
```
<hr>


##### 服务端适合SEO的history路由

```javascript
import Router from 'vue-router'
import routes from './routes'

// #hash路由用来表示定位，不是记录路由状态，不会被搜索引擎解析，没有SEO

// 适用于服务端渲染，防止内存溢出
// 每次 import 该router都将创建一个新的路由
export default () => {
    return new Router({
        routes,
        mode: 'history' // 没有#hash
    })
}
```
<hr>

##### 路由配置参数

```javascript
import Router from 'vue-router'
import routes from './routes'

// #hash路由用来表示定位，不是记录路由状态，不会被搜索引擎解析，没有SEO

// 适用于服务端渲染，防止内存溢出
// 每次 import 该router都将创建一个新的路由
export default () => {
    return new Router({
        routes,
        mode: 'history', // 没有#hash
        base: '/base/', //基路径，所有路由path前加上了/base/ loaclhost/app =>  loaclhost/base/app   //非强制，原路径仍有效
        linkActiveClass: 'active-link', // 在路由部分匹配时激活
        linkExactActiveClass: 'exact-active-link', // 在路由完全匹配时激活
        scrollBehavior(to, from, savedPosition) {
            if (savedPosition) {
                // 如果有滚动信息，则保存并返回。用来记录滚动位置
                return savedPosition
            } else {
                // 没有保存位置则滚动到页面顶部
                return { x: 0, y: 0 }
            }
        },
        parseQuery(query) {
            // 对页面参数的转换 localhost/app?a=1&b=2
            // 将页面参数由字符串转为json对象
        },
        stringifyQuery(obj) {
            // 将页面参数由json对象转为字符串
        },
        fallback: true, // 对于不支持history模式的浏览器，自动转为#hash模式，防止不支持history时每次跳转重新请求数据，变为多页应用
    })
}
```
<hr>

#####  Vue-router 路由参数传递
>app.vue

```javascript
// app.vue
// router-link 通过事件将a标签的默认跳转改为前端路由跳转
<template>
  <div id="app">
    <div id="nav">
        // path跳转
      <router-link to="/app">Home</router-link>
      <router-link to="/login">About</router-link>

        // 路由名称跳转,传入对象，需:解析
        <router-link :to="{name:'app'}">Home</router-link>
    </div>
    <router-view/>
  </div>
</template>
```
<hr>

>routes.js 路由映射关系文件

```javascript
// routes.js 路由映射关系文件

import Todo from '../views/todo/todo.vue'
import Login from '../views/login/login.vue'

export default [
    {
        path: '/',
        redirect: '/app', // 跳转到 path '/app'
    },
    {
        path: '/app',
        component: Todo,
        name: 'todo',
        meta: {
            // 页面元信息，适于SEO,用于搜索引擎解析排序保存在路由中
            // 可以在得到路由对象时修改信息
            title: 'this is app',
            description: 'app'
        },
        // 子路由,嵌套路由
        // 在Todo组件中放置 <router-view/> 以显示路由页面
        children: [
            {
                path: '/test',
                component: Login,
            }
        ]
    },
    {
        path: '/login',
        component: Login,
        name: 'login'
    }
]
```
<hr>

##### 路由过度动画

```javascript
// app.vue
// router-link 通过事件将a标签的默认跳转改为前端路由跳转
<template>
  <div id="app">
    <div id="nav">
        // path跳转
      <router-link to="/app">Home</router-link>
      <router-link to="/login">About</router-link>

        // 路由名称跳转,传入对象，需:解析
        <router-link :to="{name:'app'}">Home</router-link>
    </div>
    <transition name="fade">
        // 全部组件使用组件切换过度动画
        <router-view/>
    </transition>
  </div>
</template>

<style lang="less">

    // transition上的class: 'fade' +  '-enter-active'
    .fade-enter-active, 
    .fade-leave-active{
        transition:opacity 0.5s;
    }

    .fade-enter,
    .fade-leave-to{
        opacity:0
    }
</style>
```
<hr>

##### 路由传参

>app.vue

```javascript
// app.vue
// router-link 通过事件将a标签的默认跳转改为前端路由跳转
<template>
  <div id="app">
    <div id="nav">
        // path跳转
      <router-link to="/app/123">Home</router-link>
      <router-link to="/login">About</router-link>

        // 路由名称跳转,传入对象，需:解析
        <router-link :to="{name:'app'}">Home</router-link>
    </div>
    <transition name="fade">
        // 全部组件使用组件切换过度动画
        <router-view/>
    </transition>
  </div>
</template>



<style lang="less">

    // transition上的class: 'fade' +  '-enter-active'
    .fade-enter-active, 
    .fade-leave-active{
        transition:opacity 0.5s;
    }

    .fade-enter,
    .fade-leave-to{
        opacity:0
    }
</style>
```
>

```javascript
// 组件内获取路由参数

mounted(){
    // 当前匹配到的路由的所有信息
    console.log(this.$route)
    // fullPath,hash,matched,meta,params,path,query
    // params:{id:'123'}
}
```
<hr>

##### 路由传参-props:true

```javascript
// 路由传参-props:true ，推荐,解耦，更适合组件化
// routes.js 路由映射关系文件

import Todo from '../views/todo/todo.vue'
import Login from '../views/login/login.vue'

export default [
    {
        path: '/',
        redirect: '/app', // 跳转到 path '/app'
    },
    {
        path: '/app/:id', // 只能匹配地址 /app/xxx
        props: true, // 会将path 的id当作props传入组件Todo，组件内直接this.id取得
        component: Todo,
        name: 'todo',
    },
    {
        path: '/login',
        props: {
            // 也可以自定义 props
            id: '456',
        },
        component: Login,
        name: 'login'
    },
    {
        path: '/login2/:id',
        props: (route) => ({
            // 通过方法生成props对象
            // 自动传入路由对象
            id: route.query.b, // 通过query参数生成props
        }),
        component: Login2,
        name: 'login2'
    }
]
```
<hr>

##### 单页多routerView

```javascript
// 单页多routerView
// 适用于三栏布局，顶部切换时左侧栏变动较大
// routes.js 路由映射关系文件

import Todo from '../views/todo/todo.vue'
import Login from '../views/login/login.vue'

export default [
    {
        path: '/',
        redirect: '/app', // 跳转到 path '/app'
    },
    {
        path: '/app/:id', // 只能匹配地址 /app/xxx
        // props: true, // 会将path 的id当作props传入组件Todo，组件内直接this.id取得
        components: {
            // 单页面多个 router-view
            default: Todo, // 未命名的默认 router-view 所显示的组件为Todo
            routerViewA: Login, //命名为routerViewA的router-view 所显示的组件为Login
        },
        name: 'todo',
    },
    {
        path: '/login',
        props: {
            // 也可以自定义 props
            id: '456',
        },
        components: {
            // 单页面多个 router-view
            default: Login, // 未命名的默认 router-view 所显示的组件为Login
            routerViewA: Todo, //命名为routerViewA的router-view 所显示的组件为Todo
        },
        name: 'login'
    },
    {
        path: '/login2/:id',
        props: (route) => ({
            // 通过方法生成props对象
            // 自动传入路由对象
            id: route.query.b, // 通过query参数生成props
        }),
        component: Login2,
        name: 'login2'
    }
]
```
<hr>

#### 导航守卫

##### 全局导航守卫,index.js

```javascript
// index.js
import Vue from 'vue'
import VueRouter from 'vue-router'
import App from './app.vue'

import './assets/styles/global.styl'
import createRouter from './config/router'

Vue.use(VueRouter)

const router = createRouter()

// 全局导航守卫

// 每次路由跳转时触发
// 适合数据校验
router.beforeEach((to, from, next) => {
    console.log('before each invoked')

    if (to.fullPath === '/app') {
        // next('/login') // 跳转到登陆页
        next({
            path: '/login',
            replace // 本次跳转记录消失，防止跳回本页
        })
    } else {
        next()
    }

    // 执行next后路由才会真正跳转,否则不跳转
    // next()
})

router.beforeResolve((to, from, next) => {
    console.log('before Resolve invoked')

    // 执行next后路由才会真正跳转,否则不跳转
    next()
})

// 每次路由跳转完成后触发，无需next()
router.afterEach((to, from) => {
    console.log('after Each invoked')
})

new Vue({
    router,
    render: (h) => h(App)
}).$mount('#root')
```
<hr>

##### 路由配置钩子

>routes.js

```javascript
// 路由配置钩子
// routes.js 路由映射关系文件

import Todo from '../views/todo/todo.vue'
import Login from '../views/login/login.vue'

export default [
    {
        path: '/',
        redirect: '/app', // 跳转到 path '/app'
    },
    {
        path: '/app/:id', // 只能匹配地址 /app/xxx
        // props: true, // 会将path 的id当作props传入组件Todo，组件内直接this.id取得
        component: Todo,
        name: 'todo',
        beforeEnter(to, from, next) {
            // 每次进入路由前触发
            // 在before each 和 before resolve 中间触发
            next() // 不执行next则不会进入路由
        },
    },
    {
        path: '/login',
        props: {
            // 也可以自定义 props
            id: '456',
        },
        component: Login,
        name: 'login'
    },
]
```
<hr>

##### 组件内路由钩子

```javascript
// 组件内路由钩子

// 进入之前，用于数据获取
beforeRouteEnter(to, from, next){
    // 此时得不到组件实例this
    next(vm => {
        // 自动得到进入路由后的组件实例vm
        // 此时可以得到组件实例vm
        console.log(vm)
    })
}

// 路由跳转时，组件不变，路由参数改变时触发
// 用于数据获取,推荐
beforeRouteUpdate(to, from, next){
    // 若触发update,表示缓存了组件，不会再触发beforeRouteEnter,mounted
    next()
}

// 路由离开时
// 用于修改表单后未保存，离开之前提醒是否离开
beforeRouteLeave(to, from, next){
    // next()
    if (global.confirm('are you sure?')) {
        // 确定后才会离开
        next()
    }
}

// 触发顺序
// beforeEach
// app beforeEnter
// todo beforeEnter
// before resolve
// after each
```
<hr>

#### 异步加载组件,提高首屏加载速度

>routes.js

```javascript
// 路由配置钩子
// routes.js 路由映射关系文件
// 异步加载组件需要安装 npm i babel-plugin-syntax-dynamic-import
// .babelrc 配置 "plugins":["syntax-dynamic-import"]

// import Todo from '../views/todo/todo.vue'
import Login from '../views/login/login.vue'

export default [
    {
        path: '/',
        redirect: '/app', // 跳转到 path '/app'
    },
    {
        path: '/app/:id', // 只能匹配地址 /app/xxx
        // props: true, // 会将path 的id当作props传入组件Todo，组件内直接this.id取得
        // component: Todo,
        component: ()=> import('../views/todo/todo.vue'), // 需要时才会加载该组件，异步加载
        name: 'todo',
        beforeEnter(to, from, next) {
            // 每次进入路由前触发
            // 在before each 和 before resolve 中间触发
            next() // 不执行next则不会进入路由
        },
    },
    {
        path: '/login',
        props: {
            // 也可以自定义 props
            id: '456',
        },
        component: Login,
        name: 'login'
    },
]
```


>https://github.com/Jokcy/vue-todo-tech.git
```javascript

```
<hr>

