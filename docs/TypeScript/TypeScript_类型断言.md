#### TypeScript 类型断言

>类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。

>类型断言有两种形式

#### “尖括号”语法

```typescript
let someValue: any = "this is a string"

let strLength: number = (<string>someValue).length
```

#### as 语法
>在TypeScript里使用JSX时，只有 as语法断言是被允许的

```typescript
let someValue: any = "this is a string"

let strLength: number = (someValue as string).length
```