### 应用编程接口 使用 Flask 实现符合 REST 架构的 API

>Web 应用有种趋势，那就是业务逻辑被越来越多地移到客户端，开创出了一种称为富互联网应用（RIA，rich Internet application）的架构。

>在 RIA 中，服务器的主要功能（有时是唯一功能）是为客户端提供数据存取服务。

>在这种模式中，服务器变成了 Web 服务或应用编程接口（API，application programming interface）。

>RIA 可采用多种协议与 Web 服务通信。

>远程过程调用（RPC，remote procedure call）协议，例如 XML-RPC，

>以及由其衍生的简单对象访问协议（SOAP，simplified object access protocol），在几年前比较受欢迎。

>最近，表现层状态转移（REST，representational state transfer）架构崭露头角，成为 Web 应用的新宠，因为这种架构建立在大家熟识的万维网基础之上。

>Flask 是开发 REST 架构 Web 服务的理想框架，因为 Flask 天生轻量

### REST简介
>Roy Fielding 在其博士论文“Architectural Styles and the Design of Network-based Software Architectures”的第 5 章中描述了 Web 服务的 REST 架构方式，并列出了 6 个符合这一架构定义的特征。

##### 客户端 – 服务器
>客户端和服务器之间必须有明确的界线。

##### 无状态
>客户端发出的请求中必须包含所有必要的信息。服务器不能在两次请求之间保存客户端的任何状态。

##### 缓存
>服务器发出的响应可以标记为可缓存或不可缓存，这样出于优化目的，客户端（或客户端和服务器之间的中间服务）可以使用缓存。

##### 接口统一
>客户端访问服务器资源时使用的协议必须一致、定义良好，且已经标准化。这是 REST 架构最复杂的一方面，涉及唯一的资源标识符、资源表述、客户端和服务器之间自描述的消息，以及超媒体（hypermedia）。

##### 系统分层
>在客户端和服务器之间可以按需插入代理服务器、缓存或网关，以提高性能、稳定性和伸缩性。

##### 按需编程
>客户端可以选择从服务器中下载代码，在客户端的上下文中执行。

#### 资源就是一切
##### 资源是 REST 架构风格的核心概念。

>在 REST 架构中，资源是应用中你要着重关注的事物。

>例如，在博客应用中，用户、博客文章和评论都是资源。

##### 每个资源都要使用唯一的 URL 表示。

>对 HTTP 协议来说，资源的标识符是 URL。

>还是以博客应用为例，一篇博客文章可以使用 URL /api/posts/12345 表示，其中 12345 是这篇文章的唯一标识符，使用文章在数据库中的主键表示。

>URL 的格式或内容无关紧要，只要资源的 URL 只表示唯一的一个资源即可。

>某一类资源的集合也要有一个 URL。

>博客文章集合的 URL 可以是 /api/posts/，评论集合的 URL 可以是 /api/comments/。

##### API 
>API 还可以为某一类资源的逻辑子集定义集合 URL。

>例如，编号为 12345 的博客文章，其中的所有评论可以使用 URL /api/posts/12345/comments/ 表示。

>表示资源集合的 URL 习惯在末端加上一个斜线，代表一种“子目录”结构。

***

>注意，Flask 会特殊对待末端带有斜线的路由。

>如果客户端请求的 URL 的末端没有斜线，而唯一匹配的路由末端有斜线，Flask 会自动响应一个重定向，转向末端带斜线的 URL。反之则不会重定向。

#### 请求方法

>客户端应用在建立起的资源 URL 上发送请求，使用请求方法表示期望的操作。

>若要从博客 API 中获取博客文章列表，客户端可以向 http://www.example.com/api/posts/ 发送 GET 请求。

>若要插入一篇新博客文章，客户端可以向同一地址发送 POST 请求，而且请求主体中要包含博客文章的内容。

>若要获取编号为 12345 的博客文章，客户端可以向 http://www.example.com/api/posts/12345 发送 GET 请求。

##### REST式API使用的HTTP请求方法

