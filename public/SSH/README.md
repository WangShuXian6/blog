#### SSH

>ssh -p 2222 user@host

***

#### authorized_keys文件
>远程主机将用户的公钥，保存在登录后的用户主目录的$HOME/.ssh/authorized_keys文件中。公钥就是一段字符串，只要把它追加在authorized_keys文件的末尾就行了

>在根目录下创建.ssh文件夹

```bash
mkdir .ssh
```
>创建authorized_keys文件

```bash
cd .ssh
touch authorized_keys
```
>将公钥 id_rsa.pub 内容追加到 authorized_keys 末尾

```bash
vi authorized_keys
wq!
```
>centos 7 重启远程主机的ssh服务

```bash
systemctl restart sshd
```

>注：如遇到无法登陆，删除当前用户的.ssh文件夹后重建

>登陆

```bash
ssh -i ~/.ssh/id_rsa -p xxxxx root@222.222.222.222
```