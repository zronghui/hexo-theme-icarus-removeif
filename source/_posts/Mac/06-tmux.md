---
title: tmux
toc: true
recommend: 1
uniqueId: '2020-03-04 14:27:32/"tmux".html'
date: 2020-03-04 22:27:32
thumbnail:
categories:
- Mac
tags:
keywords:
---

[TOC]

<!--more-->

[tmux/tmux: tmux source code](https://github.com/tmux/tmux)



[技术|tmux：适用于重度命令行 Linux 用户的终端复用器](https://linux.cn/article-10480-1.html)

[MjSeven - Linux 中国翻译组（LCTT）- Linux 中国◆开源社区](https://linux.cn/lctt/MjSeven)

## 使用

- [使用 tmux 打造更强大的终端](https://linux.cn/article-8421-1.html)

- [Tmux 速成教程：技巧和调整](http://blog.jobbole.com/87584/)

- [tmux-yank](https://tmux-plugins.github.io/tmux-yank/)

- [终端复用神器 tmux 简介配置及高频操作 | 个人服务器运维指南 | 山月行](https://shanyue.tech/op/tmux-setting.html#%E9%87%8D%E5%91%BD%E5%90%8D%E7%AA%97%E5%8F%A3%E5%90%8D)

**我常用的**

```shell
tmux list-windows
tmux new -s test  # s -- session-name
[in test] tmux set -g mouse on
  
C-b "          模向分隔窗口
C-b %          纵向分隔窗口
  
tmux detach / C-b d (detach) / exit

tmux list-windows
tmux kill-window -t 0
tmux list-sessions
tmux kill-session -t test
 
tmux attach # attach 最近的 session
tmux attach -t [session-name] # 指定 session attach
```

  

加到 ~/.zshrc 中

  ```shell
tmux set -g mouse on > /dev/null 2>&1
  ```

  

  **没用过的命令**

```shell
$ sudo apt-get install tmux # ubuntu
$ sudo brew    install tmux    # osX
C-b ?          显示快捷键帮助

C-b C-o        调换窗口位置，类似与vim 里的C-w
C-b 空格键     采用下一个内置布局
C-b !          把当前窗口变为新窗口

C-b q          显示分隔窗口的编号
C-b o          跳到下一个分隔窗口
C-b 上下键     上一个及下一个分隔窗口
C-b ALT-方向键 调整分隔窗口大小
C-b c          创建新窗口
C-b 0~9        选择几号窗口
C-b c          创建新窗口

C-b w          以菜单方式显示及选择窗口
C-b t          显示时钟
C-b ;          切换到最后一个使用的面板
C-b x          关闭面板
C-b &          关闭窗口
C-b s          以菜单方式显示和选择会话

# 我试了 didn't work 的命令
C-b n (next)          选择下一个窗口(next)
C-b l (last)          切换到最后使用的窗口
C-b p (previous)      选择前一个窗口

```



## 安装

[tmux/tmux: tmux source code](https://github.com/tmux/tmux)

```shell
brew install tmux

# centos
j test
git clone https://github.com/tmux/tmux
git clone https://github.com/tmux/tmux.git
cd tmux
sh autogen.sh && ./configure && make
which tmux
./tmux -V
mv ./tmux /usr/bin/
tmux
```