请求方法 | 目标 | 说明 | HTTP 状态码
-- | -- | -- | --
GET | 单个资源的 URL | 获取目标资源 | 200
GET | 资源集合的 URL | 获取资源的集合（如果服务器实现了分页，还可以是一页中的资源） | 200
POST | 资源集合的 URL | 创建新资源，并将其加入目标集合。服务器为新资源指派 URL，并在响应的  Location首部中返回 | 201
PUT | 单个资源的 URL | 修改一个现有资源。如果客户端能为资源指派 URL，还可用来创建新资源 | 200 或 204
DELETE | 单个资源的 URL | 删除一个资源 | 200 或 204
DELETE | 资源集合的 URL | 删除目标集合中的所有资源 | 200 或 204

>REST 架构不要求必须为一个资源实现所有的请求方法。

>如果资源不支持客户端使用的请求方法，响应的状态码为 405（不允许使用的方法）。Flask 会自动处理这种错误。

>请求方法不止 GET、POST、PUT 和 DELETE。HTTP 协议还定义了其他方法，

>例如 HEAD 和 OPTIONS，这些方法由 Flask 自动实现。


#### 请求和响应主体

>在请求和响应的主体中，资源在客户端和服务器之间来回传送，但 REST 没有指定编码资源的方式。请求和响应中的 Content-Type 首部用于指明主体中资源的编码方式。使用 HTTP 协议的内容协商机制，可以找到一种客户端和服务器都支持的编码方式。

>REST 式 Web 服务常用的两种编码方式是 JavaScript 对象表示法（JSON，JavaScript object notation）和可扩展标记语言（XML，extensible markup language）。

>对基于 Web 的 RIA 来说，JSON 更具吸引力，因为 JSON 比 XML 简洁，而且 JSON 与 Web 浏览器使用的客户端脚本语言 JavaScript 联系紧密

>一篇博客文章对应的资源可以使用如下的 JSON 表示：

```JSON
{
    "self_url": "http://www.example.com/api/posts/12345",
    "title": "Writing RESTful APIs in Python",
    "author_url": "http://www.example.com/api/users/2",
    "body": "... text of the article here ...",
    "comments_url": "http://www.example.com/api/posts/12345/comments"
}
```

>self_url、author_url 和 comments_url 字段都是完整的资源 URL。这是很重要的表示方法，因为客户端可以通过这些 URL 发掘新资源。

>在设计良好的 REST 式 API 中，客户端只需知道几个顶级资源的 URL，其他资源的 URL 则从响应中包含的链接上发掘。

>这就好比浏览网络时，你在自己知道的网页中点击链接发掘新网页一样。

#### 版本

>服务器可以随时更新 Web 浏览器中的客户端，但无法强制更新智能手机中的应用，因为更新前先要获得机主的许可。即便机主想更新，也不能保证每个智能手机都更新到服务器端部署的新版本了。

>Web 服务的容错能力要比一般的 Web 应用强，而且还要保证旧版客户端能继续使用。更新 Web 服务一定要格外小心，倘若破坏了向后兼容性，如果客户端没有更新到新版，现有的客户端将无法使用。这一问题的常见解决办法是使用版本区分 Web 服务所处理的 URL。例如，首次发布的博客 Web 服务可以通过 /api/v1/posts/ 提供博客文章的集合。

>在 URL 中加入 Web 服务的版本号有助于组织化管理新旧功能，让服务器能为新客户端提供新功能，同时继续支持旧版客户端。博客服务可能会修改博客文章使用的 JSON 格式，通过 /api/v2/posts/ 提供修改后的博客文章，而客户端仍能通过 /api/v1/posts/ 获取旧的 JSON 格式。

>提供多版本支持会增加服务器的维护负担，但在某些情况下，这是不破坏现有部署且能让应用不断发展的唯一方式。等到所有客户端都升级到新版之后，可以弃用旧版服务，待时机成熟后再把旧版完全删除。

### 使用Flask实现REST式Web服务

>使用 Flask 创建 REST 式 Web 服务十分简单。

>使用熟悉的 route() 装饰器及其 methods 可选参数可以声明服务所提供资源 URL 的路由。

>处理 JSON 数据同样简单，请求中的 JSON 数据可以通过 request.get_json() 转换成字典格式，而且可以使用 Flask 提供的辅助函数 jsonify()，从 Python 字典中生成需要包含 JSON 的响应。

#### 创建API蓝本

>REST 式 API 相关的路由是应用中一个自成一体的子集。

>因此，为了更好地组织代码，最好把这些路由放到独立的蓝本中

>API 蓝本的结构

