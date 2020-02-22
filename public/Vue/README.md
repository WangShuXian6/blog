#### 安装，升级，卸载
>卸载 npm uninstall -g vue-cli

>推荐直接删除系统目录node_modules/vue-cli

>安装 npm install -g @vue/cli

>vue create my-project

>cd my-project

>npm run serve

>空格选择取消，回车确定

>或者 npx -p vue-cli vue create my-project   // 旧版

>npx -p @vue/cli vue create my-project  // 新版

>在声明组件时，使用基于类(class-based)的 API，则可以使用官方维护的 vue-class-component 装饰器：

>https://github.com/vuejs/vue-class-component


#### Vue
##### Vue实例1

>index.js

```javascript
import Vue from 'vue'

new Vue({
    el:'#root',
    template:'<div>main</div>'
})
```
<hr>

##### Vue实例2

>index.js

```javascript
import Vue from 'vue'
import App from './app.vue'

import './assets/styles/global.styl'

const root =document.createElement('div')
document.body.appendChild(root)

new Vue({
    render:(h)=>h(app)
}).$mount(root)
```
<hr>

##### Vue实例3

>index.js

```javascript
import Vue from 'vue'

const app=new Vue({
    //el:'#root',
    template:'<div>main</div>'
})

app.$mount('#root')
```
<hr>

##### vue实例属性

```javascript
import Vue from 'vue'

const app = new Vue({
    //el:'#root',
    template: "<div ref='div'>{{text}}</div>",
    data: {
        text: 0
    }
})

app.$mount('#root')

app.text += 1 // 响应式

console.log(app.$data)
console.log(app.$props)
console.log(app.$el) // <div>1</div> // 节点
console.log(app.$options) // new Vue 时传入的整个对象
app.$options.data.text += 1 // 非响应式，
app.$data.text += 1 // 响应式

setTimeout(() => {
    app.text += 1
}, 1000);

app.$options.render = (h) => {
    return h('div', {}, 'new render function')
}

// 只在下一次数据更新时起作用
// 1秒后 页面由数字变为 new render function

console.log(app.$root) // 根节点，Vue 对象
console.log(app.$root === app) // true

console.log(app.$children) // 根节点的子节点

console.log(app.$slote)
console.log(app.$scopedSlots) // 插槽

console.log(app.$refs) //空对象或DOM节点对象或组件实例 app.$refs.div

console.log(app.$isServer)
```
<hr>

##### vue实例方法-Watch 属性监听

```javascript
import Vue from 'vue'

const app = new Vue({
    //el:'#root',
    template: "<div ref='div'>{{text}}</div>",
    data: {
        text: 0
    },
    // 组件退出自动销毁，防止内存溢出
    watch: {
        text(newText, oldText) {
            // 监听text属性值变化
            console.log(`${newText}:${oldText}`)
        }
    }
})

app.$mount('#root')

// 组件退出不会自动销毁
const unWatch = app.$watch('text', (newText, oldText) => {
    // 监听text属性值变化
    console.log(`${newText}:${oldText}`)
})

unWatch() // 调用方法即可手动销毁watch监听，防止内存溢出
```
<hr>

##### vue实例方法- 事件监听

```javascript
import Vue from 'vue'

const app = new Vue({
    //el:'#root',
    template: "<div ref='div'>{{text}}</div>",
    data: {
        text: 0
    }
})

app.$mount('#root')

//$on与$emit需要同时作用于同一个Vue对象上(app)才会生效
//不会冒泡
// 监听事件
app.$on('test', (a, b) => {
    console.log(`test emited :${a}-${b}`)
})

// 触发事件
app.$emit('test', 1, 2)

// 监听事件,只触发一次
app.$once('test2', (a, b) => {
    console.log(`test emited :${a}-${b}`)
})

// 触发事件
app.$emit('test2', 1, 2)
```
<hr>

##### vue实例方法 设置数据

