---
layout: post
title: Docker Overview
category: 技术概览
keywords: Docker 虚拟机 容器 镜像
tags: Docker
---

Docker很早之前就火起来了，但是我在读书的时候都没有主动去了解过，现在学习了下真是有种相见恨晚的感觉。这也侧面说明了时刻去接触一些新的知识对于程序员而言的重要性。

我一向不是特别喜欢琐碎的介绍一个东西是怎么组成怎么使用的。一个新的技术它的意义何在，为什么它能够在同类技术中崭露头角，这方面我觉得更具有思考的价值。本文侧重于介绍Docker是为了什么而诞生的，以及为什么Docker可以成为一项普遍为人所接受的技术，其主要的创新点在哪里。当然本文还是对Docker的使用进行了最基本的说明。希望读过本文的人可以对Docker有着清晰全面的认识。

### Docker的发展Docker的发展

一家专注于云服务，名为dotCloud的公司，于2013年早些时候开源了他们的Docker项目。Dokcer基于谷歌的Golang语言，一开始仅仅只是该公司为了在其数千台服务器可以快速开发部署而衍生的技术。开源后，Docker一举受到了开源社区的重视，成为了炙手可热的开源项目。不到一年，该公司雇佣了新的CEO，加入了Linux基金会，并改名为Docker Inc，并表示将公司的业务重点集中在Docker的发展上。

Docker的迅速崛起，使它成为事实上的业界标准，由此业界不得不为容器运行环境及格式发展出一套独立于任何组织的正式标准。2015 年，开放容器促进会（Open Container Initiative，OCI）终于成立，它是一个由 Docker、微软、CoreOS 以及多个重要团体组成的管理架构，目标就是要发展出这一标准。Docker的容器格式以及运行环境组成了这一工作的基石。

Docker诞生的初衷，是为了解决繁琐的环境依赖问题。大家平时应该都接触过，安装依赖的软件，配置环境搞了一周多，开发却只用了几天的情况。现在的软件开发过程中，往往都或多或少依赖于其他的服务和应用。而你所依赖的服务，可能又有他所依赖的其他服务组间。这是个令人感到极其痛苦的依赖链路过程。基于此，Docker初衷是为了解决以下问题：
1. 依赖冲突问题。可能你的Web应用依赖于PHP 4.3版本，而其他应用需要使PHP 5.5版本。Docker为每个应用隔离出其独有的容器环境从而解决这个问题。
2. 依赖缺失问题。Docker可以直接将某个应用连同应用所依赖的服务组件一起打包。运行打包后的容器，可以完全不需要配置安装任何应用所需要的依赖组件。
3. 平台差异问题。同样的容器只要在一个机器上可以顺利运行，则肯定可以在另一个机器正常运行。

借用Docker的创始人Solomon Hykes所说的话，Docker解决的问题就是
>For a developer, shipping code to the server is hard. 

Docker容器的快速启动性质有助于开发者快速验证功能，容器的可移植性于隔离特性使得部署以及开发者之间的协作成本大大降低。

### Docker与虚拟机Docker与虚拟机

虚拟机技术在原始的系统或硬件基础上，构建了独立系统的虚拟实例，每个虚拟实例底层拥有独立的操作系统。Docker则是底层共享一个OS内核，每一个应用享受独立隔离的环境，可以看作是应用访问了一个操作系统的拷贝。换句话说，虚拟机技术虚拟出硬件并在此基础上部署虚拟系统，Docker则是虚拟了操作系统使得应用在一套隔离的环境上运行。