```
|-flasky
  |-app/
    |-api
      |-__init__.py
      |-users.py
      |-posts.py
      |-comments.py
      |-authentication.py
      |-errors.py
      |-decorators.py
```

>如果以后需要创建一个向前兼容的 API 版本，可以再添加一个带版本号的包，让应用同时支持两个版本的 API。

>在这个 API 蓝本中，各资源分别在不同的模块中实现。

>蓝本中还包含处理身份验证、错误以及提供自定义装饰器的模块。

#### app/api/__init__.py：API 蓝本的构造文件
```py
from flask import Blueprint

api = Blueprint('api', __name__)

from . import authentication, posts, users, comments, errors
```

>这个蓝本的包构造文件与其他蓝本的类似。一定要导入蓝本中的所有模块，这样才能注册路由和错误处理程序。因为很多模块要导入 api 包，所以相关模块在底部导入，以防循环依赖导致出错。

#### app/__init__.py：注册API 蓝本
```py
def create_app(config_name):
    # ...
    from .api import api as api_blueprint
    app.register_blueprint(api_blueprint, url_prefix='/api/v1')
    # ...
```

>注册 API 蓝本时指定了一个 URL 前缀，因此蓝本中所有路由的 URL 都将以 /api/v1 开头。

>注册蓝本时设置前缀是个好主意，这样就无须在蓝本的每个路由中硬编码版本号了。

#### 错误处理

>REST 式 Web 服务将请求的状态告知客户端时，会在响应中发送适当的 HTTP 状态码，并将额外信息放入响应主体。

##### API返回的常见HTTP状态码


HTTP状态码 | 名称 | 说明
-- | -- | --
200 | OK（成功） | 请求成功
201 | Created（已创建） | 请求成功，而且创建了一个新资源
202 | Accepted（已接收） | 请求已接收，但仍在处理中，将异步处理
204 | No Content（没有内容） | 请求成功处理，但是返回的响应没有数据
400 | Bad Request（坏请求） | 请求无效或不一致
401 | Unauthorized（未授权） | 请求未包含身份验证信息，或者提供的凭据无效
403 | Forbidden（禁止） | 请求中发送的身份验证凭据无权访问目标
404 | Not Found（未找到） | URL 对应的资源不存在
405 | Method Not Allowed（不允许使用的方法） | 指定资源不支持请求使用的方法
500 | Internal Server Error（内部服务器错误） | 处理请求的过程中发生意外错误

>处理 404 和 500 状态码时会遇到点小麻烦，因为这两个错误是由 Flask 自己生成的，而且一般会返回 HTML 响应。这很可能会让 API 客户端困惑，因为客户端期望所有响应都是 JSON 格式。

>为所有客户端生成适当响应的一种方法是，在错误处理程序中根据客户端请求的格式改写响应，这种技术称为内容协商。

>改进后的 404 错误处理程序，它向 Web 服务客户端发送 JSON 格式响应，除此之外则发送 HTML 格式响应。500 错误处理程序的写法类似。

##### app/api/errors.py：使用 HTTP 内容协商机制处理 404 错误

```py
@main.app_errorhandler(404)
def page_not_found(e):
    if request.accept_mimetypes.accept_json and \
            not request.accept_mimetypes.accept_html:
        response = jsonify({'error': 'not found'})
        response.status_code = 404
        return response
    return render_template('404.html'), 404
```

>这个新版错误处理程序检查 Accept 请求首部（解码为 request.accept_mimetypes），根据首部的值决定客户端期望接收的响应格式。

>浏览器一般不限制响应的格式，但是 API 客户端通常会指定。仅当客户端接受的格式列表中包含 JSON 但不包含 HTML 时，才生成 JSON 响应。

##### app/api/errors.py：API 蓝本中 403 状态码的错误处理程序
```py
def forbidden(message):
    response = jsonify({'error': 'forbidden', 'message': message})
    response.status_code = 403
    return response
```

>API 蓝本中的视图函数在必要时可以调用这些辅助函数生成错误响应。

#### 使用Flask-HTTPAuth验证用户身份

>与普通 Web 应用一样，Web 服务也需要保护信息，确保未经授权的用户无法访问。为此，RIA 必须询问用户的登录凭据，并将其传给服务器进行验证。

