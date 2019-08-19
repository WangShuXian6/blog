#### HTTP头字段 [wiki](https://zh.wikipedia.org/wiki/HTTP头字段#请求字段)

>HTTP状态码（英语：HTTP Status Code）是用以表示网页服务器超文本传输协议响应状态的3位数字代码。它由 RFC 2616 规范定义的，并得到 RFC 2518、RFC 2817、RFC 2295、RFC 2774 与 RFC 4918 等规范扩展。所有状态码的第一个数字代表了响应的五种状态之一。所示的消息短语是典型的，但是可以提供任何可读取的替代方案。 除非另有说明，状态码是HTTP / 1.1标准（RFC 7231）的一部分。[1]

>HTTP状态码的官方注册表由互联网号码分配局（Internet Assigned Numbers Authority）维护。[2]
微软互联网信息服务 （Microsoft Internet Information Services）有时会使用额外的十进制子代码来获取更多具体信息，[3]但是这些子代码仅出现在响应有效内容和文档中，而不是代替实际的HTTP状态代码。

##### 基本格式
>协议头的字段，是在请求（request）或响应（response）行（一条消息的第一行内容）之后传输的。协议头的字段是以明文的字符串格式传输，是以冒号分隔的键名与键值对，以回车(CR)加换行(LF)符号序列结尾。协议头部分的结尾以一个空白字段标识，结果就是，也就是传输两个连续的CR+LF。在历史上，很长的行曾经可能以多个短行的形式传输；在下一行的开头，输出一个空格(SP)或者一个水平制表符(HT)，表示它是一个后续行。在如今，这种换行形式已经被废弃[1]。

##### 类型
HTTP 头字段根据实际用途被分为以下 4 种类型：
通用头字段(英语：General Header Fields)
请求头字段(英语：Request Header Fields)
响应头字段(英语：Response Header Fields)
实体头字段(英语：Entity Header Fields)

##### 字段名
>在 RFC 7230、RFC 7231、RFC 7232、RFC 7233、RFC 7234 和 RFC 7235 中，对一组核心字段进行了标准化。有一份对于这些字段的官方的登记册，以及 一系列的补充规范 ，由互联网号码分配局（IANA）维护。各个应用程序也可以自行定义额外的字段名字及相应的值。头字段的永久登记表和临时登记表目前由IANA维护。其他的字段名称和允许的值可以由各应用程序定义。
按照惯例，非标准的协议头字段是在字段名称前加上X-[2]前缀来标识。但这一惯例已在2012年6月被废弃，因为按照这种惯例，非标准字段变成标准字段时会引起很多不方便之处。[3]以前曾经有的使用Downgraded-的限制也在2013年3月被解除。[4]。

##### 字段值[编辑]
>某些字段中可以包含注释内容（例如User-Agent、Server和Via字段中)，这些注释内容可由应用程序忽略[5]。
>很多字段的值中可以包含带有权重的质量（quality，常被简称为Q）的键值对，指定的“重量”会在内容协商的过程中使用[6]。

##### 大小限制
>标准中没有对每个协议头字段的名称和值的大小设置任何限制，也没有限制字段的个数。然而，出于实际场景及安全性的考虑，大部分的服务器、客户端和代理软件都会实施一些限制。例如，Apache 2.3服务器在默认情况下限制每个字段的大小不得超过8190字节，同时，单个请求中最多有100个头字段[7]。

##### 请求字段


