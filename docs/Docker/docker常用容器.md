#### docker  常用容器

#### nginx
>https://store.docker.com/images/nginx

>命名为 nginx

>将本机 9090 端口映射到 nginx 容器 80 端口

>将本机 命令行当前目录下的 /lnmp/www/ 目录 映射到 nginx 容器的 /usr/share/nginx/html 目录

```bash
docker login
docker pull nginx
docker run -p 9090:80 --name nginx -v $PWD/lnmp/www/:/usr/share/nginx/html:ro -d nginx
```
***
#### lnmp
>https://store.docker.com/community/images/twang2218/lnmp-nginx

```bash
docker run -p 9090:80 --name nginx -v $PWD/lnmp/www/:/usr/share/nginx/html:ro -v $PWD/lnmp/nginx.conf:/etc/nginx/nginx.conf:ro -d nginx
```