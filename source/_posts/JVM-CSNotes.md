---
title: JVM-CSNotes
toc: true
recommend: 1
uniqueId: '2020-09-10 01:32:08/"JVM-CSNotes".html'
date: 2020-09-10 09:32:08
thumbnail:
categories:
tags:
keywords:
---

[TOC]

<!--more-->



<img src="https://i.loli.net/2020/09/10/VWu58GtKxPXwjdJ.png" alt="image-20200910093659670" style="zoom:50%;" />

### PC register

[CS-Notes](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E8%99%9A%E6%8B%9F%E6%9C%BA?id=%e4%b8%80%e3%80%81%e8%bf%90%e8%a1%8c%e6%97%b6%e6%95%b0%e6%8d%ae%e5%8c%ba%e5%9f%9f)
[Program counter - Wikipedia](https://en.wikipedia.org/wiki/Program_counter)
[What is program counter? - Definition from WhatIs.com](https://whatis.techtarget.com/definition/program-counter)

PC (program counter)

> 程序计数器（PC）通常称为指令指针（IP），有时也称为指令地址寄存器（IAR）

> 通常，PC 在获取一条指令后递增，并保存下一条将要执行的指令的内存地址（“指向”）

A program counter is a [register](https://whatis.techtarget.com/definition/register) in a computer [processor](https://whatis.techtarget.com/definition/processor) that contains the address (location) of the [instruction](https://whatis.techtarget.com/definition/instruction) being executed at the current time. As each instruction gets [fetched](https://searchsqlserver.techtarget.com/definition/fetch), the program counter increases its stored value by 1. After each instruction is fetched, the program counter points to the next instruction in the sequence. When the computer restarts or is reset, the program counter normally reverts to 0.

所以说 **PC register 是正在执行的计算机指令的地址**



### JVM stack

一个方法对应一个栈帧，方法的调用和结束对应着栈帧的入栈和出栈

栈帧存储 常量池引用、操作数栈、局部变量表 等信息

**reference to constant pool**

常量池引用对应 runtime constant pool

**operand stack 、local variable array**

操作数栈 局部变量表

[java - What is an operand stack? - Stack Overflow](https://stackoverflow.com/questions/24427056/what-is-an-operand-stack)

[【JVM】1.1、局部变量表与操作数栈 - 简书](https://www.jianshu.com/p/a6a9734ef13d)

局部变量表是一组变量值存储空间，用于存放方法参数和方法内部定义的局部变量

虚拟机把操作数栈作为它的工作区——大多数指令都要从这里弹出数据，执行运算，然后把结果压回操作数栈

局部变量表与操作数栈加法案例

before starting // 加载100和98到局部变量表中
after iload_0   // 加载100到操作数栈中
after iload_1   // 加载98到操作数栈中
after iadd      // 操作100+98命令
after istore_2  // 弹出结果到局部变量表中



### 本地方法栈

与 jvm stack 类似，不过是为本地方法服务

本地方法一般是用其他语言编写的，是 操作系统和硬件 相关的程序



## 垃圾收集器

[【JVM】3.2、垃圾收集器（二） - 简书](https://www.jianshu.com/p/5027e9d54df7)

写的特别 NB

至少本菜鸟看懂了

再加上我做的这张图，基本可以了解 7 种垃圾收集器了

![image-20200915214400976](https://i.loli.net/2020/09/15/2jSgRPWy7AtlkB3.png)





