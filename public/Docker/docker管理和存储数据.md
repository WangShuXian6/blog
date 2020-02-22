#### docker 管理和存储数据

#### 文件系统挂载方式
> - Bind Mount 能够直接将宿主操作系统中的目录和文件挂载到容器内的文件系统中，通过指定容器外的路径和容器内的路径，就可以形成挂载映射关系，在容器内外对文件的读写，都是相互可见的。

> - Volume 也是从宿主操作系统中挂载目录到容器内，只不过这个挂载的目录由 Docker 进行管理，我们只需要指定容器内的目录，不需要关心具体挂载到了宿主操作系统中的哪里。

> - Tmpfs Mount 支持挂载系统内存中的一部分到容器的文件系统里，不过由于内存和容器的特征，它的存储并不是持久的，其中的内容会随着容器的停止而消失。

***

#### 挂载文件到容器
>在容器创建的时候通过传递 -v 或 --volume 选项来指定内外挂载的对应目录或文件

```bash
docker run -d --name nginx -v /webapp/html:/usr/share/nginx/html nginx:1.12
// -v <host-path>:<container-path> 或 --volume <host-path>:<container-path>
```
>host-path 和 container-path 分别代表宿主操作系统中的目录和容器中的目录

>定义目录时必须使用绝对路径，不能使用相对路径。

>在挂载选项 -v 后再接上 :ro 就可以只读挂载了
>通过只读方式挂载的目录和文件，只能被容器中的程序读取，但不接受容器中程序修改它们的请求

```bash
docker run -d --name nginx -v /webapp/html:/usr/share/nginx/html:ro nginx:1.12
```

#### 常见场景
>从宿主操作系统共享配置,时区配置

>借助 Docker 进行开发

>在 Docker 中，推崇直接将代码和配置打包进镜像，以便快速部署和快速重建。但这在开发过程中显然非常不方便

***

#### 挂载临时文件目录 Tmpfs Mount
>--tmpfs 只需要传递挂载到容器内的目录即可

```bash
docker run -d --name webapp --tmpfs /webapp/cache webapp:latest
```
##### 适应场景

>应用中使用到，但不需要进行持久保存的敏感数据

>读写速度要求较高，数据变化量大，但不需要持久保存的数据

***

#### 使用数据卷
>数据卷的本质其实依然是宿主操作系统上的一个目录，只不过这个目录存放在 Docker 内部，接受 Docker 的管理。

>只需要给定容器中的哪个目录会被挂载即可

>使用 -v 或 --volume 选项来定义数据卷的挂载

```bash
docker run -d --name webapp -v /webapp/storage webapp:latest
```
>可以像命名容器一样为数据卷命名

```bash
docker run -d --name webapp -v appdata:/webapp/storage webapp:latest
// -v <name>:<container-path>
```

##### 实际场景
> - 当希望将数据在多个容器间共享时，利用数据卷可以在保证数据持久性和完整性的前提下，完成更多自动化操作。

> - 当我们希望对容器中挂载的内容进行管理时，可以直接利用数据卷自身的管理方法实现。

> - 当使用远程服务器或云服务作为存储介质的时候，数据卷能够隐藏更多的细节，让整个过程变得更加简单。

***

#### 共用数据卷
>数据卷的命名在 Docker 中是唯一的

>实现容器间的目录共享

>通过挂载相同的数据卷，让容器之间能够同时看到并操作数据卷中的内容。

```bash
docker run -d --name webapp -v html:/webapp/html webapp:latest
docker run -d --name nginx -v html:/usr/share/nginx/html:ro nginx:1.12
```
>使用 -v 选项挂载数据卷时，如果数据卷不存在，Docker 会为我们自动创建和分配宿主操作系统的目录，而如果同名数据卷已经存在，则会直接引用

>不依赖于容器独立创建数据卷

```bash
docker volume create appdata
```
>列出当前已创建的数据卷

```bash
docker volume ls
```

***

#### 删除数据卷
>删除指定的数据卷

```bash
docker volume rm appdata
```
>在删除数据卷之前，必须保证数据卷没有被任何容器所使用 ( 也就是之前引用过这个数据卷的容器都已经删除 )，否则 Docker 不会允许我们删除这个数据卷。

>删除容器关联的数据卷 增加 -v

```bash
docker rm -v webapp
```

>删除没有被容器引用的数据卷

```bash
docker volume prune -f
```

***

#### 数据卷容器
>一个没有具体指定的应用，甚至不需要运行的容器，我们使用它的目的，是为了定义一个或多个数据卷并持有它们的引用

>创建数据卷容器,找个简单的系统镜像都可以完成创建

>使用数据卷容器时，我们不建议再定义数据卷的名称

```bash
docker create --name appdata -v /webapp/storage ubuntu
```

>引用数据卷容器 --volumes-from

```bash
docker run -d --name webapp --volumes-from appdata webapp:latest
```
>引用数据卷容器时，不需要再定义数据卷挂载到容器中的位置，Docker 会以数据卷容器中的挂载定义将数据卷挂载到引用的容器中。

>隐藏了数据卷的配置和定义

>更轻松的实现容器的迁移

#### 备份和迁移数据卷
>备份数据,建立一个临时的容器，将用于备份的目录和要备份的数据卷都挂载到这个容器上。

```bash
docker run --rm --volumes-from appdata -v /backup:/backup ubuntu tar cvf /backup/backup.tar /webapp/storage
// /webapp/storage 是指 容器appdata 内的数据存储目录
```
>通过 --rm 选项，我们可以让容器在停止后自动删除，而不需要我们再使用容器删除命令来删除它

>tar cvf /backup/backup.tar /webapp/storage，在镜像定义之后接上命令，可以直接替换掉镜像所定义的主程序启动命令，而去执行这一条命令

>就是先挂载 appdata 的数据卷到新建的这个容器，在把这个数据存储的目录 /webapp/storage 打包 到 容器的 /backup 目录里

>由于新容器的 /backup 目录是从宿主机挂载来的，所以这个打包文件就出现在宿主机里了

>恢复数据卷中的数据，也可以借助临时容器完成,只不过把打包的命令转换为解包的命令而已

```bash
docker run --rm --volumes-from appdata -v /backup:/backup ubuntu tar xvf /backup/backup.tar -C /webapp/storage --strip
```

***

#### 相对支持丰富的挂载方式 --mount
>--mount 通过逗号分隔这种 CSV 格式来定义多个参数

```bash
docker run -d --name webapp webapp:latest --mount 'type=volume,src=appdata,dst=/webapp/storage,volume-driver=local,volume-opt=type=nfs,volume-opt=device=<nfs-server>:<nfs-path>' webapp:latest

// 挂载的来源是一个 NFS 目录
```
>type 定义挂载类型：bind，volume 或 tmpfs

>实现集群挂载的定义 
