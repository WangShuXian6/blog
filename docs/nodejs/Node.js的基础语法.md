#### Node.js的基础语法
>type.js

```javascript
//原始类型
var price=9.9;
var name="可口可乐";
var isOnsale=false;
var exDate=null;
var area=undefined;
```

```javascript
//引用类型
var empName=new String('Tom');
var age=new Number(20);
var age=new Buffer(10);
var myObj=new Object();
function Myobj(name,age){
this.name=name;
this.age=age;
}
var han=new Myobj('hanxl',20);
console.log(han.name);
console.log(han.age);
```

>template.js

```javascript
//模板字符串
var info=`
用户名：唐牧
密码：123
`;
console.log(info);
var price=9.9,count=3;
var info2=`
单价：${price},
数量：${count},
小计：${price*count},
超过预算：${price*count>20?'是':'否'}
`;
console.log(info2);
```
>strict.js

```javascript
//"use strict";
const PI=3.14;
//PI=2;
console.log(PI);




function f1(){
//"use strict"
var name="tecu.cn";
loc="北京市";
console.log(name);
console.log(loc);
}
f1();

(function(){
'use strict';
console.log(this);
})();
```
>let.js

```javascript
"use strict";
/*var count=20;
if(count>10){
//var cur=count;
let cur=count;
}
console.log(cur);*/

for(let i=0;i<5;i++){}
console.log(i);
```
>for-of.js

```javascript
//for...in....遍历对象的成员名
var obj={
name:"tom",
age:20
};
for(var i in obj){
console.log(i+':'+obj[i]);
}
//for...in...遍历数组的下标
var arr=[10,20,30];
for(var i in arr){
console.log(i);
}
//for...of...遍历数组的元素值
for(var i of arr){
console.log(i);
}
```
>global.js

```javascript
var age=20;
console.log(age);
//console.log(global.age);


//process.kill(pid)向指定进程id发送退出信号
process.kill(3456);
```
>window.html

```html
<!DOCTYPE html>
<html>
<head lang="en">
<meta charset="UTF-8">
<title></title>
</head>
<body>
<script>
var age=20;
console.log(age);
console.log(window.age);
</script>
</body>
</html>
```

>箭头函数 =>

```javascript
/*
(function(i){
console.log(i)
})(111);
+function(){
console.log(222);
}();
(function(num){
if(num>0){
arguments.callee(num-1);
console.log(num);
}
})(5);
*/
//箭头函数 =>
(() => {
    console.log(111)
})();


((n1, n2) => {
    console.log(n1 + n2);
})(10, 20)


var f1 = (n1, n2) => {
    console.log(n1 + n2);
};

f1(1, 2)


    /*((num)=>{
    if(num>0){
    arguments.callee(num-1);
    console.log(num);
    }
    })(5); 错误！*/
```

>闭包

>closure.js

```javascript
//一个简单的一次性定时器
var i = 1;
setTimeout(() => {
    console.log(i)
}, 1000)
//一秒之后，输出0,1,2

for (var i = 0; i < 3; i++) {
    setTimeout(outer(i), 1000)
}

function outer(num) {
    return function inner() {
        console.log(num)
    }
}

//改写为箭头函数
for (var i = 0; i < 3; i++) {
    setTimeout((num => {
        return () => { console.log(num) }
    })(i), 1000)
}

```

>创建自定义对象class

>
```javascript
"use strict";

//创建自定义对象
class Emp {
    constructor(ename, salary) {//声明一个构造函数
        this.ename = ename
        this.salary = salary
    }
    work() {
        return `${this.ename} is working.`
    }
    sal() {
        return `${this.ename}'s salary is ${this.salary}.`
    }
}

var e1 = new Emp("tom", 8000)
console.log(e1.ename)
console.log(e1.work())
console.log(e1.sal())

var e2 = new Emp("Mary", 9000)
console.log(e2.ename)


