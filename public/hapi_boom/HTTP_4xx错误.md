#### HTTP 4xx错误

#### ```Boom.badRequest([message], [data]) ```
>返回一个400 Bad Request错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.badRequest('invalid query 无效查询')```

>生成以下响应有效负载：
```JSON
{
    "statusCode": 400,
    "error": "Bad Request 错误请求",
    "message": "invalid query 无效查询"
}
```


#### ```Boom.unauthorized([message], [scheme], [attributes]) ```
>返回401未经授权的错误，其中：

• message -可选消息。
• scheme 可以是以下之一：
• 认证方案名称
• 字符串值的数组。这些值将由'，'分隔，并设置为'WWW-Authenticate'标头。
• attributes-设置“ WWW-Authenticate”标头时要使用的值的对象。仅当scheme为字符串时才使用此值，否则将被忽略。每个键/值对都将以“ key =“ value””的格式包含在“ WWW-Authenticate”中，并包含在attributes键下的响应有效负载中。替代地，值可以是用于设置方案的值的字符串，例如，设置用于协商标头的令牌值。如果使用字符串，则message参数必须为null。 null并将undefined替换为空字符串。如果attributes设置为，message将用作“ WWW-Authenticate”标头的“错误”段。如果message未设置，则“错误”isMissing
如果设置scheme或attributes，则结果Boom对象将为响应设置“ WWW-Authenticate”标头。

```Boom.unauthorized('invalid password 无效密码')```

>生成以下响应：

```JSON
"payload": {
    "statusCode": 401,
    "error": "Unauthorized 未经授权",
    "message": "invalid password 无效密码"
},
"headers" {}
```

```Boom.unauthorized('invalid password', 'sample')```
```Boom.unauthorized('无效密码', '示例')```


>生成以下响应：

```JSON
"payload": {
    "statusCode": 401,
    "error": "Unauthorized 未经授权",
    "message": "invalid password 无效密码",
    "attributes": {
        "error": "invalid password 无效密码"
    }
},
"headers" {
  "WWW-Authenticate": "sample error=\"invalid password\" 示例错误= \”无效的密码\“"
}
```


```Boom.unauthorized(null, 'Negotiate', 'VGhpcyBpcyBhIHRlc3QgdG9rZW4=')```

>生成以下响应：

```JSON
"payload": {
    "statusCode": 401,
    "error": "Unauthorized 未经授权",
    "attributes": "VGhpcyBpcyBhIHRlc3QgdG9rZW4="
},
"headers" {
  "WWW-Authenticate": "Negotiate VGhpcyBpcyBhIHRlc3QgdG9rZW4="
}
```

```Boom.unauthorized('invalid password', 'sample 示例', { ttl: 0, cache: null, foo: 'bar' })```

>生成以下响应：

```JSON
"payload": {
    "statusCode": 401,
    "error": "Unauthorized 未经授权",
    "message": "invalid password 无效密码",
    "attributes": {
        "error": "invalid password 无效密码",
        "ttl": 0,
        "cache": "",
        "foo": "bar"
    }
},
"headers" {
  "WWW-Authenticate": "sample ttl=\"0\", cache=\"\", foo=\"bar\", error=\"invalid password\""
}
```


#### ```Boom.paymentRequired([message], [data]) ```

>返回402 Payment Required Required错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.paymentRequired('bandwidth used 使用的带宽')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 402,
    "error": "Payment Required 需要付款",
    "message": "bandwidth used 已使用带宽"
}
```


#### ```Boom.forbidden([message], [data]) ```

>返回403禁止错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.forbidden('try again some time 稍后再试')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 403,
    "error": "Forbidden 禁止",
    "message": "try again some time 稍后再试"
}
```

#### ```Boom.notFound([message], [data]) ```

>返回404未找到错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.notFound('missing')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 404,
    "error": "Not Found",
    "message": "missing"
}
```

#### ```Boom.methodNotAllowed([message], [data], [allow]) ```

>返回405方法不允许错误，其中：

• message -可选消息。
• data -可选的附加错误数据。
• allow -可选的字符串或字符串数​​组（由'，'组合和分隔），设置为'Allow'标头。

```Boom.methodNotAllowed('that method is not allowed 不允许使用该方法')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 405,
    "error": "Method Not Allowed 不允许使用该方法",
    "message": "that method is not allowed 不允许使用该方法"
}
```

#### ```Boom.notAcceptable([message], [data]) ```

>返回406不可接受的错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.notAcceptable('unacceptable 不可接受')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 406,
    "error": "Not Acceptable 不可接受",
    "message": "unacceptable 不可接受"
}
```

#### ```Boom.proxyAuthRequired([message], [data]) ```

>返回407所需的Proxy Authentication错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.proxyAuthRequired('auth missing 身份验证缺失')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 407,
    "error": "Proxy Authentication Required 需要代理身份验证",
    "message": "auth missing 缺少身份认证"
}
```


