#### Installation:
```bash
npm: npm install @hapi/boom

yarn: yarn add @hapi/boom
```

>https://hapi.dev/family/boom/?v=8.0.1#reformatdebug

>https://www.npmjs.com/package/@hapi/boom

>https://github.com/hapijs/boom

>v 8.0.1

#### ```boom```提供了一组用于返回HTTP错误的实用程序。

#### 每个实用程序都返回一个Boom 错误响应对象，该对象包括以下属性：

>```isBoom```-如果true，表示这是一个Boom对象实例。请注意，仅当错误是的实例时，才应使用此布尔值Error。如果不确定，请Boom.isBoom() 改用。

>```isServer``` -方便状态指示状态代码> = 500。

>```message``` -错误消息。

>```typeof```-用于创建错误的构造函数（例如Boom.badRequest）。

>```output```-格式化的响应。可以在构造对象后直接操作以返回自定义错误响应。允许的根密钥：

>>```statusCode``` -HTTP状态代码（通常为4xx或5xx）。

>>```headers``` -包含任何HTTP标头的对象，其中每个键是标头名称，值是标头内容。

>>```payload```-用作响应有效负载的格式化对象（已字符串化）。可以直接操作，但是如果reformat()调用，任何更改都将丢失。允许的任何内容，默认情况下包括以下内容：

>>>```statusCode```-HTTP状态代码，源自error.output.statusCode。

>>>```error```-从派生的HTTP状态消息（例如，“错误请求”，“内部服务器错误”）statusCode。

>>>```message```-从派生的错误消息error.message。


>>继承的Error属性。

***
