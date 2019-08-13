#### connect
>connect方法接受两个参数：mapStateToProps和mapDispatchToProps。

>它们定义了 UI 组件的业务逻辑。

>前者负责输入逻辑，即将state映射到 UI 组件的参数（props），

>后者负责输出逻辑，即将用户对 UI 组件的操作映射成 Action。

```javascript
import { connect } from 'react-redux'

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
```

#### mapStateToProps

>mapStateToProps是一个函数。它的作用就是像它的名字那样，建立一个从（外部的）state对象到（UI 组件的）props对象的映射关系

>mapStateToProps会订阅 Store，每当state更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染。

```javascript
const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}
```
>mapStateToProps的第一个参数总是state对象，还可以使用第二个参数，代表容器组件的props对象

```javascript
// 容器组件的代码
//    <FilterLink filter="SHOW_ALL">
//      All
//    </FilterLink>

const mapStateToProps = (state, ownProps) => {
  return {
    active: ownProps.filter === state.visibilityFilter
  }
}
```