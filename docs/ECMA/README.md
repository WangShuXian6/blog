#### 1
```javascript
export default class DetailCard{
}

import DetailCard from '@/components/orderDetail/detailCard'
```

#### 迭代对象
```ts
for (const value in that.model) {
            if (that.model.hasOwnProperty(value)) {
                console.log('value--', value);
            }
        }
```

#### 闭包
>https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures

>https://github.com/wintercn/blog/issues/3

>闭包这个概念第一次出现在1964年的《The Computer Journal》上，由P. J. Landin在《The mechanical evaluation of expressions》一文中提出了applicative expression和closure的概念。（在计算机领域，closure还有两个完全不同的意义：计算几何中的凸包，和编译领域的包含集）

>文中AE的概念定义如下：

>We are, therefore, interested in a class of expressions about any one of which it is appropriate to ask the following questions:

> - Q1. Is it an identifier? If so, what identifier?
> - Q2. Is it a λ-expression? If so, what identifier or identifiers constitute its bound variable part and in what arrangement? Also what is the expression constituting its λ-body?
> - Q3. Is it an operator/operand combination? If so, what is the expression constituting its operator? Also what is the expression constituting its operand?
We call these expressions applicative expressions (AEs).

>简单翻译

>这样，我们对于能够适合提问以下问题的一类特性表达式感兴趣：

> - Q1. 它是否是标识符？ 如果是的话，是什么标识符？
> - Q2. 它是否是λ表达式？ 如果是的话，哪个或者哪些的标识符构成了它的绑定变量部分，范围是什么？还有是什么表达式构成了它的λ-body？
> - Q3. 它是否是操作符/操作数的组合？如果是的话，构成它的操作符的表达式是什么？还有构成它的操作数的表达式又是什么？
我们称这一类表达式为“适用表达式”（AE）。
在AE的基础上，闭包定义为

>Also we represent the value of a λ-expression by a bundle of information called a "closure", comprising the λ-expression and the environment relative to which it was evaluated. We must therefore arrange that such a bundle is correctly interpreted whenever it has to be applied to some argument. More precisely:

>a closure has an environment part which is a list whose two items are:
> - (1) an environment
> - (2) an identifier or list of identifiers
> - and a control part which consists of a list whose sole item is an AE.

>翻译如下

>此外，我们把带有一系列信息的λ表达式称作"闭包"，其中包括λ表达式和它执行时的相关连的环境。于是我们要确保无论当它被用于什么参数时，这些信息都能够被正确解析。更精确地：
闭包包含环境部分，它是个只有两项的list，这两项分别是：

> - (1)环境
> - (2)一个标识符或者标识符列表
> - 以及控制部分，他是个只有一项的list，唯一的项是个AE

>在1975年，Scheme最早实现了闭包。

>60年代，因为受到函数式思维影响，计算机软件领域的"表达式"一般是包括λ表达式，也就是函数式语言中的函数的。所以这个闭包的定义在80年代后被更多过程式语言借鉴，其中包括C语言的static全局变量机制、C++的成员函数、JavaScript的局部函数。对于过程式语言中，因为很少有函数以外的"表达式"，所以闭包这个概念更多被用于指函数。

>在JavaScript中，我们称函数对象为闭包。根据ECMA-262规范，JavaScript的函数包含一个[[scope]]属性。[[scope]]指向scope chain（ECMA-262v3）或者Lexical Environment（ECMA-262v5）。这对应于闭包的环境部分，[[scope]]中可访问的属性列表即是标识符列表，对象本身的引用则对应于环境。控制部分即是函数对象本身了。

#### 在浏览器中使用ES6的模块功能 import 及 export
```html
<script type="module"> 
    import {addTextToBody} from './utils.js'; 
    addTextToBody('Modules are pretty cool.'); 
</script>
```