> Vue 通用(高级)组件开发

#### notification之基本组件实现-普通用法-不方便使用

>./client/components/notification/notification.vue

```vue
// ./client/components/notification/notification.vue

<template>
	<transition name="fade">
		<div class="notification">
			<span class="content">{{content}}</span>
			<a class="btn" @click="handleClick">{{btn}}</a>
		</div>
	</transition>
</template>

<script>
	export default {
		name: 'notification',
		props: {
			content: {
				type: String,
				required: true
			},
			btn: {
				type: String,
				default: '关闭'
			}
		},
		methods: {
			handleClick(e) {
				e.preventDefault() // 阻止默认事件
				this.$emit('close') // 触发父组件的事件
			}
		}
	}
</script>

<style lang="stylus" scoped>
	.fade-enter-active, .fade-leave-active
		transition:opacity .5s
	.fade-enter, .fade-leave-to
		opacity:0
</style>
```

>./client/components/notification/index.js

```javascript
// ./client/components/notification/index.js
// vue插件
// 编写可发布的vue组件


// 全局注册notification组件，在每个业务组件内自动注册notification
import Notification from './notification.vue'

export default (Vue) => {
    // 接受 Vue 参数

    // 注册组件
    Vue.component(Notification.name, Notification)
}
```

>./create-app.js

```javascript
// ./create-app.js

// 每次服务端渲染，会渲染一个新的app
// 如果单例渲染，会包含上次渲染的状态

// npm i vue-meta -S

import Vue from 'vue'
import VueRouter from 'vue-router'
import Vuex from 'vuex'
import Meta from 'vue-meta'

import App from './app.vue'
import createStore from './store/store'
import createRouter from './config/router'

import Notification from './components/notification'

import './assets/styles/global.styl'

Vue.use(VueRouter)
Vue.use(Vuex)
Vue.use(Meta)

// 插件形式注册使用组件,自动注入Vue
// 组件将定义在全局，随处可用
Vue.use(Notification)

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

>App.vue

```vue
// App.vue
<template>
    <div>
        <p>{{ textA }}</p>
        <p>{{ textPlus }}</p>

        <notification content="test notify"></notification>
    </div>
</template>
```

<hr>

#### notification : 通过方法调用,推荐

>./client/components/notification/notification.vue

```vue
// ./client/components/notification/notification.vue
// transition组件隐藏时会调用after-leave方法
// transition组件完全显示时会调用after-enter方法
<template>
	<transition name="fade" @after-leave="afterLeave" @after-enter="afterEnter">
		<div 
		class="notification" 
		:style="style" 
		v-show="visible" 
		@mouseenter="clearTimer" 
		@mouseleave="createTimer">
			<span class="content">{{content}}</span>
			<a class="btn" @click="handleClick">{{btn}}</a>
		</div>
	</transition>
</template>

<script>
	export default {
		name: 'notification',
		data() {
			return {
				visible: true
			}
		},
		props: {
			content: {
				type: String,
				required: true
			},
			btn: {
				type: String,
				default: '关闭'
			}
		},
		computed: {
			style() {
				return {}
			}
		}
		methods: {
			handleClick(e) {
				e.preventDefault() // 阻止默认事件
				this.$emit('close') // 触发父组件的事件,开始关闭 // 在父组件监听
			},
			afterLeave() {
				this.$emit('closed') // 触发父组件的事件，关闭完成 // 在父组件监听
			},
			afterEnter() {
				// 即使默认组件用不到，只在扩展组件覆盖，也要事先声明
			},
			clearTimer() {
	
			},
			createTimer() {
	
			}
		}
	}
</script>

<style lang="stylus" scoped>
	.fade-enter-active, .fade-leave-active
		transition:opacity .5s
	.fade-enter, .fade-leave-to
		opacity:0
</style>
```
>./client/components/notification/index.js

```javascript
// ./client/components/notification/index.js
// vue插件
// 编写可发布的vue组件


// 全局注册notification组件，在每个业务组件内自动注册notification
import Notification from './notification.vue'

import notify from './function'

export default (Vue) => {
    // 接受 Vue 参数

    // 注册组件
    Vue.component(Notification.name, Notification)

    // 扩展vue内置方法
    Vue.prototype.$notify = notify
}
```
>./client/components/notification/function.js

```javascript
// ./client/components/notification/function.js
// 扩展组件的功能
import Vue from 'vue'
import Component from './func-notification'

// 创建vue组件
// 通过使用 Vue.extend 返回方法创建组件
const NotificationConstructor = Vue.extend(Component)

const instances = [] // 组件实例数组
let seed = 1 // 组件id

// 删除节点防止内存溢出,需传入待删除的节点对象
const removeInstance = (instance) => {
	if (!instance) return

	const length = instances.length

	// 找到当前组件实例的索引位置
	const index = instances.findIndex(inst => instance.id === inst.id)

	//instances.splice(index,1)
	instance.splice(index, 1)

	// 删除节点时重新计算所有其他节点的高度
	if (length <= 1) return // 只剩最后节点时不需要计算
	const removeHeight = instance.vm.height
	// 从删掉的节点位置开始向上计算
	for (let i = index; i < len - 1; i++) {
		instances[i].verticalOffset = parseInt(instances[i].verticalOffset) - removeHeight - 16
	}
}

