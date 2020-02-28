---
title: 第3讲：Kubernetes 核心概念 实践
toc: true
recommend: 1
uniqueId: '2020-02-28 09:23:25/"第3讲：Kubernetes 核心概念 实践".html'
date: 2020-02-28 17:23:25
thumbnail:
categories:
- 天书
- CNCF-×-Alibaba云原生技术公开课
toc: true
tags:
keywords:
---

# 初识 K8s，创建一个guestbook留言簿应用 【K8s | from zero to hero】

[木环](https://developer.aliyun.com/profile/44amimozjxtre) 2019-11-07 876浏览量

**简介：** 本文介绍一个简单的K8s上手应用，希望通过这个简单的实践让大家对K8s的核心概念有更深入的理解。这个案例要在 Kubernetes 集群上部署一个名叫 guestbook 的 CURD 应用。guestbook 是 Kubernetes 社区的一个经典的应用示例，它有一个 Web 界面来让用户进行 CURD 操作，然后向一个 Redis 主节点写入数据，从多个 Redics 从节点读去数据。

## 课后实践：Kubernetes 核心概念

### 1. 目标概述

本文介绍一个简单的K8s上手应用，希望通过这个简单的实践让大家对K8s的核心概念有更深入的理解。

1. 巩固 Kubernetes 的基本概念
2. 学会使用 Kubernetes 部署一个标准的“多层（multi-tier）”应用
3. 了解 Kubernetes 里如何通过 Pod，Deployment，Service 等 API 原语描述“应用”

### 2. 实验概览

完成此实验后，可以掌握的能力有：

本实验主要在 Kubernetes 集群上部署一个名叫 guestbook 留言簿的 CURD (增查改删)应用。guestbook 是 Kubernetes 社区的一个经典的应用示例，它有一个 Web 界面来让用户进行 CURD 操作，然后向一个 Redis 主节点写入数据，从多个 Redics 从节点读去数据。

实验分以下几个步骤：

1. 创建 Redis 主节点
2. 创建 Redis 从节点集群
3. 创建 guestbook 应用
4. 将 guestbook 应用通过 Service 暴露出来并进行访问
5. 水平扩展 guestbook 应用

### 3. 所需资源：

一个完备的 Kubernetes 集群。您可以选择[阿里云容器服务Kubernetes（ACK）](https://www.aliyun.com/product/kubernetes)进行上手操作。

可以用 Minikube 快速启动一个单节点集群（国内建议使用[Minikube 中国版](https://github.com/AliyunContainerService/minikube)），也可以用云上的 Kubernetes 集群。本次实验演示将使用阿里云容器服务提供的 Kubernetes 集群，版本为 1.12。

你可以使用使用 `kubectl version` 查看你的集群版本同实验版本一致。

### 4. 实验详情

### 4.1 创建 Redis 主节点

在这里，我们使用一个叫做 Deployment 的 API 对象，来描述单实例的Redis 主节点。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
  labels:
    app: redis
spec:
  selector:
    matchLabels:
      app: redis
      role: master
      tier: backend
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
        role: master
        tier: backend
    spec:
      containers:
      - name: master
        image: registry.cn-hangzhou.aliyuncs.com/kubeapps/redis
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - containerPort: 6379
```

我们需要把这个上述内容保存为本地 YAML文件，名叫：`redis-master-deployment.yaml`。这个文件主要定义了两个东西：第一，Pod 里的容器的镜像是 `redis`；第二，这个 Deployment 的实例数（replicas）是 1，即指启动一个 Pod。

然后，我们使用 Kubernetes 的客户端，执行如下操作：

```
 $  kubectl apply -f  redis-master-deployment.yaml
 deployment.apps/redis-master created
```

这一步完成后，Kubernetes 就会按照这个 YAML 文件里的描述为你创建对应的 Pod。这种使用方式就是声明式 API 的典型特征。

接下来，我们可以查看到这个 Pod：

```
$ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
redis-master-68979f4ddd-pg9cv   1/1     Running   0          49s
```

可以看到，Pod 已经进入了 Running 状态，表示一切正常。这时，我们就可以查看这个 Pod 里的 Redis 的日志：

```
$ kubectl logs -f redis-master-68979f4ddd-pg9cv
1:C 26 Apr 2019 18:49:29.303 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 26 Apr 2019 18:49:29.303 # Redis version=5.0.4, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 26 Apr 2019 18:49:29.303 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
1:M 26 Apr 2019 18:49:29.304 * Running mode=standalone, port=6379.
1:M 26 Apr 2019 18:49:29.304 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 26 Apr 2019 18:49:29.304 # Server initialized
1:M 26 Apr 2019 18:49:29.304 * Ready to accept connections
```

#### 4.2 为 Redis 主节点创建 Service

Kubernetes 里要访问 Pod 最好通过 Service 的方式，这样客户端就不需要记录 Pod 的 IP 地址了。我们的 guestbook 网站需要访问 Redis 主节点的 Pod，所以也要通过 Service 来做。这个 Service API 对象的定义如下所示：

```
apiVersion: v1
kind: Service
metadata:
  name: redis-master
  labels:
    app: redis
    role: master
    tier: backend
spec:
  ports:
  - port: 6379
    targetPort: 6379
  selector:
    app: redis
    role: master
    tier: backend
```

这个 Service 名叫 `redis-master`，它声明用自己的 6379 端口代理 Pod 的 6379端口。

我们还是把上述内容保存成文件然后让 Kubernetes 为我们创建它：

```
$  kubectl apply -f redis-master-service.yaml
service/redis-master created
```

然后我们可以查看一下这个 Service：

```
$ kubectl get service
NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
kubernetes     ClusterIP   10.96.0.1        <none>        443/TCP    181d
redis-master   ClusterIP   10.107.220.208   <none>        6379/TCP   9s
```

这时候，你就可以通过 `10.107.220.208:6379` 访问到这个 Redis 主节点。

#### 4.3 创建 Redis 从节点集群

我们这个示例中，有多个 Redis 从节点来共同响应读请求。同样的，我们还是通过 Deployment 来描述"一个服务由多个相同的 Pod 实例副本组成"这种语义。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
  labels:
    app: redis
spec:
  selector:
    matchLabels:
      app: redis
      role: slave
      tier: backend
  replicas: 2
  template:
    metadata:
      labels:
        app: redis
        role: slave
        tier: backend
    spec:
      containers:
      - name: slave
        image: registry.cn-hangzhou.aliyuncs.com/kubeapps/gb-redisslave:v1
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: env
        ports:
        - containerPort: 6379
```

在这个 Deployment 中，我们指定了 `replicas: 2`，即这个 Deployment 会启动两个相同 Pod（Redis 从节点）。

此外，`gb-redisslave:v1` 这个镜像，会自动读取 `REDIS_MASTER_SERVICE_HOST` 这个环境变量的值，也就是 Redis 主节点的 Service 地址，然后用它来组建集群。这个环境变量是Kubernetes 自动根据 redis-master 这个 Service 名字，自动注入到集群的每一个 Pod 当中的。

然后，我们创建 Redis 从节点：

```
$ kubectl apply -f redis-slave-deployment.yaml
deployment.apps/redis-slave created
```

这时候，我们就可以查看这些从节点的状态：

```
$ kubectl get pods
NAME                            READY   STATUS              RESTARTS   AGE
redis-master-68979f4ddd-pg9cv   1/1     Running             0          17m
redis-slave-78b464f5cd-2kn7w    0/1     ContainerCreating   0          37s
redis-slave-78b464f5cd-582bk    0/1     ContainerCreating   0          37s
```

#### 4.4 为 Redis 从节点创建 Service

类似的，为了让 guestbook 应用访问上述 Redis 从节点，我们还需要为它们创建一个 Service。在Kubernetes 里，Service 可以通过 selector 选择代理多个 Pod，并且负责负载均衡。这个Service 内容如下所示：

```
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
  labels:
    app: redis
    role: slave
    tier: backend
spec:
  ports:
  - port: 6379
  selector:
    app: redis
    role: slave
    tier: backend
```

创建和查看 Service（ 注意：这里 6379 端口使用了简化写法，就不需要写明 targetPort了）：

```
$ kubectl apply -f redis-slave-svc.yaml
service/redis-slave created

$ kubectl get services
NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
kubernetes     ClusterIP   10.96.0.1        <none>        443/TCP    181d
redis-master   ClusterIP   10.107.220.208   <none>        6379/TCP   16m
redis-slave    ClusterIP   10.101.244.239   <none>        6379/TCP   57s
```

这样，你就可以通过 `10.10.101.244:6379` 访问到任何一个 Redis 从节点了。

#### 4.5 创建 guestbook 应用

guestbook 应用本身，依然通过一个 Deployment 来描述，如下所示：

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  labels:
    app: guestbook
spec:
  selector:
    matchLabels:
      app: guestbook
      tier: frontend
  replicas: 3
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: registry.cn-hangzhou.aliyuncs.com/kubeapps/gb-frontend:v4
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: env
        ports:
        - containerPort: 80
```

这个 YAML 定义了一个 3 副本的 Deployment，即 guestbook 应用会启动 3 个 Pod。

我们还是通过同样的步骤创建这个 Deployment：

```
$ kubectl apply -f frontend.yaml
deployment.apps/frontend created
```

查看 Pod 的状态：

```
$ kubectl get pods -l app=guestbook -l tier=frontend
NAME                       READY   STATUS    RESTARTS   AGE
frontend-78d6c59f4-2x24x   1/1     Running   0          3m4s
frontend-78d6c59f4-7mz87   1/1     Running   0          3m4s
frontend-78d6c59f4-sw7f2   1/1     Running   0          3m4s
```

#### 4.6 为 guestbook 应用创建 Service

为了能够让用户访问到 guestbook，我们也需要为 guestbook 来创建一个 Service，从而把这个应用以服务的形式暴露出来给用户使用。

而为了能够让 Kubernetes 集群以外的用户，这个 Service 就必须是一个外部可访问的 Service。这个在 Kubernetes 里有几种做法。在云上最常见的，是 LoadBalancer 模式。

```
apiVersion: v1
kind: Service
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  # 自建集群只能使用 NodePort 模式
  # type: NodePort 
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: guestbook
    tier: frontend
```

由于我的集群由阿里云容器服务提供，所以像上面这样直接用 LoadBalancer 模式即可。

```
$ kubectl apply -f frontend-service.yaml
$ kubectl get service frontend
NAME       TYPE        CLUSTER-IP      EXTERNAL-IP        PORT(S)        AGE
frontend   ClusterIP   172.19.10.209   101.37.192.20     80:32372/TCP   1m
```

现在，你只要用浏览器打开 `EXTERNAL-IP` 对应的地址： [http://101.37.192.20:31323](http://101.37.192.20:31323/) ，就可以访问到这个部署好的 guestbook 应用了。

而如果你是自建集群，那就只能用 NodePort 模式来实验（上面 YAML 的注释已经给出了使用方法）。需要注意的是 NodePort 由于安全性问题，不建议在生产环境中使用。

#### 4.7 水平扩展 guestbook 应用

要通过 Kubernetes 来水平扩展你的应用以响应更多的请求非常简单，只需要如下一条命令：

```
$ kubectl scale deployment frontend --replicas=5
deployment.extensions/frontend scaled
```

你就会立刻看到你的 guestbook 应用的实例从 3 个变成了 5 个：

```
$ kubectl get pods -l app=guestbook -l tier=frontend
NAME                       READY   STATUS    RESTARTS   AGE
frontend-78d6c59f4-2x24x   1/1     Running   0          14m
frontend-78d6c59f4-7mz87   1/1     Running   0          14m
frontend-78d6c59f4-chxwd   1/1     Running   0          19s
frontend-78d6c59f4-jrvfx   1/1     Running   0          19s
frontend-78d6c59f4-sw7f2   1/1     Running   0          14m
```

