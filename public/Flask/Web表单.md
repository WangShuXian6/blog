### Web 表单

>Flask 请求对象包含客户端在请求中发送的全部信息，

>对包含表单数据的 POST 请求来说，用户填写的信息通过 request.form 访问。

>尽管 Flask 的请求对象提供的信息足以处理 Web 表单，但有些任务很单调，而且要重复操作。比如，生成表单的 HTML 代码和验证提交的表单数据。

#### Flask-WTF
>Flask-WTF 扩展可以把处理 Web 表单的过程变成一种愉悦的体验。

>这个扩展对独立的 WTForms 包进行了包装，方便集成到 Flask 应用中。

>安装

```bash
(venv) $ pip install flask-wtf
```

#### 配置 Flask-WTF
>Flask-WTF 无须在应用层初始化，但是它要求应用配置一个密钥。

>密钥是一个由随机字符构成的唯一字符串，通过加密或签名以不同的方式提升应用的安全性。

>Flask 使用这个密钥保护用户会话，以防被篡改。

>每个应用的密钥应该不同，而且不能让任何人知道

>配置 Flask-WTF

```py
app = Flask(__name__)

app.config['SECRET_KEY'] = 'hard to guess string'
```

>app.config 字典可用于存储 Flask、扩展和应用自身的配置变量。

>使用标准的字典句法就能把配置添加到 app.config 对象中。

>这个对象还提供了一些方法，可以从文件或环境中导入配置。
>之后将介绍管理大型应用配置的合理方式。

>Flask-WTF 之所以要求应用配置一个密钥，是为了防止表单遭到跨站请求伪造（CSRF，cross-site request forgery）攻击。恶意网站把请求发送到被攻击者已登录的其他网站时，就会引发 CSRF 攻击。Flask-WTF 为所有表单生成安全令牌，存储在用户会话中。令牌是一种加密签名，根据密钥生成。

>为了增强安全性，密钥不应该直接写入源码，而要保存在环境变量中。这一技术在第 7 章介绍。

### 在视图函数中处理表单
>视图函数 index() 有两个任务：一是渲染表单，二是接收用户在表单中填写的数据。

>hello.py：使用 GET 和 POST 请求方法处理 Web 表单

```py
@app.route('/', methods=['GET', 'POST'])
def index():
    name = None
    form = NameForm()
    if form.validate_on_submit():
        name = form.name.data
        form.name.data = ''
    return render_template('index.html', form=form, name=name)
```

>app.route 装饰器中多出的 methods 参数告诉 Flask，在 URL 映射中把这个视图函数注册为 GET 和 POST 请求的处理程序。

>如果没指定 methods 参数，则只把视图函数注册为 GET 请求的处理程序

>局部变量 name 用于存放表单中输入的有效名字，如果没有输入，其值为 None。如上述代码所示，我们在视图函数中创建了一个 NameForm 实例，用于表示表单。提交表单后，如果数据能被所有验证函数接受，那么 validate_on_submit() 方法的返回值为 True，否则返回 False。这个函数的返回值决定是重新渲染表单还是处理表单提交的数据。

>用户首次访问应用时，服务器会收到一个没有表单数据的 GET 请求，所以 validate_on_submit() 将返回 False。此时，if 语句的内容将被跳过，对请求的处理只是渲染模板，并传入表单对象和值为 None 的 name 变量作为参数。用户会看到浏览器中显示了一个表单。

>用户提交表单后，服务器会收到一个包含数据的 POST 请求。validate_on_submit() 会调用名字字段上依附的 DataRequired() 验证函数。如果名字不为空，就能通过验证，validate_on_submit() 返回 True。现在，用户输入的名字可通过字段的 data 属性获取。在 if 语句中，把名字赋值给局部变量 name，然后再把 data 属性设为空字符串，清空表单字段。因此，再次渲染这个表单时，各字段中将没有内容。最后一行调用 render_template() 函数渲染模板，但这一次参数 name 的值为表单中输入的名字，因此会显示一个针对该用户的欢迎消息。

### 重定向和用户会话
>前一版 hello.py 存在一个可用性问题。用户输入名字后提交表单，然后点击浏览器的刷新按钮，会看到一个莫名其妙的警告，要求在再次提交表单之前进行确认。

>之所以出现这种情况，是因为刷新页面时浏览器会重新发送之前发送过的请求。

>如果前一个请求是包含表单数据的 POST 请求，刷新页面后会再次提交表单。多数情况下，这并不是我们想执行的操作，因此浏览器才要求用户确认。

>很多用户不理解浏览器发出的这个警告。鉴于此，最好别让 Web 应用把 POST 请求作为浏览器发送的最后一个请求。

##### Post / 重定向 /Get 模式

>这种需求的实现方式是，使用重定向作为 POST 请求的响应，而不是使用常规响应。

>重定向是一种特殊的响应，响应内容包含的是 URL，而不是 HTML 代码的字符串。

