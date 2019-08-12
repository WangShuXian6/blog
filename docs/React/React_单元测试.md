#### React 单元测试
>对代码进行测试是最佳实践，可以保证代码质量

>测试就是尽力发现软件中的缺陷（俗称 bug），当我们发现不了更多的 bug 时，说明这个软件质量可以接受了。

>测试是尽力发现软件中的 bug。当我们发现 bug 数量和严重程度呈稳定的下降趋势，直到低于一个门槛（无须降低为 0，只需要降低到可接受的程度），没有更多更严重的 bug 出现，就说明这个软件的质量可以接受，可以上线了。

>对”小块代码“的测试，也就是单元测试。

***

#### Jest
>Mocha 之类老牌单元测试框架，把所有的单元测试都放在一个环境中执行，这就使所有单元测试访问的是同样一个全局变量空间，所以只要测试代码没写好，就会互相影响。而且，为了保证执行正常，所有的单元测试必须一个接一个地执行，这是体系架构决定的，没有办法。

>Jest 不同，Jest 为每一个单元测试文件创造一个独立的运行环境，换句话说，Jest 会启动一个进程执行一个单元测试文件，运行结束之后，就把这个执行进程废弃了，这个单元测试文件即使写得比较差，把全局变量污染得一团糟，也不会影响其他单元测试文件，因为其他单元测试文件是用另一个进程来执行。

>Jest 最重要的一个特性，就是支持并行执行

>因为每个单元测试文件之间再无纠葛，Jest 可以启动多个进程同时运行不同的文件，这样就充分利用了电脑的多 CPU 多核

>使用 create-react-app 产生的项目自带 Jest 作为测试框架，不奇怪，因为 Jest 和 React 一样都是出自 Facebook。

>运行下面的命令，就可以进入交互式的”测试驱动开发“模式

```bash
npm test
```

***
#### Enzyme
>虽然最好的 React 测试框架出自 Facebook 家，最受欢迎的 React 测试工具库却出自 Airbnb，这个工具库叫做 Enzyme。Enzyme 这个单词的含义是“酶”，至于命名原因已经无法考据，可能寓意着快速分解。

>安装

```bash
npm i --save-dev enzyme enzyme-adapter-react-16
```

>enzyme-adapter-react-16，这个库是用来作为适配器的。
> - 因为不同 React 版本有各自特点，所用的适配器也会不同，
> - 我们的项目中使用的是 16.4 之后的版本，所以用 enzyme-adapter-react-16；
> - 如果用 16.3 版本，需要用 enzyme-adapter-react-16.3；
> - 如果用 16.2 版本，需要用 enzyme-adapter-react-16.2；
> - 如果用更老的版本 15.5，需要用 enzyme-adapter-react-15。
> - 具体各个 React 版本对应什么样的 Adapter，请参考 enzyme官方文档。
> - https://airbnb.io/enzyme/#installation

***
#### 单元测试
>以之前秒表应用中的 ControlButtons 组件为例

>创造一个 ControlButtons.test.js，来容纳对应的测试用例，因为所有后缀为 .test.js 的文件都会被 Jest 认作是测试用例文件。

>在代码中，需要使用 Adapter

```ts
import {configure} from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
configure({adapter: new Adapter()});
``

>我们对 ControlButtons 组件的测试，就是要渲染它一次，看看渲染结果如何，enzyme 就能帮助我们做这件事。

>比如，我们想要保证渲染出来的内容必须包含两个按钮，其中一个按钮的 class 名是 left-btn，另一个是 right-btn，那么我们就需要下面的单元测试用例：

```ts
import {shallow} from 'enzyme';

it('renders without crashing', () => {
  const wrapper = shallow(<ControlButtons />);
  expect(wrapper.find('.left-btn')).toHaveLength(1);
  expect(wrapper.find('.right-btn')).toHaveLength(1);
});
```

>shallow 和 mount 的区别，就是 shallow 只会渲染被测试的 React 组件这一层，不会渲染子组件；

>而 mount 则是完整地渲染 React 组件包括其所有子组件，包括触发 componentDidMount 生命周期函数。

>原则上，能用 shallow 就尽量用 shallow，首先是为了测试性能考虑，其次是可以减少组件之间的影响

```ts

const Foo = () => ()
    <div>
       {/* other logic */
       <Bar />
    </div>
)
```
>如果用 mount 去渲染 Foo，会连带 Bar 一起完全渲染，如果 Bar 出了什么毛病，那 Foo 的单元测试也过不了；如果用 shallow，只知道 Bar 曾经被用，即使 Bar 哪里出了问题，也不影响 Foo 的单元测试。

***
#### 代码覆盖率
>代码覆盖率必须达到 100%，也就是说，一个应用不光所有的单元测试都要通过，而且所有单元测试都必须覆盖到代码 100% 的角落。

>在 create-react-app 创造的应用中，已经自带了代码覆盖率的支持，运行下面的命令，不光会运行所有单元测试，也会得到覆盖率汇报。

```bash
npm test -- --coverage
```

>代码覆盖率包含四个方面：

> - 语句覆盖率
> - 逻辑分支覆盖率
> - 函数覆盖率
> - 代码行覆盖率

>只有四个方面都是 100%，才算真的 100%。