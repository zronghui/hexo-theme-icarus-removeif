---
title: redis
toc: true
recommend: 1
uniqueId: '2020-03-12 04:20:43/"redis".html'
date: 2020-03-12 12:20:43
thumbnail:
categories:
- 数据库
tags:
keywords:
---

[TOC]

<!--more-->

## 安装

### Mac：

```shell
brew install redis
# To have launchd start redis now and restart at login:
#   brew services start redis
# Or, if you don't want/need a background service you can just run:
#   redis-server /usr/local/etc/redis.conf
brew update;brew services start redis

cotEdit /usr/local/etc/redis.conf
注释 bind 127.0.0.1
取消注释 requirepass foobare， 并配置密码

brew services list                
# Name          Status  User         Plist
# elasticsearch stopped
# redis         started zhangronghui /Users/zhangronghui/Library/LaunchAgents/homebrew.mxcl.redis.plist
# unbound       stopped

brew services restart redis
redis-cli -a redispassword
```



### Linux 上：

<img src="https://i.loli.net/2020/03/12/CXDx73TjfPUGYwL.png" alt="CXDx73TjfPUGYwL" style="zoom:33%;" />

