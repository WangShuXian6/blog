#### Vuex
>npm i veux -S

> store.js

```javascript
// store.js

import Vuex from 'vuex'
import Vue from 'vue'

Vue.use(Vuex)

// 会缓存全局唯一的store,服务端渲染会内存溢出
const store = new Vuex.Store({
    // 初始化store
    state: {
        count: 0
    },

    // 修改store
    mutations: {
        updateCount(state, countNum) {
            state.count = countNum
        }
    }
})

export default store
```

> index.js

```javascript
// index.js

import store from './store/store'

new Vue({
    router,
    store,
    render: (h) => h(App)
}).$mount('#root')
```

> App.vue

```javascript
// App.vue
// 直接调用store

mounted(){
    console.log(this.$store) // 应用入口传入的store对象

    // 修改store
    // 传入mutation,countNum
    this.$store.commit('updateCount', 1)
}

computed: {
    count(){
        // 获得count
        return this.$store.state.count
    }
}

```
<hr>

#### 服务端渲染

> store.js

```javascript
// store.js

import Vuex from 'vuex'

//服务端渲染用法，返回函数，防止内存溢出
export default ()=>{
    return new Vuex.Store({
        // 初始化store
        state: {
            count: 0
        },
    
        // 修改store
        mutations: {
            updateCount(state, countNum) {
                state.count = countNum
            }
        }
    })
}
```

> index.js

```javascript
// index.js
// 服务端渲染配置store

import Vue from 'vue'
import VueRouter from 'vue-router'
// import store from './store/store'
import Vuex from 'vuex'
import createStore from './store/store'

vue.use(VueRouter)
Vue.use(Vuex)

const router = createRouter()
const store = createStore()

new Vue({
    router,
    store,
    render: (h) => h(App)
}).$mount('#root')
```

> App.vue

```javascript
// App.vue
// 直接调用store

mounted(){
    console.log(this.$store) // 应用入口传入的store对象

    // 修改store
    // 传入mutation,countNum
    this.$store.commit('updateCount', 1)
}

computed: {
    count(){
        // 获得count
        return this.$store.state.count
    }
}

```
<hr>

#### 重新组织,单模块的store
> ./store/state/state.js

```javascript
// ./store/state/state.js
// 单模块的state

// 所有值都要事先声明，才能实现响应式
export default {
    count: 0
}
```
> ./store/mutations/mutations.js

```javascript
// ./store/mutations/mutations.js

export default {
    updateCount(state, countNum) {
        state.count = countNum
    }
}
```
> ./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    return new Vuex.Store({
        // 初始化store
        // 单独的state,更好的组织state
        state: defaultState,

        // 修改store
        mutations, // ES6
    })
}
```
<hr>

#### Vuex : state,getters

> ./store/state/state.js

```javascript
// ./store/state/state.js
// 单模块的state

// 所有值都要事先声明，才能实现响应式
export default {
    count: 0,
    firstName: 'harry',
    lastName: 'potter',
}
```
> ./store/getters/getters.js

```javascript
// ./store/getters/getters.js
// 单模块
// 对象

// getters类似computed属性
// 用于生成在应用内可直接使用的数据，方便实用,无需在computed中格式化，使代码可复用,易维护
// 用来将后端数据格式化为适合view层显示的数据
export default {
    fullName(state) {
        // 默认接受state
        return `${state.firstNmae} ${state.lastName}`
    }
}

```
> ./store/mutations/mutations.js

```javascript
// ./store/mutations/mutations.js

export default {
    updateCount(state, countNum) {
        state.count = countNum
    }
}
```
> ./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    return new Vuex.Store({
        // 初始化store
        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6
    })
}
```
> index.js

```javascript
// index.js
// 服务端渲染配置store

import Vue from 'vue'
import VueRouter from 'vue-router'
// import store from './store/store'
import Vuex from 'vuex'
import createStore from './store/store'

vue.use(VueRouter)
Vue.use(Vuex)

const router = createRouter()
const store = createStore()

new Vue({
    router,
    store,
    render: (h) => h(App)
}).$mount('#root')
```
> App.vue

```javascript
// App.vue
// 直接调用store

mounted(){
    console.log(this.$store) // 应用入口传入的store对象

    // 修改store
    // 传入mutation,countNum
    this.$store.commit('updateCount', 1)
}

computed: {
    count(){
        // 获得count
        return this.$store.state.count
    },
    fullName(){
        return this.$store.getters.fullName
    }
}

```
<hr>

