#### TypeScript

>https://www.tslang.cn

>TypeScript是一门静态类型的语言

>TypeScript是一门开源编程语言，由Microsoft开发并维护。它首次发布于2012年10月。TypeScript是ECMAScript的超集，它支持JavaScript的所有语法和语义，并且在此之上提供了更多额外的特性，例如静态类型和更丰富的语法

***
>安装TypeScript编译器和可执行程序（tsc），并且添加到环境变量的全局路径中。

```bash

npm install-g typescript

tsc –v
```

##### 使用 third party library 
>在从 npm 安装 third party library(第三方库) 时，一定要记得同时安装此 library 的类型声明文件(typing definition)。你可以从 TypeSearch 中找到并安装这些第三方库的类型声明文件。

>举个例子，如果想安装 lodash 类型声明文件，我们可以运行下面的命令：

```bash
npm install --save-dev @types/lodash
```
>http://microsoft.github.io/TypeSearch/

***
##### 导入其他资源 
>想要在 TypeScript 中使用非代码资源(non-code asset)，我们需要告诉 TypeScript 推断导入资源的类型。在项目里创建一个 custom.d.ts 文件，这个文件用来表示项目中 TypeScript 的自定义类型声明。我们为 .svg 文件设置一个声明：

>custom.d.ts

```ts
declare module "*.svg" {
  const content: any;
  export default content;
}
```
>这里，我们通过指定任何以 .svg 结尾的导入(import)，将 SVG 声明(declare) 为一个新的模块(module)，并将模块的 content 定义为 any。我们可以通过将类型定义为字符串，来更加显式地将它声明为一个 url。同样的概念适用于其他资源，包括 CSS, SCSS, JSON 等。

>global.d.ts

```ts
declare module "*.png";
declare module "*.gif";
declare module "*.jpg";
declare module "*.jpeg";
declare module "*.svg";
declare module "*.css";
declare module "*.less";
declare module "*.scss";
declare module "*.sass";
declare module "*.styl";
declare namespace wx{

}
```
***
##### 非空断言运算符 !

>声明但未赋初始值时，确定值不会是null或undefined，才可以使用!.

>使用时值为null或undefined,编译器将不会判断为错误类型

>使用条件成立，但不推荐

```javascript
private testb!: string
console.log(this.testb)
```
>使用条件不成立，报错

```javascript
private testb!: string = 'ss'
// 不可使用
```
>类型错误，报错

```javascript
this.testb = null
console.log(this.testb)
```
>类型错误，报错

```javascript
this.testb =123
console.log(this.testb)
```
>主动告诉编译器前面的值不会是null或undefined，编译器将

> 这是一种告诉编译器的方法：“这个表达式不能为null或者在这里没有undefined ，所以不要抱怨它是null或者undefined的可能性。 有时候类型检查器本身无法做出这个决定。

>我觉得在这个解释中使用“assert”这个术语有点误导。 从开发者主张的角度来看，它是“断言”的，而不是在测试即将进行的意义上。 最后一行确实表明它没有发出JavaScript代码。

>尾随 "bang" 语法 (!),TypeScript 感叹号（!）结尾的语法，它会从前面的表达式里移除null和undefined。 所以我们也可以写成

```javascript
document.getElementById('root')!
```

<hr>

##### 通过名字后面加?为指定可选参数

```javascript
export interface Props {
  name: string;
  enthusiasmLevel?: number;
  onIncrement?: () => void;
  onDecrement?: () => void;
}
```

##### 导入 commonJs 格式包

> TypeScript 中使用 import 导入 commonJs 格式包

```typescript
import * as crypto from 'crypto-js'

crypto.MD5(paramsFianl).toString().toUpperCase()
```
***
##### 声明参数默认值

```typescript
function test(a: string, b: string, c: string = '123') {

}
```
***
##### 可选参数

>在声明函数时，可以在参数后用?表示此参数可选

>可选参数一定要放在必选参数之后

```typescript
function test(a: string, b?: string, c: string = '123') {
  console.log(a);
  console.log(b);
  console.log(c);
}
test('aaa')
// aaa,undefined,123
```
***
#### 深入理解 TypeScript
>https://jkchao.github.io/typescript-book-chinese/typings/enums.html

#### ts
>在命令行中执行 tsc --init 可以在当前目录中快速创建一个 tsconfig.json 文件.

```bash
npm init -y

npm install typescript -D

tsc --init
```

#### TypeScript 特性

##### 编译时类型检查

>TypeScript有自己的编译器，采用了静态代码分析技术
>如果采用静态类型，TypeScript会帮助我们注意对象中的各种属性，如果发生了拼写错误，编译器会产生编译时错误来发出警告。

>如果采用静态类型，TypeScript会帮助我们注意对象中的各种属性，如果发生了拼写错误，编译器会产生编译时错误来发出警告。

##### 类型注解

>使用元数据来给代码添加注解是TypeScript内置的特性，叫作类型注解。
>文本编辑器和IDE可以根据这些注解对我们的代码进行更好的静态分析。
>基于这一特性，可以提供更好的代码重构工具和自动完成功能，从而让我们在编写代码的时候效率更高、错误更少。
>一旦代码编译完成，所有类型注解都会被删除

#### 访问修饰符
>TypeScript支持以下访问修饰符：

>Public：所有定义成public的属性和方法都可以在任何地方进行访问。
>Private：所有定义成private的属性和方法都只能在类定义内部进行访问。
>Portected：所有定义成protected的属性和方法可以从类定义内部访问，也可以从子类中访问。