![虚拟机架构](http://q435xx384.bkt.clouddn.com/a1.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:K93nKVpcdQZpYNG7B_xCQp6xto8=)

虚拟机会为每个虚拟机虚拟一套硬件设备，并在此之上运行一个完整的操作系统，虚拟访问主机资源。虚拟机的监视器可以直接作用于硬件（Xen），也可以作用在该机器的原生OS上（KVM）。每个虚拟机底层都虚拟了一个操作系统，应用和硬件之间需要额外消耗一些在虚拟的系统上，导致了虚拟机上的应用性能降低。总的来说，虚拟机要求的资源比应用程序需要的更多。

![Docker架构](http://q435xx384.bkt.clouddn.com/a2.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:aQGV_FiHRnOSzqXDNZiO84qwp3o=)

Docker中的容器底层共用的是一个操作系统，通过底层公用的kernel隔离每个进程的视角和资源，让每个应用有种独享当前系统的错觉。因为底层的系统是公用的，Docker的隔离性不及虚拟机，因此就安全性来说，虚拟机比Docker更加安全。但是相对而言，Docker的容器共用了底层的kernel，整体的镜像大小相对虚拟机而言小的多，并且启动仅有秒级，非常适合分布部署。

性能上，Docker直接运行底层kernel，虚拟机多了一层虚拟消耗，直观而言前者肯定比后者性能上更好。话虽如此，15年有篇文章，通过实验对xen、kvm和Docker进行了性能测试对比。实验结果表示这三者在实际的运行过程中，性能之间的差异可以说是非常之低。        

事实上，Docker和虚拟机并不是不可兼容的竞争关系。虚拟机本身的性能开销其实并不高，二者的混合使用可以同时享受这两者所带来的技术优势。通过在虚拟机运行容器，在拥有更好的安全性同时可以享受容器所带来的便于部署易于使用的特性。其中最具代表性的当属Intel的Clear Linux项目和Hyper。
### Docker与容器技术
Docker的容器化技术其实并不是Docker独创的。容器化技术的核心是基于同一个内核，让不同的应用之间完全隔离。其中主要的技术难点就是如何做到容器之间的隔离。不同的容器实现的隔离方法大部分的核心基本上都是通过cgroup和namespace达到的资源管理以及隔离的效果。

cgroup是一套用来限制进程资源的机制。与Linux进程类似，cgroup是基于层次结构组织的，子cgroup继承其父亲属性。 同进程的主要区别在于：cgroup不是同一棵树的一部分，它们中的许多树可以同时存在。 每个cgroup可用于管理一组进程的资源。 例如，可以在cgroup内部限制CPU时间或内存。 因此，cgroup可用于调整限制容器的资源。其中存在的一个问题就是，由于容器之间共享一个kernel，而运行于容器内的进程可以看到系统的所有资源，却无法感知到他的资源其实是被限制的。

namespace隔离了一个进程在系统中的视角。目前内核中提供了6种不同方面的namespace方便系统对进程进行视角的隔离：PID, IPC, NET, MNT, UTS 以及 USER。每一个namespace抽象了系统的资源，从而隔离不同namespace的进程。例如，通过USER的namespace，可以防止一个用户在某个容器内的权限延展到另一个容器中。

| Base|Container|Library|Kernel dependence|Other dependencies|
| ------------ | ------------ | ------------ | ------------ | ------------ |
| LXC|Docker|libcontainer|cgroups + namespaces + capabilities + kernel version 3.10 or above|iptables, perl, Apparmor, sqlite, GO|
|	|LXC|liblxc|cgroups + namespaces + capabilities|GO|
|	|LXD|liblxc|cgroups + namespaces + capabilities|LXC, GO|
|	|Rocket|AppContainer|cgroups + namespaces + capabilities + kernel version 3.8 or above|cpio, GO, squashfs, gpg|
|OpenVZ|OpenVZ|libCT|patched kernel|specific components: CRIU, ploop, VCMMD|

当前的各大容器化技术除了OpenVZ外主要是基于cgroup和namespace达到隔离效果。LXD是LXC的扩展。他们同Docker的主要区别在于，LXC想要通过容器化技术达成操作系统级别的容器，而Docker的容器则是倾向于提供一个应用级别的容器。一开始，Docker在LXC的基础上提供了简单的将应用打包成容器的功能，随着Docker发展，他们自己在cgroup和namespace之上封装了一个lib库libcontainer。

OpenVZ是一套用于在一个linux系统上跑多个虚拟环境虚拟私人服务器的技术。同LXC的理念很接近，Openvz将容器视为VPS，而Dokcer则将容器视为应用程序/服务。另外一个巨大的劣势是，Openvz并没有集成一个类似Kubernetes这种资源调度管理容器的系统。

Rocket和Docker之间的爱恨情仇这里有篇文章讲的非常清楚了[rocket vs docker](https://www.kubernetes.org.cn/2250.html "rocket vs docker ")，有兴趣的可以去瞅瞅。这里面涉及到Docker与容器编排机制k8s之间的发展史，也是一个非常大的话题了。

![联合挂载](http://q435xx384.bkt.clouddn.com/a3.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:eMFiWkdde5HRMc7gYtPFXThcCKg=)

Docker同其他容器技术不同的最创新性的一点在于，采用了联合挂载的方式构建和提交镜像。联合挂载系统允许多个文件系统叠加，并表现为一个单一的文件系统。镜像本身由多个只读层组成。当Docker部署一个镜像在镜像上时，容器以镜像本身为基础上，添加了一个读写层，所有的操作变化仅记录在最上面的读写层。当用户打算将容器打包成镜像时，只需要把读写层转化为只读层，加上那一层变化即可。

由于Docker的底层共用系统的kernel，本身包含了很多公用的代码库，且新的镜像可以基于旧的镜像之上，仅需添加一层读写层的变化，因此Docker的镜像大小只有兆级别，非常轻便。将镜像本身以类似github代码库的方式进行提交管理，使得Docker的传播分享方式非常便捷。

总的来说，Docker在虚拟化技术以及众多容器技术中能脱颖而出，在于其简化了“虚拟”。不同于传统的虚拟概念，Docker容器的理念是从提供一个系统，到提供一个应用服务。增量分层设计镜像的模式，使得Docker具有轻的特质，易于部署移植。由于轻，Dokcer可以在其使用上尽量的简化，比如类似github的方式拉取镜像提交镜像。Docker因此可以让社区以较小的成本加入，拥有极为方便的分享和发展模式。当越来越多开发者加入，Dokcker的发展就像是滚雪球一样不断壮大了。

![容器标准](http://q435xx384.bkt.clouddn.com/a4.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:YhMp3lvXNfXvbyNok08BuzraWDw=)

容器的流行，簇生了OCI的出现， 定义了容器运行时标准。runC 是 Docker 按照开放容器格式标准（OCF, Open Container Format）制定的一种具体实现。runC 是从 Docker 的 libcontainer 中迁移而来的，实现了容器启停、资源隔离等功能。Docker本身的功能也为了适配容器的辨准化做出了模块的抽象拆分。containerd定制了辨准化的容器接口，使得其不强依赖于某种特定的容器引擎，除Docker Engine外其余容器技术只要遵从OCF，便可以管理控制容器。
### Docker的结构与使用

![Docker结构](http://q435xx384.bkt.clouddn.com/a5.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:ZnKsua8Wx1n0LxmTqWqEKb0x52E=)

Docker采用的传统CS架构模式，用户通过Docker client与Docker daemon建立通信。Docker daemon是Docker架构重的主要用户接口。它接受client调用内部不同的功能模块提供Docker的功能。Docker daemon主要管理的模块可分为容器管理部分以及镜像管理。前者依赖的核心技术便是上述的容器隔离技术，而后者则是借助了联合挂载的能力集成Registry做到了镜像的统一管理分发。

Registry可以说是Docker能将容器类技术发扬广大的杀手级特性了。Docker通过增量分层设计镜像的模式，让镜像本身足够轻量化，易于分享传播。同时Docker官方维护了一个公共开放的平台Registry，让开发人员可以快速的发现下载并启用一个镜像。Registry可以是公共的也可以是私有的。Docker官方维护了一个类似github模式的Docker镜像Registry。本地client通过daemon同远端的Registry进行交互，以类似github的pull/push方式管理本地镜像。

![Docker工作流示例](http://q435xx384.bkt.clouddn.com/a6.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:kqLpj1jr1Kq1hhOxrTcoRxm1JQ8=)

Docker的镜像除了Docker官方提供的类似github开源库的Registry外，允许企业设置自身的私有库。整个项目的不同角色都可以通过Registry库拉取标准的镜像，并且推送修改后的镜像。当完成开发测试后，直接将通过验证后的项目镜像部署到云上即可。

![docker search](http://q435xx384.bkt.clouddn.com/a6.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:kqLpj1jr1Kq1hhOxrTcoRxm1JQ8=)

以本地run一个docker mysql数据库的容器为例。第一步docker serach mysql搜索registry上的镜像。从中选择并pull一个mysql镜像，由于没有指定版本我这里直接拉取了最新的mysql镜像。拉取过程中可以看到这个镜像是分了很多层，属于多个只读层的增量设计。

![docker image](http://q435xx384.bkt.clouddn.com/a8.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:L-3_qfR9UI4JkQV_DMILvE62X5o=)
![docker run](http://q435xx384.bkt.clouddn.com/a9.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:_KSgR2JQ8_NuE2XyuDCO729uWWY=)

Docker images可以显示本地的镜像。通过docker run，启动指定的镜像。其中-p 3301:3306代表host3301端口和容器3306的mysql默认端口的映射关系。-d表示后台运行容器。--names指定了容器的名字。docker ps可以看到刚刚跑的容器其对应的names为mysql。之后在host本地请求3301端口的mysql服务，可以验证mysql的容器处于正常运行状态。
### Docker的容器和镜像
Docker的命令基本都是围绕容器和镜像。通过明确容器和镜像之间的概念和区别，非常有助于理解大部分的Docker命令。

![docker 镜像](http://q435xx384.bkt.clouddn.com/a9.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:_KSgR2JQ8_NuE2XyuDCO729uWWY=)

镜像是仅由只读层叠加起来的只读性质的统一的视角。镜像的多个只读层，它们重叠在一起。除了最下面一层，其它层都会有一个指针指向下一层。

![docker 容器](http://q435xx384.bkt.clouddn.com/a11.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:0ZIzlotapAGVhZWmCKgZAcAYicE=)

容器和镜像几乎不存在区别，仅是在Image上加了一层读写层。这里并没有强调容器的当前状态，容器的概念在这里很接近程序。没有运行的程序是静态的，运行的程序则是进程。同理，容器也存在静止状态和运行状态。

![docker 运行状态的容器](http://q435xx384.bkt.clouddn.com/a12.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:XxuFSIsAeIanOy3x_xvWLT2kmI4=)

运行状态的容器则是联合挂载的文件系统，加上隔离的进程空间和包含其中的进程。一个容器中的进程可能会对文件进行修改、删除、创建，这些改变都仅会作用在容器上最上层的读写层。

![分层的layer](http://q435xx384.bkt.clouddn.com/a12.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:XxuFSIsAeIanOy3x_xvWLT2kmI4=)

这里简单描述下这个分层的文件系统。分层的系统中，每一层都会有指向下一只读层的指针信息。指针信息和当前只读层的元数据储存在每一层的MD（metadata）元数据中。这种结构不仅适用于只读层，同样也适用于读写层。

![三者关系](http://q435xx384.bkt.clouddn.com/a12.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:XxuFSIsAeIanOy3x_xvWLT2kmI4=)

上图可以非常直观的了解到这三者之间的区别和联系。Docker基于image，在其之上添加一层读写层创建一个容器。容器存在运行状态和非运行状态。启动一个容器使得容器进入运行状态，借助cgroups以及namespace的隔离技术，为容器构造一个隔离的运行环境并运行相关进程。

![build 镜像](http://q435xx384.bkt.clouddn.com/a15.png?e=1578990448&token=ykGI5dRw3vl2OmiLRcp3hI5G_FrkRqL7vGtV8mso:iixUYU_3-hilHVdhFId-VzpGnSk=)

如果想要build一个新的镜像，只需要将容器的最上层读写层做一次commit，将其转化为只读的只读层，生成一个比原来镜像多一层只读层的镜像。

参考资料

[Docker开发指南](https://www.itpanda.net/book/53 "Docker开发指南")

[Docker: Lightweight Linux Containers for Consistent Development and Deployment](https://www.seltzer.com/margo/teaching/CS508.19/papers/merkel14.pdf "Docker: Lightweight Linux Containers for Consistent Development and Deployment")

[KVM, Xen and Docker: a performance analysis for ARM based NFV and Cloud computing](https://s3.amazonaws.com/academia.edu.documents/51689706/raho2015.pdf?response-content-disposition=inline%3B%20filename%3DKVM_Xen_and_Docker_a_performance_analysi.pdf&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWOWYYGZ2Y53UL3A%2F20191230%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20191230T064319Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=02ed0eb328e1145ff88727f474a520c0eec9fa6fb5ebc1483229d1fce3d4dcdc "KVM, Xen and Docker: a performance analysis for ARM based NFV and Cloud computing")

[To Docker or not to Docker: a security perspective](https://www.researchgate.net/profile/Roberto_Pietro/publication/309965523_To_Docker_or_Not_to_Docker_A_Security_Perspective/links/5bd2f7c1a6fdcc3a8da6c537/To-Docker-or-Not-to-Docker-A-Security-Perspective.pdf "To Docker or not to Docker: a security perspective")

[5 Container Alternatives to Docker](https://containerjournal.com/topics/container-ecosystems/5-container-alternatives-to-docker/ "5 Container Alternatives to Docker")

[Docker、Containerd、RunC...：你应该知道的所有](https://www.infoq.cn/article/2017/02/Docker-Containerd-RunC "Docker、Containerd、RunC...：你应该知道的所有")

[docker的容器和镜像](http://dockone.io/article/783 "docker的容器和镜像")
