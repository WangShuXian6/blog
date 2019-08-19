### TweenMax API 动画结构

***

### TweenMax()

```ts
.TweenMax( target:Object, duration:Number, vars:Object ) ;
```
>TweenMax的构造函数，用来构建一个TweenMax对象。

>如果使用TweenLite则是TweenLite();

>TweenMax()适用于TweenMaxTweenLite

>TweenMax()的参数

参数名 | 类型 | 是否必填 | 描述
-- | -- | -- | --
target | object | 是 | 需要缓动的对象
duration | number | 是 | 动画持续时间，一般是秒
vars | object | 是 | 动画参数

>往右移动500px，同时改变透明度

```html
<div class="box green"></div>
<p>往右移动500px，同时改变透明度</p>
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
new TweenMax('.box', 3, {
    x: 500,
    alpha : 0.3,
});


/*还可通过function关键词返回变化值
TweenMax.to(".box", 1, {
  x: function() {
    return Math.random() * 300;
  }
});
*/
```
>TweenMax()的补充说明

>例子中改变CSS（x: 500, alpha : 0.3）

>如果加载的是TweenLite.min.js，则还需要加载CSS插件plugins/CSSPlugin.min.js才能进行css动画。

***

### TweenMax.to()
```ts
TweenMax.to( target:Object, duration:Number, vars:Object ) : TweenMax
```
>此方法用于创建一个从当前属性到指定目标属性的TweenMax动画对象。

>以下几种方式效果相同。

```ts
TweenMax.to(obj, 1, {x:100});
var myTween = new TweenMax(obj, 1, {x:100})
var myTween = TweenMax.to(obj, 1, {x:100});
```
>对多个目标进行动画

```ts
TweenMax.to([obj1, obj2, obj3], 1, {x:100});
```

>TweenMax.to()适用于TweenMaxTweenLite

>TweenMax.to()的参数

参数名 | 类型 | 是否必填 | 描述
-- | -- | -- | --
target | object | 是 | 需要动画的对象
duration | number | 是 | 动画持续时间，一般是秒
vars | object | 是 | 动画参数（CSS属性、延迟、重复次数等）

>对三个元素进行动画，分别向X轴方向运动100px、200px、300px

```html
<div class="wrapper">
  <div class="box green"></div>
  <div class="box orange"></div>
  <div class="box grey"></div>
</div>
```
```css
.box {
    width:50px;
    height:50px;
    border-radius:6px;
    margin-top:4px;
  }
.green{
    background-color:#6fb936;
  }
.orange {
    background-color: #f38630;
}
.grey {
    background-color: #989898;
}
```
```ts
var myTween = TweenMax.to(".box", 1, {
  x: function(index, target) {
    console.log(index, target);
    return (index + 1) * 100 // 100, 200, 300
  }
})
```

***
### TweenMax.from()
```ts
TweenMax.from( target:Object, duration:Number, vars:Object ) : TweenMax
```
>通过设置动画起始点来初始化一个TweenMax，相当于动画从设置点开始。

```ts
TweenMax.from(mc, 1.5, {opacity:0, delay:2});
```
>多个目标的动画

```ts
TweenMax.from([mc1, mc2, mc3], 1.5, {opacity:0});
```

>TweenMax.from()适用于TweenMaxTweenLite

>TweenMax.from()的参数


参数名 | 类型 | 是否必填 | 描述
-- | -- | -- | --
target | object | 是 | 需要动画的对象
duration | number | 是 | 动画持续时间，一般是秒
vars | object | 是 | 设置动画的一些属性及其他参数

>元素从500px开始，返回原来位置

```html
<div class="box green"></div>
<p>元素从500px开始，返回原来位置</p>
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
  }
.green{
    background-color:#6fb936;
  }
```
```ts
var myTween = TweenMax.from(".box", 3, {
  x: 500,
})
```

***

### TweenMax.fromTo()
```ts
TweenMax.fromTo( target:Object, duration:Number, fromVars:Object, toVars:Object ) : TweenMax
```
>通过设置动画起始点和结束点来初始化一个TweenMax，相当于动画从设置点到第二个设置点。

```ts
TweenMax.fromTo([element1, element2], 1, {x:200}, {x:500});
```

>TweenMax.fromTo()适用于TweenMaxTweenLite

>TweenMax.fromTo()的参数


参数名 | 类型 | 是否必填 | 描述
-- | -- | -- | --
target | object | 是 | 需要动画的对象
duration | number | 是 | 动画持续时间，一般是秒
fromVars | object | 是 | 起始点动画参数
toVars | object | 是 | 结束点动画参数

>元素从200px移动到500px

