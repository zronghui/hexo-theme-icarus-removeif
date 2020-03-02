---
title: aria2
toc: true
recommend: 1
uniqueId: '2020-03-01 15:37:02/"aria2".html'
date: 2020-03-01 23:37:02
thumbnail:
categories:
- Mac
tags:
keywords:
---

[TOC]

<!--more-->

## 配置

主要用 aria2 gui

dir=/Volumes/My\ Passport/data/ut下载/0\ \ 未分类/ytdl\ videos/bilibili/

[Aria2配置教程（Mac和Windows） - Justin Smith - Medium](https://medium.com/@Justin___Smith/aria2%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B-mac%E5%92%8Cwindows-b31d0f64bd4e)

### Aria2 配置

[Aria2 & YAAW 使用说明](http://aria2c.com/usage.html)
[aria2c(1) — aria2 1.35.0 documentation](https://aria2.github.io/manual/en/html/aria2c.html)

### win mac aria2下载

[https://pan.baidu.com/s/1nu4UHfV#list/path=%2F&parentPath=%2Fsharelink2668081893-566048232074527](https://pan.baidu.com/s/1nu4UHfV#list/path=%2F&parentPath=%2Fsharelink2668081893-566048232074527)



## 网页端管理软件

[active: 7 - waiting: 0 - stopped: 0 — Aria2 WebUI](https://ziahamza.github.io/webui-aria2/)
[↓214 KB - Yet Another Aria2 Web Frontend](http://binux.github.io/yaaw/demo/)



查看 aria2 状态

```shell
ps aux|grep aria2c
```



```shell

/Applications/Aria2GUI.app/Contents/Resources/aria2c --dir=/Volumes/My\ Passport/data/ut下载/0\ \ 未分类/ytdl\ videos/bilibili/ --conf-path=/Applications/Aria2GUI.app/Contents/Resources/aria2.conf --input-file=/Applications/Aria2GUI.app/Contents/Resources/aria2.session --save-session=/Applications/Aria2GUI.app/Contents/Resources/aria2.session --max-concurrent-downloads=10 --max-connection-per-server=16 --min-split-size=1024K --split=16 --max-overall-download-limit=0K --max-overall-upload-limit=0K --max-download-limit=0K --max-upload-limit=0K --continue=true --auto-file-renaming=true --allow-overwrite=true --disk-cache=0M --max-tries=0 --retry-wait=5 -D
```

