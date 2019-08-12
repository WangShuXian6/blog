#### render props 模式
>render props，指的是让 React 组件的 props 支持函数这种模式。因为作为 props 传入的函数往往被用来渲染一部分界面，所以这种模式被称为 render props。

>一个最简单的 render props 组件 RenderAll

>这个 RenderAll 预期子组件是一个函数，它所做的事情就是把子组件当做函数调用，调用参数就是传入的 props，然后把返回结果渲染出来，除此之外什么事情都没有做。

>RenderAll 的内部元素即为 props.children ，调用 该内部元素并把  RenderAll 的所有 属性传给它

>作为一个通用的render props，并不知道子组件需要用到哪些props，所以应该都传递过去，用不用在他，但是传不传在你。

```tsx
const RenderAll = (props) => {
  return(
     <React.Fragment>
     	{props.children(props)}
     </React.Fragment>
  );
};
```

>使用 RenderAll 

>RenderAll 的子组件，也就是夹在 RenderAll 标签之间的部分，其实是一个函数。这个函数渲染出 \<h1\>hello world\<\/h1\>，这就是上面使用 RenderAll 渲染出来的结果。

```tsx
<RenderAll>
        {() => <h1>hello world</h1>}
      </RenderAll>
```

***
#### 传递 props
>render props 可以做很多的定制功能，我们还是以根据是否登录状态来显示一些界面元素为例，来实现一个 render props。

>实现 render props 的 Login 组件，

>render props 和高阶组件的第一个区别，就是 render props 是真正的 React 组件，而不是一个返回 React 组件的函数。

>当用户处于登录状态，getUserName 返回当前用户名，否则返回空，然后我们根据这个结果决定是否渲染 props.children 返回的结果。

```tsx
const Login = (props) => {
  const userName = getUserName();

  if (userName) {
    const allProps = {userName, ...props};
    return (
      <React.Fragment>
        {props.children(allProps)}
      </React.Fragment>
    );
  } else {
    return null;
  }
};
```

>使用上面 Login 的 JSX 代码示例

```tsx
<Login>
    {({userName}) => <h1>Hello {userName}</h1>}
  </Login>
```

>“以函数为子组件（function as child）”的模式，这可以算是 render props 的一种具体形式，也就利用 children 这个 props 来作为函数传递。

***
>render props 这个模式不必局限于 children 这一个 props，任何一个 props 都可以作为函数，也可以利用多个 props 来作为函数。

>扩展 Login，不光在用户登录时显示一些东西，也可以定制用户没有登录时显示的东西，我们把这个组件叫做 Auth

```tsx
const Auth= (props) => {
  const userName = getUserName();

  if (userName) {
    const allProps = {userName, ...props};
    return (
      <React.Fragment>
        {props.login(allProps)}
      </React.Fragment>
    );
  } else {
    <React.Fragment>
      {props.nologin(props)}
    </React.Fragment>
  }
};
```

>使用 Auth 的话，可以分别通过 login 和 nologin 两个 props 来指定用户登录或者没登录时显示什么

```tsx
<Auth
    login={({userName}) => <h1>Hello {userName}</h1>}
    nologin={() => <h1>Please login</h1>}
  />

```

***
##### 依赖注入
>render props 其实就是 React 世界中的“依赖注入”（Dependency Injection)。

> 所谓依赖注入，指的是解决这样一个问题：逻辑 A 依赖于逻辑 B，如果让 A 直接依赖于 B，当然可行，但是 A 就没法做得通用了。

>依赖注入就是把 B 的逻辑以函数形式传递给 A，A 和 B 之间只需要对这个函数接口达成一致就行，如此一来，再来一个逻辑 C，也可以用一样的方法重用逻辑 A。

>在上面的代码示例中，Login 和 Auth 组件就是上面所说的逻辑 A，而传递给组件的函数类型 props，就是逻辑 B 和 C。

***
##### render props 和高阶组件的比较
>比对一下这两种重用 React 组件逻辑的模式

> - render props 模式的应用，就是做一个 React 组件，而高阶组件，虽然名为“组件”，其实只是一个产生 React 组件的函数

> - render props 不像上一小节中介绍的高阶组件有那么多毛病，如果说 render props 有什么缺点，那就是 render props 不能像高阶组件那样链式调用，当然，这并不是一个致命缺点。

> - render props 相对于高阶组件还有一个显著优势，就是对于新增的 props 更加灵活。

>以登录状态为例，假如我们扩展 withLogin 的功能，让它给被包裹的组件传递用户名这个 props

```tsx
const withLogin = (Component) => {
  const NewComponent = (props) => {
    const userName= getUserName();
    if (userName) {
      return <Component {...props} userName={userName}/>;
    } else {
      return null;
    }
  }

  return NewComponent;
};
```
>这就要求被 withLogin 包住的组件要接受 userName 这个props。可是，假如有一个现成的 React 组件不接受 userName，却接受名为 name 的 props 作为用户名，这就麻烦了。我们就不能直接用 withLogin 包住这个 React 组件，还要再造一个组件来做 userName 到 name 的映射，十分费事。


>对于应用 render props 的 Login，就不存在这个问题，接受 name 不接受 userName

```tsx
<Login>
  {
    (props) => {
      const {userName} = props;
      return <TheComponent {...props} name={userName} />
    }
  }
</Login>
```


>所以，当需要重用 React 组件的逻辑时，建议首先看这个功能是否可以抽象为一个简单的组件；

>如果行不通的话，考虑是否可以应用 render props 模式；

>再不行的话，才考虑应用高阶组件模式。
