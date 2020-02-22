#### HTTP 请求
>POST

>当使用applicatino/x-www-form-urlencoded 或者 multipart/for-data 作为ContentType的时候，$_POST的取值才会生效。

```javascript
header: {
          // 'Content-Type': 'Application/json;charset=UTF-8'
          'Content-Type':'Application/x-www-form-urlencoded'
        }
```