>REST 式 Web 服务的特征之一是无状态，即服务器在两次请求之间不能“记住”客户端的任何信息。客户端必须在发出的请求中包含所有必要信息，因此所有请求都必须包含用户凭据。

>Flasky 应用当前的登录功能是在 Flask-Login 的帮助下实现的，数据存储在用户会话中。默认情况下，Flask 把会话保存在客户端 cookie 中，因此服务器没有保存任何用户相关信息，都转交给客户端保存。这种实现方式看起来遵守了 REST 架构的无状态要求，但在 REST 式 Web 服务中使用 cookie 有点不现实，因为 Web 浏览器之外的客户端很难提供对 cookie 的支持。鉴于此，在 API 中使用 cookie 并不是一个很好的设计选择。

>REST 架构的无状态要求看起来似乎过于严格，但这并不是随意提出的要求——无状态的服务器伸缩起来更加简单。如果服务器保存了客户端的相关信息，那么必须保证特定客户端发送的请求由同一台服务器处理，或者使用共享存储器存储客户端数据。这两点都难以实现，但是如果服务器是无状态的，这两个问题也就不复存在。

>因为 REST 架构基于 HTTP 协议，所以发送凭据的最佳方式是使用 HTTP 身份验证，基本验证和摘要验证都可以。在 HTTP 身份验证中，用户凭据包含在每个请求的 Authorization 首部中。

>HTTP 身份验证协议很简单，可以直接实现，不过 Flask-HTTPAuth 扩展提供了一个便利的包装，把协议的细节隐藏在装饰器之中，类似于 Flask-Login 提供的 login_required 装饰器。

>安装

```bash
(venv) $ pip install flask-httpauth
```

>若想使用 HTTP 基本验证初始化这个扩展，要创建一个 HTTPBasicAuth 类对象。与 Flask- Login 一样，Flask-HTTPAuth 不对验证用户凭据所需的步骤做任何假设，所需的信息在回调函数中提供

##### 初始化 Flask-HTTPAuth
>app/api/authentication.py：初始化 Flask-HTTPAuth

```py
from flask_httpauth import HTTPBasicAuth

auth = HTTPBasicAuth()

@auth.verify_password
def verify_password(email, password):
    if email == '':
        return False
    user = User.query.filter_by(email = email).first()
    if not user:
        return False
    g.current_user = user
    return user.verify_password(password)
```

>因为这种身份验证方法只在 API 蓝本中使用，所以 Flask-HTTPAuth 扩展只在蓝本包中初始化，而不像其他扩展那样要在应用包中初始化。

>电子邮件和密码使用 User 模型中现有的方法验证。如果登录凭据正确，这个验证回调函数返回 True，否则返回 False。如果请求中没有身份验证信息，Flask-HTTPAuth 也会调用回调函数，把两个参数都设为空字符串。此时，email 的值是一个空字符串，回调函数立即返回 False 以阻断请求。某些应用遇到这种情况时可以返回 True，允许匿名用户访问。这个回调函数把通过身份验证的用户保存在 Flask 的上下文变量 g 中，供视图函数稍后访问。

##### Flask-HTTPAuth 错误处理程序

>如果身份验证凭据不正确，则服务器向客户端返回 401 状态码。默认情况下，Flask- HTTPAuth 自动生成这个状态码，但为了与 API 返回的其他错误保持一致，我们可以自定义这个错误响应

>app/api/authentication.py：Flask-HTTPAuth 错误处理程序

```py
from .errors import unauthorized

@auth.error_handler
def auth_error():
    return unauthorized('Invalid credentials')
```

##### 保护路由
>若想保护路由，可使用 auth.login_required 装饰器：

```py
@api.route('/posts/')
@auth.login_required
def get_posts():
    pass
```

>不过，这个蓝本中的所有路由都要使用相同的方式进行保护，所以我们可以在 before_request 处理程序中使用一次 login_required 装饰器，将其应用到整个蓝本

>app/api/authentication.py：在 before_request 处理程序中验证身份

```py
from .errors import forbidden

@api.before_request
@auth.login_required
def before_request():
    if not g.current_user.is_anonymous and \
            not g.current_user.confirmed:
        return forbidden('Unconfirmed account')
```

>现在，API 蓝本中的所有路由都能自动验证身份。

>此外，before_request 处理程序还会拒绝已通过身份验证但还没有确认账户的用户。

