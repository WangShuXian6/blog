#### 使用SVG绘图——矩形

```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }

    #r4 {
      /*background: #00f;*/
      /*border: 5px solid #00f;*/
      fill: #00f;
      fill-opacity: .3;
      stroke: #00f;
      stroke-width: 5px;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——矩形</h3>
<svg width="600" height="400">
  <rect width="100" height="80"></rect>
  <rect width="100" height="80" x="500" y="0"></rect>
  <rect width="100" height="80" x="0" y="320" fill="#f00" fill-opacity=".3" stroke="#f00" stroke-width="5"></rect>
  <rect width="100" height="80" x="500" y="320" id="r4"></rect>
  <rect width="100" height="80" id="r5"></rect>
</svg>
<script>
  console.log(r5);
  //r5.x = 300; //只有HTMLDOM标签才有直接可用的属性，SVG标签的属性没有纳入HTMLDOM规范
  //r5.y = 200;
  //r5.fill = '#0f0';
  r5.setAttribute('x', 300);
  r5.setAttribute('y', 200);
  r5.setAttribute('fill', '#0f0');
</script>
</body>
</html>

```
![svg2-1](https://user-images.githubusercontent.com/30850497/54582668-90dec800-4a4c-11e9-8360-a6806f9b572a.jpg)

***
##### svg动画
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——矩形</h3>
<svg width="600" height="400">
  <rect id="r1" width="100" height="80" x="0" fill="rgb(0,0,0)"></rect>
</svg>
<script>
  var r = 0; //红色
  var timer = setInterval(function () {
    var x = r1.getAttribute('x');
    x = parseInt(x);
    x += 10;
    r1.setAttribute('x', x);
    r += 15;
    r1.setAttribute('fill', `rgb(${r},0,0`);
  }, 100)
</script>
</body>
</html>

```
![Kapture 2019-03-19 at 13 59 18](https://user-images.githubusercontent.com/30850497/54583501-5165ab00-4a4f-11e9-9bda-fff38fc4b2dc.gif)

***

```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——矩形</h3>
<svg width="600" height="400" id="svg9">
</svg>
<script>
  var data = [
    {"label": "1月", "value": 250},
    {"label": "2月", "value": 200},
    {"label": "3月", "value": 350},
    {"label": "4月", "value": 280}
  ];
  var html = '';
  for (var i = 0; i < data.length; i++) {
    var d = data[i];
    html += `
    <rect width="50" height="${d.value}" x="${i * 80}" y="10"></rect>
`;
  }
  svg9.innerHTML = html;
</script>
</body>
</html>

```
![svg2-2-4](https://user-images.githubusercontent.com/30850497/54583628-b9b48c80-4a4f-11e9-84d0-edf1d613e3d1.jpg)
***
##### 动态创建的SVG元素，必须指定所在的XMLNS
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——矩形</h3>
<svg width="600" height="400" id="svg10">
</svg>
<script>
  var data = [
    {"label": "1月", "value": 250},
    {"label": "2月", "value": 200},
    {"label": "3月", "value": 350},
    {"label": "4月", "value": 280}
  ];
  for (var i = 0; i < data.length; i++) {
    var d = data[i];
//动态创建的SVG元素，必须指定所在的XMLNS
//var rect = document.createElement('rect');
    var rect = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
    rect.setAttribute('width', 50);
    rect.setAttribute('height', d.value);
    rect.setAttribute('x', i * 80);
    rect.setAttribute('y', 10);
    svg10.appendChild(rect);
  }
</script>
</body>
</html>


```
![svg2-2-4](https://user-images.githubusercontent.com/30850497/54583698-f54f5680-4a4f-11e9-8d2d-d930b9434b23.jpg)


***

#### 使用SVG绘图——圆形
##### hover动画
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——圆形</h3>
<svg width="600" height="400" id="svg10">
  <circle id="c1" r="100" cx="100" cy="100"></circle>
</svg>
<script>
  c1.onmouseenter = function () {
    this.setAttribute('fill-opacity', '.3');
  }
  c1.onmouseleave = function () {
    this.setAttribute('fill-opacity', '1');
  }
</script>
</body>
</html>

```
![Kapture 2019-03-19 at 14 07 44](https://user-images.githubusercontent.com/30850497/54583832-6c84ea80-4a50-11e9-8cfe-8018d3e6acd4.gif)


***

##### 点击动画
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——圆形</h3>
<svg width="600" height="400" id="svg12">
</svg>
<script>
  //动态添加30个圆形
  for (var i = 0; i < 30; i++) {
    var c = document.createElementNS('http://www.w3.org/2000/svg', 'circle');
    c.setAttribute('r', rn(5, 100));
    c.setAttribute('cx', rn(0, 600));
    c.setAttribute('cy', rn(0, 400));
    c.setAttribute('fill', rc(0, 256));
    c.setAttribute('fill-opacity', Math.random());
    svg12.appendChild(c);
  }
  //使用事件代理方式为每个圆形绑定单击监听函数
  svg12.onclick = function (e) {
    var target = e.target; //事件源对象
//if(target.nodeName==='CIRCLE'){
    if (target.nodeName === 'circle') {
      var timer = setInterval(function () {
//让半径变大
        var r = target.getAttribute('r');
        r = parseFloat(r);
        r *= 1.1;
        target.setAttribute('r', r);
//让透明度变小
        var p = target.getAttribute('fill-opacity');
        p = parseFloat(p);
        p *= 0.8;
        target.setAttribute('fill-opacity', p);
        if (p < 0.001) { //几乎看不见时，删除元素
          clearInterval(timer);
          svg12.removeChild(target);
        }
      }, 50);
    }
  }


  /**random number: 生成指定范围内的随机整数**/
  function rn(min, max) {
    return Math.floor(Math.random() * (max - min) + min);
  }

  /**random color: 生成指定范围内的随机颜色**/
  function rc(min, max) {
    var r = rn(min, max);
    var g = rn(min, max);
    var b = rn(min, max);
    return `rgb(${r},${g},${b})`;
  }
</script>
</body>
</html>

```
![Kapture 2019-03-19 at 14 10 03](https://user-images.githubusercontent.com/30850497/54583930-b7066700-4a50-11e9-9f85-0d72da143ad4.gif)


***

#### 使用SVG绘图——椭圆
##### hover 动画
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——椭圆</h3>
<svg width="600" height="400" id="svg12">
  <ellipse id="e1" rx="200" ry="50" cx="200" cy="50" fill="#0f0" fill-opacity=".3"></ellipse>
</svg>
<script>
  e1.onmouseenter = function () {
    this.setAttribute('fill-opacity', 1);
  }
  e1.onmouseleave = function () {
    this.setAttribute('fill-opacity', 0.3);
  }
</script>
</body>
</html>

```
![Kapture 2019-03-19 at 14 13 38](https://user-images.githubusercontent.com/30850497/54584069-372ccc80-4a51-11e9-9c45-1bb3994a2947.gif)

***

#### 使用SVG绘图——直线

```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }
    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——直线</h3>
<svg width="600" height="400" id="svg12">
  <line x1="0" y1="0" x2="600" y2="400" stroke="#f00" stroke-width="10"></line>
</svg>
<script>
</script>
</body>
</html>

```
![svg2-6-1](https://user-images.githubusercontent.com/30850497/54584137-70fdd300-4a51-11e9-9166-836019027427.jpg)

***

```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——直线</h3>
<svg width="600" height="400" id="svg15">
  <!--group: 小组-->
  <g stroke="#800" stroke-width="60">
    <line x1="100" y1="100" x2="200" y2="100"></line>
    <line x1="250" y1="100" x2="500" y2="100"></line>
    <line x1="100" y1="200" x2="200" y2="200"></line>
    <line x1="250" y1="200" x2="500" y2="200"></line>
    <line x1="100" y1="300" x2="200" y2="300"></line>
    <line x1="250" y1="300" x2="500" y2="300"></line>
  </g>
</svg>
<script>
</script>
</body>
</html>

```
![svg2-6-2](https://user-images.githubusercontent.com/30850497/54584165-94288280-4a51-11e9-9d38-6de0d4d0d481.jpg)

***

#### 使用SVG绘图——多边形

```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——多边形</h3>
<svg width="600" height="400" id="svg15">
  <polygon points="0,0 600,400 600,0 0,400"></polygon>
</svg>
<script>
</script>
</body>
</html>

```

![svg2-7-1](https://user-images.githubusercontent.com/30850497/54584422-48c2a400-4a52-11e9-861d-f1d0779451ed.jpg)

***


```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——多边形</h3>
<svg width="600" height="400" id="svg15">
  <g fill="#008">
    <polygon points="100,100 300,200 500,100"></polygon>
    <polygon points="100,150 100,300 500,300 500,150 300,250"></polygon>
  </g>
</svg>
<script>
</script>
</body>
</html>

```
![svg2-7-2](https://user-images.githubusercontent.com/30850497/54584468-64c64580-4a52-11e9-91c6-bd623107b0d0.jpg)


***

#### 使用SVG绘图——文本
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——文本</h3>
<svg width="600" height="400" id="svg15">
  <!--<span>一段文本</span>-->
  <!--<p>一个段落</p>-->
  <text alignment-baseline="before-edge" font-size="40" x="100" y="100">达内科技 ® 2016</text>
</svg>
<script>
</script>
</body>
</html>

```
![svg2-8](https://user-images.githubusercontent.com/30850497/54584554-a525c380-4a52-11e9-8d1d-74b38feb8012.jpg)

***
#### 使用SVG绘图——图像
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——图像</h3>
<svg width="600" height="400" id="svg15">
  <!--<img src="img/disc.png">-->
  <image xlink:href="img/disc.png" width="400" height="200"></image>
</svg>
<script>
</script>
</body>
</html>

```
![svg2-9](https://user-images.githubusercontent.com/30850497/54584701-09e11e00-4a53-11e9-9d3e-0ef9deec3845.jpg)

***
#### 使用SVG绘图——渐变对象
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>使用SVG绘图——渐变对象</h3>
<svg width="600" height="400" id="svg15">
  <!--渐变属于特效，必须声明在“特效列表”-->
  <defs>
    <linearGradient id="g1" x1="0" y1="0" x2="100%" y2="0">
      <stop offset="0" stop-color="#f00"></stop>
      <stop offset="50%" stop-color="#ff0"></stop>
      <stop offset="100%" stop-color="#0f0"></stop>
    </linearGradient>
    <linearGradient id="g2" x1="0" y1="0" x2="100%" y2="0">
      <stop offset="0" stop-color="red"></stop>
      <stop offset="17%" stop-color="orange"></stop>
      <stop offset="33%" stop-color="yellow"></stop>
      <stop offset="50%" stop-color="green"></stop>
      <stop offset="67%" stop-color="blue"></stop>
      <stop offset="83%" stop-color="cyan"></stop>
      <stop offset="100%" stop-color="purple"></stop>
    </linearGradient>
  </defs>
  <rect width="500" height="100" x="50" y="50" fill="url(#g1)"></rect>
  <text y="300" font-size="50" fill="url(#g2)">渐变绘图svg</text>
</svg>
<script>
</script>
</body>
</html>

```
![svg2-10](https://user-images.githubusercontent.com/30850497/54584795-43198e00-4a53-11e9-9b39-f931f2377387.jpg)

***

#### SVG绘图——各阶段课程难度系数
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>SVG绘图——各阶段课程难度系数</h3>
<svg id="s1">
  <!--所有的特效对象-->
  <defs id="effectList">
    <!--<linearGradient id="g0" x1="0" y1="0" x2="0" y2="100%">
    <stop offset="0" stop-color="#f00"></stop>
    <stop offset="100%" stop-color="#f00" stop-opacity="0"></stop>
    </linearGradient>-->
  </defs>
  <!--坐标轴小组-->
  <g stroke="#555" stroke-width="2">
    <!--X轴-->
    <line x1="50" y1="450" x2="650" y2="450"></line>
    <line x1="630" y1="440" x2="650" y2="450"></line>
    <line x1="630" y1="460" x2="650" y2="450"></line>
    <!--Y轴-->
    <line x1="50" y1="450" x2="50" y2="50"></line>
    <line x1="40" y1="70" x2="50" y2="50"></line>
    <line x1="60" y1="70" x2="50" y2="50"></line>
  </g>
</svg>
<script>
  var w = 600 + 100;
  var h = 10 * 40 + 100;//难度值*40高度倍率+双倍PADDING
  s1.setAttribute('width', w);
  s1.setAttribute('height', h);
  var data = [
    {label: 'HTML', value: 3},
    {label: 'CSS', value: 5},
    {label: 'JS', value: 7},
    {label: 'DOM', value: 6},
    {label: 'jQuery', value: 5.5},
    {label: 'AJAX', value: 8},
    {label: 'HTML5', value: 6}
  ];
  var colWidth = 600 / (2 * data.length + 1);
  for (var i = 0; i < data.length; i++) {
    var d = data[i]; //遍历每个数据对象
    var cw = colWidth; //柱子的宽
    var ch = d.value * 40; //柱子的高
    var x = (2 * i + 1) * colWidth + 50;
    var y = 500 - 50 - ch;
//动态添加渐变对象
    var c = rc(); //渐变颜色
    var html = `
<linearGradient id="g${i}" x1="0" y1="0" x2="0" y2="100%">
<stop offset="0" stop-color="${c}"></stop>
<stop offset="100%" stop-color="${c}" stop-opacity="0"></stop>
</linearGradient>
`;
    effectList.innerHTML += html;
//动态创建矩形
    var rect = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
    rect.setAttribute('width', cw);
    rect.setAttribute('height', ch);
    rect.setAttribute('x', x);
    rect.setAttribute('y', y);
    rect.setAttribute('fill', `url(#g${i})`);
    s1.appendChild(rect);
  }

  //random color
  function rc() {
    var r = Math.floor(Math.random() * 256);
    var g = Math.floor(Math.random() * 256);
    var b = Math.floor(Math.random() * 256);
    return `rgb(${r}, ${g}, ${b})`;
  }
</script>
</body>
</html>

```

![svg2-11](https://user-images.githubusercontent.com/30850497/54585201-73156100-4a54-11e9-9fc3-0efc9cb861be.jpg)

***

#### SVG绘图——滤镜——高斯模糊滤镜
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    body {
      text-align: center;
    }

    svg {
      background: #f0f0f0;
    }
  </style>
</head>
<body>
<h3>SVG绘图——滤镜——高斯模糊滤镜</h3>
<svg id="s2" width="600" height="400">
  <!--定义特效对象-->
  <defs>
    <!--linearGraident-->
    <!--filter-->
    <filter id="f3">
      <feGaussianBlur stdDeviation="3"></feGaussianBlur>
    </filter>
    <filter id="f6">
      <feGaussianBlur stdDeviation="6"></feGaussianBlur>
    </filter>
  </defs>
  <text x="50" y="100" font-size="70">高斯模糊滤镜</text>
  <text x="50" y="200" font-size="70" filter="url(#f3)">高斯模糊滤镜</text>
  <text x="50" y="300" font-size="70" filter="url(#f6)">高斯模糊滤镜</text>
</svg>
<script>
</script>
</body>
</html>

```
![svg2-12](https://user-images.githubusercontent.com/30850497/54585354-cd162680-4a54-11e9-9c70-7040a782b6fc.jpg)

***