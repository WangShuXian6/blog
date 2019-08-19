#### docker 容器网络 集群部署

#### 容器网络模型 ( Container Network Model )。
> - 沙盒 提供了容器的虚拟网络栈，也就是之前所提到的端口套接字、IP 路由表、防火墙等的内容。其实现隔离了容器网络与宿主机网络，形成了完全独立的容器网络环境。
> - 网络 可以理解为 Docker 内部的虚拟子网，网络内的参与者相互可见并能够进行通讯。Docker 的这种虚拟网络也是于宿主机网络存在隔离关系的，其目的主要是形成容器间的安全通讯环境。
> - 端点 是位于容器或网络隔离墙之上的洞，其主要目的是形成一个可以控制的突破封闭的网络环境的出入口。当容器的端点与网络的端点形成配对后，就如同在这两者之间搭建了桥梁，便能够进行数据传输了。

#### Docker 网络驱动
>Bridge Driver、Host Driver、Overlay Driver、MacLan Driver、None Driver。


#### 容器互联
>最简单的 Web 应用为例，也至少需要业务应用、数据库应用、缓存应用等组成

>通过 docker create 或 docker run 创建时通过 --link 选项进行配置。

>容器能够互相连接的前提是两者同处于一个网络中 ( 这里的网络是指容器网络模型中的网络 )。


>创建一个 MySQL 容器，将运行我们 Web 应用的容器连接到这个 MySQL 容器上，打通两个容器间的网络，实现它们之间的网络互通。

```bash
docker run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
docker run -d --name webapp --link mysql webapp:latest
```

>将容器的网络命名填入到连接地址中，就可以访问需要连接的容器了。

```bash
String url = "jdbc:mysql://mysql:3306/webapp";
```

##### 暴露端口

>只有容器自身允许的端口，才能被其他容器所访问。

>可以通过 Docker 镜像进行定义

>在容器创建时进行定义端口的暴露  --expose

```bash
docker run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes --expose 13306 --expose 23306 mysql:5.7
//  暴露了 13306 和 23306 这两个端口
```
>容器暴露了端口只是类似我们打开了容器的防火墙，具体能不能通过这个端口访问容器中的服务，还需要容器中的应用监听并处理来自这个端口的请求。

#### 通过别名连接
```bash
docker run -d --name webapp --link mysql:database webapp:latest
// 别名为 database
// --link <name>:<alias>
```
>在 Web 应用中使用 MySQL 连接时，就可以使用 database 来代替连接地址了

```bash
String url = "jdbc:mysql://database:3306/webapp";
```

#### 管理网络
>启动 Docker 服务时，它会为我们创建一个默认的 bridge 网络，而我们创建的容器在不专门指定网络的情况下都会连接到这个网络上。所以我们刚才之所以能够把 webapp 容器连接到 mysql 容器上，其原因是两者都处于 bridge 这个网络上。

>创建网络，形成自己定义虚拟子网

>-d 选项我们可以为新的网络指定驱动的类型，其值可以是刚才我们所提及的 bridge、host、overlay、maclan、none，也可以是其他网络驱动插件所定义的类型。

```bash
docker network create -d bridge individual
```

#### 查看 Docker 中已经存在的网络
```bash
docker network ls
docker network list
```

>创建容器时，可以通过 --network 来指定容器所加入的网络

```bash
docker run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes --network individual mysql:5.7
```
>让运行 Web 应用的容器加入到 individual 这个网络，就可以成功建立容器间的网络连接了

```bash
docker run -d --name webapp --link mysql --network individual webapp:latest
```

#### 端口映射
>在容器外通过网络访问容器中的应用

>创建容器时使用 -p 或者是 --publish 选项

![default](https://user-images.githubusercontent.com/30850497/48543930-82956980-e8fd-11e8-84bc-7b305c7864f7.png)
```bash
docker run -d --name nginx -p 80:80 -p 443:443 nginx:1.12
// 端口映射选项格式 -p <ip>:<host-port>:<container-port>
```
>ip 是宿主操作系统的监听 ip，可以用来控制监听的网卡，默认为 0.0.0.0，也就是监听所有网卡。

>host-port 和 container-port 分别表示映射到宿主操作系统的端口和容器的端口，这两者是可以不一样的

>我们可以将容器的 80 端口映射到宿主操作系统的 8080 端口，传入 -p 8080:80 即可。

>端口映射功能是将容器端口映射到宿主操作系统的端口上，实际来说就是映射到了 Linux 系统的端口上