#### Kubernetes
>https://kubernetes.io/zh/

>https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl

>[kubə’netis]，重音在第三个音节，读音：库伯耐踢死

>Kubernetes 是用于自动部署，扩展和管理容器化应用程序的开源系统。

>它将组成应用程序的容器组合成逻辑单元，以便于管理和服务发现，Kubernetes 构建在 Google 15 年生产环境经验基础之上,并结合来自社区的最佳创意和实践。

>Kubernetes 是一个可扩展的，用于容器化应用程序编排，管理的平台

>工作在容器层而不是硬件层

>提供了一些与 PasS 类似或者共同的功能，类似部署，扩容，监控，负载均衡，日志记录等。然而它并不是个完全一体化的平台，这些功能基本都是可选可配置的。

>Kubernetes 可支持公有云，私有云及混合云等，具备良好的可移植性。我们可直接使用它或在其之上构建自己的容器/云平台，以达到快速部署，快速扩展，及优化资源使用等。

>提供通用接口类似 CNI( Container Network Interface ), CSI（Container Storage Interface）, CRI（Container Runtime Interface）等规范，

***
#### 容器编排工具
>Kubernetes,

>Mesos,

>Docker 自家的 Swarm

>较为简单的是 Swarm, 因为它本身只专注于容器编排，并且是官方团队所作，从各方面来看，对于新手都相对友好一些。但如果是用于生产中大规模使用，反而就略有不及。

>Mesos 也并不仅限于容器编排，它的创建本身是为了将数据中心的所有资源进行抽象，比如 CPU，内存，网络，存储等，将整个 Mesos 集群当作是一个大的资源池，允许各种 Framework 来进行调度。比如，可以使用 Marathon 来实现 PaaS，可以运行 Spark，Hadoop 等进行计算等。同时因为它支持比如 Docker 或者 LXC 等作资源隔离，所以前几年也被大量用于容器编排。

>随着 Kubernetes 在目前的认可度已经超过 Mesos， Docker Swarm 等，无疑它是生产环境中容器应用管理的不二之选。

***
>到现在做技术选型，其实你选择的不仅是 K8S 本身，更重要的是它的生态。现在通常都会讲 K8S 是云原生应用的基石，其实就是生态已经壮大了，无论是社区，还是云厂商。再有一点，Mesos 和 K8S 原本关注的点是不一样的，Mesos 重点在于抽象资源和调度，Mesos 有个 K8S on Mesos 的计划（具体进展没有太关注了）。好处的话，主要是生态，其次是 K8S 的初始目标本就是在容器编排上，专业的工具做专业的事儿。