//实现继承
class Programmer extends Emp {
    constructor(ename, salary, skills) {
        super(ename, salary)
        this.skills = skills
    }
    work() {
        return super.work() + 'skills are' + this.skills
    }
}

var p1 = new Programmer("Jack", 8800, "PHP&MYSQL")
console.log(p1.ename)
console.log(p1.work())

```
***

#### console
```javascript
//console.log
console.log("msg1")
console.log("msg1", "msg2", 123, true, new Date())

var user = { name: "tom", age: 20 }
console.log(`姓名：${user.name},年龄：${user.age}`)
//百分号占位符，每一个占位符一一对应后面的参数值，%s表示string类型，%d表示number类型
console.log('姓名：%s，年龄：%d', user.name, user.age)
//console.error
console.error(`姓名：${user.name},年龄：${user.age}`)

//console.trace
function outer() {
    function inner() {
        console.trace("stack trace")
    }
    inner()
}
outer()

//console.dir
console.dir(user)
var arr = [20, 30, "tom"]
console.dir(arr)
//console.log和console.dir的区别，dir()可以带一些参数
console.log(Buffer)
console.dir(Buffer, { colors: true })

//console.assert
function add(n1, n2) {
    return n1 + n2
}
var sum = 20;
console.assert(add(10, 20) == sum, "add函数执行结果不符合要求")


//console.time() console.timeEnd()
console.time("mycode") //开始计时
var sum = 0
for (var i = 0; i < 1000000; i++) {
    sum += i
}
console.timeEnd("mycode") //结束计时

```
<hr>

#### 定时器
```javascript
//一个简单的一次性定时器
var counter = 0
var timer = setTimeout(function () {
    counter++
    console.log('%d hello', counter);
    clearTimeout(timer)
}, 1000)
//周期性定时器，输出1,2,3,4,5
var counter2 = 0
var timer2 = setInterval(function () {
    counter2++
    console.log(counter2)
    if (counter2 >= 5) {
        clearInterval(timer2)
    }
}, 1000)
//练习：使用一次性定时器模拟上面周期性定时器的效果，输出1,2,3,4,5
var counter3 = 0
var timer3 = setTimeout(function () {
    counter3++
    if (counter3 <= 5) {
        console.log(counter3)
        setTimeout(arguments.callee, 1000)
    } else {
        clearTimeout(timer3)
    }
}, 1000)
//setImmediate() process.nextTick
setImmediate(function () {
    console.log("setImmediate1...")
})
process.nextTick(function () {
    console.log("nextTick1...")
})
setTimeout(function () {
    console.log("setTimeout...")
}, 100)
setImmediate(function () {
    console.log("setImmediate2...")
})
process.nextTick(function () {
    console.log("nextTick2...")
})
console.log("end...")
```
<hr>

#### 模块module
```javascript
//获取主模块对象
console.log(process.mainModule)
console.log(require.main)
//获取当前模块自己
console.log(module)
//判断当前模块是否主模块
console.log(require.main === module)


