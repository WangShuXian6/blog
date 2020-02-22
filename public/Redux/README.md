#### Redux  是一个“可预测化状态的 JavaScript 容器”
>[redux](https://github.com/reactjs/redux)

#### 安装redux
```
npm install --save redux
```
#### actionCreator
>main.html

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>

<script src="redux.js"></script>
<script src="main.js" type="module"></script>
</body>
</html>
```
>main.js

```javascript
//import { createStore } from 'redux'

let actionCreator = () => {
  //负责构建一个 action
  return {
    type: 'AN_ACTION'
  }
}
console.log(actionCreator())
//{ type: 'AN_ACTION' }
```
>
#### Reducer函数是action的订阅者。
>Reducer函数只是一个纯函数，它接收应用程序的当前状态以及发生的action，然后返回修改后的新状态（或者有人称之为归并后的状态）。

>如何把数据变更传播到整个应用程序？

>使用订阅者来监听状态的变更情况。

>
>Redux实例称为store并用以下方式创建：

>createStore 函数必须接收一个能够修改应用状态的函数

```javascript
import {createStore} from 'redux'
var store = createStore(() => {})
```
#### Flux 的Store 可以保存你的 data，而 Reducer 不能
>在 Redux 中，每次调用 reducer 时，都会传入待更新的 state。这样的话，Redux 的 store 就变成了
“无状态的 store” 并且改了个名字叫 Reducer。

>每当一个 action 发生时，Redux 都能调用这个函数。

>往 createStore 传 Reducer 的过程就是给 Redux 绑定 action 处理函数（也就是 Reducer）的过程。

>reducer

>我们的 reducer 被调用了，但我们并没有 dispatch 任何 action...

>这是因为在初始化应用 state 的时候，Redux dispatch 了一个初始化的 action

```javascript
 ({ type:'@ @redux/INIT' })
```
>在被调用时，一个 reducer 会得到这些参数：(state, action)

>在应用初始化时，state 还没被初始化，因此它的值是 "undefined"

```javascript
//import { createStore } from 'redux'
let actionCreator = () => {
  //负责构建一个 action
  return {
    type: 'AN_ACTION'
  }
}
console.log(actionCreator())
//{ type: 'AN_ACTION' }

let reducer = (...args) => {
  console.log(args)
}

let store = Redux.createStore(reducer)
//Reducer was called with args [ undefined, { type: '@@redux/INIT' } ]
```
#### 从 Redux 实例中读取 state
>一个 reducer 只是一个函数，它能收到程序当前的 state 与 action，

>然后返回一个 modify（又或者学别人一样称之为 reduce ）过的新 state ”

>我们的 reducer 目前什么都不返回，所以程序的 state 当然只能是 reducer() 返回的那个叫“undefined” 的东西

```javascript
//import { createStore } from 'redux'
let actionCreator = () => {
  //负责构建一个 action
  return {
    type: 'AN_ACTION'
  }
}
console.log(actionCreator())
//{ type: 'AN_ACTION' }

let reducer = (state, action) => {
  console.log(state)
  console.log(action)
}

let store = Redux.createStore(reducer)
//undefined
//{type: "@@redux/INITv.0.z.b.x.p"}
console.log(store.getState())
//undefined
```
>在 reducer 收到 undefined 的 state 时，给程序发一个初始状态

>调用  reducer ，只是为了响应一个派发来的 action

```javascript
let reducer = (state, action) => {
  if (typeof state === 'undefined') {
    return {}
  }
  return state
}

let store = Redux.createStore(reducer)
console.log(store.getState())
//{}
```
#### 更加清晰的ES6模式

```javascript
let reducer = (state = {}, action) => {
  return state
}

let store = Redux.createStore(reducer)
console.log(store.getState())
//{}
```
#### 常见模式：在 reducer 里用 switch 来响应对应的 action
>用 switch 的时候， **永远** 不要忘记放个 “default” 来返回 “state”，否则，你的 reducer 可能会返回 “undefined” （等于你的 state 就丢了）

>之所以这个例子能用ES7 Object Spread notation ，是因为它只对 state 里的 { message: action.value} 做了浅拷贝（也就是说， state 第一个层级的属性直接被 { message: action.value } 覆盖掉了 —— 与之相对，其实也有优雅的合并方式 ）

>但是如果数据结构更复杂或者是嵌套的，那处理state更新的时候，很可能还需要考虑一些完全不同的做法：

>- 可以考虑： Immutable.js (https://facebook.github.io/immutable-js/)
>- 可以考虑： Object.assign (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
>- 可以考虑： 手工合并
>- 又或者考虑用其它任何能满足需要且适合 state 结构的方法，Redux 对此是全无预设的方式的（要记得 Redux 只是个状态的容器）

```javascript
let reducer = (state = {}, action) => {
  switch (action.type) {
    case 'SAY_SOMETHING':
      return {
        ...state,
        message: action.value
      }
    default:
      return state;
  }
}

let store = Redux.createStore(reducer)
console.log(store.getState())
//{}
```
#### reducer 是可以处理任何类型的数据结构
>让每个 reducer 只处理整个应用的部分 state

```javascript
let userReducer = function (state = {}, action) {
  switch (action.type) {
      // etc.
    default:
      return state;
  }
}

let itemsReducer = function (state = [], action) {
  switch (action.type) {
      // etc.
    default:
      return state;
  }
}
```
#### createStore 只接收一个 reducer 函数
>combineReducers 接收一个对象并返回一个函数，当 combineReducers 被调用时，它会去调用每个
reducer，并把返回的每一块 state 重新组合成一个大 state 对象（也就是 Redux 中的 Store）

>最终的 state 完全是一个简单的对象，由userReducer 和 itemsReducer 返回的部分 state 共同组成。

```javascript
let reducer = Redux.combineReducers({
  user: userReducer,
  items: itemsReducer
})

let store = Redux.createStore(reducer)
console.log(store.getState())
//{user: {…}, items: Array(0)}
```
#### 调度action
>dispatch 函数，是 Redux 提供的，并且它会将 action 传递给任何一个 reducer

>dispatch 函数本质上是 Redux的实例的属性 "dispatch"

>每一个 reducer 都被调用了，但是没有一个 action type 是 reducer 需要的，因此 state 是不会发生变化的

```javascript
let userReducer = function (state = {}, action) {
  switch (action.type) {
    case 'SET_NAME':
      return {
        ...state,
        name: action.name
      }
    default:
      return state;
  }
}

let itemsReducer = function (state = [], action) {
  switch (action.type) {
    case 'ADD_ITEM':
      return [
        ...state,
        action.item
      ]
    default:
      return state;
  }
}

let reducer = Redux.combineReducers({
  user: userReducer,
  items: itemsReducer
})

let store = Redux.createStore(reducer)
console.log(store.getState())
//{user: {…}, items: Array(0)}
store.dispatch({
  type: 'AN_ACTION'
})
```
#### 使用 action creator 发送一个我们想要的 action
>分发 action,通过 reducer 函数修改应用状态

>处理一个 action，并且它改变了应用的 state

>至止，我们接触的应用流程是这样的:ActionCreator -> Action -> dispatcher -> reducer

>同步 action creator，它创建同步的 action

>当 action creator 被调用时，action 会被立即返回

```javascript
let userReducer = function (state = {}, action) {
  switch (action.type) {
    case 'SET_NAME':
      return {
        ...state,
        name: action.name
      }
    default:
      return state;
  }
}

let itemsReducer = function (state = [], action) {
  switch (action.type) {
    case 'ADD_ITEM':
      return [
        ...state,
        action.item
      ]
    default:
      return state;
  }
}

let reducer = Redux.combineReducers({
  user: userReducer,
  items: itemsReducer
})

let store = Redux.createStore(reducer)
console.log(store.getState())
//{user: {…}, items: Array(0)}

let setNameActionCreator = function (name) {
  return {
    type: 'SET_NAME',
    name: name
  }
}
store.dispatch(setNameActionCreator('harry'))

console.log(store.getState())
//items:[]
//user:{name: "harry"}
```
#### 使用中间件处理自定义 action，比如异步 action
```javascript
let thunkMiddleware = function ({dispatch, getState}) {
  // console.log('Enter thunkMiddleware');
  return function (next) {
    // console.log('Function "next" provided:', next);
    return function (action) {
      // console.log('Handling action:', action);
      return typeof action === 'function' ?
          action(dispatch, getState) :
          next(action)
    }
  }
}

const finalCreateStore = Redux.applyMiddleware(thunkMiddleware)(Redux.createStore)
//针对多个中间件， 使用：applyMiddleware(middleware1, middleware2, ...)(createStore)

let reducer = Redux.combineReducers({
  speaker: function (state = {}, action) {
    console.log('speaker was called with state', state, 'and action', action)

    switch (action.type) {
      case 'SAY':
        return {
          ...state,
          message: action.message
        }
      default:
        return state
    }
  }
})

const store = finalCreateStore(reducer)

let asyncSayActionCreator = function (message) {
  return function (dispatch) {
    setTimeout(function () {
      console.log(new Date(), 'Dispatch action now:')
      dispatch({
        type: 'SAY',
        message
      })
    }, 2000)
  }
}

store.dispatch(asyncSayActionCreator('HI'))

function logMiddleware({dispatch, getState}) {
  return function (next) {
    return function (action) {
      console.log('logMiddleware action received:', action)
      return next(action)
    }
  }
}

// 同样的，下面是一个中间件，它会丢弃所有经过的 action（不是很实用，
// 但是如果加一些判断就能实现丢弃一些 action，放到一些 action 给下一个中间件）：
function discardMiddleware({dispatch, getState}) {
  return function (next) {
    return function (action) {
      console.log('discardMiddleware action received:', action)
    }
  }
}
```
#### 监视 Redux store 更新
```javascript
let itemsReducer = (state = [], action) => {
  console.log('itemsReducer was called with state', state, 'and action', action)

  switch (action.type) {
    case 'ADD_ITEM':
      return [
        ...state,
        action.item
      ]
    default:
      return state;
  }
}

let reducer = Redux.combineReducers({items: itemsReducer})
let store = Redux.createStore(reducer)
store.subscribe(() => {
  console.log('store_0 has been updated. Latest store state:', store.getState());
  // 在这里更新你的视图
})

let addItemActionCreator = (item) => {
  return {
    type: 'ADD_ITEM',
    item: item
  }
}

store.dispatch(addItemActionCreator({id: 1234, description: 'anything'}))
//  items:Array(1)
//  description:"anything"
//  id:1234
```