```javascript
import Vue from 'vue'

const app = new Vue({
    //el:'#root',
    template: "<div ref='div'>{{text}}</div>",
    data: {
        text: 0,
        obj: {}
    }
})

app.$mount('#root')

app.obj.a = 1 // 新增对象属性非响应式
app.$forceUpdate() // 强制组件重新渲染一次,属性对象添加新值时，不建议使用,影响性能

app.$set(app.obj, 'a', 1) //响应式
app.$delete() //删除值

app.$nextTick(() => { }) //下一次dom更新时调用 //解决vue异步队列更新延迟渲染
```
<hr>

##### vue实例生命周期方法 lifecycle

```javascript
import Vue from 'vue'

const app = new Vue({
    el: '#root',
    // template: "<div ref='div'>{{text}}</div>", //比较耗时
    data: {
        text: 0,
        obj: {}
    },
    beforeCreate() {
        // 一次性
        // 禁止数据操作
        console.log('beforeCreate', this.$el)
        // undefined
    },
    created() {
        // 一次性
        // 可以执行与数据有关的操作
        console.log('created', this.$el)
        // undefined
    },
    beforeMount() {
        // 一次性, 服务端渲染不会调用，服务端没有dom环境
        // 可以执行与数据有关的操作
        console.log('beforeMount', this.$el)
        // <div id='root'></div>
        // 挂载节点
    },
    render(h) {
        // 比较耗时
        // 如果没有 template，则执行
        // 创建template并渲染
        return h('div', {}, this.text)
    },
    mounted() {
        // 一次性, 服务端渲染不会调用，服务端没有dom环境
        // 挂载dom
        // 执行与dom有关的操作
        console.log('mounted', this.$el)
        // <div>0</div>
        // 覆盖挂载节点
    },
    beforeUpdate() {
        // 数据更新前
        console.log('beforeUpdate', this)
    },
    updated() {
        // 数据更新时
        console.log('updated', this)
    },
    activated() {
        console.log('activated', this)
    },
    deactivated() {
        console.log('deactivated', this)
    },
    beforeDestroy() {
        console.log('beforeDestroy', this)
    },
    destroyed() {
        // 组件销毁时
        console.log('destroyed', this)
    },
    renderError(h, error) {
        // 仅在开发环境执行
        // 只在本组件发生错误时调用,不会冒泡,不会捕获子组件错误
        return h('div', {}, error.stack)
    },
    errorCaptured() {
        // 正式环境也有效
        // 可收集线上环境错误信息
        // 向上冒泡，可以捕获所有子组件错误
    }
})

setTimeout(() => {
    app.destroy() // 手动销毁组件，不推荐
}, 1000)

```
<hr>

##### vue 数据绑定,事件绑定

```javascript
import Vue from 'vue'

// v-html 显示标签之中所有的内容，可以防止被vue转义，会被注入攻击。
const app = new Vue({
    el: '#root',
    template: `
        <div ref='div'>{{isActive ? 'active' : 'not active'}}</div>
        <div>{{getJoinedArr(arr)}}</div>
        <div v-bind:id='mainId' v-on:click='handleClick'>
            <p :id='mainId2' @click='handleClickP' v-html='html'></p>
        </div>
    `, //比较耗时
    data: {
        isActive: false,
        html: '<span>123</span>', //vue默认会转译字符串，防止注入攻击
        arr: [1, 2, 3],
        mainId: 'main',
        mainId2: 'main2'
    },
    methods: {
        // vue 事件已经优化
        handleClick() {

        },
        handleClickP() {

        },
        getJoinedArr(arr) {
            return arr.join(' ')
        }
    }
})
```
<hr>

##### vue 样式绑定

```javascript
import Vue from 'vue'

// v-html 显示标签之中所有的内容，可以防止被vue转义，会被注入攻击。
const app = new Vue({
    el: '#root',
    template: `
        <div :class="{active : isActive}"></div>
        <div :class="[isActive ? 'active' : '']"></div>
        <div :class="[{active : isActive}]"></div>
        <div :style='styles'></div>
        <div :style="[styles,styles2]"></div>
    `,
    // isActive为true则使用active
    // :style 会自动补全样式前缀
    data: {
        isActive: false,
        styles: {
            color: 'red',
            appearance: 'none'
        },
        styles2: {
            color: 'black'
        }
    },
    computed: {
        classNames() {

        }
    }
})
```
<hr>

