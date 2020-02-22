#### es7 async  await

```javascript
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

async function fn() {
	const line = await asyncF('tom')
	const n = await sum(2, 3)
	console.log(line + ':' + n)
}
//2秒后终端显示
//my name is tom:5
```

#### 错误处理

```javascript
test() {
      return new Promise((reslove, reject) => {
        if(true){
          reject('wrong')
        }else{
          reslove('right')
        }
      })
    }



async asyncBegin() {
      let num = await this.test().catch((error)=>{
        console.log(error)
      })
      console.log('num', num)
    }

//wrong
//num undefined
```

#### 串行执行异步

```javascript

test1()
async test1(){
      await this.test2()
      await this.test3()

    }

    async test2(){
      let code=await this.test4()
      console.log(code)
    }

    async test3(){
      let code=await this.test5()
      console.log(code)
    }

    test4(){
      return new Promise((resolove,reject)=>{
        setTimeout(()=>{
          resolove(3000)
        },3000)
      })
    }

    test5(){
      return new Promise((resolove,reject)=>{
        setTimeout(()=>{
          resolove(2000)
        },2000)
      })
    }

// 3000
// 2000
```