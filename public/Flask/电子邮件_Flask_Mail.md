#### 电子邮件 Flask-Mail

>虽然 Python 标准库中的 smtplib 包可用于在 Flask 应用中发送电子邮件，但包装了 smtplib 的 Flask-Mail 扩展能更好地与 Flask 集成

#### 安装

```bash
pip install flask-mail
```

>Flask-Mail 连接到简单邮件传输协议（SMTP，simple mail transfer protocol）服务器，把邮件交给这个服务器发送。

>如果不进行配置，则 Flask-Mail 连接 localhost 上的 25 端口，无须验证身份即可发送电子邮件。

#### Flask-Mail SMTP服务器的配置


配置 | 默认值 | 说明
-- | -- | --
MAIL_SERVER | localhost | 电子邮件服务器的主机名或 IP 地址
MAIL_PORT | 25 | 电子邮件服务器的端口
MAIL_USE_TLS | False | 启用传输层安全（TLS，transport layer security）协议
MAIL_USE_SSL | False | 启用安全套接层（SSL，secure sockets layer）协议
MAIL_USERNAME | None | 邮件账户的用户名
MAIL_PASSWORD | None | 邮件账户的密码

#### 连接到外部 SMTP 服务器

>使用 Google Gmail 账户发送电子邮件

>配置 Flask-Mail 使用 Gmail

```py
import os

# ...
app.config['MAIL_SERVER'] = 'smtp.googlemail.com'
app.config['MAIL_PORT'] = 587
app.config['MAIL_USE_TLS'] = True
app.config['MAIL_USERNAME'] = os.environ.get('MAIL_USERNAME')
app.config['MAIL_PASSWORD'] = os.environ.get('MAIL_PASSWORD')
```

>千万不要把账户凭据直接写入脚本，特别是当你计划开源自己的作品时。

>为了保护账户信息，脚本应该从环境变量中导入敏感信息。

>基于安全方面的考虑，Gmail 账户要求外部应用连接电子邮件服务器时使用 OAuth2 验证身份。然而，Python 的 smtplib 库不支持这种身份验证方法。为了能以标准的 SMTP 身份验证方法使用 Gmail 账户，打开 Google 账户设置页面，在左边的菜单栏里点击“Signing in to Google”。在打开的页面中找到“Allow less secure apps”设置并勾选上。如果你对这样设置自己的 Gmail 账户不放心，可以注册一个二级账户，专门用于测试电子邮件发送。

#### 初始化 Flask-Mail

```py
from flask_mail import Mail

mail = Mail(app)
```

#### 定义环境变量

>保存电子邮件服务器用户名和密码的两个环境变量要在环境中定义

>Linux 或 macOS

```bash
(venv) $ export MAIL_USERNAME=<Gmail username>
(venv) $ export MAIL_PASSWORD=<Gmail password>
```

>Windows

```bash
(venv) $ set MAIL_USERNAME=<Gmail username>
(venv) $ set MAIL_PASSWORD=<Gmail password>
```

#### 在Python shell中发送电子邮件
```bash
(venv) $ flask shell
>>> from flask_mail import Message
>>> from hello import mail
>>> msg = Message('test email', sender='you@example.com',
...     recipients=['you@example.com'])
>>> msg.body = 'This is the plain text body'
>>> msg.html = 'This is the <b>HTML</b> body'
>>> with app.app_context():
...    mail.send(msg)
...
```

>Flask-Mail 的 send() 函数使用 current_app，因此要在激活的应用上下文中执行

#### 在应用中集成电子邮件发送功能

>函数可以使用 Jinja2 模板渲染邮件正文

```py
from flask_mail import Message

app.config['FLASKY_MAIL_SUBJECT_PREFIX'] = '[Flasky]'
app.config['FLASKY_MAIL_SENDER'] = 'Flasky Admin <flasky@example.com>'

def send_email(to, subject, template, **kwargs):
    msg = Message(app.config['FLASKY_MAIL_SUBJECT_PREFIX'] + subject,
                  sender=app.config['FLASKY_MAIL_SENDER'], recipients=[to])
    msg.body = render_template(template + '.txt', **kwargs)
    msg.html = render_template(template + '.html', **kwargs)
    mail.send(msg)
```

>这个函数用到了两个应用层面的配置项，分别定义邮件主题的前缀和发件人的地址。send_email() 函数的参数分别为收件人地址、主题、渲染邮件正文的模板和关键字参数列表。指定模板时不能包含扩展名，这样才能使用两个模板分别渲染纯文本正文和 HTML 正文。调用者传入的关键字参数将传给 render_template() 函数，作为模板变量提供给模板使用，用于生成电子邮件正文。

#### 每当表单接收到新的名字，应用就给管理员发送一封电子邮件

```py
# ...
app.config['FLASKY_ADMIN'] = os.environ.get('FLASKY_ADMIN')
# ...
@app.route('/', methods=['GET', 'POST'])
def index():
    form = NameForm()
    if form.validate_on_submit():
        user = User.query.filter_by(username=form.name.data).first()
        if user is None:
            user = User(username=form.name.data)
            db.session.add(user)
            session['known'] = False
            if app.config['FLASKY_ADMIN']:
                send_email(app.config['FLASKY_ADMIN'], 'New User',
                           'mail/new_user', user=user)
        else:
            session['known'] = True
        session['name'] = form.name.data
        form.name.data = ''
        return redirect(url_for('index'))
    return render_template('index.html', form=form, name=session.get('name'),
                           known=session.get('known', False))
```

>创建两个模板文件，分别用于渲染纯文本和 HTML 版本的邮件正文。

>这两个模板文件都保存在 templates 目录下的 mail 子目录中，以便和普通模板区分开来。

>电子邮件的模板中有一个模板参数是用户，因此调用 send_email() 函数时要以关键字参数的形式传入用户。

#### 异步发送电子邮件

> mail.send() 函数在发送电子邮件时停滞了几秒钟，在这个过程中浏览器就像无响应一样。

>为了在处理请求过程中避免不必要的延迟，我们可以把发送电子邮件的函数移到后台线程中

```py
from threading import Thread

def send_async_email(app, msg):
    with app.app_context():
        mail.send(msg)

def send_email(to, subject, template, **kwargs):
    msg = Message(app.config['FLASKY_MAIL_SUBJECT_PREFIX'] + subject,
                  sender=app.config['FLASKY_MAIL_SENDER'], recipients=[to])
    msg.body = render_template(template + '.txt', **kwargs)
    msg.html = render_template(template + '.html', **kwargs)
    thr = Thread(target=send_async_email, args=[app, msg])
    thr.start()
    return thr
```

>很多 Flask 扩展都假设已经存在激活的应用上下文和（或）请求上下文。

>Flask-Mail 的 send() 函数使用 current_app，因此必须激活应用上下文。

>不过，上下文是与线程配套的，在不同的线程中执行 mail.send() 函数时，要使用 app.app_context() 人工创建应用上下文。

>app 实例作为参数传入线程，因此可以通过它来创建上下文。

#### Celery 任务队列

>应用要发送大量电子邮件时，使用专门发送电子邮件的作业要比给每封邮件都新建一个线程更合适。

>例如，可以把执行 send_async_email() 函数的操作发给 Celery 任务队列。

