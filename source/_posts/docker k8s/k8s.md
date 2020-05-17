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



## *配置高效的 Kubernetes 命令行终端

[如何配置高效的 Kubernetes 命令行终端 - 陈少文的博客](https://www.chenshaowen.com/blog/how-to-configure-efficient-k8s-terminal.html)

### 1. 自动补全 - kubectl

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

### 5. 参考

- https://github.com/ahmetb/kubectx
- https://github.com/jonmosco/kube-ps1
- https://github.com/c-bata/kube-prompt
- https://github.com/cloudnativelabs/kube-shell









[给 Kubernetes 配置 Proxy - 陈少文的博客](https://www.chenshaowen.com/blog/how-to-set-proxy-for-kubernetes.html)
[Kubernetes Cheat Sheet - 陈少文的博客](https://www.chenshaowen.com/blog/kubernetes-cheat-sheet.html)





## Minikube、kubeadm、Kubespray、Kops等Kubernetes部署工具该如何选择？

简单来说就是几个工具的使用场景不一样，Minikube 通过虚拟机方式快速安装单节点 Kubernetes 集群，可用于个人电脑快速体验Kubernetes；Kubeadm 是官方推荐的Kubernetes 分发工具，该工具有助于在现有基础架构上引导最佳 Kubernetes 集群实践，优点是能够在任何地方发布最小的可行 Kubernetes 集群；但 Kubeadm 不提供基础架构配置（例如：网络、负载均衡、存储等都需要额外配置）；Kops适合于在阿里云、AWS、GCE、Azure、OpenStack等云平台上部署Kubernetes群集，目前不支持裸机部署。Kubespray是产线部署常用工具，依赖Ansible，支持AWS，GCE，Azure，OpenStack等云平台，以及物理服务器的IaaS平台。
