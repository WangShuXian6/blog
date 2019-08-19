#### K8S 的C/S 架构

>集群的 Master 包含着几个重要的组成部分，比如 API Server, Controller Manager 等。

>而 Node 上，则运行着三个必要的组件 kubelet, container runtime (一般是 Docker), kube-proxy 。

>通过所有组件的分工协作，最终实现了 K8S 对容器的编排和调度。

***

>Node 

>用于工作的服务器，它有一些状态和信息，当这些条件都满足一些条件判断时，Node 便处于 Ready 状态，可用于执行后续的工作。

>可以是一台物理机，也可以是虚拟机

>Deployment 

>对期望状态的描述，能提供对 index.html 访问的服务

>Pod 作为集群中可调度的最小单元，Nginx 和 index.html 这个组合

>Pod 可以是一组容器（也可以包含存储卷）

>Docker 是我们选择的容器运行时，可运行我们构建的服务镜像，减少在环境方面所做的重复工作，并且也非常便于部署。

>Container Runtime

***
>K8S 整体上遵循 C/S 架构，

![1546185730506](https://user-images.githubusercontent.com/30850497/50548908-67a16d80-0c8f-11e9-9c7b-1373d4cbd729.jpg)

>左侧是一个官方提供的名为 kubectl 的 CLI （Command Line Interface）工具，用于使用 K8S 开放的 API 来管理集群和操作对象等。

>右侧则是 K8S 集群的后端服务及开放出的 API 等。根据上一节的内容，我们知道 Node 是用于工作的机器，而 Master 是一种角色（Role），表示在这个 Node 上包含着管理集群的一些必要组件。

>当然在这里，只画出了一个 Master，在生产环境中，为了保障集群的高可用，我们通常会部署多个 Master 。
***

##### Master
>整个 K8S 集群的“大脑”

> - 接收：外部的请求和集群内部的通知反馈
> - 发布：对集群整体的调度和管理
> - 存储：存储

>这些功能，也通过一些组件来共同完成，通常情况下称为 control plane


![control plane](https://user-images.githubusercontent.com/30850497/53138560-c2858000-35c1-11e9-9937-965210fae739.jpg)


#####  control plane

>Cluster state store

>API Server

>Controller Manager

>Scheduler

>Node

>Kubelet

>Container runtime

>Kube Proxy

##### Cluster state store
>存储集群所有需持久化的状态，并且提供 watch 的功能支持，可以快速的通知各组件的变更等操作。

>因为目前 Kubernetes 的存储层选择是 etcd ，所以一般情况下，大家都直接以 etcd 来代表集群状态存储服务。即：将所有状态存储到 etcd 实例中。

##### API Server
>整个集群的入口，接收外部的信号和请求，并将一些信息写入到 etcd 中。

>处理逻辑

> - 请求 API Server ：“嗨，我有些东西要放到 etcd 里面”
> - API Server 收到请求：“你是谁？我为啥要听你的”
> - 从请求中，拿出自己的身份凭证（一般是证书）：“是我啊，你的master，给我把这些东西放进去”
这时候就要看是些什么内容了，如果这些内容 API Server 能理解，那就放入 etcd 中 “好的 master 我放进去了”；如果不能理解，“抱歉 master 我理解不了”

>提供了认证相关的功能，用于判断是否有权限进行操作。

>支持多种认证方法，不过一般情况下，我们都使用 x509 证书进行认证。

>API Server 的目标是成为一个极简的 server，只提供 REST 操作，更新 etcd ，并充当着集群的网关。至于其他的业务逻辑之类的，通过插件或者在其他组件中完成。

##### Controller Manager
>在后台运行着许多不同的控制器进程，用来调节集群的状态。

>当集群的配置发生变更，控制器就会朝着预期的状态开始工作。

##### Scheduler
>集群的调度器，它会持续的关注集群中未被调度的 Pod ，并根据各种条件，比如资源的可用性，节点的亲和性或者其他的一些限制条件，通过绑定的 API 将 Pod 调度/绑定到 Node 上。

>在这个过程中，调度程序一般只考虑调度开始时， Node 的状态，而不考虑在调度过程中 Node 的状态变化 ,比如节点亲和性等

##### Node
>加入集群中的机器

>运行在 Node 上的几个核心组件
![node](https://user-images.githubusercontent.com/30850497/50549127-34141280-0c92-11e9-8323-10d0ac294ef6.jpg)

##### Kubelet
>Kubelet 实现了集群中最重要的关于 Node 和 Pod 的控制功能，如果没有 Kubelet 的存在，那 Kubernetes 很可能就只是一个纯粹的通过 API Server CRUD 的应用程序。

>K8S 原生的执行模式是操作应用程序的容器，而不像传统模式那样，直接操作某个包或者是操作某个进程。基于这种模式，可以让应用程序之间相互隔离，互不影响。此外，由于是操作容器，所以应用程序可以说和主机也是相互隔离的，毕竟它不依赖于主机，在任何的容器运行时（比如 Docker）上都可以部署和运行。

>Pod 可以是一组容器（也可以包含存储卷），K8S 将 Pod 作为可调度的基本单位， 分离开了构建时和部署时的关注点：

> - 构建时，重点关注某个容器是否能正确构建，如何快速构建
> - 部署时，关心某个应用程序的服务是否可用，是否符合预期，依赖的相关资源是否都能访问到

>这种隔离的模式，可以很方便的将应用程序与底层的基础设施解耦，极大的提高集群扩/缩容，迁移的灵活性。

>Master 节点的 Scheduler 组件，它会调度未绑定的 Pod 到符合条件的 Node 上，而至于最终该 Pod 是否能运行于 Node 上，则是由 Kubelet 来裁定的。

##### Container runtime
>容器运行时最主要的功能是下载镜像和运行容器，我们最常见的实现可能是 Docker , 目前还有其他的一些实现，比如 rkt, cri-o。

>K8S 提供了一套通用的容器运行时接口 CRI (Container Runtime Interface), 凡是符合这套标准的容器运行时实现，均可在 K8S 上使用。

##### Kube Proxy
>每个 Pod 在创建后都会有一个虚拟 IP，

>K8S 中有一个抽象的概念，叫做 Service ，kube-proxy 便是提供一种代理的服务，让你可以通过 Service 访问到 Pod。

>实际的工作原理是在每个 Node 上启动一个 kube-proxy 的进程，通过编排 iptables 规则来达到此效果。

***
>K8S 的整体遵循 C/S 架构，集群的 Master 包含着几个重要的组成部分，比如 API Server, Controller Manager 等。

>而 Node 上，则运行着三个必要的组件 kubelet, container runtime (一般是 Docker), kube-proxy 。

>通过所有组件的分工协作，最终实现了 K8S 对容器的编排和调度。