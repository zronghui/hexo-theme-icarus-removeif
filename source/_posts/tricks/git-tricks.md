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
---

[TOC]

<!--more-->



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