//引入其他模块
var fs = require("fs");//引入文件系统模块
fs.readFile('./10-window.html', (err, data) => {
    if (err) {
        console.log("文件读取失败")
        console.log(err)
    } else {
        console.log("文件读取成功")
        console.log(data)
    }
});
```
>02.js
```javascript
var c=require('./02-circle.js');
console.log(c.s(5));
console.log(c.p(5).toFixed(2));
```
>02-circle.js
```javascript
//求圆的面积和周长
const PI = 3.14
var size = function (r) {
    return PI * r * r
}
var perimeter = function (r) {
    return 2 * PI * r
};
exports.s = size
exports.p = perimeter
```
>03.js
```javascript
var c=require("./03-circle.js");
var circle=new c(5);
console.log(circle.size());
console.log(circle.perimeter());
```
>03-circle.js
```javascript
//求圆的面积和周长
const PI = 3.14
function Circle(r) {
    this.size = function () {
        return PI * r * r
    };
    this.perimeter = function () {
        return 2 * PI * r
    };
}
module.exports = Circle
```
>04.js
```javascript
var m1 = require('./04-m1')
console.log(m1.data)
var m2 = require('./04-m2.js')
console.log(m2.data)
var m3 = require('./04-m3.json')
console.log(m3.data)
//引入目录模块
var m4 = require("./04-m4")
console.log(m4.data)
var m5 = require("./04-m5")
console.log(m5.data)
var m6 = require("04-m6")
console.log(m6.data)
```
>04-m1.js
```javascript
exports.data="m1";
```
>04-m2.js
```javascript
exports.data="m2";
```
>04-m3.json
```JSON
{
    "data": "m3"
}
```
>hello.js
```javascript
exports.data="m4";
```
>package.json
```JSON
{
    "main": "hello.js"
}
```
>index.js
```javascript
exports.data="m5";
```
>index.json
```JSON
{
    "data": "m6"
}
```
<hr>






>\node_modules\mypackage\index.js
```javascript
var p=require("./lib/const.js");
exports.size=function(r){
return p.PI*r*r;
};
```
>\node_modules\mypackage\lib\const.js
```javascript
exports.PI=3.14;
```
>\node_modules\mypackage\package.json
```JSON
{
    "name": "mypackage",
    "main": "index.js",
    "description": "This is mypackage.",
    "version": "1.0.0",
    "keywords": [
        "circle",
        "hanxl"
    ],
    "maintainers": [
        {
            "name": "hanxl",
            "email": "hanxl@tedu.cn"
        }
    ]
}
```
<hr>

#### NPM
>\index.js
```javascript
var p=require("./lib/const.js");
exports.size=function(r){
return p.PI*r*r;
};
```
>\node_modules\unit_0119\lib\const.js
```javascript
exports.PI=3.14;
```
>\node_modules\unit_0119\package.json
```JSON
{
    "name": "unit_0119",
    "version": "2.0.0",
    "description": "unit_0119",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [
        "circle"
    ],
    "author": "",
    "license": "ISC"
}
```

<hr>

#### Console
```javascript
//向任意输出流中执行输入
var fs = require("fs")// 引入fs模块
var out = fs.createWriteStream("./log/out.log")// 创建文件
var err = fs.createWriteStream("./log/err.log")
var c = require("console") // 引入console模块
var loger = new c.Console(out, err) // 实例化Console构造函数
loger.log("log...")
loger.error("err...")
```

<hr>

#### querystring
```javascript
var qs = require("querystring")
var str = "name=tom&age=20"
console.log(qs.parse(str))
var obj = { name: 'tom', age: '20' }
console.log(qs.stringify(obj))
console.log(qs.stringify(obj, ","))
console.log(qs.stringify(obj, ",", "-"))
```

<hr>

#### url
```javascript
var url = require("url")
var str = "http://tom:123@tedu.cn:8080/news/index.html?pid=20#section"
console.log(url.parse(str)) // 解析出url的各个组成部分
console.log(url.parse(str, true)) // 第二个参数，把查询字符串部分解析为对象

var obj = {
    protocol: "http:",
    host: "tedu.cn",
    pathname: "course_list/index.html",
    query: { cid: 30 }
};
console.log(url.format(obj))

var url1 = "project/static/base.html"
var url2 = "../img/1.jpg"
console.log(url.resolve(url1, url2))
```
<hr>

#### path
```javascript
var path = require("path")
var str = "E:/hanxl/nodejs/day02/01-module.js"
console.log(path.parse(str))

var obj = {
    dir: "e:/hanxl/day01",
    base: "01.js"
};
console.log(path.format(obj))
console.log(path.join('d:', 'hanxl', 'nodejs', '01.js'))

var path1 = "e:/htdocs/01/login"
console.log(path.resolve(path1, '../img/2.jpg'))
console.log(path.relative(path1, 'e:/htdocs/img/3.jpg'))




// { root: 'E:/',
//   dir: 'E:/hanxl/nodejs/day02',
//   base: '01-module.js',
//   ext: '.js',
//   name: '01-module' }