##### vue computed, watch

```javascript
import Vue from 'vue'

// v-html 显示标签之中所有的内容，可以防止被vue转义，会被注入攻击。
const app = new Vue({
    el: '#root',
    template: `
        <div>
            <span>name:{{firstName + ' ' + lastName}}</span>
            <span>name:{{name}}</name>
            <span>name:{{getName()}}</name>
            <p>number:{{number}}</p>
            <p><input type='text' v-model="number" /></p>

            <p>{{firstName}}</p>
            <input v-model="name2" />

            <p>{{fullName}}</p>
        </div>
    `,
    data: {
        firstName: 'harry',
        lastName: 'potter',
        number: 0,
        fullName: '',
        obj: { a: '' }
    },
    computed: {
        // 在computed对象中可以声明很多方法，返回数据
        // computed是类中的get方法
        // 会缓存正在依赖的data数据以提高性能，开销小
        // 适合大数据量
        // 不可修改正在依赖的data值
        name() {
            console.log('new name')
            return `${this.firstName} ${this.lastName}`
        },
        // 可设置
        name2: {
            get() {
                return `${this.firstName} ${this.lastName}`
            },
            set(name) {
                const names = name.split(' ')
                this.firstName = names[0]
                this.lastName = names[1]
            }
        }
    },
    watch: {
        // watch不适用于显示数据
        // watch适用于监听到数据改变时，向服务端发送请求
        // 不可修改正在依赖的data值,否则导致无限循环触发watch事件
        firstName(newName, oldName) {
            // 初始绑定不会执行
            this.fullName = newName + ' ' + this.lastName
        },
        lastName: {
            // 初始绑定后会立即执行一次
            handler(newName, oldName) {
                this.fullName = this.firsttName + ' ' + newName
            },
            immediate: true,
            deep: true //适用于对象值，开销大,监听对象值,修改对象属性时必须使用，直接修改对象整体可为false，
        },
        'obj.a': {
            // 使用字符串表示对象深入属性调用,可以降低开销,只监听最后的属性值
            // 初始绑定后会立即执行一次
            handler(newName, oldName) {
                this.fullName = this.firsttName + ' ' + newName
            },
            immediate: true,
            deep: false
        }
    },
    methods: {
        // 没有缓存，开销大，实时数据
        getName() {
            console.log('get name')
            return `${this.firstName} ${this.lastName}`
        }
    }
})
```
<hr>

##### vue 原生指令 directive

>v-text 完全替换子节点内容(innerText)

>v-html (innerHtml)

>v-show 是否显示该节点 (原理：：style="display:none")，仍然在dom中

>v-if ,v-else-if,v-else是否将节点插入dom,动态增删节点，引起重绘，性能低，大量节点非常耗时。

>v-for 循环数组或对象,key使用数据绑定唯一值，不推荐使用index,因为与数组变化无关，无法节省性能,可能导致错误缓存

>v-on,@,在vue对象实例上绑定事件,会寻找dom节点或使用$on绑定组件

>v-bind

>v-model ,默认只用于input,可改变数值。checkbox :value="1",使用动态解析，值为1，非字符串

>v-model.number 数值修饰符，将输入的字符串转为数值型.input 输入默认为字符型

>v-model.trim 去除首尾空格

>v-model.lazy 输入完毕才触发更新

>v-pre 不解析子节点，不解析{{}}

>v-cloak 直接引用vue时防止双花括号的闪现

>v-once 数据绑定只执行一次,再次变化数据不会更新渲染。不再检测虚拟dom数据，节省开销。

