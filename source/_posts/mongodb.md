---
title: mongodb
toc: true
recommend: 1
uniqueId: '2020-03-16 02:13:14/"mongodb".html'
date: 2020-03-16 10:13:14
thumbnail:
categories:
tags:
keywords:
---

[TOC]

<!--more-->

## 快速安装

MongoDB 已经宣布不再开源，从2019年9月2日开始 ，HomeBrew 也从核心仓库 (#43770) 当中移除了mongodb 模块

```shell
# brew install mongodb
brew info mongodb
# No available formula with the name "mongodb"
```

```shell
brew services stop mongodb
brew uninstall mongodb
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

### brew

> brew 又叫 Homebrew，是 Mac OSX 上的软件包管理工具，能在 Mac 中方便的安装软件或者卸载软件， 只需要一个命令， 非常方便
>
> brew 类似 ubuntu 系统下的 apt-get 的功能
>
> 安装：`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`
>
> *   brew list 列出已安装的软件
> *   brew update 更新 brew
> *   brew home 用浏览器打开 brew 的官方网站
> *   brew info 显示软件信息
> *   brew deps 显示包依赖

### 安装 mongodb

> 如果你用 brew install mongodb，可能会报错：  
> `No available formula with the name "mongodb"`
>
> 然后你需要的操作是：
>
> `brew services stop mongodb`
> `brew uninstall mongodb`
> `brew tap mongodb/brew`
> `brew install mongodb-community`
> `brew services start mongodb-community`

> 查看 mongodb 版本：mongo。  
> 接着在终端中输入 `mongod`, 新建终端接着输入`mongo`，连接成功。

### 文件路径

> 配置文件：/usr/local/etc/mongod.conf  
> 日志目录路径：/usr/local/var/log/mongodb  
> 数据目录路径：/usr/local/var/mongodb

### 启动 && 停止 mongodb-community 服务器

#### mongod 作为服务运行

> `brew services start mongodb-community`  
> `brew services stop mongodb-community`

#### 手动启动 mongod

> `mongod --config /usr/local/etc/mongod.conf`  
> 注意：如果您不包含 --config 带有配置文件路径的选项，则 MongoDB 服务器没有默认配置文件或日志目录路径，并将使用数据目录路径 / data/db。  
> 要 mongod 手动关闭，请使用 admin 数据库并运行 db.shutdownServer()：  
> `mongo admin --eval "db.shutdownServer()"`





## 参考链接

[Mac下使用brew安装mongodb--2020版 - 简书](https://www.jianshu.com/p/67b3e99bb8e3)
