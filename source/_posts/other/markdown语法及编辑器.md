---
title: markdown语法及编辑器
date: 2019-08-30 09:28:40
categories:
- other
toc: true
tags:
---
## 编辑器

 小书匠
 优点：功能齐全，全平台支持,~~有html转markdown功能~~[鸡肋]
 缺点：界面不好看，不能直接打开文件修改
    
 
其他软件：

[Markdown 编辑器推荐 | Markdown 简单的世界 ](https://wizardforcel.gitbooks.io/markdown-simple-world/content/1.html)

<!--more-->
## markdown语法学习
[Markdown 简单的世界](https://legacy.gitbook.com/book/wizardforcel/markdown-simple-world/details)


``` 
Markdown 基本语法 | Markdown 简单的世界

- 用反引号` 来标记内联代码，它们会解释成<code>标签

- 链接
[an example](http://example.com/)
[an example](http://example.com/ "Optional Title")

- 图像
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional Title")

- 自动链接
如果链接的地址和名字重复，可以用尖括号语法将其简化。+
<http://example.com/>
就相当于
[http://example.com/](http://example.com/)

- 代码区域
有两种方式标记代码区域，原生风格是行首缩进死个空格。

- 链接要么加上尖括号，要么两端加上空格。

- 还有一种是github的风格，代码段的前后用三个反引号独占一行来标记

- 可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格。下面每种写法都可以建立分隔线：+
* * *
***
*****
- - -
---------------------------------------

- 新建、删除或修改文章后，不需要重启hexo server，刷新一下即可预览

- 一行文字就是一个段落

- 如果你需要另起一段，请在两个段落之间隔一个空行

- 不隔一个空行的换行行为，在一些编辑器中被解释为换行，即插入一个<br />标签。对与另外一些编辑器，会被解释为插入一个空格。对于后者，若要插入换行标签，请在当前一行的结尾打两个空格

- 可以使用星号*或下划线_指定粗体或者斜体

- *这是斜体*
_这也是斜体_
**这是粗体**
***这是粗体+斜体***

- 一部分编辑器支持删除线，它不是经典markdown中的要素。用波浪线~定义删除线

- 通过在行首加上大于号>来添加引用格式

- 引用可以嵌套：+
> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.

- 无序列表使用星号、加号或是减号作为列表标记

- 有序列表则使用数字接着一个英文句点

· Highlighted Source : http://lnr.li/Q3Y2A/
· Original Source : https://wizardforcel.gitbooks.io/markdown-simple-world/content/2.html
```

