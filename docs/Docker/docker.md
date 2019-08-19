#### docker
>Docker 的实现，主要归结于三大技术：命名空间 ( Namespaces ) 、控制组 ( Control Groups ) 和联合文件系统 ( Union File System )

>命名空间的主要目的就是为了集合相同模块的类，区分不同模块间的同名类

>资源控制组 ( CGroups ) :控制硬件资源的分配

>联合文件系统 ( Union File System ) 是一种能够同时挂载不同实际文件或文件夹到同一目录，形成一种联合文件结构的文件系统。

>AUFS ( Advanced Union File System )将文件的更新挂载到老的文件之上，而不去修改那些不更新的内容

>在 Docker 里，最佳的实践是分别基于 Apache、MySQL 和 PHP 的镜像建立三个容器，分别运行 Apache、MySQL 和 PHP ，而它们所在的虚拟操作系统也直接共享于宿主机的操作系统。

>加入 Docker 的工作流程将更加适合持续集成 ( Continuous Integration ) 和持续交付 ( Continuous Delivery )。

>Docker 镜像-每次对镜像内容的修改，Docker 都会将这些修改铸造成一个镜像层，而一个镜像其实就是由其下层所有的镜像层所组成的。当然，每一个镜像层单独拿出来，与它之下的镜像层都可以组成一个镜像。

#### Docker 的容器应该有三项内容组成：
 - >一个 Docker 镜像
 - >一个程序运行环境
 - >一个指令集合

>数据卷 ( Volume ) 在 UnionFS 的加持下，除了能够从宿主操作系统中挂载目录外，还能够建立独立的目录持久存放数据，或者在容器间共享。

>Docker Engine - docker daemon 和 docker CLI 所组成的，正是一个标准 C/S ( Client-Server ) 结构的应用程序。衔接这两者的，正是 docker daemon 所提供的这套 RESTful API。

>社区版 ( Docker Engine CE, Community Edition ) 主要提供了 Docker 中的容器管理等基础功能，主要针对开发者和小型团队进行开发和试验。

>企业版 ( Docker Engine EE, Enterprise Edition ) 则在社区版的基础上增加了诸如容器管理、镜像管理、插件、安全等额外服务与功能，为容器的稳定运行提供了支持，适合于中大型项目的线上运行。...

>稳定版 ( Stable release ) 和预览版 ( Edge release )
>次要版本的版本号由主要版本和发布序号组成，如：17.03.2 就是对 17.03 版本的第二次修正。

>在 Windows 中，我们可以通过 Hyper-V 实现虚拟化，而在 macOS 中，我们可以通过 HyperKit 实现虚拟化。
>Docker for Windows 和 Docker for Mac 这里利用了这两个操作系统提供的功能来搭建一个虚拟 Linux 系统，并在其之上安装和运行 docker daemon。