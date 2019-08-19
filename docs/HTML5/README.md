#### HTML5
>[HTML5](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5)
>HTML5 是定义 HTML 标准的最新的版本。 该术语表示两个不同的概念：

>- 它是一个新版本的HTML语言，具有新的元素，属性和行为，
>- 它有更大的技术集，允许更多样化和强大的网站和应用程序。这个集合有时称为HTML5和朋友，通常缩写为HTML5。

***

##### 语义
>[HTML5 中的节段和提纲](

>https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/Sections_and_Outlines_of_an_HTML5_document)

```html
HTML5 中新的提纲和节段元素一览: 
<section>, <article>, <nav>, <header>, <footer>, <aside> 和 <hgroup>.
```

>[使用 HTML5 的音频和视频](
https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/Using_HTML5_audio_and_video)

```html
<audio> 和 <video> 元素嵌入和允许操作新的多媒体内容。
```
>[HTML5 的表单](
https://developer.mozilla.org/zh-CN/docs/HTML/Forms_in_HTML)

```html
看一下 HTML5 中对 web 表单的改进：
强制验证 API，一些新的属性，<input> 属性type 的一些新值 ，新的 <output> 元素。
```
>新的语义元素

```html
除了节段，媒体和表单元素之外，还有众多的新元素，
例如 <mark>， <figure>， <figcaption>， <data>， <time>， <output>， <progress>， 
或者 <meter>和<main>，这增加了有效的 HTML5 元素的数量。
```
>[iframe 的改进](
https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe)

```html
使用 sandbox， seamless， 和 srcdoc 属性，
作者们现在可以精确控制 <iframe> 元素的安全级别以及期望的渲染。
```
>[MathML](
https://developer.mozilla.org/zh-CN/docs/MathML)

```html
允许直接嵌入数学公式。
```
>[HTML5 入门](
https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/Introduction_to_HTML5)

>- 本文介绍了如何标示在网页设计或 Web 应用程序中使用 HTML5 时碰到的问题。

>[HTML5 兼容的解析器](
https://developer.mozilla.org/zh-CN/docs/HTML/HTML5/HTML5_Parser)

>- 用于把 HTML5 文档的字节转换成 DOM 的解释器，已经被扩展了，并且现在精确地定义了在所有情况下使用的行为，甚至当碰到无效的 HTML 这种情况。这就导致了 HTML5 兼容的浏览器之间极大的可预测性和互操作性。

***

##### 通信
>[Web Sockets](
https://developer.mozilla.org/zh-CN/docs/WebSockets)

>- 允许在页面和服务器之间建立持久连接并通过这种方法来交换非 HTML 数据。

>[Server-sent events](
https://developer.mozilla.org/zh-CN/docs/Server-sent_events/Using_server-sent_events)

>- 允许服务器向客户端推送事件，而不是仅在响应客户端请求时服务器才能发送数据的传统范式。

>[WebRTC](
https://developer.mozilla.org/zh-CN/docs/WebRTC)

>- 这项技术，其中的 RTC 代表的是即时通信，允许连接到其他人，直接在浏览器中控制视频会议，而不需要一个插件或是外部的应用程序。

***

##### 离线 & 存储
>[离线资源：应用程序缓存](
https://developer.mozilla.org/zh-CN/docs/HTML/Using_the_application_cache)

>- 火狐全面支持 HTML5 离线资源规范。其他大多数针对离线资源仅提供了某种程度上的支持。

>[在线和离线事件](
https://developer.mozilla.org/zh-CN/docs/Online_and_offline_events)

>- Firefox 3 支持 WHATWG 在线和离线事件，这可以让应用程序和扩展检测是否存在可用的网络连接，以及在连接建立和断开时能感知到。

>[WHATWG 客户端会话和持久化存储 (又名 DOM 存储)](
https://developer.mozilla.org/zh-CN/docs/Web/Guide/API/DOM/Storage/Storage)

>- 客户端会话和持久化存储让 web 应用程序能够在客户端存储结构化数据。

>[IndexedDB](
https://developer.mozilla.org/zh-CN/docs/IndexedDB)

>- 是一个为了能够在浏览器中存储大量结构化数据，并且能够在这些数据上使用索引进行高性能检索的 Web 标准。

>[自 web 应用程序中使用文件](
https://developer.mozilla.org/zh-CN/docs/Using_files_from_web_applications)

```html
对新的 HTML5 文件 API 的支持已经被添加到 Gecko 中，
从而使 Web 应用程序可以访问由用户选择的本地文件。
这包括使用 type file 的 <input> 元素的新的 multiple 属性针对多文件选择的支持。 
还有 FileReader。
```

***

##### 多媒体
>[使用 HTML5 音视频](
https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Multimedia_and_embedding/Video_and_audio_content)

```html
<audio> 和 <video> 元素嵌入并支持新的多媒体内容的操作。
```
>[WebRTC](
https://developer.mozilla.org/zh-CN/docs/WebRTC)

>_ 这项技术，其中的 RTC 代表的是即时通信，允许连接到其他人，在浏览器中直接控制视频会议，而不需要一个插件或是外部的应用程序。

>[使用 Camera API](
https://developer.mozilla.org/zh-CN/docs/DOM/Using_the_Camera_API)

>- 允许使用，操作计算机摄像头，并从中存储图像。

>[Track 和 WebVTT]()

```html
 <track> 元素支持字幕和章节。WebVTT 一个文本轨道格式。
```

***

##### 3D, 图像 & 效果
>[Canvas 教程](
https://developer.mozilla.org/zh-CN/docs/Canvas_tutorial)

```html
了解有关新的 <canvas> 元素以及如何在火狐中绘制图像和其他对象。
```
>[HTML5 针对 <canvas> 元素的文本 API](
https://developer.mozilla.org/zh-CN/docs/Drawing_text_using_a_canvas)

```html
HTML5 文本 API 现在由 <canvas> 元素支持。
```
>[WebGL](
https://developer.mozilla.org/zh-CN/docs/WebGL)

```html
WebGL 通过引入了一套非常地符合 OpenGL ES 2.0 
并且可以用在 HTML5 <canvas> 元素中的 API 给 Web 带来了 3D 图像功能。
```
>[SVG](
https://developer.mozilla.org/zh-CN/docs/SVG)

>- 一个基于 XML 的可以直接嵌入到 HTML 中的矢量图像格式。

***

##### 性能 & 集成
>[Web Workers](
https://developer.mozilla.org/zh-CN/docs/DOM/Using_web_workers)

>- 能够把 JavaScript 计算委托给后台线程，通过允许这些活动以防止使交互型事件变得缓慢。

>[XMLHttpRequest Level 2](
https://developer.mozilla.org/zh-CN/docs/DOM/XMLHttpRequest)

>- 允许异步读取页面的某些部分，允许其显示动态内容，根据时间和用户行为而有所不同。这是在 Ajax背后的技术。

>即时编译的 JavaScript 引擎

>- 新一代的 JavaScript 引擎功能更强大，性能更杰出。

>[History API](
https://developer.mozilla.org/zh-CN/docs/DOM/Manipulating_the_browser_history)

>- 允许对浏览器历史记录进行操作。这对于那些交互地加载新信息的页面尤其有用。

>[contentEditable 属性：把你的网站改变成 wiki !](
https://developer.mozilla.org/zh-CN/docs/HTML/Content_Editable)

>- HTML5 已经把 contentEditable 属性标准化了。了解更多关于这个特性的内容。

>[拖放](
https://developer.mozilla.org/zh-CN/docs/DragDrop/Drag_and_Drop)

>- HTML5 的拖放 API 能够支持在网站内部和网站之间拖放项目。同时也提供了一个更简单的供扩展和基于 Mozilla 的应用程序使用的 API。

>[HTML 中的焦点管理](
https://developer.mozilla.org/zh-CN/docs/HTML/Focus_management_in_HTML)

>-支持新的 HTML5 activeElement 和 hasFocus 属性。

>[基于 Web 的协议处理程序](
https://developer.mozilla.org/zh-CN/docs/Web-based_protocol_handlers)

>- 你现在可以使用 navigator.registerProtocolHandler() 方法把 web 应用程序注册成一个协议处理程序。

>[requestAnimationFrame](
https://developer.mozilla.org/zh-CN/docs/DOM/window.requestAnimationFrame)

>- 允许控制动画渲染以获得更优性能。

>[全屏 API](
https://developer.mozilla.org/zh-CN/docs/DOM/Using_fullscreen_mode)

>- 为一个网页或者应用程序控制使用整个屏幕，而不显示浏览器界面。

>[指针锁定 API](
https://developer.mozilla.org/zh-CN/docs/API/Pointer_Lock_API)

>- 允许锁定到内容的指针，这样游戏或者类似的应用程序在指针到达窗口限制时也不会失去焦点。

>[在线和离线事件](
https://developer.mozilla.org/zh-CN/docs/Online_and_offline_events)

>- 为了构建一个良好的具有离线功能的 web 应用程序，你需要知道什么时候你的应用程序确实离线了。顺便提一句，在你的应用程序又再回到在线状态时你也需要知道。

***

##### 设备访问
>[使用 Camera API](
https://developer.mozilla.org/zh-CN/docs/DOM/Using_the_Camera_API)

>- 允许使用和操作计算机的摄像头，并从中存取照片。

[触控事件](
https://developer.mozilla.org/zh-CN/docs/DOM/Touch_events)
对用户按下触控屏的事件做出反应的处理程序。

[使用地理位置定位](
https://developer.mozilla.org/zh-CN/docs/WebAPI/Using_geolocation)
让浏览器使用地理位置服务定位用户的位置。

[检测设备方向](
https://developer.mozilla.org/zh-CN/docs/Detecting_device_orientation)
让用户在运行浏览器的设备变更方向时能够得到信息。这可以被用作一种输入设备（例如制作能够对设备位置做出反应的游戏）或者使页面的布局跟屏幕的方向相适应（横向或纵向）。

[指针锁定 API](
https://developer.mozilla.org/zh-CN/docs/API/Pointer_Lock_API)
允许锁定到内容的指针，这样游戏或者类似的应用程序在指针到达窗口限制时也不会失去焦点。

***

##### 样式
>CSS 已经扩展到能够以一个更加复杂的方法给元素设置样式。这通常被称为 CSS3, 尽管 CSS 已经不再是很难触动的规范，并且不同的模块并不全部位于 level 3：其中一些位于 level 1 而另一些位于 level 4，覆盖了所有中间的层次。

>新的背景样式特性

>- 现在可以使用 box-shadow 给逻辑框设置一个阴影，而且还可以设置 多背景。

>更精美的边框

>- 现在不仅可以使用图像来格式化边框，使用 border-image 和它关联的普通属性，而且可以通过 border-radius 属性来支持圆角边框。

>为你的样式设置动画

>- 使用 CSS Transitions 以在不同的状态间设置动画，或者使用 CSS Animations 在页面的某些部分设置动画而不需要一个触发事件，你现在可以在页面中控制移动元素了。

>排版方面的改进

>- 作者们如今有更强大的能力来使自己的网页文字达到更佳的排版。他们不但可以控制 text-overflow 和 hyphenation， 还可以给它设置一个 阴影 或者更精细地控制它的 decorations。感谢新的 @font-face 规则，现在我们可以下载并应用自定义的字体了。

>新的展示性布局

>- 为了提高设计的灵活性，已经有两种新的布局被添加了进来：CSS 多栏布局, 以及 CSS 灵活方框布局。

***