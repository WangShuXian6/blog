#### JS 函数式编程
>https://legacy.gitbook.com/book/llh911001/mostly-adequate-guide-chinese/details

#### JavaScript 语言的基础概念-1

>JavaScript 的函数是可调用的

>当 hi 后面紧跟 () 的时候就会运行并返回一个值；如果没有 ()，hi 就简单地返回存到这个变量里的函数

```javascript
hi;
// function(name){
//  return "Hi " + name
// }

hi("jonas");
// "Hi jonas"
```
<hr>

>当我们说函数是“一等公民”的时候，我们实际上说的是它们和其他对象都一样...所以就是普通公民（坐经济舱的人？）。函数真没什么特殊的，你可以像对待任何其他数据类型一样对待它们——把它们存在数组里，当作参数传递，赋值给变量...等等。

>用一个函数把另一个函数包起来，目的仅仅是延迟执行，真的是非常糟糕的编程习惯

>编程界的流行的错误规范示例1：

>除了徒增代码量，提高维护和检索代码的成本外，没有任何用处

```javascript
// 太傻了
// 只是简单传递相同的参数
var getServerStuff = function(callback){
  return ajaxCall(function(json){
    return callback(json);
  });
};
```

>正确示例

```javascript
// 这才像样
var getServerStuff = ajaxCall;
```

<hr>

>编程界的流行的错误规范示例2

>如果一个函数被不必要地包裹起来了，而且发生了改动，那么包裹它的那个函数也要做相应的变更。

```javascript
httpGet('/post/2', function(json){
  return renderPost(json);
})
```
>如果 httpGet 要改成可以抛出一个可能出现的 err 异常，那我们还要回过头去把“胶水”函数也改了

```javascript
// 把整个应用里的所有 httpGet 调用都改成这样，可以传递 err 参数。
httpGet('/post/2', function(json, err){
  return renderPost(json, err);
})
```

>正确示例

>一等公民函数的形式

```javascript
httpGet('/post/2', renderPost);  // renderPost 将会在 httpGet 中调用，想要多少参数都行
```
<hr>

#### 在函数式编程中避免使用this (原型链式垃圾代码,环境变量会被轻易的改变)

<hr>

#### JavaScript 语言的基础概念-2

>纯函数是这样一种函数，即相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用

>slice 符合纯函数的定义是因为对相同的输入它保证能返回相同的输出。

>而 splice 却会嚼烂调用它的那个数组，然后再吐出来；这就会产生可观察到的副作用，即这个数组永久地改变了。

```javascript
var xs = [1,2,3,4,5];

// 纯的
xs.slice(0,3);
//=> [1,2,3]

xs.slice(0,3);
//=> [1,2,3]

xs.slice(0,3);
//=> [1,2,3]


// 不纯的
xs.splice(0,3);
//=> [1,2,3]

xs.splice(0,3);
//=> [4,5]

xs.splice(0,3);
//=> []
```
<hr>

>不纯的笨函数的依赖状态是影响系统复杂度的罪魁祸首

```javascript
// 不纯的
var minimum = 21;

var checkAge = function(age) {
  return age >= minimum;
};


// 纯的
var checkAge = function(age) {
  var minimum = 21;
  return age >= minimum;
};
```

<hr>

>不可变（immutable）对象，这样就能保留纯粹性.

>使用纯函数的形式，函数就能做到自给自足

```javascript
var immutableState = Object.freeze({
  minimum: 21
});
```

<hr>

#### JavaScript 语言的基础概念-3

>副作用是在计算结果的过程中，系统状态的一种变化，或者与外部世界进行的可观察的交互

>只要是跟函数外部环境发生的交互就都是副作用

>函数式编程的哲学就是假定副作用是造成不正当行为的主要原因

>副作用可能包含，但不限于：

-- 更改文件系统
-- 往数据库插入记录
-- 发送一个 http 请求
-- 可变数据
-- 打印/log
-- 获取用户输入
-- DOM 查询
-- 访问系统状态

<hr>

#### JavaScript 语言的基础概念-4

>函数是不同数值之间的特殊关系：每一个输入值返回且只返回一个输出值,而非多个输出值

>函数可以描述为一个集合，这个集合里的内容是 (输入, 输出) 对：[(1,2), (3,6), (5,10)]（看起来这个函数是把输入值加倍）

<hr>

#### 纯函数的意义

>纯函数就是数学上的函数，而且是函数式编程的全部