// e:/hanxl/day01\01.js
// d:\hanxl\nodejs\01.js
// e:\htdocs\01\img\2.jpg
// ..\..\img\3.jpg
```
<hr>

#### DNS
```javascript
var dns = require("dns")
//把一个域名解析为一个IP地址，参数1，错误对象；参数2，IP地址；参数3，IPV4 OR IPV6
dns.lookup('www.baidu.com', function (err, address, family) {
    if (err) {
        cosole.log(err)
    } else {
        console.log("lookup():")
        console.log(address)
        console.log(family)
    }
})
//把一个域名解析为一个DNS记录解析数组
dns.resolve('www.baidu.com', function (err, address) {
    if (err) {
        cosole.log(err)
    } else {
        console.log("resolve():")
        console.log(address)
    }
})
//把一个IP地址反向解析为一组域名
dns.reverse('60.221.254.230', function (err, hostnames) {
    if (err) {
        console.log(err)
    } else {
        console.log(hostnames)
    }
})
```
<hr>

#### util
```javascript
var util = require("util")
var obj = { name: "Cola", price: 3.9, isOnsale: false }
var s1 = util.format('产品名称：%s，价格：%d，是否促销商品：%s，%j', obj.name, obj.price, obj.isOnsale, obj)
console.log(s1)
console.log(util.inspect(obj))

//实现构造方法之间的继承
function Base() {
    this.name = "base"
}
Base.prototype.age = 20
function Sub() {
    this.name = "sub"
}
util.inherits(Sub, Base)
var user = new Sub()
console.log(user.name)
console.log(user.age)
```
<hr>

#### Buffer
```javascript
// 创建一个长度为32字节的缓冲区
var buf1 = new Buffer(32)
console.log(buf1)

// 创建一个长度为3字节的缓冲区
var buf2 = new Buffer([65, 66, 67]) // 存入十进制
console.log(buf2)
console.log(buf2.toString()) // 将buffer转换为字符串表示

// 创建一个长度为4字节的缓冲区，直接存入英文字符串
var buf3 = new Buffer("ABCD")
console.log(buf3)
console.log(buf3.length)

// 创建一个缓冲器，存入字符串（英文+中文）,指定编码方式
var buf4 = new Buffer("AB一二", "utf8")
console.log(buf4) // 一个汉字占3个字节
console.log(buf4.length)
```
<hr>

#### Fs 
```javascript
var fs = require("fs")
var path1 = "./log"
var path2 = "./log/out.log"
var stats1 = fs.statSync(path1)//同步读取目录
//console.log(stats1)
var stats2 = fs.statSync(path2)//同步读取文件
//console.log(stats2)
//异步读取文件或目录
fs.stat(path1, (err, stats) => {
    if (err) {
        console.log("文件或目录读取失败！")
    } else {
        console.log("文件读取成功！")
    }
})
console.log(stats2.isFile())//判断是否为文件
console.log(stats2.isDirectory())//判断是否为目录
```
```javascript
var fs = require("fs")
var path = './test'
//读取一个目录，如果此目录不存在，则创建，如果存在，则删除
fs.stat(path, (err, stats) => {
    if (err) {
        fs.mkdir(path)//创建指定目录
    } else {
        fs.rmdir(path)//删除指定目录
    }
})