#### 基于令牌的身份验证  itsdangerous
>每次请求，客户端都要发送身份验证凭据。

>为了避免总是发送敏感信息（例如密码），我们可以使用一种基于令牌的身份验证方案。

>在基于令牌的身份验证方案中，客户端先发送一个包含登录凭据的请求，通过身份验证后，得到一个访问令牌。这个令牌可以代替登录凭据对请求进行身份验证。出于安全考虑，令牌有过期时间。令牌过期后，客户端必须重新发送登录凭据，获取新的令牌。令牌短暂的使用期限，可以降低令牌落入他人之手所导致的安全隐患。为了生成和核查身份验证令牌，我们要在 User 模型中定义两个新方法。这两个新方法用到了 itsdangerous 包

##### app/models.py：支持基于令牌的身份验证

```py
class User(db.Model):
    # ...
    def generate_auth_token(self, expiration):
        s = Serializer(current_app.config['SECRET_KEY'],
                       expires_in=expiration)
        return s.dumps({'id': self.id}).decode('utf-8')

    @staticmethod
    def verify_auth_token(token):
        s = Serializer(current_app.config['SECRET_KEY'])
        try:
            data = s.loads(token)
        except:
            return None
        return User.query.get(data['id'])
```

>generate_auth_token() 方法使用编码后的用户 id 字段值生成一个签名令牌，还指定了以秒为单位的过期时间。

>verify_auth_token() 方法接受的参数是一个令牌，如果令牌有效就返回对应的用户。verify_auth_token() 是静态方法，因为只有解码令牌后才能知道用户是谁。

##### 改进核查回调，支持令牌
>为了能够使用令牌验证请求，我们必须修改 Flask-HTTPAuth 提供的 verify_password 回调，

>除了普通的凭据之外，还要接受令牌

>app/api/authentication.py：改进核查回调，支持令牌

```py
@auth.verify_password
def verify_password(email_or_token, password):
    if email_or_token == '':
        return False
    if password == '':
        g.current_user = User.verify_auth_token(email_or_token)
        g.token_used = True
        return g.current_user is not None
    user = User.query.filter_by(email=email_or_token).first()
    if not user:
        return False
    g.current_user = user
    g.token_used = False
    return user.verify_password(password)
```

>在这个新版本中，第一个参数可以是电子邮件地址，也可以是身份验证令牌。

>如果这个参数为空，那就和之前一样，假定是匿名用户。

>如果密码为空，那就假定 email_or_token 参数提供的是令牌，按照令牌的方式进行验证。

>如果两个参数都不为空，那么假定使用常规的邮件地址和密码进行验证。

>在这种实现方式中，基于令牌的身份验证是可选的，由客户端决定是否使用。

>为了让视图函数能区分这两种身份验证方法，我们添加了 g.token_used 变量。

##### 生成身份验证令牌
>把身份验证令牌发送给客户端的路由也要添加到 API 蓝本中

>app/api/authentication.py：生成身份验证令牌

```py
@api.route('/tokens/', methods=['POST'])
def get_token():
    if g.current_user.is_anonymous or g.token_used:
        return unauthorized('Invalid credentials')
    return jsonify({'token': g.current_user.generate_auth_token(
        expiration=3600), 'expiration': 3600})
```
>因为这个路由也在蓝本中，所以添加到 before_request 处理程序上的身份验证机制也会用在这个路由上。

>为了确保这个路由使用电子邮件地址和密码验证身份，而不使用之前获取的令牌，我们检查了 g.token_used 的值，拒绝使用令牌验证身份。

>这样做是为了防止用户绕过令牌过期机制，使用旧令牌请求新令牌。

>这个视图函数返回 JSON 格式的响应，其中包含过期时间为 1 小时的令牌。过期时间也在 JSON 响应中。

#### 资源和JSON的序列化转换
>开发 Web 服务时，经常需要在资源的内部表示和 JSON 之间进行转换。

>JSON 是 HTTP 请求和响应使用的传输格式。

>把内部表示转换成传输格式的过程称为序列化

##### 序列化
>app/models.py：把文章转换成 JSON 格式的序列化字典

```py
class Post(db.Model):
    # ...
    def to_json(self):
        json_post = {
            'url': url_for('api.get_post', id=self.id),
            'body': self.body,
            'body_html': self.body_html,
            'timestamp': self.timestamp,
            'author_url': url_for('api.get_user', id=self.author_id),
            'comments_url': url_for('api.get_post_comments', id=self.id),
            'comment_count': self.comments.count()
        }
        return json_post
```

