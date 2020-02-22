#### SVG

>stroke*：定义笔触的颜色。例如：stroke="green"

>stroke-dasharray*：定义 dash 和 gap 的长度。它主要是通过使用 , 来分隔 实线 和 间隔 的值。例如：>stroke-dasharray="5, 5" 表示，按照 实线为 5，间隔为 5 的排布重复下去。

>stroke-dasharray 并不局限于只能设置两个值，要知道，它本身的含义是设置最小重复单元，即，dash,gap,dash,gap...。比如，我定义 stroke-dasharray="15, 10, 5" 则相当于，[15,10,5] 为一段

>stroke-dashoffset*: 用来设置 dasharray 定义其实 dash 线条开始的位置。值可以为 number || percentage。百分数是相对于 SVG 的 viewport。通常结合 dasharray 可以实现线条的运动。

>stroke-linecap: 线条的端点样式。

>stroke-linejoin: 线条连接的样式

>stroke-miterlimit: 一个比较复杂的概念，如果我们只是画一些一般的线段，使用上面 linejoin 即可。如果涉及对边角要求比较高的，则可以使用该属性进行定义。它的值，其实就是角长度比上线宽：

#### 使用
```html
<?xml version="1.0" encoding="UTF-8" ?>
<!--XML Name Space：命名空间，指定当前标签用于何种语境-->
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="100">
  <rect width="100" height="50"></rect>
</svg>

```
![svg](https://user-images.githubusercontent.com/30850497/54582613-5bd27580-4a4c-11e9-9f74-33a6f9565315.jpg)


***
##### HTML5之前使用SVG的方法

```html
<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title></title>
</head>
<body>
<h3>HTML5之前使用SVG的方法</h3>
<img src="4.svg">
<hr/>
<iframe src="4.svg"></iframe>
<hr/>
<object data="4.svg"></object>
<hr/>
<embed src="4.svg">
</body>
</html>
```

##### HTML5之后使用SVG的方法
```html
<!DOCTYPE html>
<html>
<head lang="en">
  <meta charset="UTF-8">
  <title></title>
</head>
<body>
<h3>HTML5之后使用SVG的方法</h3>
<svg width="600" height="400">
  <rect width="300" height="200"></rect>
  <rect width="300" height="200" x="300" y="200"></rect>
</svg>
</body>
</html>
```