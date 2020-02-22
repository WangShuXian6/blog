#### linux 安装 docker

>需要64位系统

>https://docs.docker.com/install/linux/docker-ce/centos/#install-using-the-repository

>卸载旧版Docker

##### 查询当前的Docker版本

```bash
docker -v
```

##### 如果存在已安装的Docker，卸载

```bash
sudo yum remove docker \
docker-common \
docker-selinux \
docker-engine
```

>卸载后，/ var / lib / docker / 下内容（images, containers, volumes,networks）依然被保留

***

##### 安装所需的软件包 yum-utils、device-mapper-persistent-data和 lvm2

```bash
sudo yum install -y yum-utils \
device-mapper-persistent-data \
lvm2
```

##### 设置稳定的库

```bash
sudo yum-config-manager \
--add-repo \
https://download.docker.com/linux/centos/docker-ce.repo
```

##### 启用 docker-ce-edge和docker-ce-test（可选）

```bash
sudo yum-config-manager --enable docker-ce-edge
sudo yum-config-manager --enable docker-ce-test
```

#### 通过yum安装Docker CE

>安装最新版本的Docker CE（如果需要安装特定的版本跳过此步骤)

```bash
sudo yum install docker-ce
```

>查看库中可用的版本，安装特定的版本

>FULLY-QUALIFIED-PACKAGE-NAME为docker-ce-详细版本号，

>如docke-ce-17.06.2.ce-1.el7.centos

```bash
yum list docker-ce.x86_64  --showduplicates | sort -r

sudo yum install <FULLY-QUALIFIED-PACKAGE-NAME>
```

***

##### 启动docker

```bash
sudo systemctl start docker
```
##### 验证Docker是否正确安装

```bash
sudo docker run hello-world
```
##### 将服务加入到启动项

```bash
sudo chkconfig docker on
```
##### 卸载 Docker CE

```bash
sudo yum remove docker-ce

sudo rm -rf /var/lib/docke
```

***

#### 在 Linux 系统中安装 Docker

##### CentOS

```bash
$ sudo yum install yum-utils device-mapper-persistent-data lvm2
$
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
$ sudo yum install docker-ce
$
$ sudo systemctl enable docker
$ sudo systemctl start docker...
```

##### Debian

```bash
$ sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
$
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce
$
$ sudo systemctl enable docker
$ sudo systemctl start docker...
```

##### Fedora

```bash
$ sudo dnf -y install dnf-plugins-core
$
$ sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
$ sudo dnf install docker-ce
$
$ sudo systemctl enable docker
$ sudo systemctl start docker...
```

##### Ubuntu

```bash
sudo apt-get update
$ sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
$
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce
$
$ sudo systemctl enable docker
$ sudo systemctl start docker...
```

#### 启动 Docker 服务

```bash
sudo systemctl start docker
```

####  Docker 服务开机自启动

```bash
sudo systemctl enable docker
```

#### Docker 版本

```bash
docker version
```

#### Docker Engine 更多相关的信息

```bash
docker info
```

#### 配置国内镜像源

>在 Linux 环境下，我们可以通过修改 /etc/docker/daemon.json ( 如果文件不存在，你可以直接创建它 ) 这个 Docker 服务的配置文件达到效果。

```JSON
{
    "registry-mirrors": [
        "https://registry.docker-cn.com"
    ]
}
```
#### 重新启动 docker daemon 来让配置生效

```bash
systemctl restart docker
```
