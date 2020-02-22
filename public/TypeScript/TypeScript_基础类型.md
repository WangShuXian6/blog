#### TypeScript 基础类型

#### Declare
>https://www.tslang.cn/docs/handbook/basic-types.html


```typescript
let isDone: boolean = false

let decLiteral: number = 6

let name: string = "bob"

let list: number[] = [1, 2, 3]
let list: Array<number> = [1, 2, 3]

let x: [string, number]

enum Color {Red, Green, Blue}
let c: Color = Color.Green
let colorName: string = Color[2]

let notSure: any = 4

function warnUser(): void {
    console.log("This is my warning message")
}
let unusable: void = undefined

let u: undefined = undefined

let n: null = null

function error(message: string): never {
    throw new Error(message)
}

declare function create(o: object | null): void
```

***

#### boolean 布尔值

```typescript
let isDone: boolean = false
```
***
#### number 数字
```typescript
let decLiteral: number = 6
let hexLiteral: number = 0xf00d
let binaryLiteral: number = 0b1010
let octalLiteral: number = 0o744
```
***
#### string 字符串
```typescript
let name: string = "bob"
```
***
#### number[] 数组 
>元素类型后面接上 []，表示由此类型元素组成的一个数组
```typescript
let list: number[] = [1, 2, 3]
```
***
#### Array<元素类型> 数组泛型
```typescript
let list: Array<number> = [1, 2, 3]
```
***
#### Tuple 元组
>元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同

>当访问一个已知索引的元素，会得到正确的类型

>定义一对值分别为 string和number类型的元组

```typescript
let x: [string, number]
```
>当访问一个越界的元素，会使用联合类型替代

>OK, 字符串可以赋值给(string | number)类型

```typescript
x[3] = 'world'
```
***
#### enum 枚举
>为一组数值赋予友好的名字

>默认情况下，从0开始为元素编号

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green
```
>手动赋值

```typescript
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green
```
>由枚举的值得到它的名字

```typescript
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2]

console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```
***
#### any
>不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查

```typescript
let notSure: any = 4;
notSure = "maybe a string instead"
notSure = false
```
>一个数组，包含了不同的类型的数据

```typescript
let list: any[] = [1, true, "free"]

list[1] = 100
```
***
#### void
>没有任何类型

```typescript
function warnUser(): void {
    console.log("This is my warning message")
}
```
```typescript
let unusable: void = undefined
```
***
#### null 和 undefined
>指定了--strictNullChecks标记，null和undefined只能赋值给void和它们各自

```typescript
let u: undefined = undefined
let n: null = null
```
***
#### never
>永不存在的值的类型

>never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never

>返回never的函数必须存在无法达到的终点

```typescript
function error(message: string): never {
    throw new Error(message)
}
```
>推断的返回值类型为never

```typescript
function fail() {
    return error("Something failed")
}
```
>返回never的函数必须存在无法达到的终点

```typescript
function infiniteLoop(): never {
    while (true) {
    }
}
```
***
#### object
>表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型

>使用object类型，就可以更好的表示像Object.create这样的API

```typescript
declare function create(o: object | null): void

create({ prop: 0 }) // OK
create(null) // OK

create(42) // Error
create("string")// Error
create(false) // Error
create(undefined) // Error
```