>url、author_url 和 comments_url 字段要分别返回相应资源的 URL，因此它们的值使用 url_for() 生成，所调用的路由即将在 API 蓝本中定义。

>这段代码还说明表示资源时可以使用虚构的属性。comment_count 字段是博客文章的评论数量，并不是模型的真实属性，它之所以包含在这个资源中，是为了便于客户端使用。

>User 模型的 to_json() 方法可以使用类似的方式实现

>app/models.py：把用户转换成 JSON 格式的序列化字典

```py
class User(UserMixin, db.Model):
    # ...
    def to_json(self):
        json_user = {
            'url': url_for('api.get_user', id=self.id),
            'username': self.username,
            'member_since': self.member_since,
            'last_seen': self.last_seen,
            'posts_url': url_for('api.get_user_posts', id=self.id),
            'followed_posts_url': url_for('api.get_user_followed_posts',
                                          id=self.id),
            'post_count': self.posts.count()
        }
        return json_user
```

>为了保护隐私，这个方法没有把用户的某些属性加入响应，例如 email 和 role。这段代码再次说明，提供给客户端的资源表示没必要与数据库模型的内部定义完全一致。

##### 反序列化
>序列化的逆向操作称为反序列化。把 JSON 结构反序列化成模型时面临的问题是，客户端提供的数据可能无效、错误或者多余

>从 JSON 格式数据创建 Post 模型实例

>app/models.py：从 JSON 格式数据创建一篇博客文章

```py
from app.exceptions import ValidationError

class Post(db.Model):
    # ...
    @staticmethod
    def from_json(json_post):
        body = json_post.get('body')
        if body is None or body == '':
            raise ValidationError('post does not have a body')
        return Post(body=body)
```

>上述代码在实现过程中只选择使用 JSON 字典中的 body 属性，忽略了 body_html 属性，因为只要 body 属性的值发生变化，就会触发一个 SQLAlchemy 事件，自动在服务器端渲染 Markdown。除非允许客户端指定过去或未来的日期（这个应用并不支持该功能），否则无须使用 timestamp 属性。

>因为客户端无权选择博客文章的作者，所以没有使用 author_url 字段。

>author_url 字段唯一能使用的值是通过身份验证的用户。

>comments_url 和 comment_count 属性使用数据库关系自动生成，因此其中没有创建文章所需的有用信息。

>最后，url 字段也被忽略了，因为在这个实现中资源的 URL 由服务器指派，而不是客户端。

##### ValidationError 异常
>注意检查错误的方式。如果没有 body 字段或者其值为空，那么抛出 ValidationError 异常。

>在这种情况下，抛出异常才是处理错误的正确方式，

>因为 from_json() 方法并没有掌握处理问题的足够信息，唯有把错误交给调用者，由上层代码处理这个错误。

>ValidationError 类是 Python 中 ValueError 类的简单子类

>app/exceptions.py：ValidationError 异常

```py
class ValidationError(ValueError):
    pass
```

##### 全局异常处理 errorhandler
>应用需要处理这个异常，向客户端提供适当的响应。

>为了避免在视图函数中编写捕获异常的代码，可以使用 Flask 的 errorhandler 装饰器注册一个全局异常处理程序。

>app/api/errors.py：API 中 ValidationError 异常的处理程序

```py
@api.errorhandler(ValidationError)
def validation_error(e):
    return bad_request(e.args[0])
```

>这里使用的 errorhandler 装饰器与注册 HTTP 状态码处理程序时使用的是同一个，只不过此时接收的参数是 Exception 类，只要抛出了指定类的异常，就会调用被装饰的函数。

>注意，这个装饰器从 API 蓝本中调用，所以只有处理蓝本中的路由时抛出了异常才会调用这个处理程序。

>这样做，视图函数中的代码就可以写得十分简洁明了，而且无须检查错误。

```py
@api.route('/posts/', methods=['POST'])
def new_post():
    post = Post.from_json(request.json)
    post.author = g.current_user
    db.session.add(post)
    db.session.commit()
    return jsonify(post.to_json())
```

