#### Promise.all-按函数顺序通过一个数组返回各个值，全部成功或者一个失败即返回
```javascript
function asyncFun(a, b) {
	    return new Promise(function (resolve, reject) {
	        //resolve, reject两个参数均为函数
	        //有结果时调用resolve，并传入参数
	        //发生错误时调用reject，传入错误
	        //处理异常
	        if (typeof a !== 'number' || typeof b !== 'number') {
	            //判断参数，不合法则调用reject抛出异常并传入参数，参数为错误信息
	            reject(new Error('no number'))
	        }
	        setTimeout(function () {
	            resolve(a + b)//传入参数ab之和
	        }, 200)
	    })
	}

// let resultList=[]
// asyncFun(1,2)
// 	.then((result)=>{
// 		resultList.push(result)
// 		return asyncFun(2,3)
// 	})
// 	.then((result)=>{
// 		resultList.push(result)
// 	})

//按函数顺序通过一个数组返回各个值，全部成功或者一个失败即返回
let promise=Promise.all([
	asyncFun(1,2),
	asyncFun(2,3),
	asyncFun(3,4)
])

promise.then((result)=>{
	console.log(result)
})

//[3, 5, 7]
```

#### 2-1-Promise.race-其中一个返回或错误即返回
```javascript
function asyncFun(a, b) {
	    return new Promise(function (resolve, reject,time) {
	        //resolve, reject两个参数均为函数
	        //有结果时调用resolve，并传入参数
	        //发生错误时调用reject，传入错误
	        //处理异常
	        if (typeof a !== 'number' || typeof b !== 'number') {
	            //判断参数，不合法则调用reject抛出异常并传入参数，参数为错误信息
	            reject(new Error('no number'))
	        }
	        setTimeout(function () {
	            resolve(a + b)//传入参数ab之和
	        }, time)
	    })
	}

// let resultList=[]
// asyncFun(1,2)
// 	.then((result)=>{
// 		resultList.push(result)
// 		return asyncFun(2,3)
// 	})
// 	.then((result)=>{
// 		resultList.push(result)
// 	})


//其中一个返回或错误即返回
let promise=Promise.race([
	asyncFun(1,2,300),
	asyncFun(2,3,200),
	asyncFun(3,4,100)
])

promise.then((result)=>{
	console.log(result)
})
//3
```
#### 2-2-Promise.race(iterable) 方法返回一个 promise ，并伴随着 promise对象解决的返回值或拒绝的错误原因, 只要 iterable 中有一个 promise 对象"解决(resolve)"或"拒绝(reject)"
```javascript
var promise1 = new Promise(function (resolve, reject) {
	setTimeout(resolve, 500, 'one');
});

var promise2 = new Promise(function (resolve, reject) {
	setTimeout(resolve, 100, 'two');
});

Promise.race([promise1, promise2]).then(function (value) {
	console.log(value);
	// Both resolve, but promise2 is faster
});
// expected output: "two"
```
#### 3-Promise.resolve(value)方法返回一个以给定值解析后的Promise对象
```javascript
//Promise.resolve(value)方法返回一个以给定值解析后的Promise对象
let promise=Promise.resolve('hello')
promise.then((result)=>{
	console.log(result)
})
//hello
//
let promise1=new Promise((resolve,reject)=>{
	resolve('haha')
})
promise1.then((result)=>{
	console.log(result)
})
//haha
```
#### 4-Promise.reject(reason)方法返回一个用reason拒绝的Promise
```javascript
//Promise.reject(reason)方法返回一个用reason拒绝的Promise
let promise2=Promise.reject('error')

//第一个参数不会被执行，可以写成null
promise2.then(null,(error)=>{
	console.log(error)
})
//error
```
>如果执行了某一个then的resolve没有返回值，则默认返回一个 undefined,并传递给下一个then，下一个then()始终会执行

>如果执行了一个then的reject返回一个值或Promise，则可以执行下一个then(),并传递这个值。
>如果执行了一个then的reject，但不返回任何值或抛出异常，则不会执行下一个then(),但可以使用catch()捕获该异常