#### ```Boom.clientTimeout([message], [data]) ```

>返回408请求超时错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.clientTimeout('timed out')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 408,
    "error": "Request Time-out 请求超时",
    "message": "timed out 超时"
}
```


#### ```Boom.conflict([message], [data]) ```

>返回409冲突错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.conflict('there was a conflict 有冲突')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 409,
    "error": "Conflict 冲突",
    "message": "there was a conflict 存在冲突"
}
```


#### ```Boom.resourceGone([message], [data]) ```

>返回410 Gone错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.resourceGone('it is gone 它不见了')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 410,
    "error": "Gone 消失",
    "message": "it is gone 它不见了"
}
```


#### ```Boom.lengthRequired([message], [data]) ```

>返回411 Length Required错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.lengthRequired('length needed 所需长度')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 411,
    "error": "Length Required 需要长度",
    "message": "length needed 需要长度"
}
```

#### ```Boom.preconditionFailed([message], [data]) ```

>返回412 Precondition Failed错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.preconditionFailed()```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 412,
    "error": "Precondition Failed 前提失败"
}
```


#### ```Boom.entityTooLarge([message], [data]) ```

>返回413请求实体太大错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.entityTooLarge('too big 太大')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 413,
    "error": "Request Entity Too Large 请求实体太大",
    "message": "too big 太大"
}
```


#### ```Boom.uriTooLong([message], [data]) ```

>返回414请求URI太大错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.uriTooLong('uri is too long')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 414,
    "error": "Request-URI Too Large 请求URI太大",
    "message": "uri is too long 请求URI太大"
}
```


#### ```Boom.unsupportedMediaType([message], [data]) ```

>返回415不支持的媒体类型错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.unsupportedMediaType('that media is not supported 不支持该媒体')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 415,
    "error": "Unsupported Media Type 不支持的媒体类型",
    "message": "that media is not supported 不支持的媒体"
}
```


#### ```Boom.rangeNotSatisfiable([message], [data]) ```

>返回416 Requested Range Not Satisfiable错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.rangeNotSatisfiable()```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 416,
    "error": "Requested Range Not Satisfiable 无法满足请求的范围"
}
```


#### ```Boom.expectationFailed([message], [data]) ```

>返回417 Expectation Failed错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.expectationFailed('expected this to work 预期这可以工作')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 417,
    "error": "Expectation Failed 预期失败",
    "message": "expected this to work 预期这样可以工作"
}
```


#### ```Boom.teapot([message], [data]) ```

>返回418我是茶壶错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.teapot('sorry, no coffee.. 对不起，没有咖啡....')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 418,
    "error": "I'm a Teapot 我是茶壶",
    "message": "Sorry, no coffee... 对不起，没有咖啡..."
}
```


#### ```Boom.badData([message], [data]) ```

>返回422无法处理的实体错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.badData('your data is bad and you should feel bad 您的数据不正确，您应该感到不舒服')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 422,
    "error": "Unprocessable Entity 无法处理的实体",
    "message": "your data is bad and you should feel bad 您的数据不好，您应该感到不好"
}
```


#### ```Boom.locked([message], [data]) ```

>返回423锁定错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.locked('this resource has been locked 此资源已被锁定')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 423,
    "error": "Locked 已锁定",
    "message": "this resource has been locked 此资源已被锁定"
}
```


#### ```Boom.failedDependency([message], [data]) ```

>返回424 Failed Dependency错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.failedDependency('an external resource failed 外部资源失败')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 424,
    "error": "Failed Dependency 依赖失败",
    "message": "an external resource failed 外部资源失败"
}
```


#### ```Boom.preconditionRequired([message], [data]) ```

>返回428 Precondition Required错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.preconditionRequired('you must supply an If-Match heade 您必须提供If-Match标头r')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 428,
    "error": "Precondition Required 需要前提条件",
    "message": "you must supply an If-Match header 您必须提供If-Match标头"
}
```


#### Boom.tooManyRequests([message], [data]) 

>返回429个请求过多错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.tooManyRequests('you have exceeded your request limit 您已超出请求限制')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 429,
    "error": "Too Many Requests 请求过多",
    "message": "you have exceeded your request limit 您已超出请求限制"
}
```


#### ```Boom.illegal([message], [data]) ```

>返回“ 451由于法律原因不可用”错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.illegal('you are not permitted to view this resource for legal reasons 出于法律原因您不得查看此资源')```

>生成以下响应有效负载：


```JSON
{
    "statusCode": 451,
    "error": "Unavailable For Legal Reasons 由于法律原因不可用",
    "message": "you are not permitted to view this resource for legal reasons 由于法律原因您不允许查看该资源"
}
```