### ES6 Promise 理解1-函数

#### 1-普通函数

```javascript
function asyncFun(a, b, cb) {
    setTimeout(function () {
        cb(a + b)
    }, 200)
}
asyncFun(1, 2, function (result) {
    console.log(result)
})
console.log(2)

//2
//3
```
>
#### 2-promise产生的原因，回调嵌套，回调地狱，糟糕的逻辑
```javascript
//promise产生的原因，回调嵌套，回调地狱，糟糕的逻辑
function asyncFun(a, b, cb) {
    setTimeout(function () {
        cb(a + b)
    }, 200)
}
asyncFun(1, 2, function (result) {
    if (result > 2) {
        asyncFun(result, 2, function (result) {
            if (result > 4) {
                asyncFun(result, 2, function (result) {
                    console.log('ok')
                })
            }
        })
    }
})

console.log(2)
```
#### 3-去掉回调函数，用promise优化逻辑
```javascript
function asyncFun(a, b) {
    return new Promise(function (resolve, reject) {
        //有结果时调用resolve，并传入参数
        //发生错误时调用reject，传入错误
        setTimeout(function () {
            resolve(a + b)//传入参数ab之和
        }, 200)
    })
}
asyncFun(1, 2)
    //asyncFun 返回一个promise对象，坑定会执行第一个then()
    .then(function (result) {
        //没有错误时执行第一个函数，并接受参数result
        console.log(1)
    })

console.log(2)

//2
//1
```
#### 4-promise 多次返回Promise
```javascript
function asyncFun(a, b) {
    return new Promise(function (resolve, reject) {
        //有结果时调用resolve，并传入参数
        //发生错误时调用reject，传入错误
        setTimeout(function () {
            resolve(a + b)//传入参数ab之和
        }, 200)
    })
}
asyncFun(1, 2)
    //asyncFun 返回一个promise对象，坑定会执行第一个then()
    .then(function (result) {
        //没有错误时执行第一个函数，并接受参数result
        if (result > 2) {
            console.log(3)
            return asyncFun(result, 2)//再次返回一个promise,以便执行下一个then()
        }
    })
    .then(function (result) {
        if (result > 4) {
            console.log('ok')
        }
    })

console.log(2)

//2
//3
//ok
```
#### 5-promise-捕获异常1
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
asyncFun(1, 'a')
    //promise在内部对所有异常进行捕获，故参数错误时不会显示错误信息
    //必须通过.catch或···捕获异常
    //asyncFun 返回一个promise对象，坑定会执行第一个then()
    .then(function (result) {
        //没有错误时执行第一个函数，并接受参数result
        if (result > 2) {
            console.log(3)
            return asyncFun(result, 2)//再次返回一个promise,以便执行下一个then()
        }
    })
    .then(function (result) {
        if (result > 4) {
            console.log('ok')
        }
    })
    //捕获异常方法之一：：：最终的捕获异常函数
    .catch(function (error) {
		//发生异常，不会执行resolve
        console.log(error)
    })

console.log(2)

//2
// VM414:39 Error: no number
// at <anonymous>:12:11
// at new Promise (<anonymous>)
// at asyncFun (<anonymous>:4:9)
// at <anonymous>:21:1

```
#### 6-promise-捕获异常2
```javascript
//,去掉回调函数，用promise优化逻辑
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
asyncFun(1, 'a')
    //promise在内部对所有异常进行捕获，故参数错误时不会显示错误信息
    //必须通过.catch或每个then的第二个参数（为函数）来捕获异常
    //asyncFun 返回一个promise对象，坑定会执行第一个then()
    .then(function (result) {
        //没有错误时执行第一个函数，并接受参数result
        if (result > 2) {
            console.log(3)
            return asyncFun(result, 2)//再次返回一个promise,以便执行下一个then()
        }
    }, function (error) {
        //发生异常，不会执行resolve,而是执行reject，即本函数
        //单独的捕获异常后，则catch不会再捕获该异常
        console.log('first:', error)
    })
    .then(function (result) {
        if (result > 4) {
            console.log('ok')
        }
    })
    //捕获异常方法之一：：：最终的捕获异常函数
    .catch(function (error) {
        console.log('second:', error)
    })
console.log(2)
//2
// VM414:39 first: Error: no number
// at <anonymous>:12:11
// at new Promise (<anonymous>)
// at asyncFun (<anonymous>:4:9)
// at <anonymous>:21:1

```
