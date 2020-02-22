### TweenMax API 动画初始设置

***
### delay
```ts
delay:Number
```
>动画开始之前的延迟秒数（以帧为单位时则为帧数）。

>delay适用于TweenMaxTweenLite

>延迟三秒开始动画

```html
<div class="box green"></div>
<p>延迟三秒开始动画</p>
```
```css
body {
    background: #f8f8f8;
    font-family: Helvetica Neue, Helvetica, Arial, sans-serif;
    font-size: 14px;
    color: #000;
    margin: 0 10px;
    padding: 0;
}
.box {
    width:50px;
    height:50px;
    line-height:50px;
    text-align:center;
    color:#fff;
    font-size:18px;
    border-radius:6px;
    margin-top:4px;
    display:inline-block
  }
.green{
    background-color:#6fb936;
  }
```
```ts
new TweenMax('.box', 3, {
    x: 500,
    delay: 3
});

time={score:3}
TweenMax.to(time, 3, {
    score: 0,
    ease: Power0.easeNone,
    onUpdate:function(){
        document.querySelector(".box").innerHTML=Math.ceil(time.score);
    }
});
```

***

### ease
>ease: Ease (or Function or String)

>过渡效果的速度曲线（缓动效果）。你可以在动画的参数中设置各种缓动来控制动画的变化率，赋予其特定的“感觉”。例如Elastic.easeOut或 Strong.easeInOut。默认是Power1.easeOut。

```ts
new TweenMax('.box', 3, {
    x: 500,
    ease: Bounce.easeOut
});
```
>TweenLite中包含了基本缓动：Power0、Power1、Power2、Power3、Power4、Linear、Quad、Cubic、Quart、Quint、Strong，他们每个都含有.easeIn、.easeOut、.easeInOut参数（对于线性动画，请使用Power0.easeNone）。
而TweenMax在此基础上还另外增加了特殊缓动：Elastic、Back、Bounce、SlowMo、SteppedEase、RoughEase、Circ、Expo、Sine。
如果想在TweenLite中使用特殊缓动则需要加载缓动类easing/EasePack.min.js 。

>ease适用于TweenMaxTweenLite

>展示

```html
<div class="box green"></div>
```
```css
body {
    background: #f8f8f8;
    font-family: Helvetica Neue, Helvetica, Arial, sans-serif;
    font-size: 14px;
    color: #000;
    margin: 0 10px;
    padding: 0;
}
.box {
    width:50px;
    height:50px;
    border-radius:6px;
    margin-top:4px;
    display:inline-block
  }
.green{
    background-color:#6fb936;
  }
```
```ts
new TweenMax('.box', 3, {
    x: 500,
    ease: Bounce.easeOut
});
```

>ease的补充说明

>此外，缓动效果还可以像Jquery那样写，easeOutStrong 等同于Strong.easeOut 。

>在TweenMax2.0中，Power0取代了Linear，Power1取代了Quad，Power2取代了Cubic，Power3取代了Quart，Power4取代了Quint/Strong。

>Back还可设定强度，如ease: Back.easeOut.config(1.7)

>Elastic还可设定强度，如ease: Elastic.easeOut.config(1, 0.3)

>SlowMo还可设定强度，如ease: SlowMo.ease.config(0.7, 0.7, false)

>SteppedEase还可设定阶数，如ease: SteppedEase.config(12)

>RoughEase还需要设定其他参数


***
### paused
```ts
paused: Boolean
```
>如果设置为true，动画将在创建时立即暂停。默认false

>paused适用于TweenMaxTweenLite

>展示

```html
<div class="box green"></div>
<br>
<input type="button" id="playBtn" value="play()" title="播放">
```
```css
body {
    background: #f8f8f8;
    font-family: Helvetica Neue, Helvetica, Arial, sans-serif;
    font-size: 14px;
    color: #000;
    margin: 0 10px;
    padding: 0;
}
.box {
    width:50px;
    height:50px;
    border-radius:6px;
    margin-top:4px;
    display:inline-block
  }
.green{
    background-color:#6fb936;
  }
```
```ts
tween = new TweenMax('.box', 3, {
    x: 500,
    paused: true
});

playBtn = document.getElementById("playBtn")
playBtn.onclick = function() {
    tween.play();
}
```
***
### immediateRender
```ts
immediateRender:Boolean
```
>立即渲染，默认false。

>一般来说，TweenMax的运动对象会在下一个渲染周期前(也就是下一帧)被渲染到场景中，除非你设置了delay。如果想强制立即渲染，可以把这个参数设为true。

>另外from()方法的运动对象是立即渲染的（默认true），如果你不想该运动对象被渲染，可以把这个参数设为false。

>immediateRender适用于TweenMaxTweenLite

>橙色块设置了不立即渲染，因此动画开始前没有到达指定位置

```html
<div class="box green"></div>
<p>橙色块设置了不立即渲染，因此动画开始前没有到达指定位置</p>
<div class="box orange"></div>
```
```css
body {
    background: #f8f8f8;
    font-family: Helvetica Neue, Helvetica, Arial, sans-serif;
    font-size: 14px;
    color: #000;
    margin: 0 10px;
    padding: 0;
}
.box {
    width:50px;
    height:50px;
    border-radius:6px;
    margin-top:4px;
    display:inline-block
  }
.green{
    background-color:#6fb936;
  }
.orange {
    background-color: #f38630;
}
```
```ts
TweenMax.from('.green', 3, {
    x: 500,
    delay:3,
});
TweenMax.from('.orange', 3, {
    x: 500,
    delay:3,
    immediateRender: false,
});
```
***
### overwrite
```ts
overwrite: String (or int)
```
>用来控制同一个对象上有多个动画时的覆盖之类的情况。

```ts
TweenMax.to('.box', 6, {x: 700,y:100,});
TweenMax.to('.box', 3, {x: 200,overwrite:"none"});
//或者
TweenMax.to('.box', 3, {x: 200,overwrite:0});
```
>overwrite适用于TweenMaxTweenLite

>有以下6种模式，以上例来说明：

>overwrite的参数


模式 | 模式代码 | 说明 | 效果
-- | -- | -- | --
0 | "none"或者false | 不做任何处理 | 前三秒运行x: 200,y:100后三秒运行x: 700,y:100
1 | "all"或者true | 覆盖所有 | 只运行x: 200
2 | "auto" | 仅覆盖重复的属性默认模式 | 前三秒运行x: 200,y:100后三秒运行y:100
3 | "concurrent" | 同时发生 | 跟模式1很相似，不同是它只覆盖掉正在运行的动画属性。而放过其他的没有启动的动画属性。
4 | "allOnStart" | 开始时覆盖 | 跟模式1非常像。两点不同是他是在动画属性第一次渲染时才覆盖掉其他所有的动画属性，而且这个会把在他之后创建的动画属性也覆盖掉。
5 | "preexisting" |   | 在动画属性第一次渲染时才覆盖掉其他所有的动画属性。

>默认模式2，"auto"

```html
<p>默认模式2，"auto"</p>
<div class="box green"></div>
```
```css
.box {
    width:50px;
    height:50px;
    border-radius:6px;
    margin-top:4px;
    display:inline-block
  }
.green{
    background-color:#6fb936;
  }
```
```ts
TweenMax.to('.box', 6, {x: 700,y:100,});
TweenMax.to('.box', 3, {x: 200,overwrite:2});
```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***
###

```html

```
```css

```
```ts

```
***