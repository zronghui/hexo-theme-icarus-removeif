---
title: 《MongoDB 实战》阅读笔记
toc: true
recommend: 1
uniqueId: '2020-11-29 14:16:13/"《MongoDB 实战》阅读笔记".html'
date: 2020-11-29 22:16:13
thumbnail:
categories:
- 读书笔记
tags:
keywords:
top: 10
---

![MongoDB实战(第二版)](https://i.loli.net/2020/12/02/n58zIYeCqKAUfmR.jpg)

**豆瓣评分**：5.5

本书分三部分通过大量的实例代码介绍了 MongoDB 数据库底层的实现以及大型互联网 Web 项目数据库设计原则。部分对 MongoDB 进行了整体介绍，并介绍了实际的开发例子，另外还介绍了 JavaScript shell 和 Ruby 驱动。第二部分通过逐步实现一个电商数据模型和实现必要的 CRUD 操作来详细介绍了 MongoDB 的文档数据模型、查询语言和 CRUD（新增、读取、更新和删除）操作。本书的后部分从数据库专家的角度来看待 MongoDB，介绍了数据库的性能、部署、容错和伸缩性等所有的知识。

本书适合想深入学习 MongoDB 的开发人员，主要关注 MongoDB 数据库。

[TOC]

<!--more-->



> 图书馆借阅的书，网上也没有电子版，只能自己记了

## 困惑

MongoDB 数据存在哪里？内存 or 磁盘？缓存是怎样的？

默认有没有索引？有的话是哪个？id?

## 第一章

### MongoDB 提供了几个命令行工具

mongodump mongostore 备份和恢复数据库的工具

mongoexport mongoimport 导入或导出 json CSV等格式的数据

mongosniff 查看发送给数据库的命令，会把 BSON 转换成人类可读的

mongostat 与 iostat 类似，查看 MongoDB 数据库的状态，包括，每秒的操作数，分配虚拟内存的数量，以及服务器的连接数量

mongotop 显示 MongoDB 在每个集合里话费的读取和写入数据的时间总数

mongoperf MongoDB 操作磁盘情况

mongoplog 展示 MongoDB 日志

Bsondump bson->json 等格式



如 将 json 数据导入 MongoDB 数据库中：

```shell
cat sample.json | mongoimport -c tweets
```





### Why MongoDB ?

关系型数据库最大的特点是支持 SQL 查询语句，故 SQL 数据库的适用场景为 需要事务的系统（银行或金融）或 SQL、规范化数据模型

而像 MongoDB 的 NoSQL 更适用于 高吞吐量（如消息队列），缓存，web 网站等场景



## 第二章

### 动态操作数据

只有在第一次插入数据库和集合是才会创建数据库，所以文档的数据结构不用提前定义



### shell 操作

以操作 db 数据库中的 users 集合为例

```shell
use db
db.users.insert({username: "smith"})

db.users.find()
db.users.find({username: "jones"})
db.users.find({username: "jones"}).pretty()
# find 里的 json 的多个字段默认的联合逻辑是 AND
# 可以用 $and 和 $or 组合多个条件

db.users.update({查询条件}, {$set: {更新的字段 json}})
# 如果不加 set 就整个替换为后面的 json，之前的字段全部不保留
# 或者用 $addToSet 将查询的数据插入其他集合

db.users.remove()
db.users.remove({限制条件})
```



### 创建和查询索引

可以从 shell 方便地创建索引

```shell
db.numbers.createIndex({num: 1})
```

explain

```shell
db.numbers.find({...}).explain("executionStats")
```

explain 输出的结果的 json 中包括 totalDocsExamined 扫描的条数 indexName 使用的索引(没有使用索引就不显示这个字段)



### 查看数据库状态

```shell
# 可以在数据库或集合中使用 stats
db.stats()
db.users.stats()

```



### 数据库 集合 文档



















































