```javascript
import Vue from 'vue'

// v-html 显示标签之中所有的内容，可以防止被vue转义，会被注入攻击。
const app = new Vue({
    el: '#root',
    template: `
        <div>
            <p>name:{{text}}</p>
            <p v-text="text"></p>
            <p v-text="'name:' + text"></p>
            <p v-html="html"></p>

            <p v-show="active"></p>
            <p v-if="active"></p>
            <p v-else-if="text===0">else if content</p>
            <p v-else>else content</p>

            <ul>
                <li v-for="item in arr" :key="item">{{item}}</li>
            </ul>

            <ul>
                <li v-for="(item,index) in arr" :key="item">{{item}}:{{index}}</li>
            </ul>

            <ul>
                <li v-for="(value,key) in obj" :key="value">{{value}}:{{key}}</li>
            </ul>

            <ul>
                <li v-for="(value,key,index) in obj" :key="value">{{value}}:{{key}}:{{index}}</li>
            </ul>

            <p v-on:click="a">click</p>
            <p @click="b">click2</p>

            <p>{{text1}}</p>
            <input text="text" v-model="text1" />

            <input type="checkbox" v-model="active" />

            <div>
                <input type="checkbox" :value="1" v-model="arr" />
                <input type="checkbox" :value="2" v-model="arr" />
                <input type="checkbox" :value="3" v-model="arr" />
            </div>

            <div>
                <input type="radio" value="one" v-model="picked" />
                <input type="radio" value="two" v-model="picked" />
            </div>

            <input text="text" v-model.number="text1" />
            <input text="text" v-model.trim="text1" />
            <input text="text" v-model.lazy="text1" />

            <p v-pre>name:{{text}}</p>
            <p v-cloak>name:{{text}}</p>
            <p v-once>name:{{text}}</p>
        </div>
    `,
    data: {
        text: 0,
        active: false,
        html: '<span>main</span>',
        arr: [1, 2, 3], //绑定选中的值，默认checkbox全部选中，数组中的值对应多选框已选项的值，修改选中项，该数组也会改变 //checkbox未指定value时，value='on'
        obj: {
            a: 10,
            b: 20,
            c: 30
        },
        text1: 0,
        picked: '', //绑定选中的值，默认为空表示都不选中。 选中radio时，值会变为 'one' 或 'two'
    },
    methods: {
        a() {

        },
        b() {

        }
    }
})
```
<hr>

#### vue 组件
##### vue 定义全局组件

```javascript
import Vue from 'vue'

// 二级组件
const component = {
    template: `
        <div>
            component
        </div>
    `
}

// 定义全局组件 comp
Vue.component('CompOne', component)

// 根组件
const app = new Vue({
    el: '#root',
    template: '<CompOne></CompOne>',
    //template:'<comp-one></comp-one>',
})
```
<hr>

##### vue  定义局部组件

```javascript
import Vue from 'vue'

// 二级组件
const component = {
    template: `
        <div>
            {{text}}
        </div>
    `
}

// 根组件
const app = new Vue({
    // 局部组件，当前组件内使用
    components: {
        CompOne: component
    },
    el: '#root',
    template: `
        <div>
            
        </div>
    `,
})
```
<hr>

##### vue 父子组件传参-1

```javascript
import Vue from 'vue'

// 二级组件
const component = {
    template: `
        <div>
            {{text}}
            <span v-show="active">can see</span>
            {{propOne}}
            <span @click="handleChange"></span>
        </div>
    `,
    data() {
        // data不是通过new Vue内生成，需要以函数形式定义，返回新建对象，防止数据重叠
        return {
            text: 123
        }
    },
    props: {
        // 使组件可配置,通过父组件约束本组件
        // 不可以在组件内修改本组件的props的值，可以通过事件通知父组件修改
        // 单项数据流
        active: Boolean,
        propOne: String,
        onChange: Function
    },
    methods: {
        handleChange() {
            this.onChange()
        }
    }
}

// 根组件
const app = new Vue({
    // 局部组件，当前组件内使用
    components: {
        CompOne: component
    },
    el: '#root',
    template: `
        <div>
            <CompOne :active="true" :prop-one="prop" :onChange="handleChange"></CompOne>
            <CompOne :active="false" propOne="text2"></CompOne>
        </div>
    `,
    data: {
        prop: 0
    },
    //template:'<comp-one></comp-one>',
    methods: {
        handleChange() {
            this.prop += 1
        }
    }
})
```
<hr>