```html
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
var myTween=TweenMax.fromTo('.box', 3, {x: 200,},{x: 500,});
//从200到500
```

***

### TweenMax.staggerTo()
```ts
TweenMax.staggerTo(
    targets: Array, 
    duration: Number, 
    vars: Object, 
    stagger: Number, 
    onCompleteAll: Function, 
    onCompleteAllParams: Array, 
    onCompleteAllScope:* 
    ) : Array
```
>stagger系列方法为多个目标制作一个有间隔的动画序列，相当于多个TweenMax的数组。
需要设置每个动画的开始间隔。如不设置则为零，同时开始动画。

```ts
var objects = [obj1, obj2, obj3, obj4, obj5];
TweenMax.staggerTo(objects, 1, {y:"+=150", opacity:0}, 0.2);
```
>TweenMax.staggerTo()适用于TweenMax

>TweenMax.staggerTo()的参数


参数名 | 类型 | 是否必填 | 描述
-- | -- | -- | --
targets | Array | 是 | 要进行动画的对象，可以有多个，以数组形式传入
duration | number | 是 | 动画持续的秒数（或帧数，如果设置了useFrames:true）
vars | object | 是 | 设置动画的一些属性及其他参数
stagger | Number | 否 | 每个动画的起始间隔，默认是0
onCompleteAll | Function | 否 | 当所有显示对象都完成动画后要调用的函数
onCompleteAllParams | Array | 否 | onCompleteAll函数的参数，以数组形式传入
onCompleteAllScope |   | 否 | onCompleteAll函数的作用域，this

>每个动画序列的开始时间间隔0.5秒

```html
 <h2>TweenMax.staggerTo()</h2>
  <div id="demo">
    <p>每个动画序列的开始时间间隔0.5秒</p>
    <div class="box green"></div>
    <div class="box grey"></div>
    <div class="box orange"></div>
    <div class="box green"></div>
    <div class="box grey"></div>
    <div class="box orange"></div>
    <div class="box green"></div>
    <div class="box grey"></div>
    <div class="box orange"></div>
  </div>
```
```css
body {
  background-color:#1d1d1d;
  color:#f3f2ef;
}
h2 {
  font-family: "Signika Negative", sans-serif;
  margin: 10px 0 10px 0;
  font-size:30px;
}
p{
  line-height:22px;
  margin-bottom:16px;
}
#demo {
  height:100%;
  position:relative;
}
.box {
  width:50px;
  height:50px;
  position:relative;
  border-radius:6px;
  margin-top:4px;
  display:inline-block
}
.green{
  background-color:#6fb936;
}
.orange {
  background-color:#f38630;
}
.grey {
  background-color:#989898;
}
```
```ts
TweenMax.staggerTo(".box", 1, {rotation:360, y:100}, 0.5);
```

>TweenMax.staggerTo()的补充说明

>stagger系列方法可用于TweenMax、TimelineLite、TimelineMax，不可用于TweenLite。

>由于stagger系列方法（staggerTo、staggerFrom、staggerFromTo）返回的是一个tween的数组，所以不可以直接对其返回值直接使用tween的属性和方法。

```ts
tween=TweenMax.to(...)
tween.play()//播放动画

tween=TweenMax.staggerTo(...)
tween.play()//无法使用
```
>stagger系列方法可以使用cycle属性来循环设置动画参数值。

***

### TweenMax.staggerFrom()
```ts
TweenMax.staggerFrom( 
    targets:Array, 
    duration:Number, 
    vars:Object, 
    stagger:Number, 
    onCompleteAll:Function, 
    onCompleteAllParams:Array, 
    onCompleteAllScope:* 
    ) : Array
```
>通过设定序列动画的终点来初始化一组TweenMax。

```ts
var objects = [obj1, obj2, obj3, obj4, obj5];
TweenMax.staggerFrom(objects, 1, {y:"+=150"}, 0.2);
```
>TweenMax.staggerFrom()适用于TweenMax

>TweenMax.staggerFrom()的参数


参数名 | 类型 | 是否必填 | 描述
-- | -- | -- | --
targets | Array | 是 | 要进行动画的对象，可以有多个，以数组形式传入
duration | number | 是 | 动画持续的秒数（或帧数，如果设置了useFrames:true）
vars | object | 是 | 设置动画的一些属性及其他参数
stagger | Number | 否 | 每个动画的间隔，默认是0
onCompleteAll | Function | 否 | 当所有显示对象都完成动画后要调用的函数
onCompleteAllParams | Array | 否 | 传递给onCompleteAll函数的参数，以数组形式传入
onCompleteAllScope |   | 否 | onCompleteAll函数的作用域

>展示