//判断一个目录是否存在，若存在，读取目录下的内容
var path2 = './node_modules'
fs.stat(path2, (err, stats) => {
    if (err) {
        console.log("当前目录不存在")
    } else {
        fs.readdir(path2, (err, list) => {//读取目录下的内容
            console.log(list)
        })
    }
})
```
```javascript
var fs = require("fs")//引入fs模块
//读取文件
fs.readFile('../day02/02.js', (err, data) => {
    if (err) {
        console.log("文件读取失败")
    } else {
        console.log("文件读取成功")
        console.log(data.toString())
    }
})
//写入内容
var file1 = './log/out.log'
var data1 = "out.log test file"
fs.writeFile(file1, data1, (err) => {
    if (err) {
        console.log("文件写入失败")
        console.log(err)
    }
})
//向文件中追加内容
var file2 = './log/out2.log'
var data2 = "This is test2"
fs.appendFile(file2, data2, (err) => {
    if (err) {
        console.log("文件写入失败")
        console.log(err)
    }
})
//删除文件
fs.unlink('./log/test.txt', (err) => {
    if (err) {
        console.log("删除文件失败")
    }
})
//重命名文件
var pathOld = "./log/index.html"
var pathNew = "./log/login.html"
fs.rename(pathOld, pathNew, (err) => {
    if (err) {
        console.log("重命名失败")
    }
})
```
<hr>
***
#### 使用get()方法发送请求
```javascript
//使用get()方法发送请求
var http = require("http")
var options = {
    hostname: "www.tmooc.cn",
    port: "80",
    path: "/web/index_new.html"
}
var req = http.get(options, (res) => {
    console.log("接收到响应消息...")
    //console.log(res)
    console.log(`响应状态码：${res.statusCode}`)
    console.log(`响应头：${JSON.stringify(res.headers)}`)
    res.on("data", (chunk) => {
        console.log(chunk.toString())
    })
})
req.setTimeout(2000, () => {
    req.abort()
    console.log("请求超时，已终止请求。")
})
req.on("error", (err) => {
    console.log(`请求发生错误：${err}`)
})
```
<hr>

#### 使用request()方法发送请求
```javascript
//使用request()方法发送请求
var http = require("http")
var options = {
    hostname: "www.tmooc.cn",
    port: "80",
    path: "/web/index_new.html"
}
var req = http.request(options, (res) => {
    console.log("接收到响应消息...")
    //console.log(res)
    console.log(`响应状态码：${res.statusCode}`)
    console.log(`响应头：${JSON.stringify(res.headers)}`)
    res.on("data", (chunk) => {
        console.log(chunk.toString())
    })
})
req.setTimeout(2000, () => {
    req.abort()
    console.log("请求超时，已终止请求。")
})
req.on("error", (err) => {
    console.log(`请求发生错误：${err}`)
})
req.end()
```
<hr>

#### 创建一个简单的web服务器
```javascript
//创建一个简单的web服务器——没有给客户端响应
//引入http模块
var http = require("http")
//使用http模块，创建web服务器
var server = http.createServer()
//为服务器的request事件绑定处理函数
server.on("request", (req, res) => {
    console.log("web服务器接收到一个请求...")
    console.log(`请求方法：${req.method}`)
    console.log(`请求URL：${req.url}`)
    console.log(`请求版本：${req.httpVersion}`)
    console.log(`请求头：`)
    console.log(req.headers)
})
//启动web服务器，监听指定端口
server.listen(8000, '127.0.0.1', (err) => {
    if (err) {
        console.log("web服务器启动失败，错误消息为：")
        console.log(err)
    } else {
        console.log("web服务器启动成功，正在监听8000端口...")
    }
})
```
<hr>

#### 创建web服务器——给客户端一个固定响应
```javascript
/*创建web服务器——给客户端一个固定响应*/
//引入http模块
var http = require("http")
//创建web服务器
var server = http.createServer()
//为http.Server的request事件绑定处理函数
server.on("request", (req, res) => {
    console.log("接收到一个客户端请求...")
    res.statusCode = 200//设置响应状态码
    res.setHeader("Content-Type", "text/html;charset=UTF-8")//设置响应消息头部
    res.write("<html>")//向客户端输出的响应主体内容
    res.write("<head></head>")
    res.write("<body>")
    res.write("<h1>欢迎访问我的页面</h1>")
    res.write("</body>")
    res.write("</html>")
    res.end()//响应主体内容完毕，向客户端发送响应
})
//启动web服务器，监听指定端口
server.listen(8000, "127.0.0.1", (err) => {
    if (err) {
        console.log("服务器启动失败")
    } else {
        console.log("服务器启动成功，正在监听8000端口")
    }
})
```
<hr>

#### 创建一个web服务器——根据客户端请求响应对应的内容
```javascript
/*创建一个web服务器——根据客户端请求响应对应的内容*/
//引入模块
var http = require("http")
var url = require("url")
var fs = require("fs")
//创建web服务器
var server = http.createServer()
//为server的request事件绑定处理函数
server.on("request", (req, res) => {
    //1.解析出客户端请求的url，req.url
    var urlInfo = url.parse(req.url)
    var path = '.' + urlInfo.pathname
    //2.读取客户端请求的文件内容，fs.readFile()
    fs.readFile(path, (err, data) => {
        if (err) {
            console.log("文件读取失败")
        } else {
            //3.向客户端输出响应内容，res.write()
            res.statusCode = 200
            res.setHeader('Content-Type', 'text/html;charset=UTF-8')
            res.write(data)
            res.end()
        }
    })
})
//启动web服务器，监听指定端口
server.listen(8000, "127.0.0.1", (err) => {
    if (err) {
        console.log("服务器启动失败")
    } else {
        console.log("服务器启动成功，正在监听8000端口...")
    }
})
```
<hr>

#### 查询mysql数据库中的数据
```javascript
/*查询mysql数据库中的数据*/
//引入mysql模块
var mysql = require("mysql")
//连接到mysql服务器
var options = {
    host: "127.0.0.1",
    user: "root",
    password: "",
    database: "user_0120"
}
var conn = mysql.createConnection(options)
conn.connect()//可以省略
//提交SQL语句给mysql服务器执行
var sql = 'SELECT * FROM user'
conn.query(sql, (err, rows) => {
    if (err) {
        console.log("数据查询失败")
    } else {
        console.log("查询完成")
        //console.log(rows)
        for (var i = 0;i<rows.length;i++) {
            console.log(`用户名：${rows[i].uname},密码：${rows[i].upwd}`)
        }
    }
})
//断开与mysql服务器的连接
conn.end()
```
<hr>

#### 向mysql数据库中插入数据
```javascript
/*向mysql数据库中插入数据*/
//引入mysql模块
var mysql = require("mysql")
//创建连接
var options = {
    host: "127.0.0.1",
    user: "root",
    password: "",
    database: "user_0120"
}
var conn = mysql.createConnection(options)
//提交SQL语句
var sql = 'INSERT INTO user VALUES(NULL,"Jack","111")'
conn.query(sql, (err, data) => {
    if (err) {
        console.log("写入数据失败")
    } else {
        console.log("写入数据成功")
        console.log(`SQL语句影响的行数：${data.affectedRows}`)//获得影响的行数
        console.log(`INSERT产生的自增编号：${data.insertId}`)//获得自增主键
    }
})
//断开连接
conn.end()
```
<hr>

#### 实现动态web服务器
>
```javascript
/*实现动态web服务器*/
//引入相关模块
var http = require("http")
var url = require("url")
var fs = require("fs")
var mysql = require("mysql")
var path = require("path")
//创建web服务器
var server = http.createServer()
//为request事件绑定处理函数
server.on("request", doRequest)
//启动服务器，监听指定端口
server.listen(8001, "127.0.0.1", (err) => {
    if (err) {
        console.log("服务器启动失败")
        console.log(err)
    } else {
        console.log("服务器启动成功，正在监听8001端口")
    }
})
var urlInfo
//处理客户端的http请求
function doRequest(req, res) {
    console.log("请求的url为：" + req.url)
    urlInfo = url.parse(req.url, true)//解析请求的url
    //得到请求的url的文件名后缀
    var urlSuffix = path.parse(urlInfo.pathname).ext
    //console.log(urlSuffix)
    if (urlSuffix == ".do") {//以.do结尾的动态请求
        doDynamic(req, res)//处理动态请求
    } else {//其他结尾的静态请求
        doStatic(req, res)//处理静态请求
    }
}
//处理静态请求,直接读取指定的文件发送给客户端即可
function doStatic(req, res) {
    var path = '.' + urlInfo.pathname
    fs.readFile(path, (err, data) => {
        if (err) {
            console.log("文件读取失败")
        } else {
            //3.向客户端输出响应内容，res.write()
            res.statusCode = 200
            res.setHeader('Content-Type', 'text/htmlcharset=UTF-8')
            res.write(data)
            res.end()
        }
    })
}
//处理动态请求
function doDynamic(req, res) {
    var base = path.parse(urlInfo.pathname).base
    if (base == "login.do") {//处理登录
        console.log(urlInfo.query)
    } else if (base == "register.do") {//处理注册
    }
}
```
>index.html

```html
<!DOCTYPE html>
<html>

