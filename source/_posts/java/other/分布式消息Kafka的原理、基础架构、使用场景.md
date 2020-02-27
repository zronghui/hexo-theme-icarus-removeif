---
title: 分布式消息Kafka的原理、基础架构、使用场景
date: 2020-01-31 10:46:43
categories:
- java
- other
toc: true
tags:
---

[toc]

<!--more-->



# [分布式消息Kafka的原理、基础架构、使用场景](https://aijishu.com/a/1060000000080308)

#### **一：Kafka简介**

Kafka 是分布式发布-订阅系统，在 kafka 官网上对 kafaka 的定义：一个分布式发布订阅消息传递系统。kafka 是一种快速、可扩展的、分布式、分区的和可复制的提交日志服务。

<img src="https://aijishu.com/img/bVu3m" alt="img" style="zoom: 50%;" />

#### **二：Kafka基本架构**

它的架构包括以下组件：

<img src="https://aijishu.com/img/bVu3n" alt="img" style="zoom: 67%;" />



Topic: 特定类型的消息流。消息是字节的有效负载（payload），topic 是消息的分类名或种子（feed）名

Producer：发布消息到 topic 的对象

Broker / kafka 集群：已发布的消息保存在一组服务器中

Consumer： 可以订阅一个或多个话题，并从 Broker 拉数据，从而消费这些已发布的消息



**上图中可以看出，生产者将数据发送到Broker代理，Broker代理有多个话题topic，消费者从Broker获取数据。**



#### **三：Kafka基本原理**

![img](https://aijishu.com/img/bVu3o)

生产者将数据生产出来，交给 broker 进行存储，消费者需要消费数据了，就从broker中去拿出数据来，然后完成一系列对数据的处理操作。

kafka 官方给出的图：

![img](https://aijishu.com/img/bVu3p)

**多个 broker 协同合作，producer 和 consumer 部署在各个业务逻辑中被频繁的调用，三者通过 zookeeper管理协调请求和转发。这样一个高性能的分布式消息发布订阅系统就完成了。**

**图上有个细节需要注意，producer 到 broker 的过程是 push，也就是有数据就推送**到 broker，而 consumer 到 broker 的过程是 pull，是通过 consumer 主动去拉数据的，而不是 broker 把数据主懂发送到 consumer 端的。

#### **四：Zookeeper在kafka的作用**

![img](https://aijishu.com/img/bVu3q)

无论是 kafaka 集群还是 producer 、consume 都依赖与 zookeeper 来保证系统可用性集群保存一些 meta 信息。

kafaka 使用 zookeeper 作为其分布式协调框架，很好地将消息生产、消息存储、消息消费的过程结合在一起。

同时借助 zookeeper，kafka 能够建立生产者和消费者的订阅关系，并实现生产者与消费者的负载均衡。



#### **五：Kafka的特性**

##### **1.高吞吐量、低延迟**

kafka每秒可以处理几十万条消息，它的延迟最低只有几毫秒，每个topic可以分多个partition, consumer group 对partition进行consume操作。

##### **2.可扩展性**

kafka集群支持热扩展

##### **3.持久性、可靠性**

消息被持久化到本地磁盘，并且支持数据备份防止数据丢失

##### **4.容错性**

允许集群中节点失败（若副本数量为n,则允许n-1个节点失败）

##### **5.高并发**

支持数千个客户端同时读写

![file](https://aijishu.com/img/bVmkZ)

**Kafka的使用场景：**

###### **1.日志收集**

一个公司可以用Kafka可以收集各种服务的log，通过kafka以统一接口服务的方式开放给各种consumer，例如hadoop、Hbase、Solr等。

###### **2.消息系统**

###### **3.用户活动跟踪**

Kafka经常被用来记录web用户或者app用户的各种活动，如浏览网页、搜索、点击等活动，这些活动信息被各个服务器发布到kafka的topic中，然后订阅者通过订阅这些topic来做实时的监控分析，或者装载到hadoop、数据仓库中做离线分析和挖掘。

###### **4.运营指标**

Kafka也经常用来记录运营监控数据。包括收集各种分布式应用的数据，生产各种操作的集中反馈，比如报警和报告。

###### **5.流式处理**

比如spark streaming和storm

#### **六：ActiveMQ、Kafka、RabbitMQ消息系统的对比**

![img](https://aijishu.com/img/bVu3r)
