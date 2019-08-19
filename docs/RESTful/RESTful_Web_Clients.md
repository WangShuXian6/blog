#### RESTful Web Clients：基于超媒体的可复用客户端 笔记
>https://github.com/rwcbook

>我们迫切需要服务端和客户端相互独立演化的能力

>如果服务端与客户端紧密耦合，服务端变更就很可能造成客户端失效。例如，服务端和客户端通过 RPC 的 IDL 来约定服务接口，那么当服务端的变更引起过程名、参数或返回值的变更时，客户端就很难不受其影响。又例如，服务端的 Web Service 遵循一套 URL template 规约，客户端根据服务端提供的规约文档编码，如果服务端改变了资源名称或资源之间的关联关系，也会导致客户端失效。其次，与服务端紧密耦合的客户端显然也无法正常接纳服务端推出的新功能。

>REST 解决的思路就是将可能变化的部分抽象出来，融入服务端响应的消息体中，如果客户端拥有解读这些变化的能力，也就使双方都获得了独立演化的能力。

>客户端需要理解响应中携带的 OAA 的元数据，才能有效处理 OAA 元素的变化。这些元数据如下。

● OBJECT 元数据：服务端返回业务数据的元数据，例如对象中字段的类型、长度甚至语义等，这些元数据能够帮助客户端正确地渲染展现数据。

● ADDRESS 元数据：响应中业务数据的关联操作列表，包括名字、URL、展示名称等，能够指示客户端展现当前可操作的最新功能。

● ACTION 元数据：对 ADDRESS 列表每一项操作的详细描述，包括参数的元数据（与 OBJECT 字段元数据类似）、名字、URL、HTTP 方法、展示名称等，能够指示客户端引导用户正确地发起这个操作。

***
##### REST 的七个基本架构约束，第一个是客户-服务器（Client-Server，CS）
>分离关注点是在客户端-服务器约束背后的原则。

>功能的适当分离会简化服务器组件，从而提高可伸缩性。

>这种简化所采用的形式通常是将所有的用户界面功能（user interface functionality）移到客户组件中。

>只要通信的接口不发生改变，这种分离允许两种类型的组件独立地进化。

#####  RPW 循环
>客户端应用程序应该依赖于 Request、Parse 和 Wait 所构成的循环，我简称其为 RPW 循环。

>这是所有电脑游戏采用的实现方式，也是所有事件驱动接口从窗口式工作站（windowing-style workstation）到响应式机器接口（reactive machine interface）的工作方式。

##### 
>在 HTTP 里，POST 方法定义了一个非幂等的、不安全的操作（RFC7231）

>PUT 或 DELETE 操作（HTTP 里两个幂等而不安全的操作）

>Roy Fielding 在 2009 年的一篇博客（http://g.mamund.com/sstuc）中指出，在 Web 上只用 GET 和 POST 当然可能完成所有事情，但如果一些操作是幂等的，事情会变得更容易一点，因为这样使得处理回复失败的请求更容易

>使用非幂等的 POST 动作的确会引入轻微的并发症，在一些边界条件下，用户会因为不确信一个 POST 动作是否成功而试图第二次触发动作，这种情况下，服务器有责任防止 add 等操作的两次提交。

***
>实现 Web API 时 CRUD 模式最为常用。

>不匹配 CRUD 的操作使得 API 设计稍微有些复杂。

>我们通常会创建一个独特的 URL（例如，/tasks/assign-user 或/user/change-pw/）来处理这些特殊操作，并且执行 HTTP POST 请求，将若干参数传递给服务器。

***
>从客户端向万维网的服务器传递数据的最常见方法是通用 HTML 表单（FORM）媒体类型（application/x-www-form-urlencoded），但是这种方法还是有局限的，它只能从客户端向服务器传递简单的名值对。

>JSON 比 FORM 表单数据稍微灵活一点，因为 JSON 能够在一个请求里传递任意嵌套的树状数据。不过，在这里的实现中，我们将采用经典的 JSON 字典方法。

```

POST /task/ HTTP/1.1
            content-type: application/json
              ..
            {
              "title" : "This is my job",
              "completeFlag" : "false"
            }
```

***
#### TPS Web API 定义了以下事项：

>● 每一个 RPC 端点的 URL 和 HTTP 方法。

>● 传递给服务的数据的参数与格式。

>● 返回给客户端的 JSON 对象。


>表 1-4 Task 对象属性