协议头字段名 | 说明 | 示例 | 状态
-- | -- | -- | --
Accept | 能够接受的回应内容类型（Content-Types）。参见内容协商。 | Accept: text/plain | 常设
Accept-Charset | 能够接受的字符集 | Accept-Charset: utf-8 | 常设
Accept-Encoding | 能够接受的编码方式列表。参考HTTP压缩。 | Accept-Encoding: gzip, deflate | 常设
Accept-Language | 能够接受的回应内容的自然语言列表。参考 内容协商 。 | Accept-Language: en-US | 常设
Accept-Datetime | 能够接受的按照时间来表示的版本 | Accept-Datetime: Thu, 31 May 2007 20:35:00 GMT | 临时
Authorization | 用于超文本传输协议的认证的认证信息 | Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ== | 常设
Cache-Control | 用来指定在这次的请求/响应链中的所有缓存机制 都必须 遵守的指令 | Cache-Control: no-cache | 常设
Connection | 该浏览器想要优先使用的连接类型[8] | Connection: keep-aliveConnection: Upgrade | 常设
Cookie | 之前由服务器通过 Set- Cookie （下文详述）发送的一个 超文本传输协议Cookie | Cookie: $Version=1; Skin=new; | 常设: 标准
Content-Length | 以 八位字节数组 （8位的字节）表示的请求体的长度 | Content-Length: 348 | 常设
Content-MD5 | 请求体的内容的二进制 MD5 散列值，以 Base64 编码的结果 | Content-MD5: Q2hlY2sgSW50ZWdyaXR5IQ== | 过时的[9]
Content-Type | 请求体的 多媒体类型 （用于POST和PUT请求中） | Content-Type: application/x-www-form-urlencoded | 常设
Date | 发送该消息的日期和时间(按照 RFC 7231 中定义的"超文本传输协议日期"格式来发送) | Date: Tue, 15 Nov 1994 08:12:31 GMT | 常设
Expect | 表明客户端要求服务器做出特定的行为 | Expect: 100-continue | 常设
From | 发起此请求的用户的邮件地址 | From: user@example.com | 常设
Host | 服务器的域名(用于虚拟主机 )，以及服务器所监听的传输控制协议端口号。如果所请求的端口是对应的服务的标准端口，则端口号可被省略。[10] 自超文件传输协议版本1.1（HTTP/1.1）开始便是必需字段。 | Host: en.wikipedia.org:80Host: en.wikipedia.org | 常设
If-Match | 仅当客户端提供的实体与服务器上对应的实体相匹配时，才进行对应的操作。主要作用时，用作像 PUT 这样的方法中，仅当从用户上次更新某个资源以来，该资源未被修改的情况下，才更新该资源。 | If-Match: "737060cd8c284d8af7ad3082f209582d" | 常设
If-Modified-Since | 允许在对应的内容未被修改的情况下返回304未修改（ 304 Not Modified ） | If-Modified-Since: Sat, 29 Oct 1994 19:43:31 GMT | 常设
If-None-Match | 允许在对应的内容未被修改的情况下返回304未修改（ 304 Not Modified ），参考 超文本传输协议 的实体标记 | If-None-Match: "737060cd8c284d8af7ad3082f209582d" | 常设
If-Range | 如果该实体未被修改过，则向我发送我所缺少的那一个或多个部分；否则，发送整个新的实体 | If-Range: "737060cd8c284d8af7ad3082f209582d" | 常设
If-Unmodified-Since | 仅当该实体自某个特定时间已来未被修改的情况下，才发送回应。 | If-Unmodified-Since: Sat, 29 Oct 1994 19:43:31 GMT | 常设
Max-Forwards | 限制该消息可被代理及网关转发的次数。 | Max-Forwards: 10 | 常设
Origin | 发起一个针对 跨来源资源共享 的请求（要求服务器在回应中加入一个‘访问控制-允许来源’（'Access-Control-Allow-Origin'）字段）。 | Origin: http://www.example-social-network.com | 常设: 标准
Pragma | 与具体的实现相关，这些字段可能在请求/回应链中的任何时候产生多种效果。 | Pragma: no-cache | 常设但不常用
Proxy-Authorization | 用来向代理进行认证的认证信息。 | Proxy-Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ== | 常设
Range | 仅请求某个实体的一部分。字节偏移以0开始。参见字节服务。 | Range: bytes=500-999 | 常设
Referer [sic] [11] | 表示浏览器所访问的前一个页面，正是那个页面上的某个链接将浏览器带到了当前所请求的这个页面。 | Referer: http://en.wikipedia.org/wiki/Main_Page | 常设
TE | 浏览器预期接受的传输编码方式：可使用回应协议头 Transfer-Encoding 字段中的值；另外还可用"trailers"（与"分块 "传输方式相关）这个值来表明浏览器希望在最后一个尺寸为0的块之后还接收到一些额外的字段。 | TE: trailers, deflate | 常设
User-Agent | 浏览器的浏览器身份标识字符串 | User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:12.0) Gecko/20100101 Firefox/21.0 | 常设
Upgrade | 要求服务器升级到另一个协议。 | Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11 | 常设
Via | 向服务器告知，这个请求是由哪些代理发出的。 | Via: 1.0 fred, 1.1 example.com (Apache/1.1) | 常设
Warning | 一个一般性的警告，告知，在实体内容体中可能存在错误。 | Warning: 199 Miscellaneous warning | 常设


##### 常见的非标准请求字段