#### 使用帮助方法，更简洁的使用store中的数据，无需声明

#### mapState,mapGetters

> App.vue

```javascript
// App.vue
// 不直接调用store

// 支持对象扩展语法  npm i babel-preset-stage-1 -D
// .babelrc "presets":["env","stage-1"]
// 使用帮助方法，更简洁的使用store中的数据，无需声明
import {
    mapState,
    mapGetters
} from 'vuex'

mounted(){
    console.log(this.$store) // 应用入口传入的store对象

    // 修改store
    // 传入mutation,countNum
    this.$store.commit('updateCount', 1)
}

computed: {
    // ...mapState(['count']), // 自动获取 this.$store.state.count

    // ...mapState({
    //     counter:'count', // {{counter}},更改名称的用法,自动获取 this.$store.state.count
    // }),

    ...mapState({
        // 推荐：使用函数，更改名称的用法,自动获取 this.$store.state.count
        // 适合计算
        counter: (state) => state.count,
    }),
    // count(){
    //     // 获得count
    //     return this.$store.state.count
    // },

    ...mapGetters(['fullName']), // 自动获取 this.$store.getters.fullName

    // fullName(){
    //     return this.$store.getters.fullName
    // }
}

```
<hr>

#### Vuex : mutation,action

>所有对state的修改都使用mutation,必须是同步操作，不能是异步操作

##### mutation

> ./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

const isDev = process.env.NODE_ENV === 'development'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    return new Vuex.Store({
        // 初始化store

        // strict:true, // 防止从外部修改state, //this.$state.count=2 // 仅限开发环境，正式环境需关闭
        strict: isDev,

        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6
    })
}
```
<hr>

##### action

>./actions/actions.js

>异步修改store,用来处理异步数据修改的方法,适合异步请求数据

```javascript
// ./actions/actions.js

// 一个对象
// 异步修改store,用来处理异步数据修改的方法,适合异步请求数据

export default {
    updateCountAsync(store, data) {
        // 接受整个store对象 + 触发action时传入的唯一一个参数，一般为对象
        setTimeout(() => {
            store.commit('updateCount', data.num)
        }, data.time)
    }
}
```

>./store/mutations/mutations.js

```javascript
// ./store/mutations/mutations.js
// 适合同步修改数据
export default {
    updateCount(state, countNum) {
        state.count = countNum
    }
}
```

>./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

// 导入默认的actions
import actions from './actions/actions'

const isDev = process.env.NODE_ENV === 'development'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    return new Vuex.Store({
        // 初始化store

        // strict:true, // 防止从外部修改state, //this.$state.count=2 // 仅限开发环境，正式环境需关闭
        strict: isDev,

        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6

        // 通过提交mutation异步修改store
        actions,
    })
}
```

>./store/state/state.js

```javascript
// ./store/state/state.js
// 单模块的state

// 所有值都要事先声明，才能实现响应式
export default {
    count: 0,
    firstName: 'harry',
    lastName: 'potter',
}
```

>App.vue

```javascript
mounted(){
    // 提交action必须使用 this.$store.dispatch
    this.$store.dispatch(
        'updateCountAsync', // action的方法名字面量
        {
            // 提交action时传入的参数对象
            num: 5,
            time: 2000,
        }
    )
}
```
<hr>

##### Actions,Mutations 帮助函数

>App.vue

```javascript
// App.vue

// Actions,Mutations 帮助函数
import {
    mapState,
    mapGetters,
    mapActions, // 对应异步操作
    mapMutations // 对应同步操作
} from 'vuex'

methods: {
    ...mapActions(['updateCountAsync']),
    ...mapMutations(['updateCount'])
}

mounted(){
    // 提交action必须使用 this.$store.dispatch
    // this.$store.dispatch(
    //     'updateCountAsync', // action的方法名字面量
    //     {
    //         // 提交action时传入的参数对象
    //         num: 5,
    //         time: 2000,
    //     }
    // )

    // 使用帮助函数后可以简化调用action,无需传入方法名，只传参数对象
    this.updateCountAsync({
        num: 5,
        time: 2000
    })

    // 使用帮助函数后可以简化调用mutation,无需传入方法名，只传参数对象
    this.updateCount({
        num: i++
    })
}
```
<hr>

