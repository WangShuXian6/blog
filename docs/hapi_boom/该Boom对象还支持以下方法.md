### 该Boom对象还支持以下方法：

### ```reformat(debug) ```
>```error.output```使用其他对象属性进行重建，其中：

>debug - 一个布尔值，当true为时，将保留“内部服务器错误”消息。

>默认为false，表示已删除“内部服务器错误”消息。

>请注意，与结合使用时，Boom对象将返回，但不使用 原型（它们是普通原型或传入的错误原型）。

>这意味着 仅应使用或不通过查看原型或构造信息来测试对象。

>该限制是为了避免操纵非常慢的原型链。

>```trueinstanceof BoomBoomErrorBoominstanceof BoomBoom.isBoom()```