![Task 对象属性](https://user-images.githubusercontent.com/30850497/56468684-3d540580-6462-11e9-9bcd-57a4a25c9515.jpg)

>表 1-5 User 对象属性

![User 对象属性](https://user-images.githubusercontent.com/30850497/56468685-45ac4080-6462-11e9-9873-33af14911b1c.jpg)


```JSON
 /* TaskList */
            {
              "task": [
                {
                  "id": "dr8ar791pk",
                  "title": "compost",
                  "completeFlag": false,
                  "assignedUser": "mamund"
                }
                ...更多tasks...
              ]
            }
```
```JSON
            /* UserList */
            {
              "user": [
                {
                  "nick": "lee",
                  "name": "Lee Amundsen",
                  "password": "p@ss"
                }
                ...更多user记录...
              ]
            }
```


***
##### 改变
##### JSON Web API

##### 基于 JSON 的 RPC-CRUD Web API
>https:// github.com/RWCBook/json-crud

>在 CRUD 风格的 API 中，

>Edit 操作由 HTTP PUT 方法处理，

>而 Remove 操作由 HTTP DELETE 操作处理。

```
...
            case'POST':
              if(parts[1] && parts[1].indexOf(‘？')===-1) {
                switch(parts[1].toLowerCase()) {
                  /* Web API 不再支持使用POST的update和remove ❶
                          case "update":
                          updateTask(req, res, respond, parts[2]);
                          break;
                  case "remove":
                          removeTask(req, res, respond, parts[2]);
                          break;
                  */
                  case "completed":
                          markCompleted(req, res, respond, parts[2]);
                          break;
                  case "assign":
                          assignUser(req, res, respond, parts[2]);
                          break;
                  default:
                          respond(req, res,
                          utils.errorResponse(req, res,'Method Not Allowed', 405)
                          );
                }
              }
              else {
                addTask(req, res, respond);
              }
              break;
              /* 添加对使用PUT的update的支持 */❷
            case'PUT':
              if(parts[1] && parts[1].indexOf(‘？')===-1) {
                updateTask(req, res, respond, parts[1]);
              }
              else {
                respond(req, res,
                utils.errorResponse(req, res,'Method Not Allowed', 405)
                );
              }
              break;
              /* 添加对使用DELETE的remove的支持 */❸
            case'DELETE':
              if(parts[1] && parts[1].indexOf(‘？')===-1) {
                removeTask(req, res, respond, parts[1]);
            }
            else {
              respond(req, res,
              utils.errorResponse(req, res,'Method Not Allowed', 405)
              );
            }
            break;
            ...

```

>用 cURL 测试 TPS Web API

>展示 Task 端点的所有的 CRUD 操作，以及 TaskMarkCompleted 这个特殊操作：

```bash
/ /创建一条新的任务记录
curl -X POST -H "content-type:application/json" -d'{"title":"testing"}' http://localhost:8181/task/
 // 获取最新创建的任务记录
curl http://localhost:8181/task/1z4yb9wjwi1
 // 更新现有的任务记录
curl -X PUT -H "content-type:application/json" -d'{"title":"testing again"}'  http://localhost:8181/task/1z4yb9wjwi1
 // 将任务标记为已经完成
curl -X POST -H "content-type:application/json" http://localhost:8181/task/completed/1z4yb9wjwi1
 // 删除任务记录
curl -X DELETE -H "content-type:application/json" http://localhost:8181/task/1z4yb9wjwi1
```

***

#### JSON Web API 客户端
>识别响应中的 OBJECTS

>构建和服务交互的 ADDRESSES（即 URL）

> 处理诸如过滤、编辑和删除数据之类的 ACTIONS

##### OAA 挑战
>OAA（OBJECTS、ADDRESSES、ACTIONS）

##### Objects

>Twitter 的 API 概述页面列出了 5 个基准对象：

>用户（User）

> 推文（Tweet）

> 实体（Entity）

> 对象中的实体（Entity in Object）

> 位置（Place）

##### ADDRESSES，即它们的 URL

>大多数 JSON RPC-CRUD API 的响应都不包含 URL，即客户端所处理的对象和数组的地址。

>URL 的信息会被写在易于阅读的文档中，并由开发人员撰写文档的详细信息。

>通常，这会导致在应用中对 URL 进行硬编码（或者使用 URL 模板），

>在运行时也需要将这些地址和对象与集合进行关联，

>甚至在应用中实际使用前，还需要处理 URL 模板中的所有参数。

>编码 URL 到 JSON Web API 客户端中

```
actions.task = {
              tasks: {href:"/task/", prompt:"All Tasks"},
              active: {href:"/task/？completeFlag=false", prompt:"Active Tasks"},
              closed: {href:"/task/？completeFlag=true", prompt:"Completed Tasks"},
            }
```

##### Actions

>过滤器和针对某个 Web API 的写入操作，是 Web API 客户端所需要处理的第三个重要元素。

>比如，Twitter 用于更新现有列表的 API 共有 7 个参数

>将 action 信息编码到客户端时，有多种方式可选。比如，

>直接硬编码，

>或者把它作为元数据单独存放到本地的文件中，

>甚至还可以用 JSON 格式保存到远程配置文件中。

>对于你的客户端需要执行的所有 action 来说，这类信息的处理都是必不可少的。

```ts
actions.user = {
                add: {
                  href:"/user/",
                  prompt:"Add User",
                  method:"POST",
                  args:{
                          nick: {
                              value:"",
                              prompt:"Nickname",
                              required:true,
                              pattern:"[a-zA-Z0-9]+"
                          },
                  password: {
                              value:"",
                              prompt:"Password",
                              required:true,
                              pattern:"[a-zA-Z0-9!@#$%^&*-]+"
                          },
                  name: {
                              value:"",
                              prompt:"Full Name",
                              required:true
                  }
                }
              }
            }
```

###### 每个 JSON API 客户端也需要知道如何处理以下几个方面：
> 每个 API 中独一无二的 OBJECTS。

> API 中所有 action 的 ADDRESSES（URL 和 URL 模板）。

>ACTION 的细节，包括 HTTP POST、PUT 和 DELETE 请求在内的所有重要行为的 HTTP 方法和参数。

>对于客户端需要理解的对象属性，我们可以将它们存储在一个简单的数组中：

```ts
            g.fields.task = ["id","title","completeFlag","assignedUser"];
            g.fields.user = ["nick","name","password"];
```
>如此一来，无论客户端何时处理响应中的对象，它将会把对象的属性和它自己的属性列表做对比，并忽略掉那些它所不知道的属性。

>关于上述工作机制，我们可以看下面的这段用于处理对象在屏幕上渲染的相关代码：

```ts
// g = global storage
            // d = domHelper library
            // 处理item集合
            function items() {
              var rsp, flds, elm, coll;
              var ul, li, dl, dt, dd, p;
              rsp = g.rsp[g.context];❶
              flds = g.fields[g.context];
              elm = d.find("items");
              d.clear(elm);
              ul = d.node("ul");
              coll = rsp;
              for (var item of coll) {
                li = d.node("li");
                dl = d.node("dl");
                dt = d.node("dt");
                // 发送数据元素
                dd = d.node("dd");
                for (var f of flds) {❷
                  p = d.data({
                          text: f,
                          value: item[f]
                  });
                  d.push(p, dd);
                                }
                d.push(dt, dl, dd, li, lu);
                                }
                d.push(ul, elm);
            }
```

>Addresses 和 Actions

>在我们客户端的内部，有一个被称为 action 的集合，用来存储地址（URL）和 action（HTTP 方法和参数信息）。

>与此同时，关于每一个 action，也有一些额外的元数据，用于指示何时它需要被渲染（基于上下文信息），以及应该如何被执行（比如作为一个简单的 link、表单或是直接调用 HTTP 方法）。

```ts
// 任务上下文动作
            global.actions.task = {
              tasks: { target: "app",
                func: httpGet,
                href: "/task/",
                prompt: "Tasks"
              },❶
              active: {
                target: "list",
                func: httpGet,
                href: "/task/？completeFlag=false",
                prompt: "Active Tasks"
              },❷
              byTitle: {
                target: "list",
                func: jsonForm,
                href: "/task",
                prompt: "By Title",
                method: "GET",
                args: {
              title: {
                value: "",
                prompt: "Title",
                required: true
              }
              }
            },
            add: {
            target: "list",
            func: jsonForm,
            href: "/task/",❸
            prompt: "Add Task",
            method: "POST",
            args: {
              title: {
                value: "",
                prompt: "Title",
                required: true
              },
              completeFlag: {
                value: "",
              prompt: "completeFlag"
              }
            }
            },
            item: {
              target: "item",
              func: httpGet,
              href: "/task/{id}",
              prompt: "Item"
            },
            edit: {
              target: "single",
              func: jsonForm,
              href: "/task/{id}",❹
              prompt: "Edit",
              method: "PUT",
              args: {
            id: {
              value: "{id}",
              prompt: "Id",
              readOnly: true
            },
            title: {
              value: "{title}",
              prompt: "Title",
              required: true
            },
              completeFlag: {
              value: "{completeFlag}",
              prompt: "completeFlag"
            }
            }
            },
              del: {
                target: "single",
                func: httpDelete,
                href: "/task/{id}",❺
                prompt: "Delete",
                method: "DELETE",
                args: {}
              },
            };

```
>在上面的代码片段中，你可以看到:

>一个简单、安全且只读的 action（❶），

>和一个安全且需要用户输入的 action（❷）。

>当然，也有一些典型的 CRUD action（❸❹❺），并带有期望的 HTTP 方法名称、提示和参数列表（在某些情况下）等。

>客户端会基于运行时的上下文来选择这些 action 定义。

>举例来说，针对不同级别的上下文，target 属性指明了各级别所适用的 action，包括应用级、列表级、元素级，甚至是单个元素级等。


>通过使用上下文信息和扫描行为列表，来渲染列表级链接的。

```ts
// d = domHelper library
            // 处理列表级 action
            function actions() {
              var actions;
              var elm, coll;
              var ul, li, a;
              elm = d.find("actions");
              d.clear(elm);
              ul = d.node("ul");
              actions = g.actions[g.context];❶
              for (var act in actions) {
                link = actions[act];
                if (link.target === "list") {❷
                  li = d.node("li");
                  a = d.anchor({
                          href: link.href,
                          rel: "collection",
                          className: "action",
                          text: link.prompt
                  });
                  a.onclick = link.func;
                  a.setAttribute("method", (link.method || "GET"));
                  a.setAttribute("args", (link.args ？ JSON.stringify(link.args) : "{}"));
                  d.push(a, li);
                  d.push(li, ul);
                }
              }
              d.push(ul, elm);
            }
在上面的示例代码中你可以看到，在选择适合当前显示的链接时，便是基于对象上下文（❶）和内部渲染上下文（❷）的。

而那些包含参数细节的行为，则会在运行时通过 HTML 中的 ＜form＞ 元素（如图 2-5 所示）来渲染。相关代码如下所示：

            // d = domHelper library
            //渲染inputs
            coll
            = JSON.parse(link.getAttribute("args"));❶
            for (var prop in coll) {
              val = coll[prop].value;
              if (rsp[0][prop]) {
                val = val.replace("{" + prop + "}", rsp[0][prop]);❷
              }
              p = d.input({❸
                prompt: coll[prop].prompt,
                name: prop,
                value: val,
                required: coll[prop].required,
                readOnly: coll[prop].readOnly,
                pattern: coll[prop].pattern
              });
              d.push(p, fs);
            }
```
***

>在较为典型的 JSON API 中，为了能让客户端和服务端相互保持同步，需要它们共享一份 OBJECT、ADDRESS 和 ACTION 的 profile 文件。

>而这意味着，服务中每添加一个新特性都需要一个新版本的客户端。

###### JSON API 客户端的几个关键方面。可以概括为以下几点：

>在服务的 model 中处理关键的 OBJECTS。

> 构建并管理服务 ADDRESSES 或 URLS。

>理解所有 ACTIONS 的元数据，比如参数、HTTP 方法以及输入的规则。

>这个客户端其实使用了一个简单的循环模式：

>发送请求，解析响应，并基于上下文信息将信息渲染到屏幕上以供用户处理。

>只支持纯 JSON 格式的客户端在应对 OAA 挑战时的表现不太好。

>这些客户端在处理第三方元素时，要么崩溃，要么直接忽略。

>无论何时，只要添加、删除或修改任何一个 OBJECT、ADDRESS 和 ACTION，都需要对这些客户端重新进行编码、部署。

***

#### RESTful Web Clients：基于超媒体的可复用客户端 笔记2

#### 消息格式

>有时服务的实现方式将内部的对象模型与外部的消息格式紧密地绑定起来，这意味着内部模型的变更会泄露到外部格式中，有可能会破坏消费服务 API 的客户端应用。

>在处理在 API 消费者和 API 提供者之间传递的消息时，需要面对的另一个重要的挑战——应该怎样防止被破坏

***

##### XML
>为了处理业务计算，人们付出一系列努力来修订基于 XML 的协议，

>包括 SOAP 协议和许多后来被称为 SOA（面向服务架构）风格的协议。

##### JSON
> JavaScript 对象标记格式（JavaScript Object Notation Format），或者叫 JSON

##### CSV
>另一种在多方之间传递数据的有价值的格式是逗号分隔的值（CSV）格式

#### 超媒体格式
>从 2010 年开始出现的格式的新分支不仅提供了数据的结构，

>还包括了如何操作这些数据的指引。

>我称这些格式为超媒体格式，

>它们代表了 API 发展的另一种趋势，是一种有价值的工具，

>对于创建基于 API 的服务，它能支持大范围的变更而不破坏已经存在的客户端。

>在某些情况下，客户端应用程序甚至不需要重写和重新部署客户端代码，就能魔法般地自动获取新的特性和行为。

##### Atom 
>一个基于 XML 的格式，特意设计将读/写的语义增加到格式中。

>换言之，它像 HTML 一样描述如何添加、修改、删除服务器数据的规则。

##### 超媒体应用语言（HAL）
>HAL 格式于 2011 年由 Mike Kelly 在互联网名称与地址管理局（IANA）注册。

>HAL 格式被称为「 一种提供一致而简单的方法来超链接资源的简单格式」，

>它的设计聚焦于如何标准化这类方法，以便在消息内描述和分享链接。

>HAL 并不描述写语义，但的确利用了 URI 模板规范（RFC6570）来描述内联的查询细节。

##### Collection+JSON 格式（Cj）
>和 HAL 不同，Cj 支持一种通用 CRUD 内联语义的详细描述和一种描述输入元数据和错误的方法。
>它基本上是一个 Atom 发布协议的 JSON 格式版本的分支，
>聚焦于通用的列表管理的用例。

##### 表述实体的结构化接口（Siren）
>Siren「是一种超媒体格式，用与实体相关联的属性、子实体和动作来表示实体」。

>它拥有非常丰富的语义模型来支持众多 HTTP 动词，

>目前被用作 Zetta 物联网平台的默认格式。

##### 表述交换的通用基础（UBER）
>UBER 没有很强的消息结构，

>相反它只有一个元素（称为数据）用来在一个文档里表示所有类型的内容。

>它同时有 JSON 和 XML 两种变种。

>UBER 还未在 IANA 注册

>不存在一种 API 格式可以搞定一切

***
#### 表述器（Representor）模式

##### 分离格式处理，
>将格式支持的工作从 API 的功能中分离出来。 要确保你能持续地设计和实现基本的 API 功能

>不要紧密绑定 API 和消息格式，这对安全简单地支持多格式大有裨益

##### 实现一种一致的方式将领域数据转换为消息格式，
>将内部的领域数据（例如数据图表和动作细节）转换为消息格式和能够适应多种格式的一致算法。

>这需要某些软件模式的实现，以及处理领域模型和输出模型之间小于 100%保真度的能力。

>我们每天都这样处理 HTML（HTML 对于对象和强类型一无所知），

>也需要采用类似的方法来处理通用 API 格式。

##### 确认请求元数据以帮助我们选择合适的格式
>需要为每一个收到的请求选择适当格式的机制。

>同样的，这个机制应该是可以一致实现的算法，

>它最好依赖于 HTTP 请求里已经存在的信息，

>而不会引入客户端需要支持的新的定制元数据。
***
##### 从功能中分离格式
>将所有格式（包括 SOAP XML 输出）独立于内部对象模型是重要的，

>这样才能实现在内部对象发生改变时不必改变外部消息格式。

>为了管理这个分离，我们需要使用一些模块化，

>将领域模型转换为外部的消息的工作从操作领域对象本身的工作中剥离出来

>模块被认为是一种职责分配，而不是一个子程序。

>模块化包括那些在独立模块开始工作之前必须完成的设计决策。

>审视将内部领域数据转换为外部消息格式的工作，

>职责分配引导我们将转换过程隔离到它自己的模块。

>现在我们可以把这个模块与操作领域数据的模块分开。 

>清晰分离的一个简单例子可能看起来是这样的：
```ts
var convert = new ConversionModule(HALConverter);
var output = convert.toMessage(domainData);
```
>在上面的伪代码中，转换过程通过一个 ConversionModule()的实例来访问，

>这个实例接收一个与消息关联的转换器（在这个例子里是 HAL 转换器），

>然后使用 toMessage()函数接收 domainData 实例，产生希望的输出结果。

>内部领域模型的功能从外部格式中干净地分离出来后，

>我们还需要一些指导，用一致的方式将领域模型转换成需要的消息格式。

>但在这之前，我们需要一个选择合适格式的模式。

##### 内容协商（Content-Negotiation）
>HTTP 协议规范（RFC7231）第 3.4 节描述了内容协商的用来「表述信息」的两种模式：

>主动式

>服务端根据客户端的偏好来选择表述格式。

>被动式

>服务端向客户端提供可能的表述列表，客户端选择一个偏好的格式。

>常用的模式是主动式

>客户端将发送一个 Accept 的 HTTP 头部，

>这个头部包含一个或多个格式偏好的列表，

>服务器会用这个列表来决定哪一个格式将会在响应中使用

>（在服务器 Content-Type 的 HTTP 头部包括被选择的格式标识符）

>客户请求可能是
```
GET /users HTTP/1.1
            Accept: application/vnd.hal+json, application/vnd.uber+json
            ...
```

>对于一个支持 HAL 而不支持 UBER 的服务，响应应该是：
```
HTTP/1.1 200 OK
            Content-Type: application/vnd.hal+json
            ...
```

>客户端可以在接收列表中包括若干种媒体类型——甚至可以是「*/*」条目（表示「我能接受每一个类型」）。

>HTTP 标准中 Accept 头部还包括了被称为「q」的参数，可以限定 accept 列表中每一个条目的优先级。

>这个参数合法值是一个范围从 0.001（最不优先的条目）到 1（最优先的条目）的数字
```
GET /users/ HTTP/1.1
            application/vnd.uber+json;q=0.3,
            application/vnd.hal+json;q=1
```



##### 服务端内部
##### 适配和翻译
>架构的概念可以表达为一套通用模式

>三类模式：

>创建模式

>结构模式

> 行为模式

##### 适配器（Adapter）模式
>Adapter 结构模式可以帮我们安全、廉价又简单地实现 API 多消息格式。

>在 OODesign.com 建立的条目中，适配器模式的意图是：

>将一个类的接口转化为另外一个客户端期望的接口。

>Adapter 让多个类能够一起工作，如果没有 Adapter，这些类会因为接口不兼容而不能一起工作。

>这基本上就是我们需要做的——将一个内部类（或模型）转化为一个外部类（或消息）。

###### Adapter 模式有 4 个参与方

>Target

>定义客户端使用的领域特定的接口。这将是我们输出结果使用的消息模型或媒体类型。

>Client

>和符合 target 接口的对象相互协作。在我们的例子里，Client 是 API 服务——使用 Adapter 模式的 App。

>Adaptee

>定义一个已经存在的需要适配的接口。这是我们需要转化为目标消息模型的内部模型。

>Adapter

>将 Adaptee 接口适配到 target 接口。这会是我们编写的特定媒体类型的插件，将内部模型转换到消息模型。

![Adapter 模式](https://user-images.githubusercontent.com/30850497/56470352-c45fa880-6477-11e9-8a9b-9fc1fa96abbe.jpg)

>需要为每一个目标媒体类型（HTML、HAL、Siren、Collection+JSON，等等）编写 Adapter 插件。
>这不算很难，麻烦在于每个内部对象模型（adaptee）都会不同。
>例如，先写一个处理 User 的插件，然后再写处理 Task 的插件，以此类推。
>这意味着编写 adapter 需要很多的代码——甚至更令人失望的是——这些 adapter 也许并不是很容易复用的。

***
##### 消息翻译器模式
>基于 Adapter 模式的消息翻译器（Message Translator）。

>在其他过滤器或应用中使用一个特殊的过滤器，即 Message Translator，将一个数据格式翻译为另外一个。

>消息翻译器是 GoF 系列模式中 adapater 类的一种特殊形式。

>为了试图减少紧耦合适配器的代码

>将内部对象模型转换到外部消息模型的过程标准化。

>通用消息格式——Web 服务变迁语言（WeSTL）。

>它将作为一个标准的 adaptee，让我们可以用一种通用化的方式编写 Adapter 插件。

>现在翻译过程可以转变为一个不依赖于任何指定领域信息的算法。

>编写 WeSTL 到 HAL、WeSTL 到 Cj，或 WeSTL 到 Siren 的翻译器，

>然后剩下的唯一工作就是将内部模型转换为 WeSTL 消息。

>这样做虽然只是将复杂性移到了另外一个位置，但却有助于减少为支持多格式编写的定制代码。

***
##### 服务端模型
>表述器实现的高层细节

>实现一个表述器意味着处理以下挑战：

> 检查 HTTP 请求，识别当前请求可以接受的消息格式。

> 使用这个数据来确定哪一个消息格式将被使用。

> 将领域数据转换为目标消息格式。

>处理 HTTP Accept 头部参数

>这个实现有很大的局限性，它并不支持让服务器通过 q 值来更好地理解客户端的偏好，而该服务对 API 响应的默认类型则是 text/html。这两个假设在后续的编码中可以修改或完善

```ts
//基本的accept头部参数的处理
            var csType = ‘’;
            var htmlType = ‘text/html’
            var csAccept = req.headers[‘accept’];
            if( !csAccept || csAccept === ‘*/*’){
              csType = htmlType;
            }
            else {
              csType = csAccept.split(‘,’)[0];
            }
```

>实现消息翻译器模式

```ts
//加载表述器 ❶
            var html = require(‘./representors/html.js’);
            var haljson = require(‘./representors/haljson.js’);
            var collectionJson = require(‘./representors/cj.js’);
            var siren = require(‘./reqresentors/siren.js’);
            function processData(domainData, mimeType) {
              var doc;
              // 没提供线索？ 假设使用HTML ❷
              if (!mimeType) {
                mimeType = “text/html”;
              }
              // 分配给被要求的表述器 ❸
              switch (mimeType.toLowerCase()) {
                case “application/vnd.hal+json”:
                  doc = haljson(object);
                  break;
                case “application/vnd.collection+json”:
                  doc = collectionJson(object);
                  break;
                case “application/vnd.siren+json”:
                  doc = siren(object);
                  break;
                case “text/html”:
                default: // ❹
                  doc = html(object);
                  break;
              }
              return doc;
            }
```
>如果 mimeType 值没被传过来（或者不合法），那么 mimeType 会自动地被设置为 text/html。 这有点防御性编程的意味

>创建所有翻译器都能理解的通用格式，即 WeSTL 格式。

##### 通用表述器模块
>在消息翻译模式中，每一个格式模块（html()、haljson()等）都是翻译器的一个具体实例。

>虽然将这些模块作为领域特定的转换器（如 userObjectToHTML、userObjectToHAL 等）来实现可以满足我们当前的需要，但随着时间的推移，这种方法很难规模化应用。

>相反，我们需要一个没有任何领域特定知识的、通用目的的翻译器。

>例如，用来处理 user 领域数据的翻译器，也可以用于处理 customer 和 accounting 或任何其他领域特定的领域数据。

>为了做到这一点，我们需要创建一个将领域数据传递给格式模块的通用接口，这个通用接口是独立于任何单一的领域模型的。

##### WeSTL（发音为 wehs』-tul）
>WeSTL 让 API 服务开发者可以使用标准化的消息模型，将内部对象模型转换为外部消息格式。

>这减少了精心制作新 Translator 的成本（在时间和精力上），

>并且把内部模型转换到消息的复杂度推到单一的实例中——将内部模型转换为 WeSTL

>WeSTL 规范（http://rwcbook.github.io/wstlspec/）

>相关联的 GitHub 在线仓库（http://github.com/RWCBook/wstl-spec）。

>当用 WeSTL 设计和实现 Web API 的时候，服务开发者搜集了所有可能的状态变迁并且用 WeSTL 模型来描述它们。

> 通过状态变迁，解释了在任何服务响应中出现的所有链接和表单。

>例如，每一个响应可能会有一个指向首页的链接，一些响应会有 HTML 风格的输入表单，允许 API 客户端创建新的服务数据或修改已经存在的数据。

>还有一些服务响应甚至列举出这个服务所有的可能链接和表单（状态变迁）！

>WeSTL 聚焦于造成状态改变发生的动作，这使得 WeSTL 正好适用于创建消息翻译器

>WeSTL 用来描述变迁的一个简单例子

>设计时 WeSTL

>这个文档并没有定义 Web 资源，也没有约束这些变迁会在何处（甚至何时）出现，

>服务开发者会在其他地方的代码中处理这一工作。

>所以，这就是 WeSTL 模型在「设计时」的样子，即服务尚未启动和运行之前。

>通常，服务设计者会用这种方式来使用 WeSTL

```JSON
{
    "wstl": {
        "transitions": [
            {
                "name": "home",
                "type": "safe",
                "action": "read",
                "prompt": "Home Page"
            },
            {
                "name": "user-list",
                "type": "safe",
                "target": "user list",
                "prompt": "List of Users"
            },
            {
                "name": "change-password",
                "type": "unsafe",
                "action": "append",
                "inputs": [
                    {
                        "name": "userName",
                        "prompt": "User",
                        "readOnly": true
                    },
                    {
                        "name": "old-password",
                        "prompt": "Current Password",
                        "required": true
                    },
                    {
                        "name": "new-password",
                        "prompt": "New Password (5-10 chars)",
                        "required": true,
                        "pattern": "^[a-zA-Z0-9]{5,10}$"
                    }
                ]
            }
        ]
    }
}
```

>运行时 WeSTL

>WeSTL 模型的实例在运行时被创建时，只包含某个特定资源的合法的变迁。

>此外，该运行时实例还会包含与这个 Web 资源关联的所有数据。

>换言之，在运行时的 WeSTL 模型反映了资源的当前状态——既包括可用的数据也包括适合的变迁。

>创建运行时 WeSTL 模型的代码

>userResource()函数先是拉取了当前资源相关的数据——在这个案例里是基于 URL 中的 ID 值的单个用户记录（❶），然后从设计时期的 WeSTL 模型中拉取了三个迁移（❷），最后组装一个运行期 WeSTL 模型，将数据、迁移和有用的标题字符串结合起来（❸）。

```ts
var transitions = require(‘./wstl-designtime.js’);
            var domainData = require(‘./domain.js’);
            function userResource(root) {
              var doc, coll, data;
              data = [];
              coll = [];
              // 拉取资源数据 ❶
              data = domain.getData(‘user’,root.getID());
            // 为资源添加变迁 ❷
              tran = transitions(“home”);
              tran.href = root +”/home/”;
              tran.rel = [“http:”+root+”/rels/home”];
              coll.splice(coll.length, 0, tran);
              tran = transitions(“user-list”);
              tran.href = root +”/user/”;
              tran.rel = [“http:”+root+”/rels/collection”];
              coll.splice(coll.length, 0, tran);
              tran = transitions(“change-password”);
              tran.href = root +”/user/changepw/{id}”;
              tran.rel = [“http:”+root+”/rels/changepw”];
              coll.splice(coll.length, 0, tran);
              // 组装wstl模型 ❸
              doc = {};
              doc.wstl = {};
              doc.wstl.title = “User Management”;
              doc.wstl.transitions = coll;
              doc.wstl.data = data;
              return doc;
            }
```

>将运行期 WeSTL 文档转换为 HAL 表述的高层代码

>你可以看到有一个循环来处理传入的 WeSTL 对象（和一些次要的清理代码一起），定位和渲染 HAL 的_links 部分（❶），并且，最后处理任何 HAL 的_embedded 内容

```ts
/ 发送合法的hal消息体
            function haljson(object, root, relRoot) {
              var hal;
              hal = {};
              hal._links = {};
              root = root.replace(/^\/\//,"http://");
              for(var o in object) {
                rels = relRoot||root+"/files/hal-"+o.toLowerCase()+"-{rel}";
                hal._links = getLinks(object[o], root, o, rels); ❶
                if(object[o].content) {
                  hal.content = object[o].content;
                }
                if(object[o].related) {
                  hal.related = object[o].related;
                }
                if(object[o].data && object[o].data.length===1) {
                  hal = getProperties(hal, object[o]); ❷
                }
                else {
                  hal._embedded = getEmbedded(object[o], root, o, rels); ❸
                }
              }
              return JSON.stringify(hal, null, 2);
            }
```
>唯一需要指出的约束是在这个模型中 wstl.data 元素必须是一个数组，它可以是一个 JSON 属性（如名值对）数组、一个 JSON 对象数组，甚至是包含一个本身是高度嵌套的图的 JSON 对象数组。WeSTL 文档甚至可以包含一个描述 data 元素的模式（schema） （JSON Schema、RelexNG 等）。这些相关的 schema 信息能被格式模块用来帮助定位和处理 data 元素的内容。

>所以，WeSTL 允许服务开发者用一种通用的方法来定义 Web 资源。首先，服务设计者可以创建设计时的 WeSTL 文档，描述所有服务的可能迁移；然后，服务开发者可以将这个设计时文档作为原材料，构建运行时 WeSTL 文档，包含被选择的迁移以及关联的运行时数据。


>表述器的范例

>顶层函数（haljson()）接收一个 WeSTL 模型以及某个运行时请求级别的数据（❶）作为第一个参数，这个函数遍历了 WeSTL 运行时实例，首先处理模型中所有链接（即迁移，❷），然后处理 WeSTL 实例中的所有名值对。在这一切处理完毕后，将结果 JOSN 对象（现在是一个合法的 HAL 文档）返回给调用者

```ts
function haljson(wstl, root, rels) { ❶
              var hal;
              hal = {};
              hal._links = {};
              for(var o in wstl) {
                hal._links = getLinks(wstl[o], root, o, rels);
                if(wstl[o].data && wstl[o].data.length===1) {
                  hal = getProperties(hal, wstl[o]);
                }
              }
              return JSON.stringify(hal, null, 2); ❹
            }
            // 发送_links对象 ❷
            function getLinks(wstl, root, o, relRoot) {
              var coll, items, links, i, x;
              links = {};
              // list级别的动作
              if(wstl.actions) {
              coll = wstl.transitions;
              for(i=0,x=coll.length;i＜x;i++) {
                links = getLink(links, coll[i], relRoot);
              }
              // list级别的对象
              if(wstl.data) {
                coll = wstl.data;
                items = [];
                for(i=0,x=coll.length;i＜x;i++) {
                  item = {};
                  item.href = coll[i].meta.href;
                  item.title = coll[i].title;
                  items.push(item);
                }
                links[checkRel(o, relRoot)] = items;
                }
              }
              return links;
            }
            // 发送根属性 ❸
            function getProperties(hal, wstl) {
              var props;
              if(wstl.data && wstl.data[0]) {
                props = wstl.data[0];
                for(var p in props) {
                  if(p!==’meta’) {
                          hal[p] = props[p];
                  }
                }
              }
              return hal;
            }
            /* 此处放置额外的支撑性函数 */
```

>返回

```
{
              “_links” : { ❶
                “self” : {
                  “href” “: http://localhost:8282/user/mamund”
                },
                “http://localhost:8282/rels/home”: {
                  “href”: “http://localhost:8282/”,
                  “title”: “Home”,
                  “templated”: false
                },
                “http://localhost:8282/rels/collection”: {
                  “href”: “http://localhost:8282/user/”,
                  “title”: “All Users”,
                  “templated”: false
                },
                “http://localhost:8282/rels/changepw”: {
                  “href”: “http://localhost:8282/user/changepw/mamund”,
                  “title”: “Change Password”
                }
              },
              “userName” “: mamund”,❷
              “familyName”: “Amundsen”,
              “givenName”: “Mike”,
              “password”: “p@ss”,
              “webUrl”: “http://amundsen.com/blog/”,
              “dateCreated”: “2015-01-06T01:38:55.147Z”,
              “dateUpdated”: “2015-01-25T02:28:12.402Z”
            }
```