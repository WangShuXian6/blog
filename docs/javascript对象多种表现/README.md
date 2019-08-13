#### 对象参数的函数值
```javascript
let a={b:()=>console.log(1)}
a.b()
//1
```
```javascript
let d={a(){console.log(3)}}
d.a()
//3
```
```javascript
let e={['a'](){console.log(5)}}
e.a()
//5
e['a']()
//5
e['a']()===e.a()
```
#### 使用中括号解析变量或数值为对象的字面量属性：
##### 数值字面量属性的解析
```javascript
let c = { [1]: 2 }
console.log(c)
//{1: 2}
```
##### 值为数值的变量字面量属性的解析
```javascript
let f = 1
let d = { [f]: 2 }
console.log(d)
//{1: 2}
```
##### 值为字符串的变量字面量属性的解析
```javascript
let e = 'a'
let k = { [e]: 2 }
console.log(k)
//{a: 2}
```
***
##### 别名

```js
let a = { b: 12, d: 5 }

let { d: dd, b: c } = a
// 取出 a.d,别名为 dd
// 取出 a.b,别名为 c
console.log('c', c)

//c 12

console.log('dd', dd)
// 5
```