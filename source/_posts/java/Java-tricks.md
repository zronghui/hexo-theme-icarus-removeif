---
title: Java tricks
date: 2020-01-15 18:14:20
categories:
- java
toc: true
tags:
- tricks
- Java
---



---

[toc]

<!--more-->

### 降低 idea 的 CPU 使用率

[IntelliJ IDEA 卡顿CPU 使用率超过100% – 软体工程师的踩雷笔记](https://nullpointerexception.tangblack.com/intellij-idea-%e5%8d%a1%e9%a0%93-cpu-%e4%bd%bf%e7%94%a8%e7%8e%87%e8%b6%85%e9%81%8e100/)

# 04-03

## Java 程序该怎么优化？工具篇

原创 一猿小讲 [一猿小讲](javascript:void(0);) *1周前*

> 程序员：为什么程序总是那么慢？时间都花到哪里去了？
>
> 面试官：若你写的 Java 程序，出现了性能问题，该怎么去排查呢？

***1.*** **hprof 工具**

hprof 工具是通过织入监控代码，来对 Java 程序进行监控的一款工具。可以监控 Java 程序在运行时占用的 CPU，及统计堆内存使用等。

例如：每隔 10 毫秒采样 CPU 消耗信息，并把信息保存到 hprof.txt 文件中。

```
java -agentlib:hprof=cpu=times,interval=10,file=hprof.txt class
```

指令运行完，打开 hprof.txt 便很容易统计出哪些方法的运行耗时较长。

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBF0dEr4erzUxafQFWhAWDRhfCKEleiaBta4vDKVfnuAODd7PylcSCtOdA/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />

例如：输出 Java 应用程序中各个类所占用的内存百分比。

```
java -agentlib:hprof=heap=sites,file=hprof.txt class备注：若未指定 file=hprof.txt，则默认会生成 java.hprof.txt 文件
```



打开输出的文件，效果如下。

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFy9dlffnz5VfEiamIePAiaZCsdwWNKnN9R2AVuIOThf2l2TesD4jz0e4g/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



例如：将 Java 应用程序的堆快照保存在文件 core.hprof 中，然后就可以使用 VisualVM 等工具来分析这个堆文件啦。

```
java -agentlib:hprof=heap=dump,format=b,file=core.hprof class
```





采用 VisualVM 工具打开 core.hprof 文件进行分析堆快照，效果如下。



<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFgeCI7s2VXib2PC4QBHKTNORZbicU9L49NLllyhiafYibSlR9jHUpUY9GMg/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



***2.*** **JConsole 工具**



JConsole 是 Java 自带的图形化性能监控工具，可以让你摆脱命令行排查问题的痛苦。通过它，会非常容易的监测 Java 程序的运行情况。



<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFVNwEboHsOXeVu2M59k9RXAaw9vMhHplmvgr5MWicP8acMlByhyKPczg/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



***2.1.*** **连接要监控的 Java 程序**



首先进入 JDK 安装之后的 bin 目录，若是配置过 Java 的环境变量，直接运行 JConsole 就行，效果如下。



<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFvN5undnsvHFo8lU8EHGT0Q5SYV8R03diajgKPBr10ibnDW0F4Bia9tADA/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



若是要监控本地 Java 进程，直接选择列表中的名称进行连接即可。



若是要监控远程 Java 进程，需要在远程 Java 程序启动时，需要加上下面几句话。

```
# 远程服务器的ip地址-Djava.rmi.server.hostname=127.0.0.1 # 指定jmx监听的端口-Dcom.sun.management.jmxremote.port=8099 # 指定jmx监听的端口-Dcom.sun.management.jmxremote.rmi.port=8099 # 是否开启ssl-Dcom.sun.management.jmxremote.ssl=false # 是否开启认证-Dcom.sun.management.jmxremote.authenticate=false
```



若是在 IDEA 开发工具中进行验证，按照下图进行配置，跑程序就行。



<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFZOAulnB0HNw8v3AHdww8LUkoFl0kjjGk2TuAjOE92DsThZOeeaxGicA/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



若是命令行启动时，按照下述方式配置，启动就行。

```
java -cp . -Djava.rmi.server.hostname=127.0.0.1 -Dcom.sun.management.jmxremote.port=8099 -Dcom.sun.management.jmxremote.rmi.port=8099  -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false className
```



启动远程 Java 程序，JConsole 输入远程服务 IP 和 端口，连接即可。



<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFCiaBKnDian8Gg6kcnAl4ibmqvM45NN5MCxUib4xwZyQmlVT4ROZcuWfSKw/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



***2.2.*** **监控 Java 程序概况**

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFEup0BHf0ibVssMDSD8zWmx1d1c75ibu74HsqpRYITSBBCRX05rJEIYgA/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />

如图所示，JConosle 连接上要监控的 Java 程序后，可以很方便的查看堆内存使用量、线程数量、加载类的数量以及 CPU 的占用率。

***2.3.*** **内存监控**

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBF98JfEuG7X7ypZSVKMuTUvWGL34XstZFNUsGCdnWx3yb8SxVg7jHUdw/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



如图所示，在 JConsole 提供的内存监控页面，不仅能看到堆内存的使用情况，而且能查看非堆区的内存使用情况等等。另外，还提供了让 Java 应用强制进行一次 GC 的功能。

***2.4.*** **线程监控**

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFNM1y4BXZt5dywdXg8WrXNjC3jIFc8Lrq9icwwmicUrt61LSueWAeFEnw/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />

如图所示，通过 JConsole 提供的线程页面，可以方便查看系统内的线程数量，以及程序中所有的线程，并且还能看到线程的栈信息。另外，该页面还提供了检测死锁的支持，那么就可以快速的帮我们定位死锁的问题啦。



***2.5.*** **类加载情况**



<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFfcInWfV7XR70ibCVjibstTbzia8n21uGDfq2JGkV8u4AQnz19UcwPlTsg/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />





如图所示，JConsole 还能够显示类加载情况，包括已经装载的类数量，以及已经卸载的类数量。



***2.6.*** **VM 摘要**

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFCXkTiaR87XzRceynVNShLGMISXmadxUMaicCu6pmLiaj5WbeTgc6RL0oA/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



如图所示，JConsole 提供的 VM 概要页面，能够显示当前 Java 应用程序的基本信息，包括运行环境、系统线程信息、堆信息等等。

***2.7.*** **MBean 管理**

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFv3zFZC1sImrKgFAOXqwstHc3dKKC5n5Qm1dqq3pszV0EK6YkbsVD9Q/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom: 50%;" />



通过 JConsole 提供的 MBean 页面，我们可以对应用中的 MBean 进行统一管理，鉴于之前在剖析 Resin 服务器源码的时候，我们多次用到过，本次不再铺开去说。

***3.*** **VisualVM 工具**

Visual VM 是可以替代 jstat、jmap、jhat、jstack 命令的一款故障诊断和性能监控的可视化工具，甚至可以替代 JConsole，所以我们还是有必要进行了解一下。

***3.1*** **连接要监控的 Java 程序**

首先进入 JDK 安装的 bin 目录，运行 jvisualvm，启动起来后和 JConsole 一样，可以选择本地和远程进行连接，效果如下。

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFQSqZLRZcZc16sBZpM3ibJiafBGaZgOiaLbRY99icAXkwCDL2Kn3Eiace7fA/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFMJic6icc2YaLhVC6Dx5wPbickE7wmON4DMdOoChbzrewbHQPkSO7DGM2Q/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



***3.2*** **概述**

**
**

![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFNLzZQVYlY8DrVnOED6v7RgcTNrxYQVMX5s8ty3so2VLo24LTyKpqbg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



如图所示，通过 VisualVM 提供的概述功能页，可以很方便的查看 Java 程序的进程 ID、JVM 参数、系统属性等等信息。



***3.3*** **监视**

**
**

![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFuRk9KwXmicu9bRCArWVu367fXZ07LOFXvLH3kPiazJF3aNgiaAkofAUzg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)





这块和 JConsole 很像，VisualVM 将 CPU 使用情况、堆使用情况、类加载信息以及线程都做了图形界面展示，可以很直观的进行监测。



***3.4*** **线程监控**

![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFxTz763dcqialZRDLLbItAO3iccFkicxoRQWibB4zdY7IR7fy6Lrtkb7aUw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



VisualVM 可以展示详细的线程信息，让线程信息一览无余，并且会自动进行死锁检测，如果在当前程序中找到死锁，则会提示“检测到死锁”。另外，通过线程 Dump可以导出当前所有线程的堆栈信息。



***3.5*** **抽样器**

![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFYa7Jdib8qRffPYqicfQxHqw8ARxuWCTaSFjxUgqu8Rws7ia9kGgaBmMiag/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



Visual VM 提供 CPU 和内存两个抽样器。通过 CPU 抽样器，可以帮助我们快速找到程序中占用 CPU 时间最长的方法；通过内存抽样器，可以帮助我们查看当前程序的堆信息。



CPU抽样器效果图：

![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFZicqffvcT2jFicXg79hR88ialzkvkYxtNvY6Vib4y5fArb00rWQBOmopTQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



内存抽样器效果图：

![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFShSDkjpXuia4b3IJU1gOjcFW1jZ1mWUwh8dnt1Z223wEI6icjNibAwwVQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



***3.6*** **Profiler**

Profiler 的功能和抽样器其实差不多，只不过抽样器是抽样进行检测，而 Profiler 是全面进行检测。



![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFgKRfaM8hgPutSCvBTZiaZNraOmm4zXxe8R8kTbKFnQHx1Fk39BBkibrw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



点击 CPU 按钮，效果如上图所示，则开启一个 CPU 性能分析会话，等 VisualVM 收集和统计完相关性能数据信息，将会显示在性能分析结果。

![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFXOanMnwrY2wCicbcuqH6UCtUv0VFtOyrTYYBceEQqO25u5BmEjckUCg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



若点击内存按钮，则开启一个内存分析会话，等 VisualVM 收集和统计完相关性能数据信息，将会显示在性能分析结果，效果如上图所示。



***3.7*** **快照**

![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFuwyhcKyTzKPajh8yicq8Sticp9yYX3ocCaL4JafS2Gf2mHqq4pvAfDicw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



VisualVM 很多地方，都提供了快照功能，可以让我们保存某一个时刻应用程序的堆信息、线程堆栈等等保存成快照，以便性能优化后进行对比、分析使用。



***3.8*** **插件**

![img](https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWOAwZIKY1S6WtouZhbBbibBFEvo4YoDwuoibibBlravsmcMQObWfNyVwG02DYtuHsZ2oukq03LU5rHAQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



VisualVM 还可以通过安装插件，来实现更多可能性。

<img src="https://i.loli.net/2020/04/03/iDLq5SyVfZ1EIeb.jpg" alt="iDLq5SyVfZ1EIeb" style="zoom:67%;" />

**3.5. 数学运算，搞不好会倾家荡产。**

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWNDYjkiaHumNA9zLAOHs1y9ibrdfXibq63Z6wBpGuxgQKFKwtUCxfDwkGAZ7Z6hPrcpXBM7bJr8iaH8XQ/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



输出结果：0.010000000000005116，一般遇到需要用到浮点数运算的地方，都可以使用 java.math.BigDecimal。

建议修改为：

<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWNDYjkiaHumNA9zLAOHs1y9ibvic3HEAXuEsr9CibfvDia5ksF6iaLp5bZXSo4LRxUarpFX1icO7XSkJyCYw/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />



注意构造 BigDecimal 的参数为 String，千万别搞错了，这也是新手易犯的问题。



有些时候怎么调，都不通，就想知道 jar 包里面都写了点啥？在此，推荐一款用的最多的**反编译工具 JD-GUI**

SonarQuable，它是一个用于代码质量管理的开源平台，也有助于帮你进行代码审查，提升代码质量。



<img src="https://mmbiz.qpic.cn/mmbiz_png/waH0DGXhQWO8hhw7iboriaQYnG4pIZJMbzVKROzYtA8SZOQyVfy6H3AiaeRuzGCGRvCPD6CBNDR0sIMb6NCs95eyg/640?wx_fmt=png&amp;tp=webp&amp;wxfrom=5&amp;wx_lazy=1&amp;wx_co=1" alt="img" style="zoom:50%;" />

## Java 程序该怎么优化？技巧篇

[Java 程序该怎么优化？技巧篇](https://mp.weixin.qq.com/s/jyJ1NkmU7_kOJEaR1x3rLQ)

1.字符串处理优化，乃优化之源。
1.1.字符串分割

<img src="https://i.loli.net/2020/04/03/Ur9L7lmsEXTkWZC.jpg" alt="Ur9L7lmsEXTkWZC" style="zoom:50%;" />

从运行效果， **StringTokenizer(?) 其效率高于 split() 方法**。
所以，在能够使用 StringTokenizer 进行处理的地方，就尽量使用 StringTokenizer 进行字符串分割处理。
1.2. 字符串拼接

<img src="https://i.loli.net/2020/04/03/QSm4ZOjWqwRAv8l.png" alt="QSm4ZOjWqwRAv8l" style="zoom:50%;" />

使用 + 号拼接字符串，其效率明显较低，采用 StringBuilder 来完成字符串连接性能相对较好，同理，如果需要考虑线程安全的情况下，可以选择 StringBuffer。

2. 善用 arraycopy()，让数组复制不再难。
    对数组的操作，如果能用 **System.arraycopy()** 这个方法实现，建议尽量去使用。

  ```java
  #                   srcPos   destPos
  System.arraycopy(src, 0, dest, 0, size);
  ```

  Arrays.copyOf() 底层还是调用 System.arraycopy() 来实现

3. 集合用的多，使用场景要注意。
    充分的选择好数据结构进行数据存储，便是最好的程序优化

  <img src="https://i.loli.net/2020/04/03/eTUpxjqoa6GCVc4.jpg" alt="eTUpxjqoa6GCVc4" style="zoom:50%;" />
  <img src="https://i.loli.net/2020/04/03/qXCN53kAeG8BdS6.jpg" alt="qXCN53kAeG8BdS6" style="zoom:50%;" />
  <img src="https://i.loli.net/2020/04/03/YUmnQDwNFx9ruIa.jpg" alt="YUmnQDwNFx9ruIa" style="zoom:50%;" />

4. 缓冲，让子弹飞一会儿。

  <img src="https://i.loli.net/2020/04/03/MaIx8jJZ2klen7E.png" alt="MaIx8jJZ2klen7E" style="zoom:50%;" />

  所以，文件读写操作时尽量都使用缓冲流进行操作，有助于提升性能。

5. 缓存，让坐飞机的和坐驴车的打交道。

针对银行编码等一些使用频率较高的业务数据，或者来之不易的计算结果，都可以保存到缓存中，当再次使用时，直接从缓存中获取，而不需要再占用宝贵的系统资源。

目前有很多基于 Java 的缓存框架，而我用的最多的是 **EhCache(?)**。

2. 
    日志记的好，线上没烦恼。

3. 一个优化原则。**先实现业务功能，再考虑优化性能**，如果功能都没实现，谈其它的都白扯。
    一个调优思路。

  <img src="https://i.loli.net/2020/04/03/duRHw6hoGegFOtv.jpg" alt="duRHw6hoGegFOtv" style="zoom:50%;" />

# 03-29

### GsonFormat

我们在接外部接口时，别人给了一串JSON串，我们在代码中需要将JSON中的字段封装到一个类中，一个一个输入挺麻烦的，这时GsonFormat就可以派上用场了。它可以帮助我们根据JSON中的key快速生成我们需要的类。

它的使用快捷键是Option + S

![img](https:////upload-images.jianshu.io/upload_images/2706762-f29945eec825a296.gif?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

GsonFormat



作者：Jackeyzhe
链接：https://www.jianshu.com/p/d7282aec7665
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# 02-17

[AssertJ一分钟入门 - 简书](https://www.jianshu.com/p/e078043071ff)



# 02-14

[PatMartin/Dex: Dex : The Data Explorer -- A data visualization tool written in Java/Groovy/JavaFX capable of powerful ETL and publishing web visualizations.](https://github.com/PatMartin/Dex)

# 02-09



### 写 Excel word 

[Java POI Excel - Java代码实例™](https://www.yiibai.com/javaexamples/java_apache_poi_excel.html)
[Java POI Word - Java代码实例™](https://www.yiibai.com/javaexamples/java_apache_poi_word.html)

### 写pdf

[Java iText示例 - Java代码实例™](https://www.yiibai.com/javaexamples/java_itext.html)

### 读 PDF XML HTML

[Apache Tika示例 - Java代码实例™](https://www.yiibai.com/javaexamples/java_apache_tika.html)

### 目录 文件处理

[Java目录 - Java代码实例™](https://www.yiibai.com/javaexamples/java_directories.html)

了解如何在Java编程中访问和操作目录。下面是有关Java编程中访问和操作目录最常用的例子 -

| 示例                                                         |
| ------------------------------------------------------------ |
| [如何递归创建目录？](http://www.yiibai.com/javaexamples/dir_create.html) |
| [如何删除目录？](http://www.yiibai.com/javaexamples/dir_delete.html) |
| [如何确定一个目录是否为空？](http://www.yiibai.com/javaexamples/dir_empty.html) |
| [如何确定目录是否隐藏？](http://www.yiibai.com/javaexamples/dir_hidden.html) |
| [如何打印目录层次结构？](http://www.yiibai.com/javaexamples/dir_hierarchy.html) |
| [如何打印目录的最后修改时间？](http://www.yiibai.com/javaexamples/dir_modification.html) |
| [如何获取文件的父目录？](http://www.yiibai.com/javaexamples/dir_parent.html) |
| [如何搜索目录中的所有文件？](http://www.yiibai.com/javaexamples/dir_search.html) |
| [如何获取目录的大小？](http://www.yiibai.com/javaexamples/dir_size.html) |
| [如何遍历目录？](http://www.yiibai.com/javaexamples/dir_traverse.html) |
| [如何查找当前工作目录？](http://www.yiibai.com/javaexamples/dir_current.html) |
| [如何在系统中显示根目录？](http://www.yiibai.com/javaexamples/dir_root.html) |
| [如何在目录中搜索文件？](http://www.yiibai.com/javaexamples/dir_search_file.html) |
| [如何显示目录中的所有文件？](http://www.yiibai.com/javaexamples/dir_sub.html) |
| [如何显示目录中的所有目录？](http://www.yiibai.com/javaexamples/dir_display.html) |

[Java文件 - Java代码实例™](https://www.yiibai.com/javaexamples/java_files.html)

了解如何使用Java编程中的文件操作。下面是常用的一些例子 -

| 示例                                                         |
| ------------------------------------------------------------ |
| [如何比较两个文件的路径？](http://www.yiibai.com/javaexamples/file_compare.html) |
| [如何创建新文件？](http://www.yiibai.com/javaexamples/file_create.html) |
| [如何获取文件的最后修改日期？](http://www.yiibai.com/javaexamples/file_date.html) |
| [如何在指定的目录中创建文件？](http://www.yiibai.com/javaexamples/file_dir.html) |
| [如何检查文件是否存在？](http://www.yiibai.com/javaexamples/file_exist.html) |
| [如何设置文件为只读？](http://www.yiibai.com/javaexamples/file_read_only.html) |
| [如何重命名文件？](http://www.yiibai.com/javaexamples/file_rename.html) |
| [如何获取文件大小(以字节为单位)？](http://www.yiibai.com/javaexamples/file_size.html) |
| [如何更改文件的最后修改时间？](http://www.yiibai.com/javaexamples/file_date_modify.html) |
| [如何创建一个临时文件？](http://www.yiibai.com/javaexamples/file_create_temp.html) |
| [如何向现有文件中附加一个字符串？](http://www.yiibai.com/javaexamples/file_append.html) |
| [如何将一个文件复制到另一个文件？](http://www.yiibai.com/javaexamples/file_copy.html) |
| [如何删除文件？](http://www.yiibai.com/javaexamples/file_delete.html) |
| [如何读取一个文件？](http://www.yiibai.com/javaexamples/file_read.html) |
| [如何写入一个文件？](http://www.yiibai.com/javaexamples/file_write.html) |



# 2020年1月20日(星期一)

#### JMeter

- 官网地址 ：[https://jmeter.apache.org](https://jmeter.apache.org/)

> Apache JMeter是Apache组织开发的基于Java的压力测试工具
>
> 是的就是用来压测的，你怎么模拟很多请求呀，就用它就对了。

# 2020 年 1 月 15 日


## [可以提高千倍效率的Java代码小技巧 - 51CTO.COM](https://developer.51cto.com/art/201905/596234.htm)

## [程序员必须搞懂的20个Java类库和API - 51CTO.COM](https://developer.51cto.com/art/201905/596295.htm)

### 一、日志相关类库

Log4j 、 SLF4j 和 LogBack
应该熟悉日志记录的利弊， 并且了解为什么 SLF4J 要比 Log4J 要好

### 二、JSON 解析库

Jackson 和 Gson

### 三、单元测试库

常见的单测框架有 JUnit , Mockito 和 PowerMock 。

### 四、通用类库

例如 Apache Commons 和 Google Guava。

### 五、Http 库

Apache HttpClient 和 HttpCore 等开源类库

### 六、XML 解析库

市面上有很多 XML 解析的类库，如 Xerces , JAXB , JAXP , Dom4j , Xstream 等。
**Xerces2**是下一代高性能，完全兼容的 XML 解析工具。Xerces2 定义了 Xerces Native Interface (XNI)规范，并提供了一个完整、兼容标准的 XNI 规范实现。该解析器是完全重新设计和实现的，更简单以及模块化。

### 七、Excel 读写库

Apache POI API

### 八、字节码库

javassist 、Cglib Nodep 、 ASM 。
Javassist 使得 Java 字节码操作非常简单。它是一个为编辑 Java 字节码而生的类库。

### 九、数据库连接池库

Commons Pool 和 DBCP

### 十、消息传递库

Java 提供了 JMS Java 消息服务，但这不是 JDK 的一部分，你需要单独的引入 jms.jar。类似地，如果您准备使用第三方消息传递协议， Tibco RV 是个不错的选择。

### 十一、PDF 处理库

iText 和 Apache FOP 类库

### 十二、日期和时间库

Java 8 自带

### 十三、集合类库

虽然 JDK 有丰富的集合类，但还是有很多第三方类库可以提供更多更好的功能。如 **Apache Commons Collections 、 Goldman Sachs collections 、 Google Collections 和 Trove** 。Trove 尤其有用，因为它提供所有标准 Collections 类的更快的版本以及能够直接在原语(primitive)(例如包含 int 键或值的 Map 等)上操作的 Collections 类的功能。
**FastUtil**是另一个类似的 API，它继承了 Java Collection Framework，提供了数种特定类型的容器，包括映射 map、集合 set、列表 list、优先级队列(prority queue)，实现了 java.util 包的标准接口(还提供了标准类所没有的双向迭代器)，还提供了很大的(64 位)的 array、set、list，以及快速、实用的二进制或文本文件的 I/O 操作类。

### 十四、邮件 API

javax.mail 和 Apache Commons Email 提供了发送邮件的 API。它们建立在 JavaMail API 的基础上，提供简化的用法。

### 十五、HTML 解析库

jsoup 可以解析 HTML，创建 HTML 文档

### 十六、加密库

Apache Commons 家族中的 Commons Codec 就提供了一些公共的编解码实现，比如 Base64, Hex, MD5,Phonetic and URLs 等等。

### 十七、嵌入式 SQL 数据库库

**H2** 是一种内存数据库，可以嵌入到 Java 应用中。在跑单测的时候如果需要一个数据库，用来验证 SQL 的话，H2 是个很好的选择。还有 Apache Derby 和 HSQL 可供选择。

### 十八、JDBC 故障诊断库

有不错的 JDBC 扩展库的存在使得调试变得很容易，例如**P6spy**，这是一个针对数据库访问操作的动态监测框架，它使得数据库数据可无缝截取和操纵，而不必对现有应用程序的代码作任何修改。 P6Spy 分发包包括 P6Log，它是一个可记录任何 Java 应用程序的所有 JDBC 事务的应用程序。其配置完成使用时，可以进行数据访问性能的监测。

### 十九、序列化库

**Google Protocol Buffer**是一种轻便高效的结构化数据存储格式，可以用于结构化数据串行化，或者说序列化。它很适合做数据存储或 RPC 数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化结构数据格式。目前提供了 C++、Java、Python 三种语言的 API。

### 二十、网络库

一些有用的网络库主要有 **Netty 的和 Apache Mina** 。如果您正在编写一个应用程序，你需要做的底层网络任务，可以考虑使用这些库。

## [Java 性能瓶颈分析工具](https://developer.51cto.com/art/201905/596229.htm)

### 1. Java Mission Control

gui
Oracle Java 官方项目

### 2. Tprofiler

cli
命令行
TProfiler 是淘宝开源的一个可以在生产环境长期使用的性能分析工具。

### 3. Jprofiler

gui
JProfiler 是由 ej-technologies 公司开发的一款性能瓶颈分析工具。

### 4. Arthas

Arthas 是阿里最近刚刚开源的 Java 生成环境诊断工具。
Arthas 支持在 Linux/Unix/Mac 等平台上进行一键安装，现在处于试用于反馈阶段，感兴趣的同学可以自己研究试用。
