#### TypeScript 基础

#### tsc
```bash
$ tsc --outDir dirName
$ tsc --outDir dirName compileName # 指定输出输入位置
$ tsc --init # tsconfig.json
$ tsc -w # 动态监视
$ ts-node file # 直接运行ts文件
```

```bash
$ npm bin -g # 查看-g命令目录
$ tsc file -d # 生成.d.ts文件
```

***
#### tsconfig.json

>tsconfig.json是编译上下文，其中的compilerOptions字段提供编译选项，可以通过tsc --init生成

***

#### type
>变量使用前要定义

```ts
// let/var/const 变量名:变量类型 = 默认值

let test: string = 'test'
```

#### 变量类型

> - number: 数值类型
> - string: 字符串类型
> - boolean: 布尔类型
> - symbol: 符号类型，标识唯一对象
> - any: 任意类型
> - object: 对象类型(数组Array/<string/>，元祖[string, number]，类class，接口interface，函数function等)

```ts
any
void

boolean
number
string

null
undefined

string[]          /* or Array<string> */
[string, number]  /* tuple */

string | null | undefined   /* union */

never  /* unreachable */
```
```ts
enum Color {Red, Green, Blue = 4}
let c: Color = Color.Green
```
***
#### 类型别名
```ts
type Name = string | string[]

type StrOrNum = string | number
// 使用
let sample: StrOrNum
```

#### var,let 区别

> - 限定变量的作用范围
> - 防止变量的重复定义

#### 常量
>用处：

> - 系统配置文件路径
> - 数据库连接串
> - 公司名称，电话，邮件地址
> - 画面表示信息（登录失败，系统出错）

```ts
const name:type = initial_value

const DATA:number[] = [10, 20, 30]
```

#### 数组
```ts
let name:type[] = initial_value

let name:type[][] = [ // 二维数组
    [], [], []
]
```

#### 枚举
>枚举类型，可以增加代码的可读性。

```ts
enum name { name1, name2, name3 }

enum Sex {
    MALE,
    FEMALE,
    UNKNOWN
}
```

#### 联合类型
```ts
let a: number | null | undefined
```

#### function
```ts
function add (a: number, b: number): number {
  return a + b
}

// 返回类型可选
function add (a: number, b: number) { ... }
```

```ts
function run(a: string): string {
    return ''
}
let s = function (a: number): string {}

let t1 = (x, y) => x + y
let t2 = (x, y) => { return x + y }
let t3:(a: number, b: string) => void = function (a: number, b: string): void {}

interface P {
    (a: number, b: string): void
}
let add: P = function (a: number, b: string): void {}
```

#### 函数重载
>通过为同一个函数（同名函数）提供多个函数类型定义来实现多种功能的目的。

>需多次声明函数头，最后一个函数头是在函数体内实现函数体，但不可以用于外部。

```ts
// 重载
function padding(all: number)
function padding(topAndBottom: number, leftAndRight: number)
function padding(top: number, right: number, bottom: number, left: number)
function padding(a: number, b?: number, c?: number, d?: number) {
  if (b === undefined && c === undefined && d === undefined) {
    b = c = d = a
  } else if (c === undefined && d === undefined) {
    c = a
    d = b
  }
  return {
    top: a,
    right: b,
    bottom: c,
    left: d
  }
}
```

#### class
>静态属性，静态方法

```ts
class Point {
  x: number
  y: number
  static instances = 0
  constructor(x: number, y: number) {
    this.x = x
    this.y = y
  }
}

class Person {
    public name: string
    static age: number
    constructor (name: string) {
        this.name = name
    }
    public run () {
        console.log('run')
    }
    static work () { // 静态方法里没方法调用成员方法
        console.log('work')
    }
}

let p = new Person('s')
```
#### 抽象类，多态
>多态：父类定义一个方法不去实现，让继承的子类去实现，每一个子类有不同的表现。

```ts
// 多态
class Animal {
    protected name: string
    constructor (name: string) {
        this.name = name
    }
    public eat () {
        console.log(this.name + ' eat')
    }
}

class Dog extends Animal {
    constructor (name: string) {
        super(name)
    }
    public eat () {
       console.log(this.name + ' eat') 
    }
}

class Pig extends Animal {
    constructor (name: string) {
        super(name)
    }
    public eat () {
        console.log(this.name + ' eat')
    }
    public hoho () {
        console.log('hoho')
    }
}

let d = new Dog('dog')
d.eat()

let p = new Pig('pig')
p.hoho()
```
```ts
// 抽象类： 定义一种标准

abstract class Animal {
    abstract eat (): void
    abstract name: string
}

class Dog extends Animal {
    public name = 'dog'
    constructor () {
        super()
    }
    public eat () {}
}
```
***
#### interface
>接口运行时的影响为 0

>声明空间:

>类型声明空间与变量声明空间

>类型注解: 不能当作变量来使用，因为它没有定义在变量声明。

```ts
class Person {}
type Bas = {}
interface Test {}
```

##### 变量声明：变量仅在声明空间中使用，不能用作类型注解。

>内联

```ts
function printLabel (options: { label: string }) {
  console.log(options.label)
}

// 注意分号
function getUser (): { name: string; age?: number } {
}
```

