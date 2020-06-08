---
title: redis
toc: true
recommend: 1
uniqueId: '2020-05-29 07:56:39/"redis".html'
date: 2020-05-29 15:56:39
thumbnail:
categories:
- redis
tags:
keywords:
---



[TOC]

<!--more-->

# 学习



# 实践

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
pip install iredis
redis-cli/iredis -a redispassword --raw
```



### Linux 上

[centos安装redis - 掘金](https://juejin.im/post/5ecc754bf265da770f51f373)

```shell
# wget http://download.redis.io/releases/redis-5.0.7.tar.gz
rsync -azvhP ~/Downloads/Compressed/redis-5.0.7.tar.gz  root@47.93.53.47:/tmp
cd /tmp
tar xf /tmp/redis-5.0.7.tar.gz
cd redis-5.0.7
make
make PREFIX=/usr/local/redis install

mkdir /usr/local/redis/etc
cp redis.conf /usr/local/redis/etc
vim /usr/local/redis/etc/redis.conf

1）配置redis为后台启动：daemonize no  修改为 daemonize yes
2）开启外网访问：bind 127.0.01  注释掉
3）配置密码：requirepass 设置密码

pip install iredis
iredis --raw

vim ~/.zshrc
export PATH=/usr/local/redis/bin:$PATH

# 启动
redis-server /usr/local/redis/etc/redis.conf

iredis --raw
```



<img src="https://i.loli.net/2020/03/12/CXDx73TjfPUGYwL.png" alt="CXDx73TjfPUGYwL" style="zoom:33%;" />



# 工具

[laixintao/iredis: Interactive Redis: A Terminal Client for Redis with AutoCompletion and Syntax Highlighting.](https://github.com/laixintao/iredis)

```
pip install iredis
```

启动 redis-cli 或者更好的替代品 iredis ( pip install iredis )

```shell
iredis -a redispassword --raw # --raw 为了可以查看中文
```



[mylxsw/redis-tui: A Redis Text-based UI client in CLI](https://github.com/mylxsw/redis-tui)

# 其他

