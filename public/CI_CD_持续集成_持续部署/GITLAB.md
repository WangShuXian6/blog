#### GITLAB
>https://docs.gitlab.com/ce/README.html

##### gitlab 安装
>https://www.youtube.com/watch?v=eBOvoy6ThB0

>gitlab mac 安装教程：

>https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/doc/docker/README.md

>gitlab 其他平台安装教程：

>https://about.gitlab.com/install/

>docker 安装gitlab 命令:

```bash
sudo docker run --detach \
--hostname gitlab.example.com \
--publish 443:443 --publish 80:80 --publish 22:22 \
--name gitlab \
--restart always \
--volume /Users/wangshuxian/gitlab/config:/etc/gitlab \
--volume /Users/wangshuxian/gitlab/logs:/var/log/gitlab \
--volume /Users/wangshuxian/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ce:latest
```

>也可使用企业版 gitlab/gitlab-ee:latest

>hostname 可以为内网或本机，万维网地址。

>本机地址需在Host映射 127.0.0.1 gitlab.example.com

>局域网需在Host映射 局域网ip gitlab.example.com

>万维网同上

>volume 为本机文件夹，手动创建

>wangshuxian 对应系统当前账户文件夹

>首次安装指定安装源，以后只使用本机命命 gitlab

>首次安装后docker后台配置需要等待较长时间

>首次登陆 gitlab.example.com 需要设置管理员root密码

>可以继续使用root登陆，或者注册新账户

***
```bash
sudo docker run --detach \
--hostname gitlab.xxx.com \
--publish 4433:443 --publish 8080:80 --publish 2222:22 \
--name gitlab \
--restart always \
--volume ~/gitlab/config:/etc/gitlab \
--volume ~/gitlab/logs:/var/log/gitlab \
--volume ~/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ee:latest
```

***
##### gitlab 启用 https
>https://docs.gitlab.com/omnibus/settings/nginx.html#redirect-http-requests-to-https


>将证书文件上传至gitlab xxx.key xxx.pem

```bash
scp -P端口号 本地文件路径 username@服务器ip:目的路径
```

>编辑 gitlab.rb

```bash
cd ~/gitlab/config
vi gitlab.rb
```
>修改gitlab配置文件

```txt
external_url 'https://xxx.xxx.com/'
nginx['redirect_http_to_https'] = true
nginx['ssl_certificate'] = "/etc/gitlab/xxx.pem"
nginx['ssl_certificate_key'] = "/etc/gitlab/xxx.key"
gitlab_rails['time_zone'] = 'Asia/Shanghai'
```

>进入容器交互命令行

```bash
docker exec -it gitlab bash
```
>使配置生效

```bash
gitlab-ctl reconfigure
```
***