字段名 | 说明 | 示例
-- | -- | --
X-Requested-With | 主要用于标识 Ajax 及可扩展标记语言 请求。大部分的JavaScript框架会发送这个字段，且将其值设置为 XMLHttpRequest | X-Requested-With: XMLHttpRequest
DNT[12] | 请求某个网页应用程序停止跟踪某个用户。在火狐浏览器中，相当于X-Do-Not-Track协议头字段（自 Firefox/4.0 Beta 11 版开始支持）。Safari 和 Internet Explorer 9 也支持这个字段。2011年3月7日，草案提交IETF。[13] 万维网协会 的跟踪保护工作组正在就此制作一项规范。[14] | DNT: 1 (DNT启用)DNT: 0 (DNT被禁用)
X-Forwarded-For[15] | 一个事实标准 ，用于标识某个通过超文本传输协议代理或负载均衡连接到某个网页服务器的客户端的原始互联网地址 | X-Forwarded-For: client1, proxy1, proxy2X-Forwarded-For: 129.78.138.66, 129.78.64.103
X-Forwarded-Host[16] | 一个事实标准 ，用于识别客户端原本发出的 Host 请求头部[17]。 | X-Forwarded-Host: en.wikipedia.org:80X-Forwarded-Host: en.wikipedia.org
X-Forwarded-Proto[18] | 一个事实标准，用于标识某个超文本传输协议请求最初所使用的协议。[19] | X-Forwarded-Proto: https
Front-End-Https[20] | 被微软的服务器和负载均衡器所使用的非标准头部字段。 | Front-End-Https: on
X-Http-Method-Override[21] | 请求某个网页应用程序使用该协议头字段中指定的方法（一般是PUT或DELETE）来覆盖掉在请求中所指定的方法（一般是POST）。当某个浏览器或防火墙阻止直接发送PUT 或DELETE 方法时（注意，这可能是因为软件中的某个漏洞，因而需要修复，也可能是因为某个配置选项就是如此要求的，因而不应当设法绕过），可使用这种方式。 | X-HTTP-Method-Override: DELETE
X-ATT-DeviceId[22] | 使服务器更容易解读AT&T设备User-Agent字段中常见的设备型号、固件信息。 | X-Att-Deviceid: GT-P7320/P7320XXLPG
X-Wap-Profile[23] | 链接到互联网上的一个XML文件，其完整、仔细地描述了正在连接的设备。右侧以为AT&T Samsung Galaxy S2提供的XML文件为例。 | x-wap-profile: http://wap.samsungmobile.com/uaprof/SGH-I777.xml
Proxy-Connection[24] | 该字段源于早期超文本传输协议版本实现中的错误。与标准的连接（Connection）字段的功能完全相同。 | Proxy-Connection: keep-alive
X-Csrf-Token[25] | 用于防止 跨站请求伪造。 辅助用的头部有 X-CSRFToken[26] 或 X-XSRF-TOKEN[27] | X-Csrf-Token: i8XNjC4b8KVok4uw5RftR38Wgp2BFwql


##### 常见的非标准回应字段


字段名 | 说明 | 示例
-- | -- | --
X-XSS-Protection[39] | 跨站脚本攻击 （XSS）过滤器 | X-XSS-Protection: 1; mode=block
Content-Security-Policy, X-Content-Security-Policy, X-WebKit-CSP[40] | 内容安全策略定义。 | X-WebKit-CSP: default-src 'self'
X-Content-Type-Options[41] | 唯一允许的数值为"nosniff"，防止 Internet Explorer 对文件进行MIME类型嗅探。这也对 Google Chrome 下载扩展时适用。[42] | X-Content-Type-Options: nosniff
X-Powered-By[43] | 表明用于支持当前网页应用程序的技术（例如：PHP）（版本号细节通常放置在 X-Runtime 或 X-Version 中） | X-Powered-By: PHP/5.4.0
X-UA-Compatible[44] | 推荐指定的渲染引擎（通常是向后兼容模式）来显示内容。也用于激活 Internet Explorer 中的 Chrome Frame。 | X-UA-Compatible: IE=EmulateIE7X-UA-Compatible: IE=edgeX-UA-Compatible: Chrome=1
X-Content-Duration[45] | 指出音视频的长度，单位为秒。只受Gecko内核浏览器支持。 | X-Content-Duration: 42.666

##### 参见
>[cookie](https://zh.wikipedia.org/wiki/Cookie)