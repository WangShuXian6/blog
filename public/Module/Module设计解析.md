>JS 模块化规范目前主要分为：CommonJS、AMD、CMD、UMD 和ES6。

>ES6为名门正派，其它则为开发者草拟的野生规范_

## 设计理念
### CommonJS
```js
//定义模块 math.js
function add(a, b){
    return a + b
}

module.exports = {
    add: add
}

//加载模块
var math = require('math');
math.add(2, 3);
```

>因为require是同步的，math方法只能在math.js加载完之后才能运行，在浏览器环境如果网络不够快，会报错，所以CommonJS只适用于服务器端，也催生了AMD("Asynchronous Module Definition")的诞生。

### AMD
>由于模块化不是javascript原生支持，使用AMD需要对应的库RequireJS。

```js
define(id, dependencies, factory);
```

>id：可选参数，用来定义模块的标识，如果没有提供该参数，脚本文件名（去掉拓展名）

>dependencies：是一个当前模块依赖的模块名称数组

>factory：工厂方法，模块初始化要执行的函数或对象。如果为函数，它应该只被执行一次。如果是对象，此对象应该为模块的输出值 。

```js
//定义模块 math.js
define(['dependency'], function(dependency){
    function add(a, b){
    	return a + b
	}

    return {
        add: add
    };
});

//加载模块
require(['math'], function (math) {
    math.add(2, 3);
});
```

>AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块：dependency。

### CMD
>CMD跟AMD类似，只不过在模块定义方式和模块加载（可以说运行、解析）时机上有所不同 。

```js
//定义模块 math.js
define(function(requie, exports, module){
    var dependency = require('dependency');
    function add(a, b){
    	return a + b
	}

    return {
        add: add
    };
});

//加载模块
seajs.use(['math'], function (math) {
    math.add(2, 3);
});
```

>CMD推崇就近依赖，只有在用到某个模块('dependency')的时候再去require。

### UMD
>AMD模块以浏览器第一的原则发展，异步加载模块，CommonJS模块以服务器第一原则发展，选择同步加载，UMD是AMD和CommonJS的糅合。

>既然要通用，怎么办呢？分别判断是否支持AMD和CommonJS，这就是所谓的UMD。

```js
((root, factory) => {
  if (typeof define === 'function' && define.amd) {
    //AMD
    define(['dependency'], factory);
  } else if (typeof exports === 'object') {
    //CommonJS
    var dependency = requie('dependency');
    module.exports = factory(dependency);
  } else {
    //都不是，浏览器全局定义
    root.returnExports = factory(root.dependency);
  }
})(this, (dependency) => {
  //do something...  这里是真正的函数体
  function add(a, b){
    	return a + b
	}

    return {
        add: add
    };
});
```

## require/exports及import/export 的用法
>require/exports 的用法只有以下三种简单的写法：

```js
const fs = require('fs')
exports.fs = fs
module.exports = fs
```

>而 import/export 的写法就多种多样：

```js
import fs from 'fs'
import {default as fs} from 'fs'
import * as fs from 'fs'
import {readFile} from 'fs'
import {readFile as read} from 'fs'
import fs, {readFile} from 'fs'

export default fs
export const fs
export function readFile
export {readFile, read}
export * from 'fs'
```

##### 用法总结
* import为编译时加载，而require为运行时加载，没法在编译时做“静态优化”。
* import导入模块的属性或者方法是强绑定的，包括基础类型（即模块改变导入后的模块也改变）；而 require 则是普通的值传递或者引用传递（即模块改变导入后的模块也不会改变）。
* 使用export default时，对应的import语句不需要使用大括号；不使用export default时，对应的import语句需要使用大括号。