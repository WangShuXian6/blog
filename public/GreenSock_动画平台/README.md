#### GreenSock

>https://greensock.com/docs/NPMUsage

```bash
npm install gsap
```

***
>https://www.tweenmax.com.cn

>TweenMax中文网（GreenSock动画平台,GSAP）

>适用于移动端和现代互联网的超高性能专业级动画插件

>TweenMax/GreenSock交流QQ群：701123570

***

#### 默认使用ES模块（CommonJS / UMD仍然可用)

>导入以下各个类

```ts
import TweenMax from "gsap/TweenMax";
import Draggable from "gsap/Draggable"; 
```
***
>or

#### TweenMax包含（并导出）许多常用类

>（TweenMax包括TweenLite，TimelineLite，TimelineMax，CSSPlugin，RoundPropsPlugin，BezierPlugin，DirectionalRotationPlugin，AttrPlugin以及除CustomEase，CustomWiggle和CustomBounce之外的所有缓动）

```ts
import { TweenMax, TimelineLite, Power2, Elastic, CSSPlugin } from "gsap/TweenMax";

```
***
>or

#### “全部”文件导入/导出每个GSAP工具（除了仅限成员的奖励插件）

```ts
import { TimelineMax, CSSPlugin, ScrollToPlugin, Draggable } from "gsap/all"; 
```

***
>IMPORTANT: if your animations aren't working as expected, it's likely an issue with tree shaking (see below) which can be easily resolved by referencing any plugins you're using

>像Webpack这样的一些捆绑器提供了一个称为“树摇动”的便捷功能，它试图识别您在代码中没有引用的模块，并将它们从捆绑中删除以减小文件大小。听起来不错，但是GSAP插件（如CSSPlugin）通常不会被用户直接引用，因此它们已经成熟，可以通过树摇动而不小心被拔掉。这可能会打破你的动画。解决方案？只需在代码中的某处引用插件

```ts
import { TimelineLite, CSSPlugin, AttrPlugin }  from "gsap/all";

//without this line, CSSPlugin and AttrPlugin may get dropped by your bundler...
//没有这一行，你的捆绑包可能会丢弃CSSPlugin和AttrPlugin
const plugins = [ CSSPlugin, AttrPlugin ];

var tl = new TimelineLite();
tl.to(".myClass", 1, {x:100, attr:{width:300}});
```

>注意：为了最大化向后兼容性并避免树抖动问题，主TweenMax文件会自动激活历史上与之捆绑的所有类（如CSSPlugin，BezierPlugin，AttrPlugin等）。当然，这会相应地影响束大小。如果您只想提取基础TweenMax类（不自动激活其他类），则可以使用该TweenMaxBase文件（从2.0.0开始），如

```ts
//skips auto-activing the other plugins/classes
import { TweenMax } from "gsap/TweenMaxBase"; 
```

***
>要让树摇动在Webpack中工作，您可能需要{modules:false}在babel配置文件中设置。以下是一些可能有用的链接：

>https://webpack.js.org/guides/tree-shaking/

>https://webpack.js.org/loaders/babel-loader/

>https://rollupjs.org/guide/en#danger-zone

***

#### UMD/CommonJS

>常规的旧ES5 UMD（通用模块定义）版本的文件几乎兼容所有（RequireJS，Browserify等）

```ts
//get the UMD versions. Notice the "/umd/" in the path...
import { TweenMax, Power2, TimelineLite } from "gsap/umd/TweenMax"; 
import ScrollToPlugin from "gsap/umd/ScrollToPlugin"; 
import Draggable from "gsap/umd/Draggable";
```

***
#### 使用 MorphSVGPlugin 奖励插件，会员专用

>显然我们不能通过NPM 分发仅限会员的奖励插件，所以您需要做的就是登录您的GreenSock帐户并下载最新的zip，其中包含“bonus-files-for-npm-users”文件夹，其中包含只是奖金插件/工具。然后你可以把它放到你的项目中，比如你的/ src /文件夹（或者任何地方），然后直接导入它们。例如，为了节省一些打字，您可以将“bonus-files-for-npm-users”重命名为“gsap-bonus”，并将其放在项目的根目录中，然后

```ts
import MorphSVGPlugin from "./gsap-bonus/MorphSVGPlugin";
import SplitText from "./gsap-bonus/SplitText";
```

***

#### cdn

```html
<script src = “https://cdnjs.cloudflare.com/ajax/libs/gsap/2.1.3/TweenMax.min.js” > </ script> 
```

***
#### 为什么我不能从“gsap”导入任何类？

>因为历史上“gsap”指向TweenMax（在我们转移到ES模块之前）并且我们不想破坏现有项目或突然导致它们在构建系统中变得更大，而这些构建系统没有利用树摇动。我们非常重视向后兼容性。但是当我们迁移到3.0.0版本时

***
#### 类型定义

>我们没有任何官方的TypeScript定义文件（抱歉），但有两个可能符合您的需求：   

```ts
@types/greensock 
或 
@types/gsap
```
>如果您遇到任何问题，请联系这些回购的作者。

>此外，在某些环境（对ES模块不友好的环境）中，最好以稍微不同的方式导入UMD文件，例如

```ts
import * as Draggable from "gsap/umd/Draggable";
import * as TweenMax from "gsap/umd/TweenMax";
```

***
#### 路线图

>我们计划从一开始就完全构建3.0.0版本的ES模块，并稍微改变组织。这是一项艰巨的任务，所以请耐心等待。版本3.0.0可能会降低IE8支持并简化操作以及更改一些架构部分。