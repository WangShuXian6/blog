#### 在开发组件时，保持稳定的 DOM 结构会有助于性能的提升。
>例如，可以通过 CSS 隐藏或显示节点，而不是真的移除或添加 DOM 节点。

#### 在开发过程中，尽量减少类似将最后一个节点移动到列表首部的操作，当节点数量过大或更新操作过于频繁时，在一定程度上会影响 React 的渲染性能。

#### 不建议在 getDefaultProps、getInitialState、shouldComponentUpdate、componentWillUpdate、render 和 componentWillUnmount 中调用 setState，特别注意：不能在 shouldComponentUpdate 和 componentWillUpdate中调用 setState，会导致循环调用。

***
#### 组件接口设计的三个原则：

> - 保持接口小，props 数量要少
> - 根据数据边界来划分组件，利用组合（composition）
> - 把 state 尽量往上层组件提取

> - 避免 renderXXXX 函数
> - 给回调函数类型的 props 加统一前缀
> - 使用 propTypes 来定义组件的 props

***
>无状态组件和类组件并不是对立的概念，一个类组件如果没有自己的state，一样是无状态组件，类组件和函数形式组件才是对立的概念。
***
> - 尽量每个组件都有自己专属的源代码文件；
> - 用解构赋值（destructuring assignment）的方法获取参数 props 的每个属性值；
> - 利用属性初始化（property initializer）来定义 state 和成员函数。