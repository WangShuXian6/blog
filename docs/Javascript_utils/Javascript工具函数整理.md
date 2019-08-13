#### Javascript 工具函数整理
#### arrayToObject.js
>https://github.com/redux-utilities/redux-actions/blob/master/src/utils/arrayToObject.js

```javascript
export default (array, callback) =>
  array.reduce(
    (partialObject, element) => callback(partialObject, element),
    {}
  );
```
<hr>

#### camelCase.js
>https://github.com/redux-utilities/redux-actions/blob/master/src/utils/camelCase.js

```javascript
import camelCase from 'lodash/camelCase';

const namespacer = '/';

export default type =>
  type.indexOf(namespacer) === -1
    ? camelCase(type)
    : type
        .split(namespacer)
        .map(camelCase)
        .join(namespacer);
```
<hr>

#### compose合并函数依次执行 - 来源redux

```javascript
function compose(...funcs) {
  if (funcs.length === 0) {
    return arg => arg
  }
  if (funcs.length === 1) {
    return funcs[0]
  }
  return funcs.reduce((a, b) => (...args) => a(b(...args)))
}
```
<hr>

#### 数字转为千分位字符
```javascript
/**
 * 数字转为千分位字符
 * @param {Number} num
 * @param {Number} point 保留几位小数，默认2位
 */
function parseToThousandth(num, point = 2) {
  let [sInt, sFloat] = (Number.isInteger(num) ? `${num}` : num.toFixed(point)).split('.')
  sInt = sInt.replace(/\d(?=(\d{3})+$)/g, '$&,')
  return sFloat ? `${sInt}.${sFloat}` : `${sInt}`
}
```
<hr>

#### 带延时功能的链式调用
```javascript
// 1) 调用方式
new People('whr').sleep(3000).eat('apple').sleep(5000).eat('durian')
// 2) 打印结果
(等待3s)--> 'whr eat apple' -(等待5s)--> 'whr eat durian'
// 3) 以下是代码实现
class People {
  constructor(name) {
    this.name = name
    this.queue = Promise.resolve()
  }
  eat(food) {
    this.queue = this.queue.then(() => {
      console.log(`${this.name} eat ${food}`)
    })
    return this
  }
  sleep(time = 0) {
    this.queue = this.queue.then(() => new Promise(res => {
      setTimeout(() => {
        res()
      }, time)
    }))
    return this
  }
}

```
<hr>

#### 一行代码实现简单模版引擎
```javascript
function template(tpl, data) {
  return tpl.replace(/{{(.*?)}}/g, (match, key) => data[key.trim()])
}
// 使用：
template('我是{{name}}，年龄{{age}}，性别{{sex}}', {name: 'aa', age: 18, sex: '男'})
// "我是aa，年龄18，性别男"

```
<hr>

#### 循环对象

```javascript
buildSignString(responseData={page:1,appId:123456}){
      let signArr
      for (let key in responseData){
        // console.log('key--',key) page,appId
        // console.log('value--',responseData[key]) 1,123456
      }
      console.log('signArr---',signArr)
    }
```
<hr>

>
>
```javascript

```
<hr/>

#### 中文url编码
```javascript
newString = encodeURI(string)
```

<hr/>

#### 去掉字符串首位
```
const a='abcd'

const b=a.slice(1,-1)

console.log(b)
// bc
```

<hr/>

#### 将一位数组分割成每三个一组
```javascript
var data = ['法国','澳大利亚','智利','新西兰','西班牙','加拿大','阿根廷','美国','0','国产','波多黎各','英国','比利时','德国','意大利','意大利',];
var result = [];
for(var i=0,len=data.length;i<len;i+=3){
   result.push(data.slice(i,i+3));
}

[['法国','澳大利亚','智利'],['新西兰','西班牙','加拿大'],['阿根廷','美国','0'],['国产','波多黎各','英国'],['比利时','德国','意大利'],['意大利'],]
```

***
#### 数组浅拷贝
```ts
const newImages = state.list.slice(0)
```

***
#### JS获取URL中参数值（QueryString）
```ts
function getQueryString(name) {
    let reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
    let r = window.location.search.substr(1).match(reg); //获取url中"?"符后的字符串并正则匹配
    let context = "";
    if (r != null)
        context = r[2];
    reg = null;
    r = null;
    return context == null || context == "" || context == "undefined" ? "" : context;
}
alert(getQueryString("q"));
```

***
#### 检测是否为 PC 端浏览器
```ts
export const isPC = () => { //是否为PC端
  const userAgentInfo = navigator.userAgent;
  const Agents = ["Android", "iPhone",
    "SymbianOS", "Windows Phone",
    "iPad", "iPod"];
  let flag = true;
  for (let v = 0; v < Agents.length; v++) {
    if (userAgentInfo.indexOf(Agents[v]) > 0) {
      flag = false;
      break;
    }
  }

  return flag;
}
```

