---
title: k8s
toc: true
recommend: 1
uniqueId: '2020-05-16 06:59:37/"k8s".html'
date: 2020-05-16 14:59:37
thumbnail:
categories:
- docker k8s
tags:
keywords:
---

[TOC]

<!--more-->

# 学习

## **从零开始入门 K8s

[K8s 资源全汇总 | K8s 大咖带你 31 堂课从零入门 K8s - 掘金](https://juejin.im/post/5ea7f9ef5188256d9c25988e)

### 课时3. k8s 核心概念

#### 核心功能

调度、自动修复、水平伸缩

服务的发现与负载的均衡；
**容器的自动装箱--scheduling--调度**，把一个容器放到一个集群的某一个机器上，Kubernetes 会帮助我们去做存储的编排，让存储的声明周期与容器的生命周期能有一个连接；
**自动化的容器的恢复**。在一个集群中，经常会出现宿主机的问题或者说是 OS 的问题，导致容器本身的不可用，Kubernetes 会自动地对这些不可用的容器进行恢复；
Kubernetes 会帮助我们去做**应用的自动发布与应用的回滚**，以及与应用相关的配置密文的管理；
对于 job 类型任务，Kubernetes 可以去做批量的执行；
为了让这个集群、这个应用更富有弹性，Kubernetes 也**支持水平的伸缩**。

#### Kubernetes 的架构

<img src="https://i.loli.net/2020/05/19/Mk3d2bi7JwPyHQx.png" alt="5728b58a8d2cf6c2be789bca5acbb77dde9c4d4c" style="zoom: 25%;" />



<img src="https://i.loli.net/2020/05/19/Oi2e5o8jgNauMls.png" alt="b161e11eae280d950de7259873193f1a9359b97f" style="zoom: 25%;" />





<img src="https://i.loli.net/2020/05/19/ORNK3QshCVWFMpB.png" alt="9dbdfbe3b7068f32d95b02dc2dd29302c82c9e3e" style="zoom: 25%;" />



#### 核心概念

<img src="https://i.loli.net/2020/05/19/vRxHMos7wycODZu.png" alt="616cc75b2ce8d734d6389f521c16f4d1d6f56dff" style="zoom:25%;" />



<img src="https://i.loli.net/2020/05/19/paTk4l9nbVUgsqC.png" alt="5d7c4142358a29dee77740cb9ebe978b04aec2eb" style="zoom:25%;" />



<img src="https://i.loli.net/2020/05/19/VoYe3G6EOsq25WK.png" alt="70fdf18aac3c814fff9c0a6f2a83dd9eed29f387" style="zoom:25%;" />



<img src="https://i.loli.net/2020/05/19/o17wgJbYSUNBzd8.png" alt="ac386185dd887052ed3236a05c82cb9e565733fd" style="zoom:25%;" />



<img src="https://i.loli.net/2020/05/19/9KRm6YNWyl8sMDF.png" alt="8995bce3a619f4e182fd65410ddddf477cbf869a" style="zoom:25%;" />







一个比较有意思的 metadata 叫做“label”

这些 label 是可以被 selector，也就是选择器所查询的。这个能力实际上跟我们的 sql 类型的 select 语句是非常相似的

<img src="https://i.loli.net/2020/05/19/P8ntC5p6ydsYxjR.png" alt="1_17" style="zoom:25%;" />





### 课时4. Pod 与容器设计模式

[从零入门 K8s| 人人都能看懂 Pod 与容器设计模式](https://mp.weixin.qq.com/s/OW7zvGhPgGAnBuo4A_SXFw)

#### 为什么需要 Pod

**容器的基本概念**

容器的本质实际上是一个进程，是一个视图被隔离，资源受限的进程。很多人都说 Kubernetes 是云时代的操作系统，这个非常有意思，因为如果以此类推，容器镜像就是这个操作系统的软件安装包。

**真实操作系统里的例子**

如果说 Kubernetes 就是操作系统的话，那么不妨看一下真实的操作系统的例子。

例子里面有一个程序叫做 Helloworld，这个 Helloworld 程序实际上是由一组进程组成的，需要注意一下，这里说的进程实际上等同于 Linux 中的线程。

因为 Linux 中的线程是轻量级进程，所以如果从 Linux 系统中去查看 Helloworld 中的 pstree，将会看到这个 Helloworld 实际上是由四个线程组成的，分别是 {api、main、log、compute}。也就是说，四个这样的线程共同协作，共享 Helloworld 程序的资源，组成了 Helloworld 程序的真实工作情况。

这是操作系统里面进程组或者线程组中一个非常真实的例子，以上就是进程组的一个概念。

在真实的操作系统里面，一个程序往往是根据进程组来进行管理的。Kubernetes 把它类比为一个操作系统，比如说 Linux。针对于容器我们前面提到可以类比为进程，就是前面的 Linux 线程。那么 Pod 又是什么呢？实际上 Pod 就是我们刚刚提到的进程组，也就是 Linux 里的线程组。

**进程组概念**

若为了管理很多个进程，直接把容器里 PID=1 的进程直接改成 systemd，那么会有一些问题：

首先，PID=1 进程是应用本身，如果现在把这个 PID=1 的进程给 kill 了，或者它自己运行过程中死掉了，那么剩下三个进程的资源就没有人回收了。

其次，不能知道应用是不是退出，fail， 异常

**Pod = “进程组”**

在 Kubernetes 里面，Pod 实际上正是 Kubernetes 项目为你抽象出来的一个可以类比为进程组的概念。

Pod 在 Kubernetes 里面只有一个逻辑单位，没有一个真实的东西对应说这个就是 Pod。

真正起来在物理上存在的东西，就是容器

**为什么 Pod 必须是原子调度单位？**

为了解决Task co-scheduling 问题，具体分析过程见原文

**再次理解 Pod**

Pod 里面的容器是“超亲密关系”

超亲密关系大概分为以下几类：

- 比如说两个进程之间会发生**文件交换**，前面提到的例子就是这样，一个写日志，一个读日志；
- 两个进程之间需要通过 localhost 或者说是本地的 Socket 去进行**通信**，这种本地通信也是超亲密关系；
- 这两个容器或者是微服务之间，需要发生非常**频繁的 RPC 调用**，出于性能的考虑，也希望它们是超亲密关系；
- 两个容器或者是应用，它们需要**共享某些 Linux Namespace**。最简单常见的一个例子，就是我有一个容器需要加入另一个容器的 Network Namespace。这样我就能看到另一个容器的网络设备，和它的网络信息。

**Pod 要解决的问题**

具体的解法分为两个部分：网络和存储

- **共享网络**
- 比如说现在有一个 Pod，其中包含了一个容器 A 和一个容器 B，它们两个就要共享 Network Namespace。在 Kubernetes 里的解法是这样的：在每个 Pod 里，额外起一个 Infra container 小容器来共享整个 Pod 的  Network Namespace。（Infra container 是一个非常小的镜像，大概 100~200KB 左右。所以说一个 Pod 里面的所有容器，它们看到的网络视图是完全一样的。即：它们看到的网络设备、IP地址、Mac地址等等，跟网络相关的信息，其实全是一份）
- 所以整个 Pod 里面，必然是 Infra container 第一个启动。并且整个 **Pod 的生命周期是等同于 Infra container 的生命周期**的，与容器 A 和 B 是无关的。这也是为什么在 Kubernetes 里面，**它是允许去单独更新 Pod 里的某一个镜像**的，即：做这个操作，整个 Pod 不会重建，也不会重启，这是非常重要的一个设计。

- **共享存储**
- 比如说现在有两个容器，一个是 Nginx，另外一个是非常普通的容器，在 Nginx 里放一些文件，让我能通过 Nginx 访问到。所以它需要去 share 这个目录。我 share 文件或者是 share 目录在 Pod 里面是非常简单的，实际上就是把 volume 变成了 Pod level。然后所有容器，就是所有同属于一个 Pod 的容器，他们共享所有的 volume。

#### 详解容器设计模式

**举例**

有一个 WAR 包需要把它放到 Tomcat 的 web APP 目录下面，这样就可以把它启动起来了。可是像这样一个 WAR 包或 Tomcat 这样一个容器的话，怎么去做，怎么去发布

第一种方式：可以把 WAR 包和 Tomcat 打包放进一个镜像里面。但是这样带来一个问题，就是现在这个镜像实际上揉进了两个东西。那么接下来，无论是我要更新 WAR 包还是说我要更新 Tomcat，都要重新做一个新的镜像，这是比较麻烦的；

第二种方式：就是**镜像里面只打包 Tomcat**。它就是一个 Tomcat，但是需要使用数据卷的方式，比如说 hostPath，从宿主机上把 WAR 包挂载进我们 Tomcat 容器中，挂到我的 web APP 目录下面，这样把这个容器启用起来之后，里面就能用了。

**InitContainer**

在上图的 yaml 里，首先定义一个 Init Container，它只做一件事情，就是把 WAR 包从镜像里拷贝到一个 Volume 里面，它做完这个操作就退出了，所以 Init Container 会比用户容器先启动，并且严格按照定义顺序来依次执行

**容器设计模式：Sidecar**

在 Pod 里面，可以定义一些专门的容器，来执行主业务容器所需要的一些辅助工作，比如我们前面举的例子，其实就干了一个事儿，这个 Init Container，它就是一个 Sidecar，它只负责把镜像里的 WAR 包拷贝到共享目录里面，以便被 Tomcat 能够用起来。



### 课时5. 应用编排与管理(一)



### 课时6. 应用编排与管理(二)



### 课时7. 应用编排与管理(三)：Job & DaemonSet



### 课时8. 应用配置管理



### 课时9. 应用存储和持久化数据卷：核心知识



### 课时10. 应用存储和持久化数据卷：存储快照与拓扑调度

### 

# 实践



## Kubectl 安装、启动

### centos

安装失败

[How to Install Kubernetes(k8s) with Minikube on CentOS 8](https://www.linuxtechi.com/install-kubernetes-k8s-minikube-centos-8/)

```shell
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl"

curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.18.2/bin/darwin/amd64/kubectl"
# 没有代理的话，很慢，可以本地下载好，传到服务器
# https://storage.googleapis.com/kubernetes-release/release/v1.18.2/bin/darwin/amd64/kubectl
# rsync -azvhP kubectl root@47.93.53.47:/root/k8s

chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl
kubectl version --client


```



#### Minikube

```shell
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube
mkdir -p /usr/local/bin/
install minikube /usr/local/bin/

# 用无需虚拟机的方式启动
minikube start --driver=none

```



### Mac

[How to Install Kubernetes on Mac with Docker, Minikube, VirtualBox - Kubernetes Book](https://matthewpalmer.net/kubernetes-app-developer/articles/guide-install-kubernetes-mac.html)

/usr/local/bin/kubectl is v1.15.5, which may be incompatible with Kubernetes v1.18.2

```shell
export HOMEBREW_NO_AUTO_UPDATE=true
brew install kubectl
brew cask install virtualbox
brew install minikube
brew install bash-completion

minikube start --image-mirror-country cn \
    --iso-url=https://kubernetes.oss-cn-hangzhou.aliyuncs.com/minikube/iso/minikube-v1.9.0.iso \
    --registry-mirror=https://vbd8z16m.mirror.aliyuncs.com
minikube dashboard
```



```
# 自动补全
# 编辑 ~/.zshrc 文件
if [ $commands[kubectl] ]; then
  source <(kubectl completion zsh)
fi
# 2. 更新 plugins= 行以包含 kubectl 插件
plugins=(kubectl)

source ~/.zshrc
tail -n 5 ~/.zshrc
```

```shell
# minikube 可选参数
minikube start \
  --kubernetes-version v1.18.0 \
  --vm-driver=<driver_name> \
  --extra-config=kubelet.MaxPods=5
```

minikube stop 命令可用于停止集群

minikube delete 命令可用于删除集群

此命令将关闭并删除 Minikube 虚拟机，不保留任何数据或状态

minikube status 查看集群是否正常


### 停止k8s

<img src="https://i.loli.net/2020/05/19/sMSRDOwT654lK1j.png" alt="image-20200519222349284" style="zoom:33%;" />



### demo

```shell
1.提交一个 nginx deployment
kubectl apply -f https://k8s.io/examples/application/deployment.yaml

2.升级 nginx deployment
kubectl apply -f https://k8s.io/examples/application/deployment-update.yaml

3.扩容 nginx deployment
kubectl apply -f https://k8s.io/examples/application/deployment-scale.yaml

kubectl describe deployment nginx-deployment
kubectl get --watch deployments

kubectl delete deployment nginx-deployment
kubectl delete deployments --all
kubectl delete pods --all
```

[Kubernetes kubectl delete 命令详解 _ Kubernetes(K8S)中文文档_Kubernetes中文社区](http://docs.kubernetes.org.cn/618.html)



# 工具

## *配置高效的 Kubernetes 命令行终端

[如何配置高效的 Kubernetes 命令行终端 - 陈少文的博客](https://www.chenshaowen.com/blog/how-to-configure-efficient-k8s-terminal.html)

[交互式Kubernetes客户端，搭建高效Kubernetes命令行终端_Linux命令_云网牛站](https://ywnz.com/linuxml/3094.html)

### 1. *自动补全

**kube-shell** 好用点，支持模糊搜索

[cloudnativelabs/kube-shell: Kubernetes shell: An integrated shell for working with the Kubernetes](https://github.com/cloudnativelabs/kube-shell)

```shell
pip install kube-shell
```

[c-bata/kube-prompt: An interactive kubernetes client featuring auto-complete.](https://github.com/c-bata/kube-prompt)

OS X 安装命令：

```
brew install bash-complete@2
```

不仅仅是 kubectl ，也给其他命令行提供自动补全的命令提示。

在 .zshrc 中添加如下内容：

```
# kubectl complete
source <(kubectl completion zsh)
```

在输入 `kubectl get pod` 命令时，键入 `Tab` 会自动列举当前类型下的资源，如果没有任何资源，则列举目录文件。

[![Demo](https://i.loli.net/2020/05/16/c7G2k8mFKrzHPpq.gif)](https://www.chenshaowen.com/blog/images/2020/05/completion-demo.gif)

### 2. 环境切换和管理 - kubectx

OS X 安装命令：

```
brew install kubectx
```

提供两个命令行工具：

- kubectx ，切换不同集群

[![官方 Demo](https://i.loli.net/2020/05/16/H8k5BhUpcZnFRiW.gif)](https://www.chenshaowen.com/blog/images/2020/05/kubectx-demo.gif)

- kubens ，切换不同 Namespaces

[![官方 Demo](https://i.loli.net/2020/05/16/CmHiRVcdIZK3Agp.gif)](https://www.chenshaowen.com/blog/images/2020/05/kubens-demo.gif)

### 3. 将当前环境显示在命令中 - kube-ps1

OS X 安装命令：

```
brew install kube-ps1
```

在 .profile 中添加如下内容:

```
# kube-ps1
source "/usr/local/opt/kube-ps1/share/kube-ps1.sh"
PS1='$(kube_ps1)'$PS1
```

但是由于通常 config 中配置的 context 名比较长，同时不易区分，需要修改下：

```
sed -i'.s' -E 's/kubernetes-admin@cluster.local'/dev/ ~/.kube/config
```

将 [*kubernetes*-admin@cluster.local](mailto:kubernetes-admin@cluster.local) 替换为 dev ，可以配合 [本地快速切换不同 *Kubernetes* 环境](https://www.chenshaowen.com/blog/developing-tips-19.html#1-本地快速切换不同-Kubernetes-环境) 使用。

[![官方 Demo](https://i.loli.net/2020/05/16/9ePf2ADi5lRNdvM.gif)](https://www.chenshaowen.com/blog/images/2020/05/kube-ps1-demo.gif)

### 4. 交互式命令 - kube-prompt

kube-prompt 可以让用户省略每次都需要输入的 `kubectl` ，同时给出一些交互式的自动补全。kube-shell 也提供交互式的自动补全，但是很长时间没有更新了，使用 `pip install kube-shell` 进行安装，在服务器上可能用得上。

安装命令：

```
brew install c-bata/kube-prompt/kube-prompt
```

开始使用：

```
kube-prompt
```

[![官方 Demo](https://www.chenshaowen.com/blog/images/2020/05/kube-prompt-demo.gif)](https://www.chenshaowen.com/blog/images/2020/05/kube-prompt-demo.gif)

### 5.Kubectl Aliases

https://github.com/ahmetb/kubectl-aliases

```shell
下载 https://raw.githubusercontent.com/ahmetb/kubectl-alias/master/.kubectl_aliases
[ -f ~/dotfiles/kubectl_aliases.sh ] && source ~/dotfiles/kubectl_aliases.sh
```

别名规则

![交互式Kubernetes客户端，搭建高效Kubernetes命令行终端](https://i.loli.net/2020/05/19/9YxDIobfXj2RUWP.jpg)



### Kubeval、Kubens

Kubeval 是一个用于校验 Kubernetes YAML 或 JSON 配置文件的工具，支持多个 Kubernetes 版本，可以帮助我们解决不少的麻烦。

项目地址：https://github.com/garethr/kubeval



Kubens

该工具可以帮助您快速的在 *Kubernetes* 的多个命名空间之间切换。

项目地址：https://github.com/ahmetb/kubectx

Kubens 使用效果图：

![交互式Kubernetes客户端，搭建高效Kubernetes命令行终端](https://i.loli.net/2020/05/19/JIYwCd5VmisrMUf.jpg)

### 参考

- https://github.com/ahmetb/kubectx
- https://github.com/jonmosco/kube-ps1
- https://github.com/c-bata/kube-prompt
- https://github.com/cloudnativelabs/kube-shell





# 其他

[给 Kubernetes 配置 Proxy - 陈少文的博客](https://www.chenshaowen.com/blog/how-to-set-proxy-for-kubernetes.html)
[Kubernetes Cheat Sheet - 陈少文的博客](https://www.chenshaowen.com/blog/kubernetes-cheat-sheet.html)

## Minikube、kubeadm、Kubespray、Kops等Kubernetes部署工具该如何选择？

简单来说就是几个工具的使用场景不一样，Minikube 通过虚拟机方式快速安装单节点 Kubernetes 集群，可用于个人电脑快速体验Kubernetes；Kubeadm 是官方推荐的Kubernetes 分发工具，该工具有助于在现有基础架构上引导最佳 Kubernetes 集群实践，优点是能够在任何地方发布最小的可行 Kubernetes 集群；但 Kubeadm 不提供基础架构配置（例如：网络、负载均衡、存储等都需要额外配置）；Kops适合于在阿里云、AWS、GCE、Azure、OpenStack等云平台上部署Kubernetes群集，目前不支持裸机部署。Kubespray是产线部署常用工具，依赖Ansible，支持AWS，GCE，Azure，OpenStack等云平台，以及物理服务器的IaaS平台。



[Kubernetes 部署 Dashboard](https://liqiang.io/post/9f5d6241)
[网页界面 (Dashboard) - Kubernetes](https://kubernetes.io/zh/docs/tasks/access-application-cluster/web-ui-dashboard/)




