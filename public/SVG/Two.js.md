#### 第三方绘图工具库——Two.js的使用
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    svg,
    canvas {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>第三方绘图工具库——Two.js的使用</h3>
<div id="container"></div>
<script src="../lib/two.js"></script>
<script>
  var params = {width: 800, height: 400, type: Two.Types.svg};
  //var params = {width:800, height:400, type: Two.Types.webgl};
  //var params = {width:800, height:400, type: Two.Types.canvas};
  var two = new Two(params).appendTo(container);
  //添加一个圆形
  var circle = two.makeCircle(200, 200, 100);
  circle.fill = '#fa3';
  circle.opacity = 0.6;
  circle.stroke = '#a80';
  circle.linewidth = 8;
  //添加一个矩形——定位点在矩形中央
  var rect = two.makeRectangle(600, 200, 200, 200);
  rect.fill = '#2bf';
  rect.stroke = 'transparent';
  //记得更新界面视图，把内存中的图形绘制到浏览器
  two.update();
</script>
</body>
</html>

```
![svg2-13-1](https://user-images.githubusercontent.com/30850497/54585773-0307da80-4a56-11e9-8431-de57dbd79095.jpg)

***
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    svg,
    canvas {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>第三方绘图工具库——Two.js的使用</h3>
<div id="container"></div>
<script src="../lib/two.js"></script>
<script>
  var params = {width: 800, height: 400, type: Two.Types.svg};
  //var params = {width:800, height:400, type: Two.Types.webgl};
  //var params = {width:800, height:400, type: Two.Types.canvas};
  var two = new Two(params).appendTo(container);
  //添加一个圆形
  var circle = two.makeCircle(200, 200, 100);
  circle.fill = '#fa3';
  circle.opacity = 0.6;
  circle.stroke = '#a80';
  circle.linewidth = 8;
  //添加一个矩形——定位点在矩形中央
  var rect = two.makeRectangle(600, 200, 200, 200);
  rect.fill = '#2bf';
  rect.stroke = 'transparent';
  /*********
   *矩形旋转 —— (1)不具备累加效果 (2)旋转的轴点都是当前元素的定位点
   *********/
  /*rect.rotation = 5*Math.PI/180;
  rect.rotation = 5*Math.PI/180;
  rect.rotation = 2*Math.PI/180;*/
  /*rect.rotation = 45*Math.PI/180;*/
  //使用定时器动画
  var deg = 0;
  two.on('update', function () { //每次调用two.update()就会触发该事件
    deg += 3;
    rect.rotation = deg * Math.PI / 180;
  })
  two.play(); //每秒钟调用60次two.update()
</script>
</body>
</html>

```
![Kapture 2019-03-19 at 14 49 15](https://user-images.githubusercontent.com/30850497/54585961-8aede480-4a56-11e9-9598-a828db80f135.gif)

***
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    svg,
    canvas {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>第三方绘图工具库——Two.js的使用</h3>
<div id="container"></div>
<script src="../lib/two.js"></script>
<script>
  var params = {width: 800, height: 400, type: Two.Types.svg};
  //var params = {width:800, height:400, type: Two.Types.webgl};
  //var params = {width:800, height:400, type: Two.Types.canvas};
  var two = new Two(params).appendTo(container);
  //添加一个圆形
  //var circle = two.makeCircle(200,200,100);
  var circle = two.makeCircle(-200, 0, 100);
  circle.fill = '#fa3';
  circle.opacity = 0.6;
  circle.stroke = '#a80';
  circle.linewidth = 8;
  //添加一个矩形——定位点在矩形中央
  //var rect = two.makeRectangle(600,200,200,200);
  var rect = two.makeRectangle(200, 0, 200, 200);
  rect.fill = '#2bf';
  rect.stroke = 'transparent';
  /***
   * 使用“分组”将多个图形组合为一个大的图形
   */
  var g = two.makeGroup(circle, rect);
  //小组默认的定位点/旋转轴点在画布的(0,0)
  g.translation.x = 400; //平移小组的定位点
  g.translation.y = 200; //平移小组的定位点
  //小组内的图形的定位点都是相对于小组的轴点的
  //使用定时器动画
  var deg = 0;
  two.on('update', function () { //每次调用two.update()就会触发该事件
    deg += 3;
//rect.rotation = deg*Math.PI/180;
    g.rotation = deg * Math.PI / 180;
  })
  two.play(); //每秒钟调用60次two.update()
</script>
</body>
</html>

```
![Kapture 2019-03-19 at 14 51 09](https://user-images.githubusercontent.com/30850497/54585943-77db1480-4a56-11e9-83ff-ae9dc1c1fa79.gif)

***