<head lang="en">
    <meta charset="UTF-8">
    <title>首页</title>
    <style>
        a {
            display: inline-block;
            width: 100px;
            height: 30px;
            line-height: 30px;
            text-align: center;
            background: #00aa22;
            color: #fff;
        }

        a:hover {
            color: #fff;
            background: #009900;
        }
    </style>
</head>

<body>
    <h1>欢迎来到我的主页</h1>
    <a href="login.html">登录</a>
    <a href="register.html">注册</a>
</body>

</html>
```
>login.html

```html
<!DOCTYPE html>
<html>

<head lang="en">
    <meta charset="UTF-8">
    <title></title>
</head>

<body>
    <h1>欢迎登录</h1>
    <form action="login.do">
        <p>用户名：
            <input type="text" name="uname" />
        </p>
        <p>密码：
            <input type="password" name="upwd" />
        </p>
        <button type="submit">登录</button>
    </form>
</body>

</html>
```
>register.html

```html
<!DOCTYPE html>
<html>

<head lang="en">
    <meta charset="UTF-8">
    <title></title>
</head>

<body>
    <h1>欢迎注册</h1>
    <form action="register.do">
        <p>用户名：
            <input type="text" name="uname" />
        </p>
        <p>密码：
            <input type="password" name="upwd" />
        </p>
        <button type="submit">注册</button>
    </form>