##### 1-可缓存性（Cacheable）

>纯函数总能够根据输入来做缓存。实现缓存的一种典型方式是 memoize 技术：
```javascript
var squareNumber  = memoize(function(x){ return x*x; });

squareNumber(4);
//=> 16

squareNumber(4); // 从缓存中读取输入值为 4 的结果
//=> 16

squareNumber(5);
//=> 25

squareNumber(5); // 从缓存中读取输入值为 5 的结果
//=> 25
```

```javascript
// 简单的实现，尽管它不太健壮
var memoize = function(f) {
  var cache = {};

  return function() {
    var arg_str = JSON.stringify(arguments);
    cache[arg_str] = cache[arg_str] || f.apply(f, arguments);
    return cache[arg_str];
  };
};
```

##### 通过延迟执行的方式把不纯的函数转换为纯函数

>并没有真正发送 http 请求——只是返回了一个函数，当调用它的时候才会发请求。

>这个函数之所以有资格成为纯函数，是因为它总是会根据相同的输入返回相同的输出：给定了 url 和 params 之后，它就只会返回同一个发送 http 请求的函数。

>我们的 memoize 函数工作起来没有任何问题，虽然它缓存的并不是 http 请求所返回的结果，而是生成的函数

```javascript
var pureHttpCall = memoize(function(url, params){
  return function() { return $.getJSON(url, params); }
})
```
<hr>

##### 2-可移植性／自文档化（Portable / Self-Documenting）
>在 JavaScript 的设定中，可移植性可以意味着把函数序列化（serializing）并通过 socket 发送。也可以意味着代码能够在 web workers 中运行。总之，可移植性是一个非常强大的特性

>命令式编程中“典型”的方法和过程都深深地根植于它们所在的环境中，通过状态、依赖和有效作用（available effects）达成；

>纯函数与此相反，它与环境无关，只要我们愿意，可以在任何地方运行它。

>面向对象语言的问题是，它们永远都要随身携带那些隐式的环境。你只需要一个香蕉，但却得到一个拿着香蕉的大猩猩...以及整个丛林

>纯函数是完全自给自足的，它需要的所有东西都能轻易获得

>纯函数的依赖很明确，因此更易于观察和理解——没有偷偷摸摸的小动作

>纯函数对于其依赖必须要诚实，这样我们就能知道它的目的

>纯函数能够提供多得多的信息

>通过强迫“注入”依赖，或者把它们当作参数传递，我们的应用也更加灵活；因为数据库或者邮件客户端等等都参数化了

```javascript
// 不纯的
var signUp = function(attrs) {
  var user = saveUser(attrs);
  welcomeUser(user);
};

var saveUser = function(attrs) {
    var user = Db.save(attrs);
    ...
};

var welcomeUser = function(user) {
    Email(user, ...);
    ...
};

// 纯的
var signUp = function(Db, Email, attrs) {
  return function() {
    var user = saveUser(Db, attrs);
    welcomeUser(Email, user);
  };
};

var saveUser = function(Db, attrs) {
    ...
};

var welcomeUser = function(Email, user) {
    ...
};
```
<hr>

##### 3-可测试性（Testable）
>纯函数让测试更加容易。我们不需要伪造一个“真实的”支付网关，或者每一次测试之前都要配置、之后都要断言状态（assert the state）。只需简单地给函数一个输入，然后断言输出就好了

>函数式编程的社区正在开创一些新的测试工具，能够帮助我们自动生成输入并断言输出。

> Quickcheck——一个为函数式环境量身定制的测试工具。

<hr>

##### 4-合理性（Reasonable）
>引用透明性（referential transparency）

>如果一段代码可以替换成它执行所得的结果，而且是在不改变整个程序行为的前提下替换的，那么我们就说这段代码是引用透明的

>由于纯函数总是能够根据相同的输入返回相同的输出，所以它们就能够保证总是返回同一个结果，这也就保证了引用透明性

>根据一个参数修改其他参数的值

```javascript
var Immutable = require('immutable');

var decrementHP = function(player) {
  return player.set("hp", player.hp-1);
};

var isSameTeam = function(player1, player2) {
  return player1.team === player2.team;
};

var punch = function(player, target) {
  if(isSameTeam(player, target)) {
    return target;
  } else {
    return decrementHP(target);
  }
};

var jobe = Immutable.Map({name:"Jobe", hp:20, team: "red"});
var michael = Immutable.Map({name:"Michael", hp:20, team: "green"});

punch(jobe, michael);
//=> Immutable.Map({name:"Michael", hp:19, team: "green"})
```