>显式

```ts
interface LabelOptions {
  label: string
}

function printLabel(options: LabelOptions) { ... }
```

##### 可选属性
```ts
interface User {
  name: string,
  age?: number
}
```

>只读
```ts
interface User {
  readonly name: string
}
```

>动态key
```ts
{
  [key: string]: Object[]
}
```

##### 接口是规范的定义。
```ts
// 属性类型接口
interface Color {
    firstName: string
    name: string
}

let a: Color = { // 对象约束
    name: 'tan',
    firstName: 'pink'
}
```
```ts
// 函数类型接口
interface encrypt {
    (key: string, val: string): string
}

let md5: encrypt = (key, val): string => {}

const simple: (foo: number) => string = foo => foo.toString()

// 可索引接口，数组的约束
interface UserArr {
    [index: number]: string
}

let arr:UserArr = ['a', 'b']

// 可索引接口，对象的约束
interface UserObj {
    [index: string]: string
}

let obj: UserObj = {
    name: 's'
}

// 类接口约束， 对类的约束， 实现接口类
interface Animate {
    name: string
    eat(e: string): void
}

class Pig implements Animate {
    name: string
    constructor () {}
    eat () {}
}
```

##### type与interface

>能用interface实现，就用interface, 如果不能就用type

>相同点：

> - 都可以描述一个对象或者函数
> - 都允许拓展（extends）: 并且两者并不是相互独立的，interface可以extends type, type 也可以 extends interface

```ts
// interface
interface User {
  name: string
  age: number
}

interface SetUser {
  (name: string, age: number): void
}

// type
type User = {
    name: string
    age: number
}
type SetUser = {
    (name: string, age: number): void
}

// interface extends interface
interface Name {
    name: string
}
interface User extends Name {
    age: number
}

// type extends type
type Name = {
    name: string
}
type User = Name & { age: number }

// interface extends type
type Name = {
    name: string
}
interface User extends Name {
    age: number
}

// type extends interface
interface Name {
    name: string
}
type User = Name & {
    age: number
}
```

>不同点：

> - type 可以声明基本类型别名，联合类型，元组等类型
> - interface 能够声明合并
```ts
// 基本类型别名
type Name = string

// 联合类型
interface Dog {
    wong();
}
interface Cat {
    miao();
}

type Pet = Dog | Cat

// 具体定义数组每个位置的类型
type PetList = [Dog, Pet]

// 获取一个变量的类型时，使用typeof, 如果不存在，则获取该类型的推论类型
let div = document.createElement('div')
type B = typeof div

interface Person {
  name: string;
  age: number;
  location?: string;
}

const jack: Person = { name: 'jack', age: 100 };
type Jack = typeof jack; // -> Person

function foo(x: number): Array<number> {
  return [x];
}

type F = typeof foo; // -> (x: number) => number[] // 类型推导

// interface 声明合并 // 定义相同的接口，合并成新的接口描述
interface User {
  name: string
  age: number
}

interface User {
  sex: string
}
```


#### generics
>任何出现类型的地方都可以使用

>泛 -- 宽泛

>作用：解决类，接口，方法的复用性，以及对不特定数据类型的支持（类型校验）

```ts
// 泛型函数
function getData<T> (value: T): T {
    return value
}


// 泛型类
class Greeter<T> {
  greeting: T
  constructor(message: T) {
    this.greeting = message
  }
}

let greeter = new Greeter<string>('Hello, world')


// 接口泛型
interface Array<T> {
  reverse(): T[]
  sort(compare?: (a:T, b:T) => number): T[]
}

// 接口函数泛型
interface ConfingFn {
    <T>(val1: T, val2: T): T
}
interface ConfingFn<T> {
    (val1: T, val2: T): T
}
```

#### 类型断言
```ts
let len: number = (input as string).length
let len: number = (<string> input).length  /* 不能再 JSX 中使用 */
```

#### other
##### 使用风格

> - 箭头函数代替匿名函数表达式

```ts
x => x + x
(x, y) => x + y
<T>(x: T, y: T) => x === y
```
> - 使用{}把循环体和条件语句括起来

```ts
for (let i = 0; i < 10; i++) {}
if (x < 10) {}
```
> - 每一个变量声明语句只声明一个变量。let x = 1; let y = 1; 并不是let x = 1, y = 1
> - 如果函数没有返回值，使用void
> - 需要的时候才把箭头函数的参数括起来。

***
##### 处理json和字符串

```ts
let person = '{"name": "pink", "age": 22}'

const jsonParse: ((key: string, value: any) => any) | undefined = undefined
let objectConverted = JSON.parse(person, jsonParse)
```

##### 转换为Number, String
>ts中推荐使用Number(), String()

```ts
Number('10') // 10
String(10) // '10'
```

##### 对象属性不存在错误
> - 能修改该值的类型声明，那么添加上缺损值的属性
> - 使用// @ts-ignore
> - 使用类型断言，强制为any类型: (this.props as any).flag

##### 类型不明确的错误

>优先使用类型保护。

> - 使用类型保护(type guards)
> - 使用类型断言
> - 使用 // @ts-ignore 注释