##### vue 父子组件传参-2,推荐

>通过this.$emit('change') 触发自身组件的事件属性 @Change // 在父组件内的自身组件

```javascript
import Vue from 'vue'

// 二级组件
const component = {
    template: `
        <div>
            {{text}}
            <span v-show="active">can see</span>
            {{propOne}}
            <span @click="handleChange"></span>
        </div>
    `,
    data() {
        // data不是通过new Vue内生成，需要以函数形式定义，返回新建对象，防止数据重叠
        return {
            text: 123
        }
    },
    props: {
        // 使组件可配置,通过父组件约束本组件
        // 不可以在组件内修改本组件的props的值，可以通过事件通知父组件修改
        // 单项数据流
        active: Boolean,
        propOne: String,
        // onChange: Function // 使用 this.$emit('change') 后，无需定义此处
    },
    methods: {
        handleChange() {
            // this.onChange()
            this.$emit('change')
            // 触发自身组件的事件属性 @Change // 在父组件内的自身组件
        }
    }
}

// 根组件
const app = new Vue({
    // 局部组件，当前组件内使用
    components: {
        CompOne: component
    },
    mounted() {
        console.log(this.$refs.comp1) // 组件实例
    },
    el: '#root',
    template: `
        <div>
            <-- <CompOne :active="true" :prop-one="prop" :onChange="handleChange"></CompOne> -->
            <CompOne ref="comp1" :active="true" :prop-one="prop" @change="handleChange"></CompOne>
            <CompOne :active="false" propOne="text2"></CompOne>
        </div>
    `,
    data: {
        prop: 0
    },
    //template:'<comp-one></comp-one>',
    methods: {
        handleChange() {
            this.prop += 1
        }
    }
})
```
<hr>

##### vue props 数据验证

```javascript
props: {
        // 使组件可配置,通过父组件约束本组件
        // 不可以在组件内修改本组件的props的值，可以通过事件通知父组件修改
        // 单项数据流
        active: {
            type: Boolean, // 类型
            required: true, // 是否必须
            default: false, // 默认值 // 如果为true,可以不定义属性 // default 与 required 一般不会同时使用
        },
        obj2: {
            default() {
                // 对象值必须使用函数返回新对象.防止组件共用数据
                return {

                }
            },
            validator(value) {
                // 自定义验证传入的props值
                // 无需type
                return typeof value === 'object'
            }
        },
        propOne: String,
        // onChange: Function // 使用 this.$emit('change') 后，无需定义此处
    }
```
<hr>

##### vue 组件继承-1

```javascript
import Vue from 'vue'

// 仅是对象
const component = {
    template: `
        <div>
           {{propOne}}
        </div>
    `,
    props: {
        active: Boolean,
        propOne: String,
    },
    data() {
        return {
            text: 0
        }
    },
    methods: {
        handleChange() {
        }
    },
    mounted() {

    }
}

// 扩展对象,得到Vue类的一个子类,并传入component对象配置文件
const CompVue = Vue.extend(component)

// 直接声明一个组件并挂在到root节点
new CompVue({
    el: '#root',
    propsData: {
        // 定义属性
        propOne: 'prop-1'
    },
    data: {
        // 定义data,与对象data合并，相同data属性会覆盖
        text: 123
    },
    mounted() {
        // 与对象内的合并，并且不会覆盖,后调用
    }
})
```
<hr>

##### vue 组件继承，扩展，适用于对公用组件的继承和扩展

```javascript
import Vue from 'vue'

// 仅是对象
const component = {
    template: `
        <div>
           {{propOne}}
        </div>
    `,
    props: {
        active: Boolean,
        propOne: String,
    },
    data() {
        return {
            text: 0
        }
    },
    methods: {
        handleChange() {
        }
    },
    mounted() {

    }
}

// 
const componet2 = {
    extends: compoent, //继承自compoent,扩展组件，覆盖数据，添加新属性
    data() {
        return {
            text: 1
        }
    }
}

// 直接声明一个组件并挂在到root节点
new Vue({
    el: '#root',
    components: {
        Comp: componet2
    },
    template: `<comp></comp>`
})
```
<hr>

