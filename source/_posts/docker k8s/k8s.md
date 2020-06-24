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

[任务 - Kubernetes](https://kubernetes.io/zh/docs/tasks/#%e4%bd%bf%e7%94%a8-kubectl-%e5%91%bd%e4%bb%a4%e8%a1%8c)

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

- Sidecar：应用与日志收集
- 业务容器将日志写在一个 Volume 里面,Sidecar 容器一定可以通过共享该 Volume，直接把日志文件读出来，然后存到远程存储里面，或者转发到另外一个例子
- Sidecar：代理容器
- 单独写一个小的 Proxy，用来处理对接外部的服务集群，它对外暴露出来只有一个 IP 地址就可以了。所以接下来，业务容器主要访问 Proxy，然后由 Proxy 去连接这些服务集群
- Sidecar：适配器容器
- 有个例子：现在业务容器暴露出来的监控接口是 /metrics，访问这个容器的 metrics 的 URL 就可以拿到了。可是现在，这个监控系统升级了，它访问的 URL 是 /health，我只认得暴露出 health 健康检查的 URL，才能去做监控，metrics 不认识。那这个怎么办？那就需要改代码了，但可以不去改代码，额外写一个 Adapter，用来把所有对 health 的这个请求转发给 metrics 就可以了，所以这个 Adapter 对外暴露的是 health 这样一个监控的 URL，这就可以了，你的业务就又可以工作了。

### 课时5. 应用编排与管理(一)：核心原理

#### 资源元信息

元数据 包括 labels annotations owereference, 常用命令：

```shell
  # 查看所有 pods
  kubectl get pods
  kubectl apply -f pod1.yaml
  kubectl apply -f pod2.yaml
  # 查看所有 pods, 并且显示 labels
  kubectl get pods —show-labels
  kubectl get pods nginx1 -o yaml | less
  # 增 label
  kubectl label pods nginx1 env=test
  # 改 label
  kubectl label pods nginx1 env=test —overwrite
  kubectl get pods —show-labels
  # 删 label
  kubectl label pods nginx tie-
  kubectl get pods —show-labels
  # 查 label
  kubectl get pods —show-labels -l env=test
  kubectl get pods —show-labels -l env=test,env=dev # , 与 的关系
  kubectl get pods —show-labels -l env=dev,tie=front
  kubectl get pods —show-labels -l ’env in (dev,test)’
  
  # 增加 annotation
  kubectl annotate pods nginx1 my-annotate=‘my annotate,ok’
  kubectl get pods nging1 -o yaml | less
  kubectl apply -f rs.yaml
  kubectl get replicasets  nginx-replicasets -o yaml |less
  kubectl get pods
  kubectl get pods nginx-replicasets-rhd68 -o yaml | less
```
#### 控制器模式

1. 控制循环
2. Sensor
3. 控制循环例子-扩容

没看懂

总结：

1. 两种 API 设计方法

   **Kubernetes 控制器模式依赖声明式的 API**。另外一种常见的 API 类型是命令式 API。为什么 Kubernetes 采用声明式 API，而不是命令式 API 来设计整个控制器呢？


   首先，比较两种 API 在交互行为上的差别。在生活中，常见的**命令式**的交互方式是家长和孩子交流方式，因为孩子欠缺目标意识，无法理解家长期望，家长往往通过**一些命令**，教孩子一些**明确的动作**，比如说：吃饭、睡觉类似的命令。我们在容器编排体系中，命令式 API 就是通过向系统发出明确的操作来执行的。


   而常见的**声明式**交互方式，就是老板对自己员工的交流方式。老板一般不会给自己的员工下很明确的决定，实际上可能老板对于要操作的事情本身，还不如员工清楚。因此，老板通过给员工**设置可量化的业务目标**的方式，来发挥员工自身的主观能动性。比如说，老板会要求某个产品的市场占有率达到 80%，而不会指出要达到这个市场占有率，要做的具体操作细节。


   类似的，在容器编排体系中，我们可以执行一个应用实例副本数保持在 3 个，而不用明确的去扩容 Pod 或是删除已有的 Pod，来保证副本数在三个。

2. 命令式 API 的问题

   <img src="https://i.loli.net/2020/05/24/c2WNj5UPGnXBVYk.jpg" alt="img" style="zoom:50%;" />

3. 控制器模式总结

   控制型模式中最核心的就是控制循环的概念；

   两种 API 设计方法：声明式 API 和命令式 API ；Kubernetes 所采用的控制器模式，是由声明式 API 驱动的。

### 课时6. 应用编排与管理(二)：Deployment

#### 一、需求来源

**Deployment：管理部署发布的控制器**

Deployment 能帮我们做什么事情呢？

首先，**Deployment 定义了一种 Pod 期望数量**，比如说应用 A，我们期望 Pod 数量是四个，那么这样的话，controller 就会持续维持 Pod 数量为期望的数量。当我们与 Pod 出现了网络问题或者宿主机问题的话，controller 能帮我们恢复，也就是新扩出来对应的 Pod，来保证可用的 Pod 数量与期望数量一致；
配置 Pod 发布方式，也就是说 controller 会**按照用户给定的策略来更新 Pod**，而且更新过程中，也可以设定不可用 Pod 数量在多少范围内；
如果更新过程中发生问题的话，即所谓**“一键”回滚**，也就是说你通过一条命令或者一行修改能够将 Deployment 下面所有 Pod 更新为某一个旧版本 。

#### 二、用例解读

**Deployment 语法**

![c3](https://i.loli.net/2020/05/25/U86LwdBntlyrhsM.png)

“apiVersion：apps/v1”，也就是说 Deployment 当前所属的组是 apps，版本是 v1

Deployment.spec 中首先要有一个核心的字段，即 replicas，这里定义期望的 Pod 数量为三个；selector 其实是 Pod 选择器，那么所有扩容出来的 Pod，它的 Labels 必须匹配 selector 层上的 image.labels，也就是 app.nginx

- MinReadySeconds：Deployment 会根据 Pod ready 来看 Pod 是否可用，ready 的 Pod 不一定是 available 的，它一定要超过 MinReadySeconds 之后，才会判断为 available；
- revisionHistoryLimit：保留历史 revision，即保留历史 ReplicaSet 的数量，默认值为 10 个。这里可以设置为一个或两个，如果回滚可能性比较大的话，可以设置数量超过 10；
- paused：paused 是标识，Deployment 只做数量维持，不做新的发布，这里在 Debug 场景可能会用到；
- progressDeadlineSeconds：前面提到当 Deployment 处于扩容或者发布状态时，它的 condition 会处于一个 processing 的状态，processing 可以设置一个超时时间。如果超过超时时间还处于 processing，那么 controller 将认为这个 Pod 会进入 *failed* 的状态。


![c29](https://i.loli.net/2020/05/25/P3HKm8ZdVowSxiq.png)



**升级策略字段解析**

Deployment 在 RollingUpdate 中主要提供了两个策略，一个是 MaxUnavailable，另一个是 MaxSurge。这两个字段解析的意思，可以看下图中详细的 comment，或者简单解释一下：

- MaxUnavailable：滚动过程中最多有多少个 Pod 不可用；
- MaxSurge：滚动过程中最多存在多少个 Pod 超过预期 replicas 数量。


 MaxSurge 和 MaxUnavailable 不能同时为 0。

**查看 Deployment 状态**

kubectl get deployment

![c4](https://i.loli.net/2020/05/25/SbPa2fpuKtLQRyo.png)

**查看 Pod**

![c5](https://i.loli.net/2020/05/25/dJ6wFpXaNSfCuMV.png)

Pod 名字格式我们不难看到。

最前面一段：nginx-deployment，其实是 Pod 所属 Deployment.name；中间一段：template-hash，这里三个 Pod 是一样的，因为这三个 Pod 其实都是同一个 template 中创建出来的。最后一段，是一个 random 的字符串



**更新镜像**

kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.9.1

![c6](https://i.loli.net/2020/05/25/ZO9FANjJDqmHpxP.png)

首先 kubectl 后面有一个 set image 固定写法，这里指的是设定镜像；
其次是一个 deployment.v1.apps，这里也是一个固定写法，写的是我们要操作的资源类型，deployment 是资源名、v1 是资源版本、apps 是资源组，这里也可以简写为 deployment 或者 deployment.apps，比如说写为 deployment 的时候，默认将使用 apps 组 v1 版本。
第三部分是要更新的 deployment 的 name，也就是我们的 nginx-deployment；再往后的 nginx 其实指的是 template，也就是 Pod 中的 container.name；这里我们可以注意到：一个 Pod 中，其实可能存在多个 container，而我们指定想要更新的镜像的 container.name，就是 nginx。
最后，指定我们这个容器期望更新的镜像版本，这里指的是 nginx: 1.9.1。如下图所示：当执行完这条命令之后，可以看到 deployment 中的 template.spec 已经更新为 nginx: 1.9.1。

**快速回滚**

![c7](https://i.loli.net/2020/05/25/CtnsmlSfHrdEqG8.png)

**DeploymentStatus**

![c8](https://i.loli.net/2020/05/25/WB1saQiNyUSx3Gv.png)

Processing 指的是 Deployment 正在处于扩容和发布中。比如说 Processing 状态的 deployment，它所有的 replicas 及 Pod 副本全部达到最新版本，而且是 available，这样的话，就可以进入 complete 状态。而 complete 状态如果发生了一些扩缩容的话，也会进入 processing 这个处理工作状态。


如果在处理过程中遇到一些问题：比如说拉镜像失败了，或者说 readiness probe 检查失败了，就会进入 failed 状态；如果在运行过程中即 complete 状态，中间运行时发生了一些 pod readiness probe 检查失败，这个时候 deployment 也会进入 failed 状态。进入 failed 状态之后，除非所有点 replicas 均变成 available，而且是 updated 最新版本，deployment 才会重新进入 complete 状态。

#### 三、操作演示

略

#### 四、架构设计

管理模式

![c23](https://i.loli.net/2020/05/25/Vt9fZGaIzbNkLeO.png)



### 课时7. 应用编排与管理(三)：Job & DaemonSet

#### Job

Job 为我们**提供的功能**

- 它可以创建一个或多个 Pod 来指定 Pod 的数量，并可以监控它是否成功地运行或终止；
- 我们可以根据 Pod 的状态来给 Job 设置重置的方式及重试的次数；
- 我们还可以根据依赖关系，保证上一个任务运行完成之后再运行下一个任务；
- 同时还可以控制任务的并行度，根据并行度来确保 Pod 运行过程中的并行次数和总体完成大小。



**Job 语法**

<img src="https://i.loli.net/2020/05/25/C149GlHXgQrxFvj.png" alt="image-20200525164348534" style="zoom:33%;" />

上图是 Job 最简单的一个 yaml 格式，这里主要新引入了一个 kind 叫 Job，这个 Job 其实就是 job-controller 里面的一种类型。 然后 metadata 里面的 name 来指定这个 Job 的名称，下面 spec.template 里面其实就是 pod 的 spec。

所以在 Job 里面，我们主要重点关注的是 **restartPolicy 重启策略**和 **backoffLimit 重试次数限制**。

Never、OnFailure、Always

**Job 状态**

<img src="https://i.loli.net/2020/05/25/eOglzMFTWyAZ1xq.png" alt="image-20200525164532590" style="zoom:33%;" />

Job 创建完成之后，我们就可以通过 kubectl get jobs 这个命令，来查看当前 job 的运行状态。得到的值里面，基本就有 Job 的名称、当前完成了多少个 Pod 以及运行了多长时间。

**查看 Pod**

kubectl get pods 

kubectl get pods pi-4cids -o yaml



Pod 的名称会以“${job-name}-${random-suffix}”

**并行运行 Job**

主要看两个参数：一个是 completions，一个是 parallelism

分别含义为： Job 指定的可以运行的总次数 、 并行执行的个数

比如说 Job 一定要执行 8 次（completions），每次并行 2 个 Pod（parallelism），这样的话，一共会执行 4 个批次。

**查看并行 Job 运行**

#### cronjob

<img src="https://i.loli.net/2020/05/25/qv7UAIsGMFxNYVj.png" alt="image-20200525164906142" style="zoom:50%;" />

startingDeadlineSeconds：如果等待时间超过startingDeadlineSeconds的话，CronJob 就会停止这个 Job；

concurrencyPolicy：就是说是否允许并行运行。所谓的并行运行就是，当第二个 Job 要到时间需要去运行的时候，上一个 Job 还没完成。如果这个 policy 设置为 **true** 的话，那么不管你前面的 Job 是否运行完成，每分钟都会去执行；如果是 false，它就会等上一个 Job 运行完成之后才会运行下一个；

JobsHistoryLimit：这个就是每一次 CronJob 运行完之后，它都会遗留上一个 Job 的运行历史、查看时间。当然这个额不能是无限的，所以需要设置一下历史存留数，一般可以设置默认 10 个或 100 个都可以

#### 操作演示

**略**

Job 的编排文件
Job 的创建及运行验证
并行 Job 的编排文件
并行 Job 的创建及运行验证
Cronjob 的编排文件
Cronjob 的创建及运行验证

#### 架构设计

**略**

Job 管理模式
Job 控制器

#### DaemonSet

DaemonSet：守护进程控制器

<img src="https://i.loli.net/2020/05/25/hylNU3JPtaWoDO1.png" alt="image-20200525165437046" style="zoom: 50%;" />

**查看 DaemonSet 状态**

kubectl get ds

<img src="https://i.loli.net/2020/05/25/ArkPTVvJClXh79H.png" alt="image-20200525165535852" style="zoom:50%;" />

有几个参数，分别是：需要的 pod 个数、当前已经创建的 pod 个数、就绪的个数，以及所有可用的、通过健康检查的 pod；还有 **NODE SELECTOR**，因为 NODE SELECTOR 在 DaemonSet 里面非常有用。

有时候我们可能**希望只有部分节点去运行这个 pod 而不是所有的节点**，所以有些节点上被打了标的话，DaemonSet 就只运行在这些节点上。比如，我只希望 master 节点运行某些 pod，或者只希望 Worker 节点运行某些 pod，就可以使用这个 NODE SELECTOR。

**更新 DaemonSet**

<img src="https://i.loli.net/2020/05/25/LN4gjsvqcA1Cbxn.jpg" alt="img" style="zoom:50%;" />

#### 操作演示

**略**

DaemonSet 的编排
DaemonSet 的创建与运行验证
DaemonSet 的更
DaemonSet 管理模式
DaemonSet 控制器

### 课时8. 应用配置管理：ConfigMap Secret

[从零开始入门 K8s | 如何实现应用配置管理？](https://mp.weixin.qq.com/s/8r-_Ekje__GVHsKLfJ-66A)

#### ConfigMap 和 Secret

首先介绍了 ConfigMap 和 Secret 的创建方法和使用场景，然后对 ConfigMap 和 Secret 的常见使用注意点进行了分类和整理。最后介绍了私有仓库镜像的使用和配置；

**使用场景**：一些可变的配置。因为我们不可能把一些可变的配置写到镜像里面，当这个配置需要变化的时候，可能需要我们重新编译一次镜像，这个肯定是不能接受的，所以有了 **ConfigMap**; 一些敏感信息的存储和使用。比如说应用需要使用一些密码，或者用一些 token， 所以有了 Secret。

**创建方法**：

![640](https://i.imgur.com/0LzM8Gh.jpg)

主要用 --from-file 和 --from-literal 引入配置

![下载](https://i.imgur.com/QEyF03B.jpg)

一二联合使用，首先引入配置为环境变量，再用 $x。三是挂载配置文件到容器内

![下载](https://i.imgur.com/A8vWl2L.jpg)





**Pod 身份认证**: 首先介绍了 ServiceAccount 和 Secret 的关联关系，然后从源码角度对 Pod 身份认证流程和实现细节进行剖析，同时引出了 Pod 的权限管理 (即 RBAC 的配置管理)；

![下载](https://i.imgur.com/qBoFE5t.jpg)



![下载](https://i.imgur.com/beMXTne.jpg)



容器资源和安全：首先介绍了容器常见资源类型 (CPU/Memory) 的配置，然后对 Pod 服务质量分类进行详细的介绍。同时对 SecurityContext 有效层级和权限配置项进行简要说明；
InitContainer: 首先介绍了 InitContainer 和普通 container 的区别以及 InitContainer 的用途。然后基于实际用例对InitContainer 的用途进行了说明。

略

### 课时9. 应用存储和持久化数据卷：核心知识

首先来看一下 Pod Volumes 的使用场景：

- 场景一：如果 pod 中的某一个容器在运行时异常退出，被 kubelet 重新拉起之后，如何保证之前容器产生的重要数据没有丢失？
- 场景二：如果同一个 pod 中的多个容器想要共享数据，应该如何去做？

以上两个场景，其实都可以借助 Volumes 来很好地解决，接下来首先看一下 Pod Volumes 的常见类型：

1. 本地存储，常用的有 emptydir/hostpath；
2. 网络存储：网络存储当前的实现方式有两种，一种是 in-tree，它的实现代码是放在 K8s 代码仓库中的，随着 K8s 对存储类型支持的增多，这种方式会给 K8s 本身的维护和发展带来很大的负担；而第二种实现方式是 out-of-tree，它的实现其实是给 K8s 本身解耦的，通过抽象接口将不同存储的 driver 实现从 K8s 代码仓库中剥离，因此 **out-of-tree 是后面社区主推的一种实现网络存储插件的方式**；
3. Persistent Volumes：它其实是将一些配置信息，如 secret/configmap 用卷的形式挂载在容器中，让容器中的程序可以通过 POSIX 接口来访问配置数据；



课时 9 后面的暂时就不看了



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



### lens

[最华丽的 Kubernetes 桌面客户端：Lens](https://mp.weixin.qq.com/s/6O_9wEppjaB8GiqoSDIdWQ)

### python: Official Python client library for kubernetes

[kubernetes-client/python: Official Python client library for kubernetes](https://github.com/kubernetes-client/python)

# 其他

[给 Kubernetes 配置 Proxy - 陈少文的博客](https://www.chenshaowen.com/blog/how-to-set-proxy-for-kubernetes.html)
[Kubernetes Cheat Sheet - 陈少文的博客](https://www.chenshaowen.com/blog/kubernetes-cheat-sheet.html)

## Minikube、kubeadm、Kubespray、Kops等Kubernetes部署工具该如何选择？

简单来说就是几个工具的使用场景不一样，Minikube 通过虚拟机方式快速安装单节点 Kubernetes 集群，可用于个人电脑快速体验Kubernetes；Kubeadm 是官方推荐的Kubernetes 分发工具，该工具有助于在现有基础架构上引导最佳 Kubernetes 集群实践，优点是能够在任何地方发布最小的可行 Kubernetes 集群；但 Kubeadm 不提供基础架构配置（例如：网络、负载均衡、存储等都需要额外配置）；Kops适合于在阿里云、AWS、GCE、Azure、OpenStack等云平台上部署Kubernetes群集，目前不支持裸机部署。Kubespray是产线部署常用工具，依赖Ansible，支持AWS，GCE，Azure，OpenStack等云平台，以及物理服务器的IaaS平台。



[Kubernetes 部署 Dashboard](https://liqiang.io/post/9f5d6241)
[网页界面 (Dashboard) - Kubernetes](https://kubernetes.io/zh/docs/tasks/access-application-cluster/web-ui-dashboard/)




