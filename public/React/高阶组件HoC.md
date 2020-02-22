#### 高阶组件（HoC）

> - 高阶组件不能去修改作为参数的组件，高阶组件必须是一个纯函数，不应该有任何副作用。
> - 高阶组件返回的结果必须是一个新的 React 组件，这个新的组件的 JSX 部分肯定会包含作为参数的组件。
> - 高阶组件一般需要把传给自己的 props 转手传递给作为参数的组件。


>对于很多网站应用，有些模块都需要在用户已经登录的情况下才显示。比如，对于一个电商类网站，“退出登录”按钮、“购物车”这些模块，就只有用户登录之后才显示，对应这些模块的 React 组件如果连“只有在登录时才显示”的功能都重复实现，那就浪费了。

>这时候，我们就可以利用“高阶组件（HoC）”这种模式来解决问题。

#### 基本形式

>“高阶组件”名为“组件”，其实并不是一个组件，而是一个函数，只不过这个函数比较特殊

>它接受至少一个 React 组件为参数，并且能够返回一个全新的 React 组件作为结果

>这个新产生的 React 组件是对作为参数的组件的包装，所以，有机会赋予新组件一些增强的“神力”。

>高阶组件的命名一般都带 with 前缀，命名中后面的部分代表这个高阶组件的功能。

```tsx
const withDoNothing = (Component) => {
  const NewComponent = (props) => {
    return <Component {...props} />;
  };
  return NewComponent;
};
```
***
##### 用高阶组件抽取共同逻辑
>只有在登录时才显示

>假设我们已经有一个函数 getUserId 能够从 cookies 中读取登录用户的 ID，如果用户未登录，这个 getUserId 就返回空，那么“退出登录按钮“就需要这么写：

```tsx
const LogoutButton = () => {
  if (getUserId()) {
    return ...; // 显示”退出登录“的JSX
  } else {
    return null;
  }
};
```
>购物车

```tsx
const ShoppintCart = () => {
  if (getUserId()) {
    return ...; // 显示”购物车“的JSX
  } else {
    return null;
  }
};
```

>两个组件明显有重复的代码，我们可以把重复代码抽取出来，形成 withLogin 这个高阶组件，

```tsx
const withLogin = (Component) => {
  const NewComponent = (props) => {
    if (getUserId()) {
      return <Component {...props} />;
    } else {
      return null;
    }
  }

  return NewComponent;
};
```
>只需要这样定义 LogoutButton 和 ShoppintCart

```tsx
const LogoutButton = withLogin((props) => {
  return ...; // 显示”退出登录“的JSX
});

const ShoppingCart = withLogin(() => {
  return ...; // 显示”购物车“的JSX
});
```
>避免了重复代码，以后如果要修改对用户是否登录的判断逻辑，也只需要修改 withLogin，而不用修改每个 React 组件。

***
##### 高阶组件的高级用法

>高阶组件只需要返回一个 React 组件即可，没人规定高阶组件只能接受一个 React 组件作为参数，完全可以传入多个 React 组件给高阶组件。

>改进上面的 withLogin，让它接受两个 React 组件，根据用户是否登录选择渲染合适的组件。

```tsx
const withLoginAndLogout = (ComponentForLogin, ComponentForLogout) => {
  const NewComponent = (props) => {
    if (getUserId()) {
      return <ComponentForLogin {...props} />;
    } else {
      return <ComponentForLogout{...props} />;
    }
  }
  return NewComponent;
};
```

>有了上面的 withLoginAndLogout，就可以产生根据用户登录状态显示不同的内容。

```tsx
const TopButtons = withLoginAndLogout(
  LogoutButton,
  LoginButton
);
```

***
##### 链式调用高阶组件
>高阶组件最巧妙的一点，是可以链式调用。

>假设，你有三个高阶组件分别是 withOne、withTwo 和 withThree，那么，如果要赋予一个组件 X 某个高阶组件的超能力，那么，你要做的就是挨个使用高阶组件包装

```tsx
const X1 = withOne(X);
const X2 = withTwo(X1);
const X3 = withThree(X2);
const SuperX = X3; //最终的SuperX具备三个高阶组件的超能力
```

>直接连续调用高阶组件

```tsx
const SuperX = withThree(withTwo(withOne(X)));
```

>高阶组件本身就是一个纯函数，纯函数是可以组合使用的，

>所以，可以把多个高阶组件组合为一个高阶组件，然后用这一个高阶组件去包装X

```tsx
const hoc = compose(withThree, withTwo, withOne);
const SuperX = hoc(X);
```

>compose，是函数式编程中很基础的一种方法，作用就是把多个函数组合为一个函数

```ts
export default function compose(...funcs) {
  if (funcs.length === 0) {
    return arg => arg
  }

  if (funcs.length === 1) {
    return funcs[0]
  }

  return funcs.reduce((a, b) => (...args) => a(b(...args)))
}
```

>React 组件可以当做积木一样组合使用，现在有了 compose，我们就可以把高阶组件也当做积木一样组合，进一步重用代码。

>假如一个应用中多个组件都需要同样的多个高阶组件包装，那就可以用 compose 组合这些高阶组件为一个高阶组件，这样在使用多个高阶组件的地方实际上就只需要使用一个高阶组件了。

***
##### 不要滥用高阶组件
>高阶组件不得不处理 displayName，不然 debug 会很痛苦。

>当 React 渲染出错的时候，靠组件的 displayName 静态属性来判断出错的组件类，而高阶组件总是创造一个新的 React 组件类，所以，每个高阶组件都需要处理一下 displayName。


>如果要做一个最简单的什么增强功能都没有的高阶组件，也必须要写下面这样的代码：

```tsx
const withExample = (Component) => {
  const NewComponent = (props) => {
    return <Component {...props} />;
  }
  
  NewComponent.displayName = `withExample(${Component.displayName || Component.name || 'Component'})`;
  
  return NewCompoennt;
};
```

>对于 React 生命周期函数，高阶组件不用怎么特殊处理，但是，如果内层组件包含定制的静态函数，这些静态函数的调用在 React 生命周期之外，那么高阶组件就必须要在新产生的组件中增加这些静态函数的支持，这更加麻烦。

>使用高阶组件，一定要非常小心，要避免重复产生 React 组件，比如，下面的代码是有问题的：

```tsx
const Example = () => {
  const EnhancedFoo = withExample(Foo);
  return <EnhancedFoo />
}
```
>每一次渲染 Example，都会用高阶组件产生一个新的组件，虽然都叫做 EnhancedFoo，但是对 React 来说是一个全新的东西，在重新渲染的时候不会重用之前的虚拟 DOM，会造成极大的浪费。

>正确的写法是下面这样，自始至终只有一个 EnhancedFoo 组件类被创建：

```tsx
const EnhancedFoo = withExample(Foo);

const Example = () => {
  return <EnhancedFoo />
}
```