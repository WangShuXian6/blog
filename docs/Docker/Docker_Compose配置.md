#### Docker Compose 配置
>编写 Docker Compose 配置文件，其本质就是根据我们所设计的应用架构，对不同应用容器进行配置并加以组合

#### 定义服务
```yaml
version: "3"

services:

  redis:
    image: redis:3.2
    networks:
      - backend
    volumes:
      - ./redis/redis.conf:/etc/redis.conf:ro
    ports:
      - "6379:6379"
    command: ["redis-server", "/etc/redis.conf"]

  database:
    image: mysql:5.7
    networks:
      - backend
    volumes:
      - ./mysql/my.cnf:/etc/mysql/my.cnf:ro
      - mysql-data:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=my-secret-pw
    ports:
      - "3306:3306"

  webapp:
    build: ./webapp
    networks:
      - frontend
      - backend
    volumes:
      - ./webapp:/webapp
    depends_on:
      - redis
      - database

  nginx:
    image: nginx:1.12
    networks:
      - frontend
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/conf.d:/etc/nginx/conf.d:ro
      - ./webapp/html:/webapp/html
    depends_on:
      - webapp
    ports:
      - "80:80"
      - "443:443"

networks:
  frontend:
  backend:

volumes:
  mysql-data:
```

>Docker Compose 就是从配置文件中读取出这些内容，代我们创建和管理这些容器的

***

#### 指定镜像
>每个服务必须指定镜像

>在 Docker Compose 里，通过两种方式为服务指定所采用的镜像

> - 1 通过 image 这个配置，这个相对简单，给出能在镜像仓库中找到镜像的名称即可

> - 2 直接采用 Dockerfile 来构建镜像，通过 build 这个配置我们能够定义构建的环境目录，这与 docker build 中的环境目录是同一个含义。如果我们通过这种方式指定镜像，那么 Docker Compose 先会帮助我们执行镜像的构建，之后再通过这个镜像启动容器。

>在 docker build 里我们还能通过选项定义许多内容，这些在 Docker Compose 里依然可以

```Dockerfile
## ......
  webapp:
    build:
      context: ./webapp
      dockerfile: webapp-dockerfile
      args:
        - JAVA_VERSION=1.6
## .........
```

***

#### 依赖声明
>如果我们的服务间有非常强的依赖关系，我们就必须告知 Docker Compose 容器的先后启动顺序。

>只有当被依赖的容器完全启动后，Docker Compose 才会创建和启动这个容器。

>定义依赖 depends_on

>通过它列出这个服务所有依赖的其他服务即可

>在 Docker Compose 为我们启动项目的时候，会检查所有依赖，形成正确的启动顺序并按这个顺序来依次启动容器。

#### 文件挂载
>使用 volumes 配置可以像 docker CLI 里的 -v 选项一样来指定外部挂载和数据卷挂载。

>能够直接挂载宿主机文件系统中的目录，也可以通过数据卷的形式挂载内容。

>在使用外部文件挂载的时候，我们可以直接指定相对目录进行挂载，这里的相对目录是指相对于 docker-compose.yml 文件的目录。

>由于有相对目录这样的机制，我们可以将 docker-compose.yml 和所有相关的挂载文件放置到同一个文件夹下，形成一个完整的项目文件夹。这样既可以很好的整理项目文件，也利于完整的进行项目迁移。

>正式环境，Docker 提倡将代码或编译好的程序通过构建镜像的方式打包到镜像里，随整个 CI 流部署到服务器中

>在开发时，推荐直接将代码挂载到容器里，而不是通过镜像构建的方式打包成镜像。

>在开发过程中，对于程序的配置等内容，我们也建议直接使用文件挂载的形式挂载到容器里，避免经常修改所带来的麻烦。

#### 使用数据卷
>可以让 Docker Compose 自动完成对数据卷的创建

>独立于 services 的 volumes 配置就是用来声明数据卷的。定义数据卷最简单的方式仅需要提供数据卷的名称，

>对于在 Docker Engine 中创建数据卷时能够使用的其他定义，也能够放入 Docker Compose 的数据卷定义中。

>如果想把属于 Docker Compose 项目以外的数据卷引入进来直接使用，我们可以将数据卷定义为外部引入，通过 external 这个配置就能完成这个定义。

>加入 external 定义后，Docker Compose 在创建项目时不会直接创建数据卷，而是优先从 Docker Engine 中已有的数据卷里寻找并直接采用。

```Dockerfile
## ......
volumes:
  mysql-data:
    external: true
## ......
```

***

#### 配置网络
>在 Docker Compose 里，可以为整个应用系统设置一个或多个网络。

>声明网络的配置同样独立于 services 存在，是位于根配置下的 networks 配置

>可以显式的指定网络的参数。

```Dockerfile
networks:
  frontend:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 10.10.1.0/24
## .........
```

#### 使用网络别名
>为服务单独设置网络别名，在其他容器里，将这个别名作为网络地址进行访问。

>网络别名的定义方式 : 将之前简单的网络 List 定义结构修改成 Map 结构，以便在网络中加入更多的定义。

```Dockerfile
## ......
  database:
    networks:
      backend:
        aliases:
          - backend.database
## ......
  webapp:
    networks:
      backend:
        aliases:
          - backend.webapp
      frontend:
        aliases:
          - frontend.webapp
## .........
```
>可以使用这里我们所设置的网络别名对其他容器进行访问

#### 端口映射
>利用它进行宿主机与容器端口的映射，这个配置与 docker CLI 中 -p 选项的使用方法是近似的

>使用引号将端口映射的定义包裹起来，避免歧义
