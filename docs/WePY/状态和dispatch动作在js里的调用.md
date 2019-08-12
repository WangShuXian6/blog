#### 状态和dispatch动作在js里的调用
>混入

```javascript
@connect({
    header(state){
      return state.header
    }
  },{
    select:SELECT_PERSON
  })
```

>dispatch Action

```javascript
this.methods.select(info)
```

>状态

```javascript
console.log(this.header)
```