#### 实现资源的各个端点
>实现处理不同资源的路由。GET 请求往往是最简单的，因为它们只返回信息，而不做任何改动

##### app/api/posts.py：文章资源 GET 请求的处理程序
```py
@api.route('/posts/')
def get_posts():
    posts = Post.query.all()
    return jsonify({ 'posts': [post.to_json() for post in posts] })

@api.route('/posts/<int:id>')
def get_post(id):
    post = Post.query.get_or_404(id)
    return jsonify(post.to_json())
```

>第一个路由处理获取文章集合的请求。这个函数使用列表推导生成所有文章的 JSON 版本。

>第二个路由返回单篇博客文章，如果在数据库中没找到指定 id 对应的文章，则返回 404 错误。

##### 文章资源 POST 请求的处理程序
>博客文章资源的 POST 请求处理程序把一篇新博客文章插入数据库
>app/api/posts.py：文章资源 POST 请求的处理程序

```py
@api.route('/posts/', methods=['POST'])
@permission_required(Permission.WRITE)
def new_post():
    post = Post.from_json(request.json)
    post.author = g.current_user
    db.session.add(post)
    db.session.commit()
    return jsonify(post.to_json()), 201, \
        {'Location': url_for('api.get_post', id=post.id)}
```

>这个视图函数包含在 permission_required 装饰器（下一个示例定义）中，确保通过身份验证的用户有写博客文章的权限。

>得益于前面实现的错误处理程序，创建博客文章的过程变得很直观。

>博客文章从 JSON 数据中创建，其作者就是通过身份验证的用户。

>这个模型写入数据库之后，返回 201 状态码，并把 Location 首部的值设为刚创建的这个资源的 URL。

>注意，为便于客户端操作，响应的主体中包含了新建的资源。如此一来，客户端就无须在创建资源后再立即发起一个 GET 请求以获取资源。

##### permission_required 装饰器
>用来防止未授权用户创建新博客文章的 permission_required 装饰器与应用中使用的类似，但要针对 API 蓝本做些定制

>app/api/decorators.py：permission_required 装饰器

```py
def permission_required(permission):

    def decorator(f):

        @wraps(f)
        def decorated_function(*args, **kwargs):
            if not g.current_user.can(permission):
                return forbidden('Insufficient permissions')
            return f(*args, **kwargs)
        return decorated_function
    return decorator
```

##### 文章资源 PUT 请求的处理程序
>博客文章 PUT 请求的处理程序用于更新现有资源

>app/api/posts.py：文章资源 PUT 请求的处理程序

```py
@api.route('/posts/<int:id>', methods=['PUT'])
@permission_required(Permission.WRITE)
def edit_post(id):
    post = Post.query.get_or_404(id)
    if g.current_user != post.author and \
            not g.current_user.can(Permission.ADMIN):
        return forbidden('Insufficient permissions')
    post.body = request.json.get('body', post.body)
    db.session.add(post)
    db.session.commit()
    return jsonify(post.to_json())
```

>本例中要进行的权限检查更为复杂。

>检查用户是否有写博客文章的权限通过装饰器实现，但为了确保用户能编辑博客文章，这个函数还要保证用户是文章的作者或管理员。此项检查直接添加到视图函数中。

>如果这种检查要应用于多个视图函数，为避免代码重复，最好的做法是定义装饰器。

>因为应用不允许删除文章，所以没必要实现 DELETE 请求方法的处理程序。

>用户资源和评论资源的处理程序实现方式类似

##### Flasky应用的API资源


资源URL | 方法 | 说明
-- | -- | --
/users/<int:id> | GET | 返回一个用户
/users/<int:id>/posts/ | GET | 返回一个用户发布的所有博客文章
/users/<int:id>/timeline/ | GET | 返回一个用户所关注用户发布的所有文章
/posts/ | GET | 返回所有博客文章
/posts/ | POST | 创建一篇博客文章
/posts/<int:id> | GET | 返回一篇博客文章
/posts/<int:id> | PUT | 修改一篇博客文章
/posts/<int:id/>comments/ | GET | 返回一篇博客文章的评论
/posts/<int:id/>comments/ | POST | 在一篇博客文章中添加一条评论
/comments/ | GET | 返回所有评论
/comments/<int:id> | GET | 返回一条评论

#### 分页大型资源集合
>对大型资源集合来说，获取集合的 GET 请求消耗很大，而且难以管理

