---
title: git tricks
toc: true
recommend: 1
uniqueId: '2020-03-04 08:34:59/"git tricks".html'
date: 2020-03-04 16:34:59
thumbnail:
categories:
- tricks
tags:
keywords:
encrypt: true
password: 1
abstract: 咦，这是一篇加密文章，好像需要输入密码才能查看呢！
message: 嗨，请准确无误地输入密码查看哟
wrong_pass_message: 不好意思，密码没对哦，在检查检查呢！
wrong_hash_message: 不好意思，信息无法验证！
---

[TOC]

<!--more-->

## 移动文件

```shell
git mv from to
```



## 撤销 push 到 GitHub 的 commit

[git push提交成功后如何撤销回退 - 掘金](https://juejin.im/post/5a4c274851882560b652b23a)

```shell
git reset --hard commit-id
git push -u origion master -f
# commit id 可通过 git reflog 查看
```





## Git-本地已有仓库推送到远程

[Git-本地已有仓库推送到远程 - 简书](https://www.jianshu.com/p/53f7c7c75d72)

在GitHub 建立新仓库，注意不要勾选 readme



**git init**

**git remote add origin <你的远程仓库地址>**

编写 .gitignore

**git add .**

**git commit -m '...'**

**git push -u origin master** 



若勾选 readme：

**git pull origin master --allow-unrelated-histories**

如果进入到merge信息界面，说明成功了，只需要输入:wq,回车，如果不报错误，直接执行下面的语句即可

**git push -u origin master** 

## git ignore 

[gitignore.io - Create Useful .gitignore Files ForYour Project](https://gitignore.io/)

[如何在IntelliJ IDEA中使用.ignore插件忽略不必要提交的文件_idea,git,插件_qq_34590097的博客-CSDN博客](https://blog.csdn.net/qq_34590097/article/details/56284935)

安装插件 .gitignore

![7t3G8amIFX1ZQxC](https://i.loli.net/2020/02/04/7t3G8amIFX1ZQxC.png)

![dDcBSZC68KaT5oV](https://i.loli.net/2020/02/04/dDcBSZC68KaT5oV.png)

自动生成后，使用 vim .gitignore 添加(因为安装插件后，gitignore 只读)

.idea/
target/

注意以下几个的区别

.idea/  所有目录下

./.idea/ 当前目录

## [怎样删除github上的仓库 - 简书](https://www.jianshu.com/p/a2cf6ce1dfbc)

点击进入你要删除的仓库

找到Settings按钮，点击

拉到最下面找到Delete this repository按钮





## github 新仓库指南

### **Quick setup** — if you’ve done this kind of thing before

[ Set up in Desktop](x-github-client://openRepo/https://github.com/zronghui/hexo)

**or**

HTTPSSSH

Get started by [creating a new file](https://github.com/zronghui/hexo/new/master) or [uploading an existing file](https://github.com/zronghui/hexo/upload). We recommend every repository include a [README](https://github.com/zronghui/hexo/new/master?readme=1), [LICENSE](https://github.com/zronghui/hexo/new/master?filename=LICENSE.md), and [.gitignore](https://github.com/zronghui/hexo/new/master?filename=.gitignore).

### …or create a new repository on the command line



```
echo "# hexo" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:zronghui/hexo.git
git push -u origin master
```

### …or push an existing repository from the command line



```
git remote add origin git@github.com:zronghui/hexo.git
git push -u origin master
```

### …or import code from another repository

You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

[Import code](https://github.com/zronghui/hexo/import)



## Git 多远程仓库设置

vim .git/config

```shell
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = git@codehub.devcloud.cn-north-4.huaweicloud.com:zrhz-xxxtsx-UI00001/zronghui_xxxt.git
	url = git@github.com:zronghui/zronghui_xxxt.git
	url = root@47.93.53.47:/root/code/xxxtbare.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master

```



## 用 git hooks 进行自动部署

将本地 cat ~/.ssh/id_rsa.pub 复制到 服务器 ~/.ssh/authorized_keys 中

### 服务器

服务器两个仓库 xxxtbare.git 和 zronghui_xxxt

```shell
# 建立 bare 仓库
git init --bare xxxtbare.git
vim xxxtbare.git/hooks/post-receive

#!/bin/sh
echo '======上传代码到服务器======'
cd /root/code/zronghui_xxxt || exit
unset GIT_DIR #还原环境变量
git pull origin master
echo $(date) >> hook.log
echo '======代码更新完成======'

chmod u+x hooks/post-receive
```

### 本地

```shell
在仓库中
url = root@47.93.53.47:/root/code/xxxtbare.git
```

### 参考：

学习之前：好难懂；学会之后：都一堆废话

[用 git hooks 进行自动部署，从此不需要登录服务器 | Pure White](https://purewhite.io/2017/04/27/git-hooks-auto-deploy/)
[如何使用Git实现自动化部署你的项目 - 知乎](https://zhuanlan.zhihu.com/p/127355490)
[git hooks 实现自动部署 - 简书](https://www.jianshu.com/p/f9312b51e011)
