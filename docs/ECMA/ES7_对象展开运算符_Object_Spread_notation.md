#### 示例1
```javascript
let objectA = {
	a: 1,
	b: 2
}

let objectB = {
	...objectA,
	c: 3
}
console.log(objectB)
//{a: 1, b: 2, c: 3}
```
>...Object. 对象展开操作符，类似数组展开操作符，会将一个对象的一级属性全部取出：

```javascript
let objectB = {
	...objectA,
	c: 3
}
```
>等同于
```javascript
let objectB = {
	a: 1,
	b: 2,
	c: 3
}
```
#### 示例2
```javascript
let objectC = {
	a: 1,
	b: 2
}

let objectD = {
	...objectC,
	b: 9,
	c: 3
}
console.log(objectD)
//{a: 1, b: 9, c: 3}
```
>被展开的对象后面位置的参数都将合并到被展开的对象内返回，并且覆盖同名参数。

>上例等同于

```javascript
let objectC = {
	a: 1,
	b: 2
}

let objectD = {
	a: 1,
	b: 2,
	b: 9,
	c: 3
}
console.log(objectD)
//{a: 1, b: 9, c: 3}
```
>于是后面的参数b将前面的参数b覆盖了。