---
title: cli
toc: true
recommend: 1
uniqueId: '2020-03-04 03:28:19/"cli".html'
date: 2020-03-04 11:28:19
thumbnail:
categories:
- Mac
tags:
keywords:
---

[TOC]

<!--more-->

## jq

```shell
echo '{"e":1,"m":"今天已经填报了","d":{}}' | jq -r .e

jq [options] filter [files]
**options：**
--version：输出jq的版本信息并退出
--slurp/-s：读入整个输入流到一个数组。
--raw-input/-R：不作为JSON解析，将每一行的文本作为字符串输出到屏幕。
--null-input/ -n：不读取任何输入，过滤器运行使用null作为输入。一般用作从头构建JSON数据。
--compact-output /-c：使输出紧凑，而不是把每一个JSON对象输出在一行。
--colour-output / -C：打开颜色显示
--monochrome-output / -M：关闭颜色显示
--ascii-output /-a：指定输出格式为ASCII
-raw-output /-r ：如果过滤的结果是一个字符串，那么直接写到标准输出（去掉字符串的引号）

**filter：**
.   : 默认输出
.foo: 输出指定属性，foo代表属性。
.[foo] ：输出指定数组元素。foo代表数组下标。
.[]：输出指定数组中全部元素
， ：指定多个属性作为过滤条件时，用逗号分隔
| ： 将指定的数组元素中的某个属性作为过滤条件

**files：**
    JOSN格式文件。
```

-r ：如果过滤的结果是一个字符串，那么直接写到标准输出（去掉字符串的引号）



## curl

```bash
-o 指定下载文件名
-O 默认下载文件名
-s 不输出没用的统计信息
-x 192.168.100.198:8888 代理
-A/--user-agent <string>
-e/--referer    来源网址
# 循环下载
curl -O http://www.51cto.com/justin[1-5].png
# 分块下载  -r 0-100
-r/--range <range>
# 下载进度条  -s 不会显示下载进度信息
-#/--progress-bar
# 断点续传
-C/--continue-at <offset>
# 上传文件
-T/--upload-file <file>    上传文件
# 如 curl -T justin.png -u justin:peng ftp://www.baidu.com/img/
-F 表单提交
# 如 curl -F prefile=@portrait.jpg https://example.com/upload.cgi
```

