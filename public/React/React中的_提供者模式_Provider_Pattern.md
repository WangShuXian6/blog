####  React 中的“提供者模式”（Provider Pattern）

>解决跨级的信息传递

>避免 props 逐级传递，即是提供者的用途。

>虽然这个模式叫做“提供者模式”，但是其实有两个角色，一个叫“提供者”（Provider），另一个叫“消费者”（Consumer），这两个角色都是 React 组件。

>其中“提供者”在组件树上居于比较靠上的位置，“消费者”处于靠下的位置。

>既然名为“提供者”，它可以提供一些信息，而且这些信息在它之下的所有组件，无论隔了多少层，都可以直接访问到，而不需要通过 props 层层传递。

***

#### 如何实现提供者模式
>实现提供者模式，需要 React 的 Context 功能，可以说，提供者模式只不过是让 Context 功能更好用一些而已。

>所谓 Context 功能，就是能够创造一个“上下文”，在这个上下文笼罩之下的所有组件都可以访问同样的数据。

>提供者模式的一个典型用例就是实现“样式主题”（Theme），由顶层的提供者确定一个主题，下面的样式就可以直接使用对应主题里的样式。

>这样，当需要切换样式时，只需要修改提供者就行，其他组件不用修改。

#### React v16.3.0 之前的提供者模式
>在 React v16.3.0 之前，要实现提供者，就要实现一个 React 组件，不过这个组件要做两个特殊处理。

> - 需要实现 getChildContext 方法，用于返回“上下文”的数据；
> - 需要定义 childContextTypes 属性，声明“上下文”的结构。

#### 一个实现“提供者”的例子，组件名为 ThemeProvider
```tsx
class ThemeProvider extends React.Component {
  getChildContext() {
    return {
      theme: this.props.value
    };
  }

  render() {
    return (
      <React.Fragment>
        {this.props.children}
      </React.Fragment>
    );
  }
}

ThemeProvider.childContextTypes = {
  theme: PropTypes.object
};
```
>getChildContext 只是简单返回名为 value 的 props 值，但是，因为 getChildContext 是一个函数，它可以有更加复杂的操作，比如可以从 state 或者其他数据源获得数据。

>对于 ThemeProvider，我们创造了一个上下文，这个上下文就是一个对象，结构是这样：

```JSON
{
  "theme": {
    //一个对象
  }
}
```

>做两个消费（也就是使用）这个“上下文”的组件，第一个是 Subject，代表标题；第二个是 Paragraph，代表章节。

##### 把 Subject 实现为一个类
```tsx
class Subject extends React.Component {
  render() {
    const {mainColor} = this.context.theme;
    return (
      <h1 style={{color: mainColor}}>
        {this.props.children}
      </h1>
    );
  }
}

Subject.contextTypes = {
  theme: PropTypes.object
}
```
>在 Subject 的 render 函数中，可以通过 this.context 访问到“上下文”数据，因为 ThemeProvider 提供的“上下文”包含 theme 字段，所以可以直接访问 this.context.theme。

>Subject 必须增加 contextTypes 属性，必须和 ThemeProvider 的 childContextTypes 属性一致，不然，this.context 就不会得到任何值。

>为什么要求“提供者”用 childContextTypes 定义一次上下文结构，又要求“消费者”再用 contextTypes 再重复定义一次呢？

>React 这么要求，是考虑到“上下文”可能会嵌套，就是一个“提供者”套着另一个“提供者”，这时候，底层的消费者组件到底消费哪一个“提供者”呢？通过这种显示的方式指定。

#### 也可以把消费者实现为一个纯函数组件，只不过访问“上下文”的方式有些不同，我们用纯函数的方式实现另一个消费者 Paragraph
```tsx
const Paragraph = (props, context) => {
  const {textColor} = context.theme;
  return (
    <p style={{color: textColor}}>
      {props.children}
    </p>
  );
};

Paragraph.contextTypes = {
  theme: PropTypes.object
};
```
>从上面的代码可以看到，因为 Paragraph 是一个函数形式，所以不可能访问 this.context，但是函数的第二个参数其实就是 context。

>不要忘了设定 Paragraph 的 contextTypes，不然参数 context 也不会是上下文。


#### 结合”提供者“和”消费者“。

>做一个组件来使用 Subject 和 Paragraph，这个组件不需要帮助传递任何 props
```tsx
const Page = () => (
  <div>
    <Subject>这是标题</Subject>
    <Paragraph>
      这是正文
    </Paragraph>
  </div>
);
```

>上面的组件 Page 使用了 Subject 和 Paragraph，现在我们想要定制样式主题，只需要在 Page 或者任何需要应用这个主题的组件外面包上 ThemeProvider，对应的 JSX 代码

```tsx
<ThemeProvider value={{mainColor: 'green', textColor: 'red'}} >
    <Page />
  </ThemeProvider>

```
>当我们需要改变一个样式主题的时候，改变传给 ThemeProvider的 value 值就搞定了。

***
#### React v16.3.0 之后的提供者模式
>需要让“提供者”和“消费者”共同依赖于一个 Context 对象

>而且消费者也要使用 render props 模式。

#### 首先，要用新提供的 createContext 函数创造一个“上下文”对象。
>创造“提供者”极大简化了，不需要创造一个 React 组件类。
```tsx
const ThemeContext = React.createContext();
```

>这个“上下文”对象 ThemeContext 有两个属性，分别就是Provider 和 Consumer。

```tsx
const ThemeProvider = ThemeContext.Provider;
const ThemeConsumer = ThemeContext.Consumer;
```

#### 使用“消费者” 
>Subject

```tsx
class Subject extends React.Component {
  render() {
    return (
      <ThemeConsumer>
        {
          (theme) => (
            <h1 style={{color: theme.mainColor}}>
              {this.props.children}
            </h1>
          )
        }
      </ThemeConsumer>
    );
  }
}
```
>上面的 ThemeConsumer 其实就是一个应用了 render props 模式的组件，它要求子组件是一个函数，会把“上下文”的数据作为参数传递给这个函数，而这个函数里就可以通过参数访问“上下文”对象。

>在新的 API 里，不需要设定组件的 childContextTypes 或者 contextTypes 属性

>Subject 没有自己的状态，没必要实现为类，我们用纯函数的形式实现 Paragraph

```tsx
const Paragraph = (props, context) => {
  return (
    <ThemeConsumer>
      {
        (theme) => (
          <p style={{color: theme.textColor}}>
            {props.children}
          </p>
          )
      }
    </ThemeConsumer>
  );
};
```

#### 实现 Page 的方式并没有变化，而应用 ThemeProvider 的代码和之前也完全一样
```tsx
<ThemeProvider value={{mainColor: 'green', textColor: 'red'}} >
    <Page />
  </ThemeProvider>
```

***

>在新版 Context API 中，需要一个“上下文”对象（上面的例子中就是 ThemeContext)，使用“提供者”的代码和“消费者”的代码往往分布在不同的代码文件中，那么，这个 ThemeContext 对象放在哪个代码文件中呢？

>最好是放在一个独立的文件中

>为了避免依赖关系复杂，每个应用都不要滥用“上下文”，应该限制“上下文”的使用个数。