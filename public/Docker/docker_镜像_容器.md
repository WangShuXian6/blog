#### docker 镜像 容器

#### 查看镜像
```bash
docker images
```

#### 镜像命名
>没有 username 这个部分的镜像，表示镜像是由 Docker 官方所维护和提供的

 - >username： 主要用于识别上传镜像的不同用户，与 GitHub 中的用户空间类似。
 - >repository：主要用于识别进行的内容，形成对镜像的表意描述。
 - >tag：主要用户表示镜像的版本，方便区分进行内容的不同细节

>Docker 对容器的设计和定义是微型容器而不是庞大臃肿的完整环境 ( 这当然归功于容器技术在实现虚拟化过程中性能几乎无损 )，这就使得我们通常会只在一个容器中运行一个应用程序，这样的好处自然是能够大幅降低程序之间相互的影响，也有利于利用容器技术控制每个程序所使用的资源...

#### 容器运行的状态流转图
![default](https://user-images.githubusercontent.com/30850497/48540210-6e993a00-e8f4-11e8-8ee1-200976d7bdae.png)

#### 容器的生命周期
>核心状态 - Created、Running、Paused、Stopped、Deleted

>容器即应用，应用即容器

>虽然在 Docker 中我们也能够实现在同一个容器中运行多个不同类型的程序，但这么做的话，Docker 就无法跟踪不同应用的生命周期，有可能造成应用的非正常关闭，进而影响系统、数据的稳定性。

#### 镜像仓库
>将开发环境上所使用的镜像推送至镜像仓库，并在测试或生产环境上拉取到它们，而这个过程仅需要几个命令，甚至自动化完成。

#### 获取镜像
```bash
docker pull ubuntu
docker pull openresty/openresty:1.13.6.2-alpine
```
>Docker 首先会拉取镜像所基于的所有镜像层，之后再单独拉取每一个镜像层并组合成这个镜像。当然，如果在本地已经存在相同的镜像层 ( 共享于其他的镜像 )，那么 Docker 就直接略过这个镜像层的拉取而直接采用本地的内容。

#### Docker Hub hub.docker.com
>Docker 官方建立的中央镜像仓库，除了普通镜像仓库的功能外，它内部还有更加细致的权限管理，支持构建钩子和自动构建

#### docker search

#### 管理镜像 docker inspect
```bash
docker inspect redis:3.2
docker inspect 2fef532e // 镜像 ID 或容器 ID
```

#### 删除镜像 docker rmi 镜像的名称或 ID
```bash
docker rmi ubuntu:latest
docker rmi redis:3.2 redis:4.0
```

***

>Created：容器已经被创建，容器所需的相关资源已经准备就绪，但容器中的程序还未处于运行状态。

>Running：容器正在运行，也就是容器中的应用正在运行。

>Paused：容器已暂停，表示容器中的所有程序都处于暂停 ( 不是停止 ) 状态。

>Stopped：容器处于停止状态，占用的资源和沙盒环境都依然存在，只是容器中的应用程序均已停止。

>Deleted：容器已删除，相关占用的资源及存储在 Docker 中的管理信息也都已释放和移除。...


#### 创建容器
```bash
docker create nginx:1.12
docker create --name nginx nginx:1.12 // 配置容器名
```
>启动容器

```bash
docker start nginx
```

>通过 docker run 这个命令将 docker create 和 docker start 这两步操作合成为一步

```bash
docker run --name nginx -d nginx:1.12
```
>docker run 在启动容器时，会采用“前台”运行这种方式，这时候我们的控制台就会衔接到容器上，不能再进行其他操作了。我们可以通过 -d 或 --detach 这个选项告诉 Docker 在启动后将程序与控制台分离，使其进入“后台”运行。

#### 列出 Docker 中的容器
>默认情况下，docker ps 列出的容器是处于运行中的容器，如果要列出所有状态的容器，需要增加 -a 或 --all 选项。

```bash
docker ps
docker ps -a
```
>结果中的 COMMAND 表示的是容器中主程序 ( 也就是与容器生命周期所绑定进程所关联的程序 ) 的启动命令，这条命令是在镜像内定义的，而容器的启动其实质就是启动这条命令

>结果中的 STATUS 表示容器所处的状态

> - Created 此时容器已创建，但还没有被启动过。
> - Up [ Time ] 这时候容器处于正在运行状态，而这里的 Time 表示容器从开始运行到查看时的时间。
> - Exited ([ Code ]) [ Time ] 容器已经结束运行，这里的 Code 表示容器结束运行时，主程序返回的程序退出码，而 Time 则表示容器结束到查看时的时间。

#### 停止容器
>容器停止后，其维持的文件系统沙盒环境还是存在的，内部被修改的内容也都会保留，我们可以通过 docker start 命令将这个容器再次启动。

```bash
docker stop nginx
```

#### 完全删除容器
>正在运行中的容器默认情况下是不能被删除的，我们可以通过增加 -f 或 --force 选项来让 docker rm 强制停止并删除容器，不过这种做法并不妥当

>短时间内不需要使用容器时，最佳的做法是删除它而不是仅仅停止它

>由于数据卷是独立于容器存在的，所以其能保证数据不会随着容器的删除而丢失

```bash
docker rm nginx
```

#### 进入容器 docker exec
>docker exec 命令能帮助我们在正在运行的容器中运行指定命令

```bash
docker exec nginx more /etc/hostname // 用容器中的 more 命令查看容器的主机名定义
```

>-i ( --interactive ) 表示保持我们的输入流，只有使用它才能保证控制台程序能够正确识别我们的命令。
>-t ( --tty ) 表示启用一个伪终端，形成我们与 bash 的交互，如果没有它，我们无法看到 bash 内部的执行结果
```bash
docker exec -it nginx bash
```

#### 衔接到容器 docker attach
>将当前的输入输出流连接到指定的容器上

>将容器中的主程序转为了“前台”运行 ( 与 docker run 中的 -d 选项有相反的意思 )

>通过 Ctrl + C 来向程序发送停止信号，让程序停止 ( 从而容器也会随之停止 )

```bash
docker attach nginx
```
***
>并不是所有的镜像都会持续运行的。如果你下一个装有服务程序的镜像，比如 MySQL，Redis，那它们本身目的是用来提供服务的，自然镜像的制作者会配上能够直接运行的命令。而像 Python 这样的镜像，大部分时候只是用来作为基础镜像或者执行特定命令的，其本身也没有定义一个持续运行的机制，自然不是起来就一直能运行。