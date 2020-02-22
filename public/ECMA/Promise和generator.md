#### 1-Promise和generator-不完善的包装器
```javascript

function asyncFn(name) {
	return new Promise((resolve) => {
		setTimeout(() => {
			resolve('my name is ' + name)
		}, 1000)
	})
}

function fn1() {
	console.log(asyncFn('leo'))
}
fn1()
//Promise 对象
//
function* fn2() {
	console.log(yield asyncFn('leo')) //调用next()时，运行到yield暂停，返回yield右侧的值。而函数体内默认返回undefined
}

let generatorFn = fn2()

function exec(generatorFn, value) {
	let result = generatorFn.next(value)
	if (!result.done) {
		//如果没有执行完毕
		if (result.value instanceof Promise) {
			//如果返回的是Promise对象
			result.value.then((result) => {
				exec(generatorFn, result)
			})
		} else {
			//如果返回的不是Promise对象//执行到最后时，此时yield返回一个值
			exec(generatorFn, result.value)
		}
	}
}
exec(generatorFn) //my name is leo
```
#### 2-Promise和generator
```javascript

//Promise和generator
function asyncFn(name) {
	return new Promise((resolve) => {
		setTimeout(() => {
			resolve('my name is ' + name)
		}, 1000)
	})
}

function sum(a, b) {
	return new Promise((resolve) => {
		setTimeout(() => {
			resolve(a + b)
		}, 1000)
	})
}

function fn(name) {
	sum(1, 5).then((num) => {
		if (num > 6) {
			asyncFn(name).then((value) => {
				console.log(value)
			})
		} else {
			console.log('error')
		}
	})
}
fn('harry') //error
```
#### 3-最终效果
>generator函数可以在一次 next() 时返回一个Promise对象并停止，
>在返回的Promise对象里处理接受的参数并执行下一个 next()

```javascript
//安装Generator函数的执行器(包装器) co
//npm install --save-dev co
//
let co = require('co')

function asyncF(name) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve('my name is ' + name)
		}, 1000)
	})
}

function sum(a, b) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve(a + b)
		}, 1000)
	})
}

function* fn(name) {
	if ((yield sum(2, 5)) > 6) {
		console.log(yield asyncF(name))
	} else {
		console.log('error')
	}
}

let fnx = co.wrap(fn)
fnx('leo')
//node yield.js
//1秒后终端显示
//my name is leo
```