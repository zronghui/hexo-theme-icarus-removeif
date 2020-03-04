---
title: ssh config editor
toc: true
uniqueId: '2020-03-02 03:07:19/"ssh config editor".html'
date: 2020-03-02 11:07:19
thumbnail:
categories:
- Mac
tags:
keywords:
---


[TOC]

<!--more-->

[SSH Config Editor Pro 1.11.5 破解版 for Mac 管理您的SSH配置文件](https://www.macwk.com/soft/ssh-config-editor)

<img src="https://i.loli.net/2020/03/02/iaAwzsQY4SJkZ2e.png" alt="iaAwzsQY4SJkZ2e" style="zoom: 33%;" />



<img src="https://i.loli.net/2020/03/02/z4VTgQKGlSZ6M8e.png" alt="z4VTgQKGlSZ6M8e" style="zoom:33%;" />



## 不借助软件的配置

打开~/.ssh/config文件，添加以下内容：

```ruby
Host server-alias           # server-alias为SSH链接的服务器别名
  HostName server-ip  # 服务器地址
  Port 22
  User username           # 服务器端用户名
  PreferredAuthentications publickey 
  IdentityFile ~/.ssh/id_rsa   # 私钥地址，默认为 ~/.ssh/id_rsa
```

验证
 以后即可通过以下命令登录远程服务器

```csharp
ssh server-alias
```

## 解决mac下ssh自动中断

```shell
echo "ServerAliveInterval 30" >>  ~/.ssh/config
```

每隔30秒，mac客户端会主动向服务器发出一次请求。
这样就使得服务器端认为客户端是一直在线状态，也就不会主动断开连接了。