</body>

</html>
```
<hr>

#### 使用express搭建服务器
```javascript
/*使用express搭建服务器*/
//引入express模块
var express = require("express")
//创建express类的实例，作为http服务器
var app = express()
//启动express服务器，监听8002端口
app.listen(8002)
//访问根目录
app.get("/", (req, res) => {
    res.send("您访问的是根目录")
})
//访问具体目录
app.get("/html/index.html", (req, res) => {
    res.send("您访问的是html目录")
})
//使用正则匹配
app.get(/\/log/, (req, res) => {
    res.send("您访问的是log目录")
})
```
```javascript
/*使用express搭建服务器*/
//引入express模块
var express = require("express")
//创建express类的实例，作为http服务器
var app = express()
//启动express服务器，监听8002端口
app.listen(8003)

//访问静态资源
app.use(express.static(__dirname))
//app.use("/h",express.static("html"))
app.get('/html/login.do', (req, res) => {
})
app.get('/html/register.do', (req, res) => {
})

```
***
#### node process env
>export NODE_ENV=production && node xxx.js  这样在当前命令行下后续的命令中读取 NODE_ENV，都会得到  production 值；

>如果直接使用 NODE_ENV=production node xxx.js，则 NODE_ENV 的有效性仅限当前命令，不会对后续命令有影响

>bash

```bash
“dev-mac”: " export NODE_ENV=development&& nodemon --harmony --use_strict index.js  -w ",
“dev-win”: " set NODE_ENV=development&& nodemon --harmony --use_strict index.js  -w ",
```
>bash

```bash
export NODE_ENV=production && node xxx.js
NODE_ENV=production node xxx.js
```
>bash cross-env

```bash
cross-env NODE_ENV=production wepy build --no-cache
```
<hr>

>/node1/process.js

```javascript
console.log(process.env.NODE_ENV)
```
>bash

```bash
NODE_ENV=1 node node1/process
// 1
```
<hr>