
#### redux-actions是一个自动生成redux的动作和处理函数的库
>https://redux-actions.js.org

>https://github.com/redux-utilities/redux-actions

>异步action的使用

>store/types/index.js

```javascript
export const ASYNC_INCREMENT = 'ASYNC_INCREMENT'
```
>store/actions/async.js

```javascript
import {ASYNC_INCREMENT} from '../types/index'
import {createAction} from 'redux-actions'

export const asyncIncrement = createAction(ASYNC_INCREMENT, (payload) => {
  return new Promise((resolve, reject) => {
    // 可以对参数进行处理
    setTimeout(() => {
      resolve(payload)
    }, 2000)
  })
})
```
>store/reducers/async.js

>注意：引入action只能通过直接引入动作，如：

> import {asyncIncrement} from '../store/actions'

>不可以用定义的字面量方式引入异步动作:如：import {CLEARCART, ASYNC_INCREMENT} from '../store/types'

>只有同步动作可以用字面量方式引入

>以字面量方式引入动作将强制为同步方式，即处理动作时将立即dispatch动作

```javascript
import {handleActions} from 'redux-actions'
import {ASYNC_INCREMENT} from '../types/index'

export default handleActions({
  [ASYNC_INCREMENT](state, action) {
    console.log('action.payload', action.payload)
    return action.payload
  }
}, {
  num: 0
})
```
>pages/cart.wpy

```vue
<style lang="less">

</style>
<template>
  <view class="cart">
    <view @tap="asyncIncrementSelf">异步操作</view>
    <view>异步显示:::{{asyncnum.num}}</view>
  </view>
</template>
<script>
  import wepy from 'wepy'
  import {connect} from 'wepy-redux'
  import {asyncIncrement} from '../store/actions'   // 关键
  @connect({
    asyncnum(state) {
      return state.asyncnum
    }
  }, {
    asyncIncrement
  })

  export default class Cart extends wepy.component {
    props = {}

    data = {}
    computed = {}

    events = {}

    watch = {}

    methods = {
      asyncIncrementSelf() {
        this.methods.asyncIncrement({num: 2})
      }
    }

    onLoad() {

    }
  }
</script>

```