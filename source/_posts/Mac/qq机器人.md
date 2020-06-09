---
title: qq机器人
toc: true
uniqueId: '2020-06-09 06:24:46/"qq机器人".html'
top: -1
encrypt: true
password: 1
abstract: 咦，这是一篇加密文章，好像需要输入密码才能查看呢！
message: 嗨，请准确无误地输入密码查看哟！
wrong_pass_message: 不好意思，密码没对哦，在检查检查呢！
wrong_hash_message: 不好意思，信息无法验证！
date: 2020-06-09 14:24:46
thumbnail:
categories:
- Mac
tags:
keywords:
---


[TOC]

<!--more-->

## 安装启动

[QQ机器人 | ASPIRE](https://ixyzero.com/blog/archives/4463.html)

[Docker - CQHTTP 插件文档](https://cqhttp.cc/docs/4.15/#/Docker)

[30分钟写一个简单QQ自动回复机器人 - 友人C](https://www.ihewro.com/archives/979/comment-page-1)

```shell
docker pull richardchien/cqhttp:latest
mkdir -p ~/data/coolq
docker run -ti -d --rm --name cqhttp-test  -v ~/data/coolq:/home/user/coolq  -p 4990:9000  -p 5700:5700 -e VNC_PASSWD=12345678 -e COOLQ_ACCOUNT=1326053532 -e CQHTTP_POST_URL=http://host.docker.internal:4991  -e CQHTTP_SERVE_DATA_FILES=yes  richardchien/cqhttp:latest
```

登录地址 http://127.0.0.1:4990



## cqhttp

[richardchien/coolq-http-api: 为 酷Q 提供通过 HTTP 或 WebSocket 接收事件和调用 API 的能力](https://github.com/richardchien/coolq-http-api)
[cqmoe/python-cqhttp: A Python SDK for CQHTTP.](https://github.com/cqmoe/python-cqhttp)

```shell
pip install cqhttp
```







## 问题

### VNC登录QQ

[Docker配置并使用CoolQ机器人 - 简书](https://www.jianshu.com/p/baa747a54d5e)

登录时要是一路顺风就没什么好说的，但会出现一个坑，就是异地登录，如果遇到异地登录会让你用chrome插件来验证

解决方法就是，在提示你用chrome的时候，你选择否，再登录一次就会使用到旧版的VNC。后面会让你开启设备锁发短信来验证登录（来自论坛管理回复，亲测有效）