<hr>

##### 5-并行代码

>可以并行运行任意纯函数。因为纯函数根本不需要访问共享的内存，而且根据其定义，纯函数也不会因副作用而进入竞争态（race condition）

>并行代码在服务端 js 环境以及使用了 web worker 的浏览器那里是非常容易实现的，因为它们使用了线程（thread）。不过出于对非纯函数复杂度的考虑，当前主流观点还是避免使用这种并行

<hr>

#### 柯里化（curry）

>使用柯里化（curry）的原因

>纯函数程序写起来就有点费力

>通过到处传递参数来操作数据，禁止使用状态

>curry 的概念：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。

>高阶函数（higher order function）：参数或返回值为函数的函数

>每传递一个参数调用函数，就返回一个新函数处理剩余的参数。这就是一个输入对应一个输出。

>哪怕输出是另一个函数，它也是纯函数。

>当然 curry 函数也允许一次传递多个参数，但这只是出于减少 () 的方便。

>curry 函数能够让函数式编程不那么繁琐和沉闷

>通过简单地传递几个参数，就能动态创建实用的新函数；而且还能带来一个额外好处，那就是保留了数学的函数定义，尽管参数不止一个。

>创建一些 curry 函数

>策略性地把要操作的数据（String， Array）放到最后一个参数里

```javascript
var curry = require('lodash').curry;

var match = curry(function(what, str) {
  return str.match(what);
});

var replace = curry(function(what, replacement, str) {
  return str.replace(what, replacement);
});

var filter = curry(function(f, ary) {
  return ary.filter(f);
});

var map = curry(function(f, ary) {
  return ary.map(f);
});
```

>使用 curry 函数

>这里表明的是一种“预加载”函数的能力，通过传递一到两个参数调用函数，就能得到一个记住了这些参数的新函数

```javascript
match(/\s+/g, "hello world");
// [ ' ' ]

match(/\s+/g)("hello world");
// [ ' ' ]

var hasSpaces = match(/\s+/g);
// function(x) { return x.match(/\s+/g) }

hasSpaces("hello world");
// [ ' ' ]

hasSpaces("spaceless");
// null

filter(hasSpaces, ["tori_spelling", "tori amos"]);
// ["tori amos"]

var findSpaces = filter(hasSpaces);
// function(xs) { return xs.filter(function(x) { return x.match(/\s+/g) }) }

findSpaces(["tori_spelling", "tori amos"]);
// ["tori amos"]

var noVowels = replace(/[aeiou]/ig);
// function(replacement, x) { return x.replace(/[aeiou]/ig, replacement) }

var censored = noVowels("*");
// function(x) { return x.replace(/[aeiou]/ig, "*") }

censored("Chocolate Rain");
// 'Ch*c*l*t* R**n'
```

>curry 函数工具库

>ramda http://ramdajs.com/

>lodash-fp https://github.com/lodash/lodash-fp

<hr>

#### 代码组合（compose）
>两个函数组合之后返回了一个新函数

>组合某种类型（本例中是函数）的两个元素本就该生成一个该类型的新元素

>f 和 g 都是函数，x 是在它们之间通过“管道”传输的值。

>g 将先于 f 执行，因此就创建了一个从右到左的数据流。

>这样做的可读性远远高于嵌套一大堆的函数调用

>让代码从右向左运行，而不是由内而外运行

```javascript
var compose = function(f,g) {
  return function(x) {
    return f(g(x));
  };
}
```
> 使用组合

```javascript
var toUpperCase = function(x) { return x.toUpperCase(); };
var exclaim = function(x) { return x + '!'; };
var shout = compose(exclaim, toUpperCase);

shout("send in the clowns");
//=> "SEND IN THE CLOWNS!"
```

>顺序很重要

>reverse 反转列表，head 取列表中的第一个元素；所以结果就是得到了一个 last 函数（即取列表的最后一个元素），虽然它性能不高。这个组合中函数的执行顺序应该是显而易见的。尽管我们可以定义一个从左向右的版本，但是从右向左执行更加能够反映数学上的含义

```javascript
var head = function(x) { return x[0]; };
var reverse = reduce(function(acc, x){ return [x].concat(acc); }, []);
var last = compose(head, reverse);

last(['jumpkick', 'roundhouse', 'uppercut']);
//=> 'uppercut'
```

