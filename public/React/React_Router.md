#### React Router
>要实现“单页应用”，一个最要紧的问题就是做好“路由”（Routing)，也就是处理好下面两件事：

> - 把 URL 映射到对应的页面来处理；
> - 页面之间切换做到只需局部更新。;
***
#### react router v4 的动态路由
>动态路由，指的是路由规则不是预先确定的，而是在渲染过程中确定的

>安装

>react-router-dom 依赖于 react-router ，所以 react-router 也会被自动安装上。

```bash
npm install react-router-dom
```

***

>react-router 的工作方式，是在组件树顶层放一个 Router 组件，然后在组件树中散落着很多 Route 组件（注意比 Router 少一个“r”），顶层的 Router 组件负责分析监听 URL 的变化，在它保护伞之下的 Route 组件可以直接读取这些信息。

>Router 和 Route 的配合，就是“提供者模式”，Router 是“提供者”，Route是“消费者”。

>Router 其实也是一层抽象，让下面的 Route 无需各种不同 URL 设计的细节

***
#### BrowserRouter
>第一种很自然，比如 / 对应 Home 页，/about 对应 About 页，但是这样的设计需要服务器端渲染，因为用户可能直接访问任何一个 URL，服务器端必须能对 /的访问返回 HTML，也要对 /about 的访问返回 HTML。
***
#### HashRouter 
>第二种看起来不自然，但是实现更简单。只有一个路径 /，通过 URL 后面的 # 部分来决定路由，/#/ 对应 Home 页，/#/about 对应 About 页。因为 URL 中#之后的部分是不会发送给服务器的，所以，无论哪个 URL，最后都是访问服务器的 / 路径，服务器也只需要返回同样一份 HTML 就可以，然后由浏览器端解析 # 后的部分，完成浏览器端渲染。

>把 Router 用在 React 组件树的最顶层，这是最佳实践。

```tsx
import {HashRouter} from 'react-router-dom';

ReactDOM.render(
  <HashRouter>
    <App />
  </HashRouter>,
  document.getElementById('root')
);
```
***
#### 使用 Link
>Navigation 导航栏

```tsx
const ulStyle = {
  'list-style-type': 'none',
  margin: 0,
  padding: 0,
};

const liStyle = {
  display: 'inline-block',
  width: '60px',
};

const Navigation = () => (
  <header>
    <nav>
      <ul style={ulStyle}>
        <li style={liStyle}><Link to='/'>Home</Link></li>
        <li style={liStyle}><Link to='/about'>About</Link></li>
      </ul>
    </nav>
  </header>
)
```
***
#### 使用 Route 和 Switch
>Content 内容组件，这里会用到 react-router 最常用的两个组件 Route 和 Switch。

```tsx
const Content = () => (
  <main>
    <Switch>
      <Route exact path='/' component={Home}/>
      <Route path='/about' component={About}/>
    </Switch>
  </main>
)
```
>当访问 /about 页面时，不光匹配 /about，也配中 /，界面上会把 Home 和 About 都渲染出来的。

>解决方法，可以在想要精确匹配的 Route 上加一个属性 exact，或者使用 Switch 组件。

***
#### 动态路由
>假设，我们增加一个新的页面叫 Product，对应路径为 /product，但是只有用户登录了之后才显示。如果用静态路由，我们在渲染之前就确定这条路由规则，这样即使用户没有登录，也可以访问 product，我们还不得不在 Product 组件中做用户是否登录的检查。

>如果用动态路由，则只需要在代码中的一处涉及这个逻辑：

```tsx
 <Switch>
      <Route exact path='/' component={Home}/>
      {
        isUserLogin() &&
        <Route exact path='/product' component={Product}/>,
      }  
      <Route path='/about' component={About}/>
    </Switch>
```

>可以用任何条件决定 Route 组件实例是否渲染，比如，可以根据页面宽度、设备类型决定路由规则，动态路由有了最大的自由度。

