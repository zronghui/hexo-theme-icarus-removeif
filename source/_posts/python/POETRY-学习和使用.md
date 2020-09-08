---
title: POETRY 学习和使用
toc: true
recommend: 1
uniqueId: '2020-08-26 01:52:11/"POETRY 学习和使用".html'
date: 2020-08-26 09:52:11
thumbnail:
categories:
- python
tags:
keywords:
---

[TOC]

<!--more-->

原文地址 \[testerhome.com\](https://testerhome.com/topics/20929)

Poetry 是啥？
----------

是一个 Python 虚拟环境和依赖管理工具，另外它还提供了包管理功能，比如打包和发布。  
可以用来管理 python 库和 python 程序。

安装 Poetry
---------

```
curl -sSL https://raw.githubusercontent.com/sdispater/poetry/master/get-poetry.py | python3
```

使用 pip 安装
---------

```
pip3 install --user poetry
```

确认是否安装成功以及查看版本号
---------------

```
poetry --version
```

在 python 项目中使用 Poetry
---------------------

### 在现有项目中使用：

如果是在已有项目中使用 poetry，你只需要执行一下命令来创建一个 pyproject.toml 文件即可：

```
poetry init
```

### 使用 poetry 创建一个新项目：

```
poetry new project\_name (项目名字）
```

#### 项目结构如下图：

[![](https://testerhome.com/uploads/photo/2019/f80f2f6e-a54c-40e4-8981-742f8f9f0db1.png!large)](https://testerhome.com/uploads/photo/2019/f80f2f6e-a54c-40e4-8981-742f8f9f0db1.png!large)

### 结构介绍

*   \*_pyproject.toml \*_: 使用此文件管理依赖列表和项目的各种 meta 信息，用来替代 Pipfile、requirements.txt、setup.py、setup.cfg、MANIFEST.in 等等各种配置文件。

创建虚拟环境
------

Tips: 确保当前目录存在 pyproject.toml 文件

```
poetry install
```

这个命令会读取 pyproject.toml 中的所有依赖并安装（包括开发依赖），如果不想安装开发依赖可以附加：--no-dev 选项。如果项目根目录有 poetry.lock 文件，会安装这个文件中列出的锁定版本的依赖。如果执行 add/remove 命令的时候没有检测到虚拟环境，也会为当前目录自动创建虚拟

激活虚拟环境
------

```
poetry shell
```

查看 python 版本
------------

```
poetry run python -V
```

执行脚本
----

```
poetry run python app.py
```

安装包
---

```
poetry add flask
```

_添加 --dev 参数为开发依赖_：

```
poetry add pytest --dev
```

追踪 & 更新包
--------

```
poetry show
```

_添加 --tree 参数选项可以查看依赖关系_：

```
poetry show --tree
```

_查看可以更新的依赖_：

```
poetry show --outdated
```

### 更新所有锁定版本的依赖：

```
poetry update
```

### 更新某个指定的依赖：

```
poetry update dep\_name (依赖名字）
```

卸载包
---

```
poetry remove dep\_name
```

让 poetry 使用 python3
-------------------

```
poetry env use python3.7
```

*   [Poetry using the wrong Python version (not related to pyenv) #655](https://github.com/sdispater/poetry/issues/655)

常用配置
----

Q&A
---

1, 推荐使用 python3

2, poetry 版本很重要，最好使用最新版本