##### vue 自定义双向绑定,实现v-model

```javascript
import Vue from 'vue'

// 仅是对象
const component = {
    template: `
        <div>
           {{propOne}}
           <input type="text" @input="handleInput" :value="value" />
        </div>
    `,
    props: {
        active: Boolean,
        propOne: String,
        value: ''
    },
    data() {
        return {
            text: 0
        }
    },
    methods: {
        handleChange() {
        },
        handleInput(event) {
            // 触发在父组件上的自身组件的input,并传入值
            this.$emit('input', event.target.value)
        }
    },
    mounted() {

    }
}

// 
const componet2 = {
    extends: compoent, // 继承自compoent,扩展组件，覆盖数据，添加新属性
    data() {
        return {
            text: 1
        }
    }
}

// 直接声明一个组件并挂在到root节点
new Vue({
    el: '#root',
    components: {
        Comp: componet2
    },
    data() {
        return {
            value: '123'
        }
    },
    // input 事件在自组件内被触发,执行 value=arguments[0]。
    // 接受arguments，并将arguments[0]赋值给value
    template: `<comp :value="value" @input=" value=arguments[0] "></comp>`
})
```
<hr>

##### vue 插槽,用来定义布局组件，在父组件内调用，以放置内容

>具名插槽

```javascript
import Vue from 'vue'

// 布局组件,只有样式，不放内容
const component = {
    template: `
        <div :style="style">
           <div class="header">
                <slot name="header" ></slot>
           </div>

           <div class="body">
                <slot name="body" ></slot>
           </div>
        </div>
    `,
    props: {

    },
    data() {
        return {
            style: {
                width: '200px',
                height: '200px',
                border: "1px solid #aaa"
            }
        }
    },
    methods: {

    }
}

// 
const componet2 = {
    extends: compoent, // 继承自compoent,扩展组件，覆盖数据，添加新属性
    data() {
        return {
            text: 1
        }
    }
}

// 直接声明一个组件并挂在到root节点
new Vue({
    el: '#root',
    components: {
        Comp: componet2
    },
    data() {
        return {
            value: '123'
        }
    },
    // 直接在组件内放置内容无效，需要使用插槽
    template: `
        <comp">
            <span slot="header">header content</span>
            <span slot="body">body content</span>
        </comp>
        `
})
```
<hr>

##### vue 作用域插槽

```javascript
import Vue from 'vue'

// 布局组件,只有样式，不放内容
const component = {
    template: `
        <div :style="style">
           <slot value="3"></slot>
        </div>
    `,
    props: {

    },
    data() {
        return {
            style: {
                width: '200px',
                height: '200px',
                border: "1px solid #aaa"
            },
            a: 1
        }
    },
    methods: {

    }
}

// 
const componet2 = {
    extends: compoent, // 继承自compoent,扩展组件，覆盖数据，添加新属性
    data() {
        return {
            text: 1
        }
    }
}

// 直接声明一个组件并挂在到root节点
new Vue({
    el: '#root',
    components: {
        Comp: componet2
    },
    data() {
        return {
            text: 1
        }
    },
    mounted() {
        console.log(this.$refs.comp.a) // 1,组件实例的值，不推荐调用实例
        console.log(this.$refs.span) // DOM 对象
    },
    // 直接在组件内放置内容无效，需要使用插槽
    // 作用域插槽，添加变量范围,使用定义在组件slot上的属性值.同时仍可调用本地属性
    template: `
        <comp ref="comp">
            <span slot-scope="props" ref="span">{{props.value}}:{{text}}</span>
        </comp>
        `
})
```
<hr>


##### vue provide,不推荐，可能会被废弃，提供跨层级通信

```javascript
import Vue from 'vue'

const ChildComponent = {
    template: `
    <div>child:{{data.value}}</div>
