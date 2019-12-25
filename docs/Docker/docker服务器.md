#### 删除容器
```bash
docker rm -f nginx
```

***

#### yapi 接口服务器

>https://yapi.ymfe.org YApi 接口管理平台 旨在为开发、产品、测试人员提供更优雅的接口管理服务。可以帮助开发者轻松创建、发布、维护 API

>https://github.com/ymfe/yapi

>http://yapi.demo.qunar.com

##### yapi-mongo.bash
```bash
docker run -d --name mongo-yapi mongo
```

##### yapi-docker.bash

```bash
docker run -d \
  --name yapi \
  --link mongo-yapi:mongo \
  --workdir /api/vendors \
  --volume ~/yapi/api/config.json:/api/config.json \
  -p 4434:3000 \
  registry.cn-hangzhou.aliyuncs.com/anoy/yapi \
  server/app.js
```


##### yapi 配置

>config.json

```JSON
{
  "port": "3000",
  "adminAccount": "admin@xxx.com",
  "closeRegister":false,
  "db": {
    "servername": "127.0.0.1",
    "DATABASE": "yapi",
    "port": 27017,
    "user": "root",
    "pass": "xxx",
    "authSource": ""
  },
  "mail": {
    "enable": true,
    "host": "smtp.163.com",
    "port": 465,
    "from": "***@163.com",
    "auth": {
      "user": "***@163.com",
      "pass": "*****"
    }
  }
}
```


##### 启动yapi

```bash
docker start mongo-yapi
docker start yapi
```

#### nginx 反向代理服务器

##### nginx-docker.bash

```bash
docker run -p 80:80 -p 443:443 -p 2233:22 \
--name nginx \
--restart always \
--volume ~/nginx/nginx.conf:/etc/nginx/nginx.conf \
--volume ~/nginx/cert:/etc/nginx/cert \
--volume ~/nginx/error.log:/var/log/nginx/error.log \
-d nginx
```

##### 启动 nginx

```bash
docker start nginx
```

***

#### nexus
```bash
docker pull bradbeck/nexus-https

docker volume create --name nexus-data

docker run -d -p 8081:8081 -p 8443:8443 -v nexus-data:/nexus-data -v ~/nexus/ssl:/opt/sonatype/nexus/etc/ssl --name nexus bradbeck/nexus-https
```

***