>浏览器收到这种响应时，会向重定向的 URL 发起 GET 请求，显示页面的内容。

>这个页面的加载可能要多花几毫秒，因为要先把第二个请求发给服务器。

>除此之外，用户不会察觉到有什么不同。

>现在，前一个请求是 GET 请求，所以刷新命令能像预期的那样正常运作了。

>这个技巧称为 Post / 重定向 /Get 模式。

>但这种方法又会引起另一个问题。应用处理 POST 请求时，可以通过 form.name.data 获取用户输入的名字，然而一旦这个请求结束，数据也就不见了。因为这个 POST 请求使用重定向处理，所以应用需要保存输入的名字，这样重定向后的请求才能获得并使用这个名字，从而构建真正的响应。

##### session 用户会话

>应用可以把数据存储在用户会话中，以便在请求之间“记住”数据。

>用户会话是一种私有存储，每个连接到服务器的客户端都可访问。

>用户会话是请求上下文中的变量，名为 session，像标准的 Python 字典一样操作。

>默认情况下，用户会话保存在客户端 cookie 中，使用前面设置的密钥加密签名。如果篡改了 cookie 的内容，签名就会失效，会话也将随之失效。

>index() 视图函数的新版本，实现了重定向和用户会话

```py
from flask import Flask, render_template, session, redirect, url_for

@app.route('/', methods=['GET', 'POST'])
def index():
    form = NameForm()
    if form.validate_on_submit():
        session['name'] = form.name.data
        return redirect(url_for('index'))
    return render_template('index.html', form=form, name=session.get('name'))
```

>应用的前一个版本在局部变量 name 中存储用户在表单中输入的名字。这个变量现在保存在用户会话中，即 session['name']，所以在两次请求之间能记住输入的值。

>现在，包含有效表单数据的请求最后会使视图函数调用 redirect() 函数。这是 Flask 提供的辅助函数，用于生成 HTTP 重定向响应。redirect() 函数的参数是重定向的 URL，这里使用的重定向 URL 是应用的根 URL，因此重定向响应本可以写得更简单一些，写成 redirect('/')，不过这里却使用了 Flask 提供的 URL 生成函数 url_for()（参见第 3 章）。

>url_for() 函数的第一个且唯一必须指定的参数是端点名，即路由的内部名称。默认情况下，路由的端点是相应视图函数的名称。在这个示例中，处理根 URL 的视图函数是 index()，因此传给 url_for() 函数的名字是 index。

>最后一处改动位于 render_template() 函数中，现在我们使用 session.get('name') 直接从会话中读取 name 参数的值。与普通的字典一样，这里使用 get() 获取字典中键对应的值，可以避免未找到键时抛出异常。如果指定的键不存在，则 get() 方法返回默认值 None。

### 闪现消息

>请求完成后，有时需要让用户知道状态发生了变化，可以是确认消息、警告或者错误提醒。

>一个典型例子是，用户提交有一项错误的登录表单后，服务器发回的响应重新渲染登录表单，并在表单上面显示一个消息，提示用户名或密码无效。

>Flask 本身内置这个功能,flash() 函数可实现这种效果。

#### flash()

>闪现消息

```py
from flask import Flask, render_template, session, redirect, url_for, flash

@app.route('/', methods=['GET', 'POST'])
def index():
    form = NameForm()
    if form.validate_on_submit():
        old_name = session.get('name')
        if old_name is not None and old_name != form.name.data:
            flash('Looks like you have changed your name!')
        session['name'] = form.name.data
        return redirect(url_for('index'))
    return render_template('index.html',
        form = form, name = session.get('name'))
```

>在这个示例中，每次提交的名字都会和存储在用户会话中的名字进行比较，而会话中存储的名字是前一次在这个表单中提交的数据。如果两个名字不一样，就会调用 flash() 函数，在发给客户端的下一个响应中显示一个消息。

>仅调用 flash() 函数并不能把消息显示出来，应用的模板必须渲染这些消息。最好在基模板中渲染闪现消息，因为这样所有页面都能显示需要显示的消息。Flask 把 get_flashed_messages() 函数开放给模板，用于获取并渲染闪现消息，如示例 4-7 所示。

>templates/base.html：渲染闪现消息

>使用 Bootstrap 提供的 CSS alert 样式渲染警告消息

```py
{% block content %}
<div class="container">
    {% for message in get_flashed_messages() %}
    <div class="alert alert-warning">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        {{ message }}
    </div>
    {% endfor %}

    {% block page_content %}{% endblock %}
</div>
{% endblock %}
```

#### 闪现消息 get_flashed_messages()

>这里使用了循环，因为在之前的请求循环中每次调用 flash() 函数时都会生成一个消息，所以可能有多个消息在排队等待显示。

>get_flashed_messages() 函数获取的消息在下次调用时不会再次返回，因此闪现消息只显示一次，然后就消失了。