#### Redux 使用模式
>创建一个独立于这两个组件的对象，在这个对象中存放共享的数据，没错，这个对象，相当于一个 Store。

>如果只是一个简单对象，那么任何人都可以修改 Store，这不大合适。所以我们做出一些限制，让 Store 只接受某些『事件』，如果要修改 Store 上的数据，就往 Store 上发送这些『事件』，Store 对这些『事件』的响应，就是修改状态。

>这里所说的『事件』，就是 action，而对应修改状态的函数，就是 reducer。

***
>第一步，看这个状态是否会被多个 React 组件共享。

>所谓共享，就是多个组件需要读取或者修改这个状态，如果是，那不用多想，应该放在 Store 上，因为 Store 上状态方便被多个组件共用，避免组件之间传递数据；如果不是，继续看第二步。

>第二步，看这个组件被 unmount 之后重新被 mount，之前的状态是否需要保留。

>举个简单例子，一个对话框组件。用户在对话框打开的时候输入了一些内容，不做提交直接关闭这个对话框，这时候对话框就被 unmount 了，然后重新打开这个对话框（也就是重新 mount），需求是否要求刚才输入的内容依然显示？如果是，那么应该把状态放在 Store 上，因为 React 组件在 unmount 之后其中的状态也随之消失了，要想在重新 mount 时重获之前的状态，只能把状态放在组件之外，Store 当然是一个好的选择；如果需求不要求重新 mount 时保持 unmount 之前的状态，继续看第三步。

>第三步，到这一步，基本上可以确定，这个状态可以放在 React 组件中了。

***
#### 代码组织方式

>更好的方法，是把源代码文件分类放在不同的目录中，根据分类方式，可以分为两种：

> - 基于角色的分类（role based）
> - 基于功能的分类（feature based）

>基于角色的分类

>把所有 reducer 放在一个目录（通常就叫做 reducers)，把所有 action 放在另一个目录（通常叫 actions），最后，把所有的纯 React 组件放在另一个目录。

>基于功能的分类方式，是把一个模块相关的所有源代码放在一个目录。

>例如，对于博客系统，有 Post（博客文章）和 Comment（注释）两个基本模块，建立两个目录 Post 和 Comment，每个目录下都有各自的 action.js 和 reducer.js 文件，如下所示，每个目录都代表一个模块功能，这就是基于功能的分类方式。

```
Post -- action.js
     |_ reucer.js
     |_ view.js
Comment -- action.js
        |_ reucer.js
        |_ view.js     
```
>基于功能的分类方式更优。因为每个目录是一个功能的封装，方便共享

***
#### react-redux 
>安装

```bash
npm install redux react-redux
```

>react-redux 就是『提供者模式』的实践。在组件树的一个比较靠近根节点的位置，我们通过 Provider 来引入一个 store

```tsx
import {createStore} from 'redux';
import {Provider} from 'react-redux';

const store = createStore(...);

// JSX
  <Provider store={store}>
    { // Provider之下的所有组件都可以connect到给定的store }
  </Provider>
```
>这个 Provider 当然也是利用了 React 的 Context 功能。在这个 Provider 之下的所有组件，如果使用 connect，那么『链接』的就是 Provider 的 state。

>connect 的用法，首先，我们需要一个『傻瓜组件』，可以由纯函数实现

```tsx
const CounterView = ({count, onIncrement}) => {
  return (
    <div>
      <div>{count}</div>
      <button onClick={onIncrement}>+</button>
    </div>
  );
};
```

>把 CounterView 和 store 连接起来

```tsx
import {connect} from 'react-redux';

const mapStateToProps = (state) => {
  return {
    count: state.count
  };
}

const mapDispatchToProps = (dispatch) => ({
  onIncrement: () => dispatch({type: 'INCREMENT'})
});

const Counter = connect(mapStateToProps, mapDispatchToProps)(CounterView);
```
>这里的 connect 函数接受两个参数，一个 mapStateToProps 是把 Store 上的 state 映射为 props；另一个 mapDispatchToProps 则是把回调函数类型的 props 映射为派发 action 的动作，connect 函数调用会产生一个『高阶组件』。

>connect 产生的高阶组件产生了一个新的 React 组件 Counter，这个 Counter 其实就是一个『聪明组件』，它负责管理状态，而 CounterView 是一个『傻瓜组件』，只负责渲染。

>在 react-redux 中，应用了三个 React 模式：

> - 提供者模式
> - 高阶组件
> - 聪明组件和傻瓜组件的分离
***
#### Redux 和 React 结合的最佳实践
>1-Store 上的数据应该范式化。
>所谓范式化，就是尽量减少冗余信息，像设计 MySQL 这样的关系型数据库一样设计数据结构。

>2-使用 selector。

>对于 React 组件，需要的是『反范式化』的数据，当从 Store 上读取数据得到的是范式化的数据时，需要通过计算来得到反范式化的数据。你可能会因此担心出现问题，这种担心不是没有道理，毕竟，如果每次渲染都要重复计算，这种浪费积少成多可能真会产生性能影响，所以，我们需要使用 seletor。业界应用最广的 selector 就是 reslector 。

>reselector 的好处，是把反范式化分为两个步骤，第一个步骤是简单映射，第二个步骤是真正的重量级运算，如果第一个步骤发现产生的结果和上一次调用一样，那么第二个步骤也不用计算了，可以直接复用缓存的上次计算结果。

>3-只 connect 关键点的 React 组件

当 Store 上状态发生改变的时候，所有 connect 上这个 Store 的 React 组件会被通知：『状态改变了！』

>然后，这些组件会进行计算。connect 的实现方式包含 shouldComponentUpdate 的实现，可以阻挡住大部分不必要的重新渲染，但是，毕竟处理通知也需要消耗 CPU，所以，尽量让关键的 React 组件 connect 到 store 就行。

>一个实际的例子就是，一个列表种可能包含几百个项，让每一个项都去 connect 到 Store 上不是一个明智的设计，最好是只让列表去 connect，然后把数据通过 props 传递给各个项。

>使用 react-redux 的话，虽然 Provider 可以嵌套，但是，最里层的 Provider 提供的 store 才生效。
***
#### 如何实现异步操作
>最简单的 redux-thunk，代码量少，只有几行，用起来也很直观，但是开发者要写很多代码；

>而比较复杂的 redux-observable 相当强大，可以只用少量代码就实现复杂功能，但是前提是你要学会 RxJS，RxJS 本身学习曲线很陡，内容需要 一本书 的篇幅来介绍，这就是代价