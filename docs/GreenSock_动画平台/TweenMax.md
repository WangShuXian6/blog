####  TweenMax
>greensock-js文件包是免费开源部分，包含有核心工具、过渡时间曲线、基本插件、拓展插件等。

>此外，GreenSock还有一些商业插件。

>下载免费greensock-js文件包和破解版商业插件

>https://www.tweenmax.com.cn/source/

***
#### 加载文件

```html
<script src="js/greensock-js/TweenLite.min.js"> </script>  -- 核心工具，可初始化TweenLite对象
<script src="js/greensock-js/plugins/CSSPlugin.min.js"> </script>  -- 基础插件，用于制作CSS动画2D，3D动画
<script src="js/greensock-js/plugins/BezierPlugin.min.js"> </script>  -- 基础插件，用于制作贝塞尔曲线
<script src="js/greensock-js/TimelineLite.min.js"> </script>  -- 核心工具，创建时间轴管理动画
<script src="js/greensock-js/easing/EasePack.min.js"> </script>  -- 拓展的时间曲线，例如bounce
```

#### 使用TweenMax上面的加载可以简约为

```html
<script src="js/greensock-js/TweenMax.min.js"> </script>
```

***

#### 制作动画

>动画的三要素：

>1.动画目标对象

>2.动画的持续时间

>3.变化的属性

![tweenmax-started-01](https://user-images.githubusercontent.com/30850497/59652425-417d7480-91c0-11e9-9d9e-ea36071f8c0b.png)

***
#### 制作一个简单的CSS动画。

>制作CSS动画需要用到CSSPlugin（已经包含于TweenMax）。

>使用TweenMax的to()方法，将一个id为"obj"的DOM元素的CSS属性的left属性从当前值过渡到500px，从而产生移动效果。持续时间3秒。

```html
<div id="obj"></div>
```

```css
#obj{
  position:relative;
  left:200px;
  top:20px;
  width:50px;
  height:50px;
  background:#6fb936;
  border-radius:5px;
}
```

```ts
TweenMax.to("#obj", 3, {left:500});
```
***
>left属性动画需要position:reletive支持，如需相对位置移动可使用x(CSS3的2D动画)更为简便

```ts
TweenMax.to("#obj", 3, {x:200});//在原有位置向右移动200px
TweenMax.to("#obj", 3, {x:200, y:100});//向右移动200px的同时向下移动100px
```

***
>元素从左边200px的位置开始运动到指定的位置

```ts
TweenLite.fromTo($box, 2, {x: '-=200px'}, {x: 150});
```
>x:150会覆盖在CSS中定义的transform: translate(–50%, –50%)的样式，用新的transform: matrix(1, 0, 0, 1, 150, -50);样式来代替。