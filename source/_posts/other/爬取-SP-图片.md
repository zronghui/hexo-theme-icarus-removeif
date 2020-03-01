---
title: 爬取 SP 图片
toc: true
recommend: 1
uniqueId: '2020-03-01 06:09:16/"爬取 SP 图片".html'
date: 2020-03-01 14:09:16
thumbnail:
categories:
- other
tags:
keywords:
---

[TOC]

<!--more-->

爬取网站下的所有卡片

https://southparkphonedestroyer.fandom.com/wiki/Characters

 

## 用 siteSucker 爬取网页内容

## 命令行

```shell
grep -o --nocolor --nofilename --nonumbers "https://vignette.wikia.nocookie.net/southparkphonedestroyer/images.*png" | sort | uniq | grep -v '"' | grep -v ' ' | pbcopy
cd 图片保存目录
pbpaste | xargs wget
```

## 解释

Grep 是 ag 的alias

wget 是 axel 的 alias

ag 默认支持正则，注意需要用引号包裹

```shell
-o --only-matching      Prints only the matching part of the lines
--[no]numbers
```

grep -v 反选

## 感想

命令行还挺好用的，熟悉的话比 Python 脚本高效