#### Vuex 模块化

>./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

// 导入默认的actions
import actions from './actions/actions'

const isDev = process.env.NODE_ENV === 'development'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    return new Vuex.Store({
        // 初始化store

        // strict:true, // 防止从外部修改state, //this.$state.count=2 // 仅限开发环境，正式环境需关闭
        strict: isDev,

        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6

        // 通过提交mutation异步修改store
        actions,

        // 模块化,带作用域
        modules: {
            a: {
                state: {
                    text: 1
                }
            },
            b: {
                state: {
                    text: 2
                }
            }
        }
    })
}
```
##### 使用方法直接调用模块化的store

>App.vue

```javascript
// App.vue

<p>{{textA}}</p>

computed:{
    // 模块后，带命名空间的调用
    textA(){
        return this.$store.state.a.text
    }
}
```

##### 使用帮助函数调用模块化的store

>App.vue

```javascript
// App.vue

<p>{{ textA }}</p>

// Actions,Mutations 帮助函数
import {
    mapState,
    mapGetters,
    mapActions, // 对应异步操作
    mapMutations // 对应同步操作
} from 'vuex'

computed: {
    // 模块后，带命名空间的调用
    // textA(){
    //     return this.$store.state.a.text
    // }

    // 使用帮助函数调用
    ...mapState({
        textA: state => state.a.text // 必须使用方法返回
    })
}
```
<hr>

##### Vuex 默认会将全部mutation,包括模块化的，放入全局mutations

>./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

// 导入默认的actions
import actions from './actions/actions'

const isDev = process.env.NODE_ENV === 'development'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    return new Vuex.Store({
        // 初始化store

        // strict:true, // 防止从外部修改state, //this.$state.count=2 // 仅限开发环境，正式环境需关闭
        strict: isDev,

        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6

        // 通过提交mutation异步修改store
        actions,

        // 模块化,带作用域
        modules: {
            a: {
                state: {
                    text: 1
                },
                mutations: {
                    updateText(state, text) {
                        // 自动获得a模块的state,非全局state
                        console.log('a.state', state)
                        state.text = text
                    }
                }
            },
            b: {
                state: {
                    text: 2
                }
            }
        }
    })
}
```

>App.vue

```javascript
// App.vue

<p>{{ textA }}</p>

// Actions,Mutations 帮助函数
import {
    mapState,
    mapGetters,
    mapActions, // 对应异步操作
    mapMutations // 对应同步操作
} from 'vuex'

methods: {
    ...mapActions(['updateCountAsync']),
    ...mapMutations(['updateCount','updateText']), // 帮助函数声明模块化的 updateText mutation
    // Vuex 默认会将全部mutation,包括模块化的，放入全局mutations
},

mounted(){
    this.updateText('123') //调用模块化的 updateText
},

computed: {
    // 模块后，带命名空间的调用
    // textA(){
    //     return this.$store.state.a.text
    // }

    // 使用帮助函数调用模块化的 state
    ...mapState({
        textA: state => state.a.text // 必须使用方法返回
    })
}

```
<hr>

##### 启用模块命名空间后，namespaced:true,需通过变量方式调用模块的mutation

>./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

// 导入默认的actions
import actions from './actions/actions'

const isDev = process.env.NODE_ENV === 'development'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    return new Vuex.Store({
        // 初始化store

        // strict:true, // 防止从外部修改state, //this.$state.count=2 // 仅限开发环境，正式环境需关闭
        strict: isDev,

        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6

        // 通过提交mutation异步修改store
        actions,

        // 模块化,带作用域
        modules: {
            a: {
                namespaced:true, // 使a模块的mutations具有独立命名空间，不在全局mutations中。防止命名冲突。
                state: {
                    text: 1
                },
                mutations: {
                    updateText(state, text) {
                        // 自动获得a模块的state,非全局state
                        console.log('a.state', state)
                        state.text = text
                    }
                }
            },
            b: {
                state: {
                    text: 2
                }
            }
        }
    })
}
```

>App.vue

```javascript
// App.vue

<p>{{ textA }}</p>

// Actions,Mutations 帮助函数
import {
    mapState,
    mapGetters,
    mapActions, // 对应异步操作
    mapMutations // 对应同步操作
} from 'vuex'

