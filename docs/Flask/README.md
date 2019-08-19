#### Flask

>可扩展的框架

#### Flask 有 3 个主要依赖：

> 路由、调试和 Web 服务器网关接口（WSGI，Web server gateway interface）

>子系统由 Werkzeug 提供；

>模板系统由 Jinja2 提供；

>命令行集成由 Click 提供。

>这些依赖全都是 Flask 的开发者 Armin Ronacher 开发的

#### 安装

##### 进入项目目录，创建虚拟环境

```bash
python3 -m venv venv
```

##### 使用虚拟环境

```bash
source venv/bin/activate
```

>虚拟环境中的工作结束后，在命令提示符中输入 deactivate

>还原当前终端会话的 PATH 环境变量，把命令提示符重置为最初的状态

##### 安装 Flask 包

```bash
(venv) $ pip install flask
```

>执行这个命令后，pip 不仅安装 Flask 自身，还会安装它的所有依赖

##### 验证 Flask 是否正确安装

>可以启动 Python 解释器，尝试导入 Flask

```bash
(venv) $ python

>>> import flask

>>>

```

>如果没有看到错误提醒，那么恭喜你
