---
title: bio nio
date: 2020-01-11 13:42:06
categories:
- java
- 基础
toc: true
tags:
- Java
- bio
- nio
---
# bio nio
<!--more-->
[JavaGuide/BIO-NIO-AIO.md at master · Snailclimb/JavaGuide](https://github.com/Snailclimb/JavaGuide/blob/master/docs/java/BIO-NIO-AIO.md)
同步 异步？
阻塞 非阻塞？
## bio
blocking IO
socket 和 serverSocket
socket.read() socket.accept() socket.write() 都是同步阻塞的
服务端为一个客户端建立一个线程
改进：线程池实现伪异步IO
## nio
new IO
可以理解为non-blocking IO
实现了socketChannel serverSocketChannel

## nio与bio的区别：
### 阻塞与非阻塞
### 3个主要组件：buffer channel selector
#### buffer


#### channel
channel是双向的，而bio中的流是单向的

#### selector
单个线程使用selector选择多个channel，已实现减少线程开销的目的

JDK自带的nio很难用，netty是一个很好地替代