`,
    mounted() {
        console.log(this.$parent.$options.name)
        console.log(this.grandFather) // 祖父实例
    },
    inject: ['grandFather', 'data'], // 注入grandFather,data // 必须存在树状结构关系
}

const component = {
    name: 'comp',
    components: {
        ChildComponent
    },
    template: `
        <div>
           <ChildComponent />
        </div>
    `,
    props: {

    },
    data() {
        return {
            a: 1
        }
    },
    methods: {

    }
}

// 
const componet2 = {
    extends: compoent, // 继承自compoent,扩展组件，覆盖数据，添加新属性
    data() {
        return {
            text: 1
        }
    }
}

// 直接声明一个组件并挂在到root节点
new Vue({
    el: '#root',
    components: {
        Comp: componet2
    },
    provide() {
        // 默认非响应式，只提供一次数据，不会实时更新 // 除非定义
        const data = {}
        Object.defineProperty(data, 'value', {
            get: () => this.value, // 在子组件调用value 时,调用get()，返回最新的this.value
            enumerable: true // 允许被读取
        })

        return {
            grandFather: this, // 将自生提供出去以引用
            data: data
        }
    },
    data() {
        return {
            text: 1,
            value: 1
        }
    },
    mounted() {

    },
    template: `
        <comp ></comp>
        `
})
```
<hr>


##### vue render function ，虚拟DOM，VNode

```javascript
import Vue from 'vue'

// 布局组件,只有样式，不放内容
const component = {
    name: 'comp',
    // template: `
    //     <div :style=""style>
    //         <slot></slot>
    //     </div>
    // `,
    props: {

    },
    data() {
        return {
            a: 1,
            style: {
                color: 'red'
            }
        }
    },
    methods: {

    },
    render(createElement) {
        return createElement(
            'div',
            {
                style: this.style
            },
            // 没有名字的slot,如果有名字使用 this.$slots.xxx
            this.$slots.default
        )
    }
}

// 
const componet2 = {
    extends: compoent, // 继承自compoent,扩展组件，覆盖数据，添加新属性
    data() {
        return {
            text: 1
        }
    }
}

// 直接声明一个组件并挂在到root节点
new Vue({
    el: '#root',
    components: {
        Comp: componet2
    },
    data() {
        return {
            text: 1,
            value: 1
        }
    },
    mounted() {

    },
    // template: `
    //     <comp ref="comp" >
    //         <span ref="span">{{value}}</span>
    //     </comp>
    // `,
    render(createElement) {
        // 虚拟DOM，创建VNode类,存储在内存中。与真实DOM对比,将差异更新到DOM,提高性能。
        // template模版的最终编译渲染。等同直接声明template属性
        // return this.$createElement() // render()  也会自动传入此函数
        return createElement(
            'comp',
            {
                // 传递属性
                ref: 'comp'
            },
            [
                createElement(
                    'span',
                    {
                        ref: 'span'
                    },
                    this.value
                )
            ])
    }
})
```

#### vscode 插件
##### vue-beautify

#### VUE 事件
>鼠标移入移出事件，类CSS hover 事件
```html
<label for="main-link" class="link" @mouseenter="showSub" @mouseleave="hideSub">集团信息</label>
``` 

#### Vue技术内幕,源码解析
>http://hcysun.me/vue-design/

#### Vue 判断浏览器

##### 在主入口文件中获取浏览器信息
>src/main.ts
```typescript
import Vue from 'vue';
import App from './App.vue';
import router from './router';
import store from './store';
import './registerServiceWorker';

Vue.config.productionTip = false;

console.log('navigator.userAgent;--',navigator.userAgent)

let na=navigator.userAgent
Vue.prototype.$na=na

new Vue({
  router,
  store,
  render: (h) => h(App),
}).$mount('#app');

```

##### 在vue中使用已获取的信息
>src/components/HelloWorld.vue
```vue
<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
  created(){
    console.log('na--',this.$na)
  }
}
</script>
```

>打印结果
```bash
Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1
```

***

#### 探测当前设备
>https://github.com/greedying/vue-visitor  探测当前设备

***