```html
<h2>TweenMax.staggerFrom() with cycle</h2>
<div id="demo">
   <p>cycle属性允许你在所有TweenMax、TimelineLite和 TimelineMax插件中基于stagger的函数里面，定义一个array的属性值或者function的属性值 (注释中有详细列表)。</p>
   
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
</div>
```
```css
body {
  background-color:#1d1d1d;
  color:#989898;
}
h2{
  margin: 10px 0 10px 0;
  font-size:30px;
}
p{
  line-height:22px;
  margin-bottom:16px;
  width:650px;
}
#demo {
  height:100%;
  position:relative;
}
.box {
  width:50px;
  height:50px;
  position:relative;
  border-radius:6px;
  margin:4px;
  display:inline-block;
  background:grey;
}

.green{
  background-color:#6fb936;
}

.orange {
  background-color:#f38630;
}
.grey {
  background-color:#989898;
}

```
```ts
TweenMax.staggerFrom(".box", 1, {
  cycle:{
    //an array of values
    backgroundColor:["red", "white", "#00f"],
    //function that returns a value
    y:function(index){
      return index * 20;
    }
  }
}, 0.5);

/* The cycle property is available in:

TweenMax.staggerFromTo()
TweenMax.staggerFrom()
TweenMax.staggerTo()
TimelineLite.staggerFromTo()
TimelineLite.staggerFrom()
TimelineLite.staggerTo()
TimelineMax.staggerFromTo()
TimelineMax.staggerFrom()
TimelineMax.staggerTo()

*/
```
>stagger系列方法可以使用cycle属性来循环设置动画参数值

>TweenMax.staggerFrom() with cycle

>cycle属性允许你在所有TweenMax、TimelineLite和 TimelineMax插件中基于stagger的函数里面，定义一个array的属性值或者function的属性值 (注释中有详细列表)。

***

### TweenMax.staggerFromTo()
```ts
TweenMax.staggerFromTo( 
    targets:Array, 
    duration:Number, 
    fromVars:Object, 
    toVars:Object, 
    stagger:Number, 
    onCompleteAll:Function, 
    onCompleteAllParams:Array, 
    onCompleteAllScope:* 
    ) : Array
```
>通过设定序列动画的起点和终点来初始化一个TweenMax。

```ts
var objects = [obj1, obj2, obj3, obj4, obj5];
TweenMax.staggerFromTo(objects, 1, {opacity:1}, {opacity:0}, 0.2);
```
>还可以使用cycle属性。

>TweenMax.staggerFromTo()适用于TweenMax

>TweenMax.staggerFromTo()的参数


参数名 | 类型 | 是否必填 | 描述
-- | -- | -- | --
targets | Array | 是 | 要进行动画的对象，可以有多个，以数组形式传入
duration | number | 是 | 动画持续的秒数（或帧数，如果设置了useFrames:true）
fromVars | object | 是 | 设置动画的一些属性及其他参数
toVars | object | 是 | 设置动画的一些属性及其他参数
stagger | Number | 否 | 每个动画的间隔，默认是0
onCompleteAll | Function | 否 | 当所有显示对象都完成动画后要调用的函数
onCompleteAllParams | Array | 否 | 传递给onCompleteAll函数的参数，以数组形式传入
onCompleteAllScope |   | 否 | onCompleteAll函数的作用域

>展示

```html
<h2>TweenMax.staggerFromTo() with cycle</h2>
<div id="demo">
  <p>cycle属性允许你在所有TweenMax、TimelineLite和 TimelineMax插件中基于stagger的函数里面，定义一个array的属性值或者function的属性值 (注释中有详细列表)。</p>
   
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
  <div class="box"></div>
</div>
```
```css
body {
  background-color:#1d1d1d;
  font-family: "Asap", sans-serif;
  color:#989898;
  margin:0 10px;
  font-size:16px;
}

h1, h2, h3 {
  font-family: "Signika Negative", sans-serif;
  margin: 10px 0 10px 0;
  color:#f3f2ef;
}

h1 { 
  font-size:36px;
}

h2 {
  font-size:30px;
}

h3 {
  font-size:24px;
}

p{
  line-height:22px;
  margin-bottom:16px;
  width:650px;
}

#demo {
  position:relative;
}
.box {
  width:50px;
  height:50px;
  position:relative;
  border-radius:6px;
  margin:4px;
  display:inline-block;
  background:grey;
}
.green{
  background-color:#6fb936;
}
.orange {
  background-color:#f38630;
}
.grey {
  background-color:#989898;
}
```
```ts
TweenMax.staggerFromTo(".box", 1, {
  cycle:{
    //an array of values
    backgroundColor:["red", "white", "#00f"],
    //function that returns a value
    y:function(index){
      return index * 10;
    }
  }
},
  
 {
  cycle:{
    //an array of values
    backgroundColor:["green", "orange", "pink"],
    //function that returns a value
    y:function(){
      return (Math.random() * 100) + 20;
    }
  }
  
}, 0.5);

/* The cycle property is available in:

TweenMax.staggerFromTo()
TweenMax.staggerFrom()
TweenMax.staggerTo()
TimelineLite.staggerFromTo()
TimelineLite.staggerFrom()
TimelineLite.staggerTo()
TimelineMax.staggerFromTo()
TimelineMax.staggerFrom()
TimelineMax.staggerTo()

*/
```
***

