### 基于 Docker for MAC 的 Kubernetes 本地环境搭建与应用部署
>https://github.com/maguowei/k8s-docker-for-mac

>https://zhuanlan.zhihu.com/p/32978449

>在 Deamon 选项中 设置 Docker 中国官方镜像加速 registry mirror https://registry.docker-cn.com

>修改 ./images 文件定制你自己需要的镜像

```bash
./load_images.sh
```
```bash

k8s.gcr.io/kube-proxy-amd64:v1.10.3=registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy-amd64:v1.10.3
k8s.gcr.io/kube-controller-manager-amd64:v1.10.3=registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager-amd64:v1.10.3
k8s.gcr.io/kube-scheduler-amd64:v1.10.3=registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler-amd64:v1.10.3
k8s.gcr.io/kube-apiserver-amd64:v1.10.3=registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver-amd64:v1.10.3
k8s.gcr.io/k8s-dns-dnsmasq-nanny-amd64:1.14.8=registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-dnsmasq-nanny-amd64:1.14.8
k8s.gcr.io/k8s-dns-sidecar-amd64:1.14.8=registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-sidecar-amd64:1.14.8
k8s.gcr.io/k8s-dns-kube-dns-amd64:1.14.8=registry.cn-hangzhou.aliyuncs.com/google_containers/k8s-dns-kube-dns-amd64:1.14.8
k8s.gcr.io/pause-amd64:3.1=registry.cn-hangzhou.aliyuncs.com/google_containers/pause-amd64:3.1
k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.1=registry.cn-hangzhou.aliyuncs.com/google_containers/kubernetes-dashboard-amd64:v1.10.1
k8s.gcr.io/etcd-amd64:3.1.12=registry.cn-hangzhou.aliyuncs.com/google_containers/etcd-amd64:3.1.12
gcr.io/kubernetes-helm/tiller:v2.12.1=registry.cn-hangzhou.aliyuncs.com/google_containers/tiller:v2.12.1
```

>或者使用代理下载

***

##### 可选的步骤: 切换Kubernetes运行上下文至 docker-for-desktop
> 一般只有在之前用其他方式运行过Kubernetes才需要

```bash
$ kubectl config use-context docker-for-desktop
```
***
##### 验证 Kubernetes 集群状态
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
```
***
##### 开启本机访问代理
```bash
$ kubectl proxy
```
***
##### 访问 Dashboard: 
>http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

***
##### 创建Dashboard管理员用户并用Token登陆: >https://github.com/kubernetes/dashboard/wiki/Creating-sample-user

>创建 dashboard-adminuser.yaml

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kube-system
```
>执行

```bash
kubectl apply -f dashboard-adminuser.yaml
```

>获取登陆用的token

```bash
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')
```

>使用 token 登陆
***