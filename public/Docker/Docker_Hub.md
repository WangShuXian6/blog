#### Docker Hub

#### Alpine 镜像
>基于 Alpine 系统建立的软件镜像远远小于基于其他系统的软件镜像

>镜像标签中的 Alpine 其实指的是这个镜像内的文件系统内容，是基于 Alpine Linux 这个操作系统的。Alpine Linux 是一个相当精简的操作系统，而基于它的 Docker 镜像可以仅有数 MB 的尺寸

>如果软件基于这样的系统镜像之上构建而得，可以想象新的镜像也是十分小巧的。

>在 Alpine 中缺少很多常见的工具和类库，以至于如果我们想基于软件 Alpine 标签的镜像进行二次构建，那搭建的过程会相当烦琐


#### 对容器进行配置
>在 MySQL 镜像的维护者们为我们打造了一些自动化脚本，通过它们，只需要简单的传入几个参数，就能够快速实现对 MySQL 数据库的初始化。

>对于 MySQL 镜像来说，进行软件配置的方法是通过环境变量的方式来实现的 ( 在其他的镜像里，还有通过启动命令、挂载等方式来实现的 )。我们只需要通过这些给出的环境变量，就可以初始化 MySQL 的配置了。

>通过下面的命令来直接建立 MySQL 中的用户和数据库

>通过这条命令启动的 MySQL 容器，在内部就已经完成了用户的创建和数据库的创建，我们通过 MySQL 客户端就能够直接登录这个用户和访问对应的数据库了。
```bash
docker run --name mysql -e MYSQL_DATABASE=webapp -e MYSQL_USER=www -e MYSQL_PASSWORD=my-secret-pw -d mysql:5.7
```
>MySQL 正是利用了 ENTRYPOINT 指令进行初始化这种任务安排，对容器中的 MySQL 进行初始化的。

>docker-entrypoint.sh

>https://github.com/docker-library/mysql/blob/master/5.7/docker-entrypoint.sh

***

#### 自动构建镜像
>只需要提供 Dockerfile 和相关的基本文件，Docker Hub 就能够在云端自动将它们构建成镜像，之后便可以让其他开发者通过 docker pull 命令拉取到这一镜像

>在 Docker Hub 中并不直接存放我们用于构建的 Dockerfile 和相关文件，我们必须将 Docker Hub 账号授权到 GitHub 或是 Bitbucket 来从这些代码库中获取 Dockerfile 和相关文件。

>在连接到 GitHub 或 Bitbucket 后，我们就可以选择我们存放 Dockerfile 和相关文件的代码仓库用来创建自动构建了。

>在基本信息填写完成，点击创建按钮后，Docker Hub 就会开始根据我们 Dockerfile 的内容构建镜像了。而此时，我们也能够访问我们镜像专有的详情页面了。