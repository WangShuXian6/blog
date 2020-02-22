#### seafile docker
***
#### ubuntu linux 安装

>用 Docker 部署 Seafile 专业版

>https://docs.seafile.com/published/seafile-manual-cn/docker/pro-edition/用Docker部署Seafile.md

#### 安装 docker-compose
>因为 Seafile v7.x.x 容器是通过 docker-compose 命令运行的，所以您应该先在服务器上安装该命令。

###### for CentOS
```bash
yum install docker-compose -y
```
﻿
###### for Ubuntu
```bash
apt-get install docker-compose -y
```

#### 修改 docker-compose.yml
>下载 docker-compose.yml 示例文件到您的服务器上，然后根据您的实际环境修改该文件。尤其是以下几项配置：

>MySQL root 用户的密码 (MYSQL_ROOT_PASSWORD and DB_ROOT_PASSWD)

>持久化存储 MySQL 数据的 volumes 目录 (volumes)

>持久化存储 Seafile 数据的 volumes 目录 (volumes)

>持久化存储 Elasticsearch 索引数据的 volumes 目录 (volumes)

```yml
version: '2.0'
services:
  db:
    image: mariadb:10.1
    container_name: seafile-mysql
    environment:
      - MYSQL_ROOT_PASSWORD=db_dev  # Requested, set the root's password of MySQL service.
      - MYSQL_LOG_CONSOLE=true
    volumes:
      - ~/seafile/seafile-mysql/db:/var/lib/mysql  # Requested, specifies the path to MySQL data persistent store.
    networks:
      - seafile-net

  memcached:
    image: memcached:1.5.6
    container_name: seafile-memcached
    entrypoint: memcached -m 256
    networks:
      - seafile-net

  elasticsearch:
    image: seafileltd/elasticsearch-with-ik:5.6.16
    container_name: seafile-elasticsearch
    environment:
      - discovery.type=single-node
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms1g -Xmx1g"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    mem_limit: 2g
    volumes:
      - ~/seafile/seafile-elasticsearch/data:/usr/share/elasticsearch/data  # Requested, specifies the path to Elasticsearch data persistent store.
    networks:
      - seafile-net
          
  seafile:
    image: docker.seafile.top/seafileltd/seafile-pro-mc:latest
    container_name: seafile
    ports:
      - "80:80"
#     - "443:443"  # If https is enabled, cancel the comment.
    volumes:
      - ~/seafile/seafile-data:/shared   # Requested, specifies the path to Seafile data persistent store.
    environment:
      - DB_HOST=db
      - DB_ROOT_PASSWD=db_dev  # Requested, the value shuold be root's password of MySQL service.
#      - TIME_ZONE=Asia/Shanghai # Optional, default is UTC. Should be uncomment and set to your local time zone.
      - SEAFILE_ADMIN_EMAIL=me@example.com # Specifies Seafile admin user, default is 'me@example.com'
      - SEAFILE_ADMIN_PASSWORD=asecret     # Specifies Seafile admin password, default is 'asecret'
      - SEAFILE_SERVER_LETSENCRYPT=false   # Whether to use https or not
      - SEAFILE_SERVER_HOSTNAME=example.seafile.com # Specifies your host name if https is enabled
    depends_on:
      - db
      - memcached
      - elasticsearch
    networks:
      - seafile-net

networks:
  seafile-net:
```

#### 自定义管理员用户名和密码
>默认的管理员账号是 me@example.com 并且该账号的密码是 asecret，您可以在 docker-compose.yml 中配置不同的用户名和密码，为此您需要做如下配置：

```yml
seafile:
    ...
﻿
    environment:
        ...
        - SEAFILE_ADMIN_EMAIL=me@example.com
        - SEAFILE_ADMIN_PASSWORD=a_very_secret_password
```

#### 新建 映射文件夹
>~/seafile/seafile-mysql/db

>~/seafile/seafile-elasticsearch/data

>~/seafile/seafile-data

#### 启动 Seafile 服务
```bash
docker-compose up -d
```
>需要等待几分钟，等容器首次启动时的初始化操作完成后，您就可以在浏览器上访问http://seafile.example.com 来打开 Seafile 主页。

>注意：您应该在 docker-compose.yml文件所在的目下执行以上命令。

#### 映射本机到 seafile.example.com

```bash
sudo vi /etc/hosts
```
>添加一行

```
127.0.0.1 seafile.example.com
```

#### 映射局域网指定 ip 到 seafile.example.com
>hosts 添加

```
66.22.22.2 seafile.example.com
```

#### 重启/启动
```bash
docker-compose restart
```