// 方法
const notify = (options) => {
	// 有DOM 操作，不能在服务端运行
	if (Vue.prototype.$isServer) return

	//得到autoClose，解构剩下的参数
	const { autoClose, ...rest } = options

	const instance = new NotificationConstructor({
		propsData:...rest, // 传入参数,覆盖主组件的props
		data: {
			autoClose: autoClose === undefined ? 3000 : autoClose
		}, // 传入参数,覆盖主组件的data
	})

	const id = `notification_${seed++}`
	instance.id = id

	// 生成组件的DOM对象，但未插入页面。因为没有指定插入节点
	instance.vm = instance.$mount() // 返回vue对象

	// 将组件dom插入页面
	document.body.appendChild(instance.vm.$el)
	instance.vm.visible = true

	// 计算每个提醒组件的定位
	let verticalOffset = 0
	instance.forEach((item) => {
		verticalOffset += item.$el.offsetHeight + 16 // 组件之间间隔16
	})
	verticalOffset += 16 // 默认距离屏幕底部16
	instance.verticalOffset = verticalOffset
	instances.push(instance)

	// 监听组件的事件'closed',销毁节点对象，防止多个节点内存溢出
	instance.vm.$on('closed', () => {
		removeInstance(instance)
		document.body.removeChild(instance.vm.$el) // 移除节点dom
		instance.vm.$destory() // 销毁实例
	})

	instance.vm.$on('close', () => {
		instance.vm.visible = false
		// 之后会触发closed事件
	})

	return instance.vm // 用于调用方法后继续操作该组件对象
}

export default notify
```
>./client/components/notification/func-notification.js

```javascript
// ./client/components/notification/func-notification.js
// 通过方法调用的专用组件
// 扩展组件,无需模版。复用组件的模版

import Notification from './notification.vue'

export default {
	extends: Notification, // 继承主组件以扩展
	computed: {
		style() {
			// 覆盖主组件的属性
			return {
				position: 'fixed',
				right: '20px',
				bottom: `${this.verticalOffset}px`,
			}
		}
	},
	data() {
		return {
			verticalOffset: 0, // 声明默认值
			autoClose: 3000, // 提醒出现3秒后自动关闭
			height: 0,
			visible: false,
		}
	},
	mounted() {
		this.createTimer()
	},
	methods: {
		createTimer() {
			// 提醒出现3秒后自动关闭
			if (this.autoClose) {
				this.timer = setTimeout(() => {
					this.visible = false
				}, this.autoClose)
			}
		},
		clearTimer() {
			if (this.timer) {
				clearTimeout(this.timer)
			}
		},
		afterEnter() {
			// 覆盖主组件的方法
			this.height = this.$el.offsetHeight // 获取实际高度
		}
	},
	beforeDestory() {
		// 退出组件时销毁定时器防止内存溢出
		this.clearTimer()
	}
}
```
>App.vue

```vue
// App.vue
<template>
    <div>
        <p>{{ textA }}</p>
        <p>{{ textPlus }}</p>
        <button @click="notify">click notify</button>
        <!-- <notification content="test notify"></notification> -->
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
    
            notify() {
                // 调用组件的方法
                this.$notify({
                    content: 'test $notify',
                    btn: 'close'
                })
            }
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

#### tabs组件之基本组件实现

>不要在组件内部修改自己props,应该有父组件提供修改

>./client/components/tabs/tab.vue

```javascript
// ./client/components/tabs/tab.vue
<script>
	export default {
		name:'Tab',
		props:{
			index:{
				// 判断选中的状态
				type:[String,Number],
				required:true
			},
			label:{
				// 内容
				type:String,
				default:'tab'
			}
		},
		inject:['value'],
		computed:{
			active(){
				// return this.$parent.value===this.index
				return this.value===this.index
			}
		},
		methods:{
			handleClick(){
				this.$parent.onChange(this.index)
			}
		},
		mounted(){
			this.$parent.panes.push(this)
		},
		render(){
			const tab=this.$slots.label || <span>{this.label}</span>

			const classNames={
				tab:true,
				active:this.active
			}

			return (
				<li class={classNames} on-click={this.handleClick}>
					{tab}
				</li>
			)
		}
	}
</script>
```
>./client/components/tabs/tabs.vue

```javascript
// ./client/components/tabs/tabs.vue

<script>
	export default {
		name:'Tabs',
		props:{
			value:{
				// 通过value控制显示哪一个tab
				type:[String,Number]，
				require:true
			}
		},
		provide(){
			return {
				value:this.value
			}
		},
		data(){
			return {
				panes:[]
			}
		},
		render(){
			const contents=this.panes.map(pane=>{
				return pane.active ? pane.$slots.default :null
			})

			return (
				<div class='tabs'>
					<ul class='tabs-header'>
						{this.$slots.default}
					</ul>
					<div class="tab-content">
						{contents}
					</div>
				</div>
			)
		},
		methods:{
			onChange(index){
				this.$emit('change',index)
			}
		}
	}
</script>
```
>./client/components/tabs/index.js

```javascript
// ./client/components/tabs/index.js

import Tabs from './tabs.vue'
import Tab from './tab.vue'

export default (Vue)=>{
    Vue.component(Tabs.name,Tabs)
    Vue.component(Tab.name,Tab)
}
```
>App.vue

```javascript
// App.vue
<template>
    <div>
        <p>{{ textA }}</p>
        <p>{{ textPlus }}</p>
        <button @click="notify">click notify</button>
        <!-- <notification content="test notify"></notification> -->

        <!-- <tabs>
            <tab label="text">
                <span slot="label"></span>
                <p>this is tab content</p>
            </tab>
        </tabs> -->

        <tabs value="1" @change="handleChangeTab">
            <tab label="tab1" index="1">
                <span>tab content 1</span>
            </tab>
            <tab index="1">
                <span slot="label" style="color:red;">tab2</span>
                <span>tab content 2</span>
            </tab>
            <tab label="tab3" index="3">
                <span>tab content 3</span>
            </tab>
        </tabs>
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
    
            notify() {
                // 调用组件的方法
                this.$notify({
                    content: 'test $notify',
                    btn: 'close'
                })
            },
            handleChangeTab(value){
                this.tabValue=value
            }
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