methods: {
    ...mapActions(['updateCountAsync']),
    ...mapMutations(['updateCount', 'a/updateText']), // 帮助函数声明模块化的 updateText mutation
    // Vuex 默认会将全部mutation,包括模块化的，放入全局mutations
    // 启用命名空间后，调用模块内的mutations --- a/updateText
},

mounted(){
    // this.updateText('123') //调用模块化的 updateText // 未启用模块命名空间

    // 启用模块命名空间后，namespaced:true,需通过变量方式调用模块的mutation
    this['a/updateText']('123')
},

computed: {
    // 模块后，带命名空间的调用
    // textA(){
    //     return this.$store.state.a.text
    // }

    // 使用帮助函数调用模块化的 state
    ...mapState({
        textA: state => state.a.text // 必须使用方法返回
    })
}

```
<hr>

##### 启用命名空间的 getters,actions

>./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

// 导入默认的actions
import actions from './actions/actions'

const isDev = process.env.NODE_ENV === 'development'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    return new Vuex.Store({
        // 初始化store

        // strict:true, // 防止从外部修改state, //this.$state.count=2 // 仅限开发环境，正式环境需关闭
        strict: isDev,

        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6

        // 通过提交mutation异步修改store
        actions,

        // 模块化,带作用域
        modules: {
            a: {
                namespaced:true, // 使a模块的mutations具有独立命名空间，不在全局mutations中。防止命名冲突。
                state: {
                    text: 1
                },
                mutations: {
                    updateText(state, text) {
                        // 自动获得a模块的state,非全局state
                        console.log('a.state', state)
                        state.text = text
                    }
                },
                getters:{
                    // 启用命名空间的 getters
                    textPlus(state){
                        // 自动获得a模块的state,非全局state
                        return state.text+1
                    }
                }
            },
            b: {
                state: {
                    text: 2
                }
            }
        }
    })
}
```
>App.vue

```javascript
// App.vue

<p>{{ textA }}</p>

// Actions,Mutations 帮助函数
import {
    mapState,
    mapGetters,
    mapActions, // 对应异步操作
    mapMutations // 对应同步操作
} from 'vuex'

methods: {
    ...mapActions(['updateCountAsync']),
    ...mapMutations(['updateCount', 'a/updateText']), // 帮助函数声明模块化的 updateText mutation
    // Vuex 默认会将全部mutation,包括模块化的，放入全局mutations
    // 启用命名空间后，调用模块内的mutations --- a/updateText
},

mounted(){
    // this.updateText('123') //调用模块化的 updateText // 未启用模块命名空间

    // 启用模块命名空间后，namespaced:true,需通过变量方式调用模块的mutation
    this['a/updateText']('123')

    // 启用模块命名空间后，namespaced:true,需通过变量方式调用模块的Getter
    console.log(this['a/textPlus'])
},

computed: {
    // 模块后，带命名空间的调用
    // textA(){
    //     return this.$store.state.a.text
    // }

    // 使用帮助函数调用模块化的 state
    ...mapState({
        textA: state => state.a.text // 必须使用方法返回
    }),

    // 使用帮助函数调用模块化的 启用命名空间的  getters // 'a/textPlus'
    // 'a/textPlus' 无法在模版中直接使用
    ...mapGetters(['fullName', 'a/textPlus'])
}

```
<hr>

##### 使用帮助函数 重命名getters，方便模版使用

>App.vue

```javascript
// App.vue

<p>{{ textA }}</p>
    <p>{{ textPlus }}</p>

// Actions,Mutations 帮助函数
import {
    mapState,
    mapGetters,
    mapActions, // 对应异步操作
    mapMutations // 对应同步操作
} from 'vuex'

methods: {
    ...mapActions(['updateCountAsync']),
    ...mapMutations(['updateCount', 'a/updateText']), // 帮助函数声明模块化的 updateText mutation
    // Vuex 默认会将全部mutation,包括模块化的，放入全局mutations
    // 启用命名空间后，调用模块内的mutations --- a/updateText
},

mounted(){

},

computed: {
    // 模块后，带命名空间的调用
    // textA(){
    //     return this.$store.state.a.text
    // }

    // 使用帮助函数调用模块化的 state
    ...mapState({
        textA: state => state.a.text // 必须使用方法返回
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

```
<hr>