### TweenMax.delayedCall()
```ts
TweenMax.delayedCall( 
    delay:Number, 
    callback:Function, 
    params:Array, 
    scope:*, 
    useFrames:Boolean 
    ) : TweenMax
```
>提供一种在设定的时间（或帧）后调用函数的简单方法。

```ts
//1秒后执行myFunction并传递两个参数:
TweenMax.delayedCall(1, myFunction, ["param1 value", "param2 value"],document,true);
function myFunction(param1, param2) {
    console.log(param1+param2+this)
}
```
>TweenMax.delayedCall()适用于TweenMaxTweenLite

>TweenMax.delayedCall()的参数


参数名 | 类型 | 是否必填 | 描述
-- | -- | -- | --
delay | Number | 是 | 要延迟的秒数（或帧数，如果设置了useFrames:true）
callback | Function | 是 | 要延迟执行的函数
params | Array | 否 | 传递给onComplete函数的参数，以数组形式传入
scope | * | 否 | 函数的作用域，this的指向，默认为空
useFrames | Boolean | 否 | 设定延迟的时间模式是基于秒数还是帧数 ，默认false：秒

>展示 延迟2秒输出"+param1+'和'+param2

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
var myTween=new TweenMax('.box', 3, {
    x: 500,
})
var myTween2=TweenMax.delayedCall(2,myFunction,["参数1","参数2"])
function myFunction(param1, param2) {
   alert("延迟2秒输出"+param1+'和'+param2);
}
```

***

### TweenMax.set()
```ts
TweenMax.set( target:Object, vars:Object ) : TweenMax
```
>立即设置目标的属性值而不产生过渡动画，相当于0的动画时间。

返回TweenMax对象。

```ts
//以下两个设置作用相同
TweenMax.set(myObject, {x:100});
TweenMax.to(myObject, 0, {x:100});
```
>TweenMax.set()适用于TweenMaxTweenLite

>TweenMax.set()的参数


参数名 | 类型 | 是否必填 | 描述
-- | -- | -- | --
target | object | 是 | 需要移动的对象
vars | object | 是 | 动画参数

>右边的方块设置了透视transformPerspective，因此产生了3D效果

```html
<p>右边的方块设置了透视transformPerspective，因此产生了3D效果</p>
<div class="wrapper">
<div class="box"></div>
<div class="nbox box"></div>
</div>
```
```css
.box {
  width:100px;
  height:100px;
  background-color:#6fb936;
  margin:10px 100px 0;
  display:inline-block;
}
.600box {
  margin-top: 10px;
}
.nbox {
  display:inline-block;
  margin: 10px auto 0;
  width:100px;
  height:100px;
}
```
```ts
TweenMax.set(".nbox",{transformPerspective:300});
//或者设置父级元素，使其全部子元素产生3D效果 TweenMax.set(".wrapper",{perspective:200});
TweenMax.to(".box", 3, {rotationY:360, transformOrigin:"left top"})
```

>为一个数组设置属性

```ts
TweenMax.set([obj1, obj2, obj3], {x:100, y:50, opacity:0});
```

#### 3D效果

>在平时的动效开发中，为了使动效具有立体的效果，一般会用到CSS3中的3D transform这一属性。在TweenMax中也提供了3D transform功能，支持CSS3D的全部属性，使用起来比CSS3更加方便。
perspective和transformPerspective两个属性。它们是TweenMax中运行的基础，使用它们才能使元素具有一个3D空间透视的表现能力。

>transformPerspective是针对单个元素使用的，而perspective则是使用在父级元素上，使用它会使该父级元素下的子元素具有3D空间透视的一个展现形式。
只需要在父级使用perspective的方法，可以同时使它的子元素都具有3D的展现。

#### transformOrigin

>transformOrigin是用来设置元素在做transform位移变换时的原点的。默认值是元素的中心点即("50%,50%")。transformOrigin有三个值(x,y,z)，z值是一个可选项。

>可以使用"top", "left", "right",或者是"bottom"值亦或是百分值(比如bottom right可以设置为 "100% 100%)。