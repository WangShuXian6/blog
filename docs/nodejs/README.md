#### node

>Install Node.js on Ubuntu 16.04

```bash

sudo apt-get update
cd ~
curl -sL https://deb.nodesource.com/setup_8.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt-get install nodejs
sudo apt-get install npm
nodejs -v
```



***

>npm 短语 test start stop 不需要加run

>npm test

>npm start

>npm stop

>npm run xxxx

>npm 前置钩子,后置钩子

>'pretest':'echo 111',

>'posttest':'echo 222',

>'test':'echo 333'

>传递参数     --xxxx

>配置变量
'config':{
	'xxx':'xxxx'
}

>使用

>linux,mac

>$npm_package_config_xxx

>windows

>%npm_package_config_xxx

>文件操作推荐使用包，防止跨系统区别

<hr>

>npm全称Node Package Manager

<hr>

#### Node.js概述：

>Node.js官网：www.nodejs.org
>1.Node.js是C++编写的基于V8引擎的JS运行时环境。
>2.Node.js是一门基于ECMAScript开发的服务器端语言，提供了（全端JS没有的）很多扩展对象。

##### 前端js：
>1.ES原生对象：String、Number、Boolean、Math、Date、Error、Function、Object、Array、RegExp...

>2.BOM&DOM

>3.自定义对象

##### Node.js：
>1.ES原生对象

>2.Node.js内置对象

>3.大量的第三方对象

>4.自定义对象

##### 3.Node.js可以编写独立的服务器应用，无需借助于其他web服务器。

##### Node.js的意义：
> 1.执行效率比PHP/JSP/JAVA要快

>2.用一种语言统一了前后端开发。

>特点：

>1.单线程逻辑处理

>2.非阻塞

>3.异步I/O处理

>4.事件驱动编程

##### 2.Node.js的两种运行方式：
>1.交互模式——用于测试

>读取用户输入、执行运算、输出执行结果、继续下一循环

>执行方法：输入一行js语句，回车执行

>2.脚本模式——用于开发

>把要执行的所有js语句编写在一个独立的js文件中，一次性的提交给nodejs处理。此文件可以没有后缀

>执行方法：node d:\xx\xx.js

##### 3.Node.js的基础语法及ES6新特性
>1.数据类型：

>（1）原始类型

>string、number、boolean...

>原始类型数据直接赋值即可

>（2）引用类型

>ES原生对象、Node.js对象、自定义对象

>引用类型通常需要使用new关键字创建

>2.模板字符串

>ES6中提供的一种新的字符串形式

>（1）使用模板方式定义字符串，数据可以实现换行

>（2）可以使用${}拼接变量，并且可以执行运算

>3.严格模式

>ES5中新增一种比普通模式更为严格的js运行模式。

>使用方法：

>（1）在整个脚本文件中启用严格模式

>在脚本文件的开头："use strict";

>用于新项目

>（2）在单个函数内启用严格模式

```javascript
		function info(){
			"use strict";
			.........
		}
```
>用于老项目维护

>规则：

-（1）修改常量的值是非法的——将静默失败升级为错误
-（2）不允许对未声明的变量赋值
-（3）匿名函数的this不再指向全局

>4.变量的作用域

>全局作用域、局部作用域、块级作用域（ES6中专有）

>块级作用域：只在当前代码块中使用

>代码块：任何一个{}都是一个代码块，if/for/while...

>代码块中使用“let”声明块级作用域变量，出了代码块，便不可使用。使用let需要启用严格模式。

>5.循环结构

>for...of...(ES6)

>遍历数组的元素值

>6.函数

>匿名函数的自调

>=> 箭头函数

```javascript
		() => {
	
		}
```
>箭头函数只用于匿名函数

>箭头函数中不存在arguments对象

>7.对象

>创建对象的方式：

-（1）对象直接量
-（2）构造函数方式
-（3）原型继承方式
-（4）class方式——ES新增

>class	类：是一组相似对象的属性和行为的抽象集合。（描述一类事物统一的属性和功能的程序结构）

>事物的属性就是class的属性，事物的功能就是class的方法。

>使用class方式创建自定义对象是，必须启用严格模式。

```javascript
		"use strict";
		//创建自定义对象
		class Emp {
  			constructor(ename){   
				this.ename=ename; 
  			}
  			work(){    
 			 }
		}
		var e1=new Emp("tom");

		// 实现继承：
		class Programmer extends Emp{
  			constructor(ename,salary,skills){
    				super(ename,salary);
    				this.skills=skills;
  			}  
		}
```

##### 4.全局对象
>Node.js中的全局对象是Global.

>在交互模式下，声明的全局变量是global的成员。——全局对象的污染

>在脚本模式下，声明的全局变量不是global的成员。——避免了全局对象的污染

>Global对象的成员属性和成员方法

>1.console 用于向stdout(标准输出)和stderr(标准错误)输出信息。
	
>>console.log()	//向stdout输出日志信息
>>console.info()	//同上
>>console.error()	//向stderr输出错误信息
>>console.warn()	//同上
>>console.trace()	//向stderr输出栈轨迹信息
>>console.dir()	//向stdout输出指定对象的字符串表示
>>console.assert()	//断言，为真的断言，错误信息不会输出；为假的断言，抛出错误对象，输出错误信息，并且终止脚本的运行，可以自定义错误信息。
>>console.time()  console.timeEnd()//测试代码的执时间	

>注意：console中的成员方法是异步的，输出顺序和书写顺序不一定完全一致。

>2.process 进程

>>process.arch	//获取CPU架构类型
>>process.platform	//获取操作系统类型
>>process.env	//获取操作系统环境变量
>>process.cwd()	//获取当前文件所在工作目录
>>process.execPath	//获取Node.js解释器所在目录
>>process.versions	//获取Node.js版本信息
>>process.uptime()	//获取Node.js进程运行时间(s)
>>process.memoryUsage()//获取Node.js进程内存使用信息
>>process.pid	//获取进程ID号
>>process.kill( pid )	//向指定进程ID号发送退出信号

>3.定时器

>>global.setTimeout()	//一次性定时器
>>global.setInterval()	//周期性定时器
>>global.setImmediate()//在下次事件循环开始之前立即执行的定时器
>>process.nextTick()	//本次事件循环结束之后立即执行的定时器



##### 5.模块系统：

>Node.js中使用“Module(模块)”来规划不同的功能对象。

>模块的分类：

-（1）核心模块——Node.js的内置模块（有些不需引入可直接使用，有些需要引入）
-（2）第三方模块
-（3）自定义模块
>每一个被加载的js文件对应一个模块对象，包含响应的功能和函数。

>模块中声明的变量或函数的作用域叫做“模块作用域”，这些变量和函数都不是global的成员，默认只能在当前的js文件（当前模块）中使用。

>Node.js启动时运行的第一个模块叫做“主模块”——main module，也是自身模块。

>获取主模块对象：

>process.mainModule

>require.main

>除主模块之外的其他模块都称为子模块。

>默认情况下，某一个模块不能直接使用其他模块中封装的数据，但是每个模块可以导出(exports)自己内部的对象供其他模块使用，也可用引入(require)并使用其他模块导出的对象。
	
>每一个模块内部都可以使用一个变量：module，指向当前模块自己。

```javascript
	//判断当前模块是否主模块
	console.log(require.main===module);
```
>模块的引入：require()

>(在交互模式下，Node.js自带的模块无需引入，直接使用)
<hr>