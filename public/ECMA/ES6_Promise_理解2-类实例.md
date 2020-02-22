#### 1-类

```javascript
'use strict'

class User {
	constructor(name) {
		this.name = name
	}

	send(cb) {
		let name = this.name
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				//模拟异步
				if (name === 'leo') {
					resolve('success')
				} else {
					reject('error')
				}
			}, 500)
		})
	}
}

const user = new User('tom')
user.send()
	.then((result) => {
		//如果执行成功则执行本函数，并接受参数'success'
		console.log(result)
	}).catch((error) => {
		//参数检测不通过，执行reject，本函数，接受参数'error'
		console.log(error)
	})

	//error
```

#### 2-名称密码验证
```javascript
'use strict'

class User {
	constructor(name, password) {
		this.name = name
		this.password = password
	}

	validateName(cb) {
		let name = this.name
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				//模拟异步
				if (name === 'leo') {
					resolve('success')
				} else {
					reject('error')
				}
			}, 500)
		})
	}

	validatePassword(cb) {
		let password = this.password
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				//模拟异步
				if (password === '123') {
					resolve('success')
				} else {
					reject('error')
				}
			}, 500)
		})
	}
}



const user = new User('tom', '123')
user.validateName()
	.then((result) => {
		//如果名称验证通过，标示执行成功，则执行本函数，并接受参数'success'
		console.log(result)
		//执行下一个验证
		return user.validatePassword() //返回一个Promise,下一个then即可以执行
	}).then((result) => {
		//如果密码验证通过，则执行本函数
		console.log(result)
	}).catch((error) => {
		// 任意一个参数检测不通过，执行reject，本函数，接受参数'error'
		console.log(error)
	})

	//error
```

#### 3-可以直接返回一个非Promise
```javascript
'use strict'

class User {
	constructor(name, password) {
		this.name = name
		this.password = password
	}

	validateName(cb) {
		let name = this.name
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				//模拟异步
				if (name === 'leo') {
					resolve('success')
				} else {
					reject('error')
				}
			}, 500)
		})
	}

	validatePassword(cb) {
		let password = this.password
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				//模拟异步
				if (password === '123') {
					resolve('success')
				} else {
					reject('error')
				}
			}, 500)
		})
	}
}



const user = new User('leo', '123')
user.validateName()
	.then((result) => {
		//如果名称验证通过，标示执行成功，则执行本函数，并接受参数'success'
		console.log(result)
		//可以直接返回一个非Promise
		return 'name ok'
               //会自动封装成 Promise.resolve('name ok') 返回，供下一个then调用
	}).then((result) => {
		//接受上一个的返回值
		console.log(result)
	}).catch((error) => {
		// 任意一个参数检测不通过，执行reject，本函数，接受参数'error'
		console.log(error)
	})

	// success
	// name ok
```
#### 4-异常处理1
```javascript
'use strict'

class User {
	constructor(name, password) {
		this.name = name
		this.password = password
	}

	validateName(cb) {
		let name = this.name
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				//模拟异步
				if (name === 'leo') {
					resolve('success')
				} else {
					reject('error-name')
				}
			}, 500)
		})
	}

	validatePassword(cb) {
		let password = this.password
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				//模拟异步
				if (password === '123') {
					resolve('success')
				} else {
					reject('error')
				}
			}, 500)
		})
	}
}



const user = new User('tom', '123')
user.validateName()
	.then((result) => {
		//如果名称验证通过，标示执行成功，则执行本函数，并接受参数'success'
		console.log(result)

		throw new Error('1-error')
		//没有处理.validateName()的异常，该函数不执行，所以下一个then也不执行
		return user.validatePassword()
	}).then((result) => {
		//接受上一个的返回值
		console.log(5,result)
		throw new Error('2-error')
	}).catch((error) => {
		// 任意一个参数检测不通过，执行reject，本函数，接受参数'error'
		//名称验证通过时
		//处理第一个then的错误
		console.log(error)
	})

	// error-name
```
#### 5-异常处理2
```javascript
'use strict'

class User {
	constructor(name, password) {
		this.name = name
		this.password = password
	}

	validateName(cb) {
		let name = this.name
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				//模拟异步
				if (name === 'leo') {
					resolve('success')
				} else {
					reject('error-name')
				}
			}, 500)
		})
	}

	validatePassword(cb) {
		let password = this.password
		return new Promise((resolve, reject) => {
			setTimeout(() => {
				//模拟异步
				if (password === '123') {
					resolve('success')
				} else {
					reject('error')
				}
			}, 500)
		})
	}
}



const user = new User('tom', '123')
user.validateName()
	.then((result) => {
		//如果名称验证通过，标示执行成功，则执行本函数，并接受参数'success'
		console.log(result)

		throw new Error('1-error')

		return user.validatePassword()
	}, (error) => {
		//名称验证未通过时
		//处理validateName()的错误,可以向下执行
		console.log(error)
	}).then((result) => {
		//接受上一个的返回值
		console.log(5, result)
		throw new Error('2-error')
	}).catch((error) => {
		// 任意一个参数检测不通过，执行reject，本函数，接受参数'error'
		//名称验证通过时
		//处理第一个then的错误
		console.log(error)
	})

	// error-name
	//5 undefined
	//Error: 2-error
  	//  at user.validateName.then.then (<anonymous>:56:9)
```