#### 判断是否是在safari浏览器中打开

```
var issafariBrowser = /Safari/.test(navigator.userAgent) && !/Chrome/.test(navigator.userAgent);
```
***

#### 横竖屏检测最佳实践
```javascript
export const isHorizontalScreen = () => {
  const clientWidth = document.documentElement.clientWidth;
  const screenWidth = window.screen.width;
  const screenHeight = window.screen.height;
  // 2.在某些机型（如华为P9）下出现 srceen.width/height 值交换，所以进行大小值比较判断
  const realScreenWidth = screenWidth < screenHeight ? screenWidth : screenHeight;
  const realScreenHeight = screenWidth >= screenHeight ? screenWidth : screenHeight;
  if (clientWidth == realScreenWidth) {
    // 竖屏
    return false
  }
  if (clientWidth == realScreenHeight) {
    // 横屏
    return true
  }
}
```

***

#### image to dataurl
```ts
export function getDataUri(url, callback) {
    let image = new Image();

    image.onload = function () {
        let canvas = document.createElement('canvas');
        canvas.width = image.width; // or 'width' if you want a special/scaled size
        canvas.height = image.height; // or 'height' if you want a special/scaled size

        canvas.getContext('2d').drawImage(image, 0, 0);

        // Get raw image data
        //callback(canvas.toDataURL('image/png').replace(/^data:image\/(png|jpg);base64,/, ''));

        // ... or get as Data URI
        callback(canvas.toDataURL('image/png'));
    };

    image.src = url;
}

// Usage
getDataUri('/logo.png', function (dataUri) {
    // Do whatever you'd like with the Data URI!
});
```

***

#### 计时

>console.time()和console.timeEnd()方法

>Chrome等浏览器自带一个console.time()和console.timeEnd()方法，能够用更简单的代码实现上述功能。

>当需要统计一段代码的执行时间时，可以使用console.time方法与console.timeEnd方法，其中console.time方法用于标记开始时间，console.timeEnd方法用于标记结束时间，并且将结束时间与开始时间之间经过的毫秒数在控制台中输出。这两个方法的使用方法如下所示。

>console.time(label)

>console.timeEnd(label)

>这两个方法均使用一个参数，参数值可以为任何字符串，但是这两个方法所使用的参数字符串必须相同，才能正确地统计出开始时间与结束时间之间所经过的毫秒数。

```ts
console.time(label)
console.timeEnd(label)
```

***

>
```ts
let start = window.performance.now();
...
let end = window.performance.now();
let time = end - start;
```

***
#### 去掉当前页面的 hash 而不刷新页面
```ts
window.history.pushState(
    {},
    '',
    window.location.href.slice(
        0,
        window.location.href.indexOf('#')
            ? window.location.href.indexOf('#')
            : 0))

```
***
#### 获取前N天的日期

>方法一： 用setDate();
```ts
function getDate(index){
	let date = new Date(); //当前日期
	let newDate = new Date();
	newDate.setDate(date.getDate() + index);//官方文档上虽然说setDate参数是1-31,其实是可以设置负数的
	let time = newDate.getFullYear()+"-"+(newDate.getMonth()+1)+"-"+newDate.getDate();
	return time;
}
console.log(getDate(7)); // 2019-7-10
console.log(getDate(-7)); // 2019-6-26
```

>方法二： 时间戳进行转换
```ts
let date= new Date();
let newDate = new Date(date.getTime() - 7*24*60*60*1000);
let time = newDate.getFullYear()+"-"+(newDate.getMonth()+1)+"-"+newDate.getDate();
conosole.log(time);
```

***
#### 获取近n天的日期列表
```ts
function getDateList(index) {
  let list = []
  if (index >= 0) {
    for (let i = 0; i < index; i++) {
      list.push(getDate(i))
    }
  } else {
    for (let i = 0; i > index; i--) {
      list.push(getDate(i))
    }
    list.reverse()
  }
  return list
}

console.log(getDateList(-2)) // ["2019-7-2", "2019-7-3"]
```
#### 复制内容到剪贴板
```ts
function copy(content) {
    const input = document.createElement('input');
    input.setAttribute('readonly', 'readonly');
    input.setAttribute('value', content);
    document.body.appendChild(input);
    input.setSelectionRange(0, 9999);
    input.select()
    if (document.execCommand('copy')) {
      const result = document.execCommand('copy');
      if (result) {
        console.log('复制成功');
      } else {
        console.warn('复制失败')
      }

    }
    document.body.removeChild(input);
  }
```