##### 在模块内获取全局的getters, state - rootState

>./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

// 导入默认的actions
import actions from './actions/actions'

const isDev = process.env.NODE_ENV === 'development'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    return new Vuex.Store({
        // 初始化store

        // strict:true, // 防止从外部修改state, //this.$state.count=2 // 仅限开发环境，正式环境需关闭
        strict: isDev,

        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6

        // 通过提交mutation异步修改store
        actions,

        // 模块化,带作用域
        modules: {
            a: {
                // namespaced: true 会在调用mutation时自动补全模块名前缀 a/
                namespaced: true, // 使a模块的mutations具有独立命名空间，不在全局mutations中。防止命名冲突。
                state: {
                    text: 1
                },
                mutations: {
                    updateText(state, text) {
                        // 自动获得a模块的state,非全局state
                        console.log('a.state', state)
                        state.text = text
                    }
                },
                getters: {
                    // 启用命名空间的 getters
                    textPlus(state, getters, rootState) {
                        // 自动获得a模块的state,非全局state
                        // 自动获得全局的，所有的getter方法-getters
                        // 自动获得全局的 state - rootState
                        // return state.text + rootState.count // 全局的count
                        return state.text + rootState.b.text // b模块的text
                    }
                },
                actions: {
                    add({ state, commit, rootState }) {
                        // 自动获取a模块,包括state,mutations,getters,rootState

                        // 默认commit 本模块的mutation
                        commit('updateText', rootState.count)

                        // 手动commit 全局的mutation
                        commit('updateCount', { num: rootState.count }, { root: true })
                    }
                },
                modules: {
                    // 模块内可以再声明模块
                }
            },
            b: {
                state: {
                    text: 2
                },
                actions: {
                    textAction({ commit }) {
                        commit('a/updateText', 'text text', { root: true }) // 通过指定完整命名空间，调用其他模块的mutation
                    }
                }
            }
        }
    })
}
```
>App.vue

```javascript
// App.vue

<p>{{ textA }}</p>
    <p>{{ textPlus }}</p>

// Actions,Mutations 帮助函数
import {
    mapState,
    mapGetters,
    mapActions, // 对应异步操作
    mapMutations // 对应同步操作
} from 'vuex'

methods: {
    ...mapActions(['updateCountAsync', 'a/add', 'textAction']), // b模块未声明命名空间，textAction无需写模块名
    ...mapMutations(['updateCount', 'a/updateText']), // 帮助函数声明模块化的 updateText mutation
    // Vuex 默认会将全部mutation,包括模块化的，放入全局mutations
    // 启用命名空间后，调用模块内的mutations --- a/updateText
},