#### nginx.conf
```conf
user root;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    client_max_body_size 500m;

    server {
       listen       80; #监听端口
       server_name  inner.xxx.com api.xxx.com docker.xxx.com npm.xxx.com pypi.xxx.com log.xxx.com; #请求域名
       return      301 https://$host$request_uri; #重定向至https访问。
   }

    server {
       listen  443 ssl; #监听端口
       server_name  inner.xxx.com; #请求域名
       ssl_certificate /etc/nginx/cert/inner.xxx.com.pem; #pem证书路径
       ssl_certificate_key     /etc/nginx/cert/inner.xxx.com.key; #pem证书key路径
       ssl_session_timeout     5m; #会话超时时间
       ssl_ciphers     ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4; #加密算法
       ssl_protocols   TLSv1 TLSv1.1 TLSv1.2; #SSL协议

      # 拦截所有请求
       location / {
            proxy_http_version 1.1; #代理使用的http协议
            proxy_set_header Host $host; #header添加请求host信息
            proxy_set_header X-Real-IP $remote_addr; # header增加请求来源IP信息
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 增加代理记录
            proxy_pass https://00.00.00.00:4433; #服务inner访问地址
        }
        
        # 拦截websocket请求
        location /websocket {
           proxy_pass https://00.00.00.00:4433;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection "upgrade";
        }
   }

   server {
       listen  443 ssl; #监听端口
       server_name  api.xxx.com; #请求域名
       ssl_certificate /etc/nginx/cert/api.xxx.com.pem; #pem证书路径
       ssl_certificate_key     /etc/nginx/cert/api.xxx.com.key; #pem证书key路径
       ssl_session_timeout     5m; #会话超时时间
       ssl_ciphers     ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4; #加密算法
       ssl_protocols   TLSv1 TLSv1.1 TLSv1.2; #SSL协议

      # 拦截所有请求
       location / {
            proxy_http_version 1.1; #代理使用的http协议
            proxy_set_header Host $host; #header添加请求host信息
            proxy_set_header X-Real-IP $remote_addr; # header增加请求来源IP信息
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 增加代理记录
            proxy_pass http://00.00.00.00:4434; #服务 yapi API访问地址
        }
        
        # 拦截websocket请求
        location /websocket {
           proxy_pass http://00.00.00.00:4434;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection "upgrade";
        }
   }

server {
       listen  80;
       server_name  nexus.xxx.com; #请求域名

      # 拦截所有请求
       location / {
            proxy_http_version 1.1; #代理使用的http协议
            proxy_set_header Host $host; #header添加请求host信息
            proxy_set_header X-Real-IP $remote_addr; # header增加请求来源IP信息
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 增加代理记录
            proxy_pass http://00.00.00.00:8081/; #服务 nexus 访问地址
        }
        
        # 拦截websocket请求
        # location /websocket {
        #    proxy_pass http://00.00.00.00:4434;
        #    proxy_http_version 1.1;
        #    proxy_set_header Upgrade $http_upgrade;
        #    proxy_set_header Connection "upgrade";
        # }
   }

   server {
       listen  443 ssl; #监听端口
       server_name  nexus.xxx.com; #请求域名
       ssl_certificate /etc/nginx/cert/api.xxx.com.pem; #pem证书路径
       ssl_certificate_key     /etc/nginx/cert/api.xxx.com.key; #pem证书key路径
       ssl_session_timeout     5m; #会话超时时间
       ssl_ciphers     ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4; #加密算法
       ssl_protocols   TLSv1 TLSv1.1 TLSv1.2; #SSL协议

      # 拦截所有请求
       location / {
            proxy_http_version 1.1; #代理使用的http协议
            proxy_set_header Host $host; #header添加请求host信息
            proxy_set_header X-Real-IP $remote_addr; # header增加请求来源IP信息
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 增加代理记录
            proxy_pass https://00.00.00.00:8443/; #服务 nexus 访问地址
        }
        
        # 拦截websocket请求
        # location /websocket {
        #    proxy_pass http://00.00.00.00:4434;
        #    proxy_http_version 1.1;
        #    proxy_set_header Upgrade $http_upgrade;
        #    proxy_set_header Connection "upgrade";
        # }
   }

   server {
       listen  443 ssl; #监听端口
       server_name  docker.xxx.com; #请求域名
       ssl_certificate /etc/nginx/cert/api.xxx.com.pem; #pem证书路径
       ssl_certificate_key     /etc/nginx/cert/api.xxx.com.key; #pem证书key路径
       ssl_session_timeout     5m; #会话超时时间
       ssl_ciphers     ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4; #加密算法
       ssl_protocols   TLSv1 TLSv1.1 TLSv1.2; #SSL协议

      # 拦截所有请求
       location / {
            proxy_http_version 1.1; #代理使用的http协议
            proxy_set_header Host $host; #header添加请求host信息
            proxy_set_header X-Real-IP $remote_addr; # header增加请求来源IP信息
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 增加代理记录
            proxy_pass https://00.00.00.00:8443/repository/docker-group/; #服务 docker 镜像仓库访问地址
        }
        
        # 拦截websocket请求
        # location /websocket {
        #    proxy_pass http://00.00.00.00:4434;
        #    proxy_http_version 1.1;
        #    proxy_set_header Upgrade $http_upgrade;
        #    proxy_set_header Connection "upgrade";
        # }
   }


   server {
       listen  443 ssl; #监听端口
       server_name  npm.xxx.com; #请求域名
       ssl_certificate /etc/nginx/cert/api.xxx.com.pem; #pem证书路径
       ssl_certificate_key     /etc/nginx/cert/api.xxx.com.key; #pem证书key路径
       ssl_session_timeout     5m; #会话超时时间
       ssl_ciphers     ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4; #加密算法
       ssl_protocols   TLSv1 TLSv1.1 TLSv1.2; #SSL协议

      # 拦截所有请求
       location / {
            proxy_http_version 1.1; #代理使用的http协议
            proxy_set_header Host $host; #header添加请求host信息
            proxy_set_header X-Real-IP $remote_addr; # header增加请求来源IP信息
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 增加代理记录
            proxy_pass https://00.00.00.00:8443/repository/npm-group/; #服务 npm 仓库访问地址
        }
        
        # 拦截websocket请求
        # location /websocket {
        #    proxy_pass http://00.00.00.00:4434;
        #    proxy_http_version 1.1;
        #    proxy_set_header Upgrade $http_upgrade;
        #    proxy_set_header Connection "upgrade";
        # }
   }

   server {
       listen  443 ssl; #监听端口
       server_name  pypi.xxx.com; #请求域名
       ssl_certificate /etc/nginx/cert/api.xxx.com.pem; #pem证书路径
       ssl_certificate_key     /etc/nginx/cert/api.xxx.com.key; #pem证书key路径
       ssl_session_timeout     5m; #会话超时时间
       ssl_ciphers     ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4; #加密算法
       ssl_protocols   TLSv1 TLSv1.1 TLSv1.2; #SSL协议

      # 拦截所有请求
       location / {
            proxy_http_version 1.1; #代理使用的http协议
            proxy_set_header Host $host; #header添加请求host信息
            proxy_set_header X-Real-IP $remote_addr; # header增加请求来源IP信息
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 增加代理记录
            proxy_pass https://00.00.00.00:8443/repository/pypi-group/; #服务 pypi 仓库访问地址
        }
        
        # 拦截websocket请求
        # location /websocket {
        #    proxy_pass http://00.00.00.00:4434;
        #    proxy_http_version 1.1;
        #    proxy_set_header Upgrade $http_upgrade;
        #    proxy_set_header Connection "upgrade";
        # }
   }

   server {
       listen  443 ssl; #监听端口
       server_name  log.xxx.com; #请求域名
       ssl_certificate /etc/nginx/cert/inner.xxx.com.pem; #pem证书路径
       ssl_certificate_key     /etc/nginx/cert/inner.xxx.com.key; #pem证书key路径
       ssl_session_timeout     5m; #会话超时时间
       ssl_ciphers     ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4; #加密算法
       ssl_protocols   TLSv1 TLSv1.1 TLSv1.2; #SSL协议

      # 拦截所有请求
       location / {
            proxy_http_version 1.1; #代理使用的http协议
            proxy_set_header Host $host; #header添加请求host信息
            proxy_set_header X-Real-IP $remote_addr; # header增加请求来源IP信息
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 增加代理记录
            proxy_pass http://00.00.00.00:9000; #服务访问地址
        }

        # 拦截websocket请求
        location /websocket {
           proxy_pass http://00.00.00.00:9000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection "upgrade";
        }
   }

    include /etc/nginx/default.d/*.conf;
}
```