>结合律（associativity）

>符合结合律意味着不管你是把 g 和 h 分到一组，还是把 f 和 g 分到一组都不重要

```javascript
// 结合律（associativity）
var associative = compose(f, compose(g, h)) == compose(compose(f, g), h);
// true
```


```javascript
compose(toUpperCase, compose(head, reverse));

// 或者
compose(compose(toUpperCase, head), reverse);
```

>可变组合（variadic compose）

>组合是高于其他所有原则的设计原则

>如何为 compose 的调用分组不重要，所以结果都是一样的

>运用结合律能为我们带来强大的灵活性，还有对执行结果不会出现意外的那种平和心态

```javascript
// 前面的例子中我们必须要写两个组合才行，但既然组合是符合结合律的，我们就可以只写一个，
// 而且想传给它多少个函数就传给它多少个，然后让它自己决定如何分组。

var lastUpper = compose(toUpperCase, head, reverse);

lastUpper(['jumpkick', 'roundhouse', 'uppercut']);
//=> 'UPPERCUT'


var loudLastUpper = compose(exclaim, toUpperCase, head, reverse)

loudLastUpper(['jumpkick', 'roundhouse', 'uppercut']);
//=> 'UPPERCUT!'
```

>结合律的一大好处是任何一个函数分组都可以被拆开来，然后再以它们自己的组合方式打包在一起

>最佳实践是让组合可重用，就像 last 和 angry 那样。

>如果熟悉 Fowler 的《重构》一书的话，你可能会认识到这个过程叫做 “extract method”——只不过不需要关心对象的状态。

```javascript
var loudLastUpper = compose(exclaim, toUpperCase, head, reverse);

// 或
var last = compose(head, reverse);
var loudLastUpper = compose(exclaim, toUpperCase, last);

// 或
var last = compose(head, reverse);
var angry = compose(exclaim, toUpperCase);
var loudLastUpper = compose(angry, last);

// 更多变种...
```
<hr>

>pointfree 模式

>函数无须提及将要操作的数据是什么样的。一等公民的函数、柯里化（curry）以及组合协作起来非常有助于实现这种模式

>通过管道把数据在接受单个参数的函数间传递。

>利用 curry，我们能够做到让每个函数都先接收数据，然后操作数据，最后再把数据传递到下一个函数那里去。

>在 pointfree 版本中，不需要 word 参数就能构造函数；而在非 pointfree 的版本中，必须要有 word 才能进行进行一切操作。

>pointfree 模式能够帮助我们减少不必要的命名，让代码保持简洁和通用。

>对函数式代码来说，pointfree 是非常好的石蕊试验，因为它能告诉我们一个函数是否是接受输入返回输出的小函数

```javascript
// 非 pointfree，因为提到了数据：word
var snakeCase = function (word) {
  return word.toLowerCase().replace(/\s+/ig, '_');
};

// pointfree
var snakeCase = compose(replace(/\s+/ig, '_'), toLowerCase);
```

>注意：

>组合的一个常见错误是，在没有局部调用之前，就组合类似 map 这样接受两个参数的函数。

```javascript
// 错误做法：我们传给了 `angry` 一个数组，根本不知道最后传给 `map` 的是什么东西。
var latin = compose(map, angry, reverse);

latin(["frog", "eyes"]);
// error


// 正确做法：每个函数都接受一个实际参数。
var latin = compose(map(angry), reverse);

latin(["frog", "eyes"]);
// ["EYES!", "FROG!"])
```

>debug 组合

>使用不纯的 trace 函数来追踪代码的执行情况

>trace 函数允许我们在某个特定的点观察数据以便 debug

```javascript
var trace = curry(function(tag, x){
  console.log(tag, x);
  return x;
});

var dasherize = compose(join('-'), toLower, split(' '), replace(/\s{2,}/ig, ' '));

dasherize('The world is a vampire');
// TypeError: Cannot read property 'apply' of undefined
```

```javascript
var dasherize = compose(join('-'), toLower, trace("after split"), split(' '), replace(/\s{2,}/ig, ' '));
// after split [ 'The', 'world', 'is', 'a', 'vampire' ]
```

```javascript
var dasherize = compose(join('-'), map(toLower), split(' '), replace(/\s{2,}/ig, ' '));

dasherize('The world is a vampire');

// 'the-world-is-a-vampire'
```