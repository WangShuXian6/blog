#### Docker Compose 管理容器
>多容器定义和运行软件

>将所有与应用系统相关的软件及它们对应的容器进行配置，之后使用 Docker Compose 提供的命令进行启动

>使用 Docker 来部署的分布式计算服务

>将大型服务拆分成较小的微服务，分别部署到独立的机器或容器中

>Dockerfile 是将容器内运行环境的搭建固化下来

>Docker Compose 将多个容器运行的方式和配置固化下来。

#### 安装 Docker Compose
>Docker Compose 是一个由 Python 编写的软件，在拥有 Python 运行环境的机器上，我们可以直接运行它，不需要其它的操作。

>下载 Docker Compose 到应用执行目录，并附上运行权限，这样 Docker Compose 就可以在机器中使用了

```bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$
$ sudo docker-compose version
docker-compose version 1.21.2, build a133471
docker-py version: 3.3.0
CPython version: 3.6.5
OpenSSL version: OpenSSL 1.0.1t  3 May 2016
```

>也能够通过 Python 的包管理工具 pip 来安装 Docker Compose。

```bash
sudo pip install docker-compose
```

>不论你是使用 Docker for Win 还是 Docker for Mac，亦或是 Docker Toolbox 来搭建 Docker 运行环境，你都可以直接使用 docker-compose 这个命令。这三款软件都已经将 Docker Compose 内置在其中，供我们使用。

***

#### Docker Compose 的基本使用逻辑

> - 如果需要的话，编写容器所需镜像的 Dockerfile；( 也可以使用现有的镜像 )
> - 编写用于配置容器的 docker-compose.yml；
> - 使用 docker-compose 命令启动应用。

#### 编写 Docker Compose 配置
>定义组成应用服务容器群的各项配置

>基于 YAML 格式

>YAML 是一种清晰、简单的标记语言

>缺省的文件名 docker-compose.yml

```yaml
version: '3'

services:

  webapp:
    build: ./image/webapp
    ports:
      - "5000:5000"
    volumes:
      - ./code:/code
      - logvolume:/var/log
    links:
      - mysql
      - redis

  redis:
    image: redis:3.2
  
  mysql:
    image: mysql:5.7
    environment:
      - MYSQL_ROOT_PASSWORD=my-secret-pw

volumes:
  logvolume: {}
```

>version ，这代表定义的 docker-compose.yml 文件内容所采用的版本

>services ，整个 docker-compose.yml 的核心部分，其定义了容器的各项细节。

>在 Docker Compose 里不直接体现容器这个概念

>service 代表的是一个应用集群的配置。每个 service 定义的内容，可以通过特定的配置进行水平扩充，将同样的容器复制数份形成一个容器集群。而 Docker Compose 能够对这个集群做到黑盒效果，让其他的应用和容器无法感知它们的具体结构。

#### 启动和停止

>docker-compose up 命令类似于 Docker Engine 中的 docker run，它会根据 docker-compose.yml 中配置的内容，创建所有的容器、网络、数据卷等等内容，并将它们启动。

>与 docker run 一样，默认情况下 docker-compose up 会在“前台”运行，我们可以用 -d 选项使其“后台”运行。

```bash
docker-compose up -d
```

>docker-compose 命令默认会识别当前控制台所在目录内的 docker-compose.yml 文件，而会以这个目录的名字作为组装的应用项目的名称。

>如果需要改变它们，可以通过选项 -f 来修改识别的 Docker Compose 配置文件，通过 -p 选项来定义项目名。

```bash
docker-compose -f ./compose/docker-compose.yml -p myapp up -d
```

>docker-compose down 命令用于停止所有的容器，并将它们删除，同时消除网络等配置内容，也就是几乎将这个 Docker Compose 项目的所有影响从 Docker 中清除

>短时间内不再需要时，通过 docker-compose down 清理它

```bash
docker-compose down
```

>通过 Docker 让我们能够在开发过程中搭建一套不受干扰的独立环境，让开发过程能够基于稳定的环境下进行。而 Docker Compose 则让我们更近一步，同时让我们处理好多套开发环境，并进行快速切换。

#### 容器命令
>服务可以看成是一组相同容器的集合，所以操作服务就有点像操作容器一样。

>在 Docker Compose 下运行的服务，其命名都是由 Docker Compose 自动完成

>查看容器中主进程的输出内容 docker-compose logs

>在 docker-compose logs 衔接的是 Docker Compose 中所定义的服务的名称。
```shell
docker-compose logs nginx
```

>通过 docker-compose create，docker-compose start 和 docker-compose stop 我们可以实现与 docker create，docker start 和 docker stop 相似的效果，只不过操作的对象由 Docker Engine 中的容器变为了 Docker Compose 中的服务。

```shell
$ sudo docker-compose create webapp
$ sudo docker-compose start webapp
$ sudo docker-compose stop webapp
```