##### app/api/posts.py：分页文章资源
```py
@api.route('/posts/')
def get_posts():
    page = request.args.get('page', 1, type=int)
    pagination = Post.query.paginate(
        page, per_page=current_app.config['FLASKY_POSTS_PER_PAGE'],
        error_out=False)
    posts = pagination.items
    prev = None
    if pagination.has_prev:
        prev = url_for('api.get_posts', page=page-1)
    next = None
    if pagination.has_next:
        next = url_for('api.get_posts', page=page+1)
    return jsonify({
        'posts': [post.to_json() for post in posts],
        'prev_url': prev,
        'next_url': next,
        'count': pagination.total
    })
```

>JSON 格式响应中的 posts 字段依旧包含一系列文章，但现在这只是其中一页，而不是完整的集合。

>prev_url 和 next_url 字段分别是前一页和后一页资源的 URL，如果某个方向没有更多分页了，则相应字段的值为 None。count 是集合中元素的总数。

>这种技术可应用于所有返回集合的路由。

#### 使用HTTPie测试Web服务
>测试 Web 服务必须使用 HTTP 客户端。

>在命令行中测试 Web 服务最常使用的两个客户端是 cURL 和 HTTPie。这两个工具都很强大，但后者的命令行句法更简洁，可读性也更高，而且为 API 请求做了特别优化

##### 安装 HTTPie

```bash
(venv) $ pip install httpie
```

##### 发起 GET 请求
>假设开发服务器运行在默认地址 http://127.0.0.1:5000 上。在另一个终端窗口中，可按照如下的方式发起 GET 请求：

```bash
(venv) $ http --json --auth <email>:<password> GET \
> http://127.0.0.1:5000/api/v1/posts
HTTP/1.0 200 OK
Content-Length: 7018
Content-Type: application/json
Date: Sun, 22 Dec 2013 08:11:24 GMT
Server: Werkzeug/0.9.4 Python/2.7.3

{
    "posts": [
        ...
    ],
    "prev_url": null
    "next_url": "http://127.0.0.1:5000/api/v1/posts/?page=2",
    "count": 150
}
```

>注意响应中的分页链接。因为这是第一页，所以没有上一页，不过返回了获取下一页的 URL 和总数

##### 发送 POST 请求，添加一篇新博客文章
```bash
(venv) $ http --auth <email>:<password> --json POST \
> http://127.0.0.1:5000/api/v1/posts/ \
> "body=I'm adding a post from the *command line*."
HTTP/1.0 201 CREATED
Content-Length: 360
Content-Type: application/json
Date: Sun, 22 Dec 2013 08:30:27 GMT
Location: http://127.0.0.1:5000/api/v1/posts/111
Server: Werkzeug/0.9.4 Python/2.7.3

{
    "author": "http://127.0.0.1:5000/api/v1/users/1",
    "body": "I'm adding a post from the *command line*.",
    "body_html": "<p>I'm adding a post from the <em>command line</em>.</p>",
    "comments": "http://127.0.0.1:5000/api/v1/posts/111/comments",
    "comment_count": 0,
    "timestamp": "Sun, 22 Dec 2013 08:30:27 GMT",
    "url": "http://127.0.0.1:5000/api/v1/posts/111"
}

```

##### 使用令牌
>如果不想使用用户名和密码验证身份，而是使用令牌，要先向 /api/v1/tokens/ 发送 POST 请求：

```bash
(venv) $ http --auth <email>:<password> --json POST \
> http://127.0.0.1:5000/api/v1/tokens/
HTTP/1.0 200 OK
Content-Length: 162
Content-Type: application/json
Date: Sat, 04 Jan 2014 08:38:47 GMT
Server: Werkzeug/0.9.4 Python/3.3.3

{
    "expiration": 3600,
    "token": "eyJpYXQiOjEzODg4MjQ3MjcsImV4cCI6MTM4ODgyODMyNywiYWxnIjoiSFMy..."
}
```

>在接下来的 1 小时中，可以使用这个令牌访问 API。

>请求时要把用户名字段设为这个令牌，密码字段则留空：

```bash
(venv) $ http --json --auth eyJpYXQ...: GET http://127.0.0.1:5000/api/v1/posts/
```

>令牌过期后，请求会返回 401 错误，指明需要获取新令牌。


