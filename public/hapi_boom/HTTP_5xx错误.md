#### HTTP 5xx错误
>所有500个错误都会向最终用户隐藏您的消息。


#### ```Boom.badImplementation([message], [data])- （别名：internal）```

>返回500 Internal Server Error错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.badImplementation('terrible implementation 糟糕的实现')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 500,
    "error": "Internal Server Error 内部服务器错误",
    "message": "An internal server error occurred 发生内部服务器错误"
}
```

#### ```Boom.notImplemented([message], [data]) ```

>返回501未实现错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.notImplemented('method not implemented 方法未实现')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 501,
    "error": "Not Implemented 未实现",
    "message": "method not implemented 方法未实现"
}
```

#### ```Boom.badGateway([message], [data]) ```

>返回502 Bad Gateway错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.badGateway('that is a bad gateway 错误的网关')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 502,
    "error": "Bad Gateway 错误的网关",
    "message": "that is a bad gateway 错误的网关"
}
```

#### ```Boom.serverUnavailable([message], [data])```
 
>返回503服务不可用错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.serverUnavailable('unavailable 不可用')```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 503,
    "error": "Service Unavailable 服务不可用",
    "message": "unavailable 不可用"
}
```

#### ```Boom.gatewayTimeout([message], [data]) ```

>返回504网关超时错误，其中：

• message -可选消息。
• data -可选的附加错误数据。

```Boom.gatewayTimeout()```

>生成以下响应有效负载：

```JSON
{
    "statusCode": 504,
    "error": "Gateway Time-out 网关超时"
}
```