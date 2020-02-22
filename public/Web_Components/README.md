#### Web Components

>不添加任何依赖来构建自己的定制组件

>带有样式，拥有交互功能并且在各自文件中优雅组织的 HTML 标签

>https://developer.mozilla.org/zh-CN/docs/Web/Web_Components

>Web Components是一套不同的技术，允许您创建可重用的定制元素（它们的功能封装在您的代码之外）并且在您的web应用中使用它们。

>示例

>https://github.com/mdn/web-components-examples

>polyfill

>https://www.webcomponents.org/polyfills

>https://github.com/webcomponents/polyfills/tree/master/packages/webcomponentsjs

>https://unpkg.com/browse/@webcomponents/webcomponentsjs@2.2.10/

>npm install @webcomponents/webcomponentsjs

```html
<!-- load webcomponents bundle, which includes all the necessary polyfills -->
<script src="node_modules/@webcomponents/webcomponentsjs/webcomponents-bundle.js"></script>

<!-- load the element -->
<script type="module" src="my-element.js"></script>

<!-- use the element -->
<my-element></my-element>
```
***
#### 
>Web Component 是一系列 web 平台的 API，它们可以允许你创建全新可定制、可重用并且封装的 HTML 标签

>定制的组件基于 Web Component 标准构建，可以在现在浏览器上使用，也可以和任意与 HTML 交互的 JavaScript 库和框架配合使用。

>它赋予了仅仅使用纯粹的JS/HTML/CSS就可以创建可重用组件的能力。如果 HTML 不能满足需求，我们可以创建一个可以满足需求的 Web Component。

>举个例子，你的用户数据和一个 ID 有关，你希望有一个可以填入用户 ID 并且可以获取相应数据的组件。HTML 可能是下面这个样子：

```html
<user-card  user-id="1"></user-card>
```
***
### Web Component 的四个核心概念
>HTML 和 DOM 标准定义了四种新的标准来帮助定义 Web Component。这些标准如下：

#### 定制元素(Custom Elements): 
>web 开发者可以通过定制元素创建新的 HTML 标签、增强已有的 HTML 标签或是二次开发其它开发者已经完成的组件。这个 API 是 Web Component 的基石。

#### HTML 模板(HTML Templates): 
>HTML 模板定义了新的元素，描述一个基于 DOM 标准用于客户端模板的途径。模板允许你声明标记片段，它们可以被解析为 HTML。这些片段在页面开始加载时不会被用到，之后运行时会被实例化。

#### Shadow DOM: 
>Shadow DOM 被设计为构建基于组件的应用的一个工具。它可以解决 web 开发的一些常见问题，比如允许你把组件的 DOM 和作用域隔离开，并且简化 CSS 等等。

#### HTML 引用(HTML Imports): 
>HTML 模板(HTML Templates)允许你创建新的模板，同样的，HTML 引用(HTML imports)允许你从不同的文件中引入这些模板。通过独立的HTML文件管理组件，可以帮助你更好的组织代码。

***
#### 组件的命名
>定制元素的名称必须包含一个短横线。所以 \<my-tabs\> 和 \<my-amazing-website\> 是合法的名称, 而 \<foo\> 和  \<foo_bar\> 不行。

>在 HTML 添加新标签时需要确保向前兼容，不能重复注册同一个标签。

>定制元素标签不能是自闭合的，因为 HTML 只允许一部分元素可以自闭合。需要写成像 \<app-drawer\>\<\/app-drawer\> 这样的闭合标签形式。

***
#### 拓展组件

>创建组件时可以使用继承的方式。

>举个例子，如果想要为两种不同的用户创建一个 UserCard，

>你可以先创建一个基本的 UserCard 然后将它拓展为两种特定的用户卡片。

>Google web developers’ article https://developers.google.com/web/fundamentals/web-components/customelements#extend

***
#### 组件元素是类的实例
>组件元素是类的实例，就可以在这些类中定义公用方法。

>这些公用方法可以用来允许其它定制组件/脚本来和这些组件产生交互，而不是只能改变这些组件的属性。

***
#### 定义私有方法
>可以通过多种方式定义私有方法。我倾向于使用（立即执行函数），因为它们易写和易理解。

```ts
(function() {})();
```
#### 冻结类
>为了防止新的属性被添加，需要冻结你的类。

>这样可以防止类的已有属性被移除，或者已有属性的可枚举、可配置或可写属性被改变，同样也可以防止原型被修改。

```ts
class  MyComponent  extends  HTMLElement { ... }
const  FrozenMyComponent = Object.freeze(MyComponent);
customElements.define('my-component', FrozenMyComponent);
```
> 冻结类会阻止你在运行时添加补丁并且会让你的代码难以调试。

#### 服务器渲染 项目 注意事项

>鉴于 服务器的根路径的配置不统一

>import 可以使用绝对路径

>import 的 js 内部不可以再次 import ,会出现路径错误

```html
<script type="module" async>
    import 'https://xxx/button.js';
</script>
```