mounted(){
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
        textA: state => state.a.text // 必须使用方法返回
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

```
<hr>

##### 动态注册模块

> index.js

```javascript
// index.js
// 服务端渲染配置store

import Vue from 'vue'
import VueRouter from 'vue-router'
// import store from './store/store'
import Vuex from 'vuex'
import createStore from './store/store'

vue.use(VueRouter)
Vue.use(Vuex)

const router = createRouter()
const store = createStore()

// 动态注册模块，配合异步加载组件
store.registerModule('c', {
    state: {
        text: 3
    }
})

new Vue({
    router,
    store,
    render: (h) => h(App)
}).$mount('#root')
```
>App.vue

```javascript
computed: {
    // 使用帮助函数调用模块化的 state
    ...mapState({
        textA: state => state.a.text, // 必须使用方法返回
        textC: state => state.c.text, // 使用动态注册的模块c
    })
}
```
<hr>

#### Vuex  热更新

>./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

// 导入默认的actions
import actions from './actions/actions'

const isDev = process.env.NODE_ENV === 'development'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    const store = new Vuex.Store({
        // 初始化store

        // strict:true, // 防止从外部修改state, //this.$state.count=2 // 仅限开发环境，正式环境需关闭
        strict: isDev,

        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6

        // 通过提交mutation异步修改store
        actions,

        // 模块化,带作用域
        modules: {
            a: {
                // namespaced: true 会在调用mutation时自动补全模块名前缀 a/
                namespaced: true, // 使a模块的mutations具有独立命名空间，不在全局mutations中。防止命名冲突。
                state: {
                    text: 1
                },
                mutations: {
                    updateText(state, text) {
                        // 自动获得a模块的state,非全局state
                        console.log('a.state', state)
                        state.text = text
                    }
                },
                getters: {
                    // 启用命名空间的 getters
                    textPlus(state, getters, rootState) {
                        // 自动获得a模块的state,非全局state
                        // 自动获得全局的，所有的getter方法-getters
                        // 自动获得全局的 state - rootState
                        // return state.text + rootState.count // 全局的count
                        return state.text + rootState.b.text // b模块的text
                    }
                },
                actions: {
                    add({ state, commit, rootState }) {
                        // 自动获取a模块,包括state,mutations,getters,rootState

                        // 默认commit 本模块的mutation
                        commit('updateText', rootState.count)

                        // 手动commit 全局的mutation
                        commit('updateCount', { num: rootState.count }, { root: true })
                    }
                },
                modules: {
                    // 模块内可以再声明模块
                }
            },
            b: {
                state: {
                    text: 2
                },
                actions: {
                    textAction({ commit }) {
                        commit('a/updateText', 'text text', { root: true }) // 通过指定完整命名空间，调用其他模块的mutation
                    }
                }
            }
        }
    })

    if (module.hot) {
        // 启用Vuex热更新,开发时不刷新页面更新，防止刷新后丢失状态
        module.hot.accept([
            './state/state',
            './mutations/mutations',
            './getters/getters',
            './actions/actions'
        ], () => {
            // 开启热更新
            // import 只能用于代码最外层
            const newState = require('./state/state').dafault
            const newMutations = require('./mutations/mutations').default
            const newGetters = require('./getters/getters').default
            const newActions = require('./actions/actions').default

            // 动态更新模块
            store.hotUpdate({
                state: newState,
                mutations: newMutations,
                getters: newGetters,
                actions: newActions
            })
        })
    }

    return store
}
```
<hr>

#### Vuex API和配置

##### 解绑模块

>index.js

```javascript
// 解绑模块
store.unregisterModule('c')
```
<hr>

>store.watch
>index.js
```javascript
store.watch(
    (state) => {
        // 监听state
        // 相当于 getter方法
        state.count + 1
    },
    (newCount) => {
        // 当第一个方法参数发生变动时触发,并传入改变的状态
        console.log('new count watched:', newCount)

    }
)
```
<hr>

##### 监听mutation ,监听action

>index.js

```javascript
// 用于制作Vuex插件
// store订阅,监听mutation // 可以获得所有mutation的变化
store.subscribe((mutation, state) => {
    // 每次有一个mutation被调用，都会执行回调函数,传入正在调用的mutation,和最新state
    console.log(mutation.type) // updateCount
    console.log(mutation.payload) // mutation接受的参数 {num:3}
})

// store订阅,监听action
store.subscribeAction((action, state) => {
    // 每次有一个action被调用，都会执行回调函数,传入正在调用的action,和最新state
    console.log(action.type) // updateCountAsync
    console.log(action.payload) // action接受的参数 {num:5,time:2000}
})
```
<hr>

#### Vuex 插件

>./store/store.js

```javascript
// ./store/store.js

import Vuex from 'vuex'

// 导入默认state
import defaultState from './state/state'

// 导入默认mutations
import mutations from './mutations/mutations'

// 导入默认的getters
import getters from './getters/getters'

// 导入默认的actions
import actions from './actions/actions'

const isDev = process.env.NODE_ENV === 'development'

//服务端渲染用法，返回函数，防止内存溢出
export default () => {
    const store = new Vuex.Store({
        // 初始化store

        // strict:true, // 防止从外部修改state, //this.$state.count=2 // 仅限开发环境，正式环境需关闭
        strict: isDev,

        // 单独的state,更好的组织state
        state: defaultState,

        getters,

        // 修改store
        mutations, // ES6

        // 通过提交mutation异步修改store
        actions,

        // 使用插件，按顺序执行
        plugins: [
            (store) => {
                // 初始接受store
                // vue初始化时调用plugins
                store.subscribe(xxx) // 订阅事件
                store.watch(xxx)
            }
        ],

        
    })

    
    return store
}
```
![vuex](https://user-images.githubusercontent.com/30850497/41214167-65ad1018-6d7c-11e8-9d16-d1503214ef4b.jpg)
