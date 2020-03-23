---
title: Python tricks
date: 2020-01-15 16:36:18
categories:
- python
toc: true
tags:
- tricks
- Python
---

[toc]

<!--more-->

# 03-23

## vardbg

一个 Python 脚本的调试工具，并可以生成脚本运行流程的可视化视频。

```shell
pip3 install vardbg
# run file
vardbg run insertion_sort.py -o qsort.json
# or run a method
# vardbg run sort.py quick_sort -o qsort.json -a 9 -a 3 -a 5 -a 1
vardbg replay qsort.json -v sort_vis.mp4
```



# 03-12

### Environs 读取环境变量

[Python 使用 environs 库来更好地定义环境变量 | 静觅](https://cuiqingcai.com/8947.html)

[environs: 简化环境变量解析](https://python.freelycode.com/contribution/detail/168)
[sloria/environs: simplified environment variable parsing](https://github.com/sloria/environs)

可以从当前 shell 的环境变量中获取，或者 .env 文件

```
# .env 没有前缀名
# 不用写 export
APP_DEBUG=false
APP_ENV=prod
```



```shell
export GITHUB_USER=sloria
export MAX_CONNECTIONS=100
export SHIP_DATE='1984-06-25'
export TTL=42
export ENABLE_LOGIN=true
export GITHUB_REPOS=webargs,konch,ped
export COORDINATES=23.3,50.0
export LOG_LEVEL=DEBUG
```



```python
from environs import Env

env = Env()
env.read_env()  # read .env file, if it exists
# required variables
gh_user = env("GITHUB_USER")  # => 'sloria'
secret = env("SECRET")  # => raises error if not set

# casting
max_connections = env.int("MAX_CONNECTIONS")  # => 100
ship_date = env.date("SHIP_DATE")  # => datetime.date(1984, 6, 25)
ttl = env.timedelta("TTL")  # => datetime.timedelta(0, 42)
log_level = env.log_level("LOG_LEVEL")  # => logging.DEBUG

# providing a default value
enable_login = env.bool("ENABLE_LOGIN", False)  # => True
enable_feature_x = env.bool("ENABLE_FEATURE_X", False)  # => False

# parsing lists
gh_repos = env.list("GITHUB_REPOS")  # => ['webargs', 'konch', 'ped']
coords = env.list("COORDINATES", subcast=float)  # => [23.3, 50.0]
```



# 03-11

[Python自动化](https://martinheinz.dev/blog/17)

[Python in GitHub Actions · Homepage of Hynek Schlawack](https://hynek.me/articles/python-github-actions/)
[github action python test - Google 搜索](https://www.google.com/search?q=github+action+python+test&oq=github+action+python+test&aqs=chrome..69i57j69i64.9173j0j7&sourceid=chrome&ie=UTF-8)



### Django 调用流程图

[meshy/django-schema-graph: An interactive graph of your Django model structure](https://github.com/meshy/django-schema-graph)

### 死链检测

检测 URL、目录、文档的死链

[butuzov/deadlinks: Health checks for your documentation links. https://pypi.org/project/deadlinks](https://github.com/butuzov/deadlinks)

### creating command line user interfaces创建命令行用户界面

能用 py_cui 写 2048 游戏[jwlodek/py_cui_2048: A command line ascii version of the 2048 game written with the help of the py_cui library.](https://github.com/jwlodek/py_cui_2048)

[jwlodek/py_cui: A python library for intuitively creating CUI/TUI interfaces with widgets, inspired by gocui.](https://github.com/jwlodek/py_cui)

### 自定义快捷键的功能

[dibsonthis/keyboard_listener: Keyboard Listener is a module that allows you to create custom hotkeys (combinations) or custom keywords and bind them to custom functions in Python](https://github.com/dibsonthis/keyboard_listener)

### pytest-responsemock

Mock response

[idlesign/pytest-responsemock: Simplified requests calls mocking for pytest](https://github.com/idlesign/pytest-responsemock)

### 简繁转换

[简易中文简繁转换 — zhconv 1.2.1 文档](https://pythonhosted.org/zhconv/#id2)

```shell
pip install zhconv
```

```python
from zhconv import convert
print(convert('我幹什麼不干你事。', 'zh-cn'))
```

zh-cn 大陆简体
zh-tw 台灣正體
zh-hk 香港繁體
zh-sg 马新简体
zh-hans 简体
zh-hant 繁體



# 03-10

```python
os.path.dirname(__file__)
```



# 03-06

[pip离线安装依赖包 - 曾春云 - 博客园](https://www.cnblogs.com/zengchunyun/p/9344664.html)

#### pip安装离线本地包

- 导出本地已有的依赖包

```shell
pip freeze > requirements.txt
```

- 将依赖包下载到本地

```shell
# 下载到当前目录,指定pip源
pip download -r requirements.txt -d . -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
```

- 创建虚拟环境

```shell
# -q 安静的方式创建
# --no-site-packages 不拷贝本地的第三方包,创建干净的虚拟python运行环境
# --python=python3.7 指定创建python版本环境
# .venv 虚拟环境目录
virtualenv -q --no-site-packages --python=python3.7 .venv
```

- 进入虚拟环境

```shell
source .venv/bin/activate
```

- 安装本地依赖包

```shell
pip install --no-index --find-links=. -r requirements.txt
```

#### pip 其它使用方式

- 安装最新版本

```shell
pip install 'SomeProject'
```

- 安装指定版本

```shell
pip install 'SomeProject==1.4'
```

- **ss**

```shell
pip install 'SomeProject>=1,<2'
```

- 安装兼容某个版本的包

```shell
pip install 'SomeProject~=1.4.2'
```

- 升级安装

```shell
pip install --upgrade SomeProject
```

- 指定依赖文件安装

```shell
pip install -r requirements.txt
```

- 安装从版本控制服务器

```shell
pip install -e git+https://git.repo/some_pkg.git#egg=SomeProject          # from git
pip install -e hg+https://hg.repo/some_pkg#egg=SomeProject                # from mercurial
pip install -e svn+svn://svn.repo/some_pkg/trunk/#egg=SomeProject         # from svn
pip install -e git+https://git.repo/some_pkg.git@feature#egg=SomeProject  # from a branch
```

- 安装从其它索引服务器

```shell
pip install --index-url http://my.package.repo/simple/ --trusted-host my.package.repo SomeProject
```

- 安装时，如果默认索引服务器没有该依赖包则提供搜索额外的索引服务器进行搜索获取

```shell
pip install --extra-index-url http://my.package.repo/simple SomeProject
```

- 安装从本地

```shell
pip install -e <path>
```

或者

```shell
pip install <path>
```

- 安装从压缩包

```shell
pip install ./downloads/SomeProject-1.0.4.tar.gz
```

- 安装从本地目录搜索依赖包

```shell
pip install --no-index --find-links=file:///local/dir/ SomeProject
pip install --no-index --find-links=/local/dir/ SomeProject
pip install --no-index --find-links=relative/dir/ SomeProject
```

- 安装从其它源

```shell
pip install --extra-index-url http://localhost:7777 SomeProject
```

- 安装预发布版本

```shell
pip install --pre SomeProject
```

- 安装前配置

```shell
$ pip install SomePackage[PDF]
$ pip install SomePackage[PDF]==3.0
$ pip install -e .[PDF]==3.0  # editable project in current directory
```

# 02-28

[Personalize your python prompt | Arpit Bhayani](https://arpitbhayani.me/blogs/python-prompts)

# 02-14

[Debugging in Python — A cakewalk with pdb - Python Features - Medium](https://medium.com/python-features/debugging-in-python-a-cakewalk-with-pdb-cd748ca62ee7)

# 02-13

### Pathlib (3.4+)

```python
from pathlib import Path
root = Path('post_sub_folder')
print(root)
path = root / 'happy_user'
print(path.resolve())
```

### Type hinting (3.5+)

```python
def sentence_has_animal(sentence: str) -> bool:
  return "animal" in sentence
sentence_has_animal("Donald had a farm without animals")
```

### Enumerations (3.4+)

Python3提供的Enum类让你很容就能实现一个枚举类型：

```python
from enum import Enum, auto

class Monster(Enum):
  ZOMBIE = auto()
  WARRIOR = auto()
  BEAR = auto()
print(Monster.ZOMBIE)
for monster in Monster:
  print(monster)
```

### Built-in LRU cache (3.2+)

缓存是现在的软件领域经常使用的技术，Python3提供了一个lru_cache装饰器，来让你更好的使用缓存。

```python
from functools import lru_cache

@lru_cache(maxsize=512)
def fib_memoization(number: int) -> int:
  if number == 0: return 0
  if number == 1: return 1
  return fib_memoization(number-1) + fib_memoization(number-2)
start = time.time()
fib_memoization(40)
print(f'Duration: {time.time() - start}s')
```

### Extended iterable unpacking (3.0+)

***_用来抛弃元素**

```python
head, *body, tail = range(5)
print(head, body, tail)
# 输出： 0 [1, 2, 3] 4

py, filename, *cmds = "python3.7 script.py -n 5 -l 15".split()
print(py)
print(filename)
print(cmds)
# 输出：python3.7
# script.py
# ['-n', '5', '-l', '15']

first, _, third, *_ = range(10)
print(first, third)
# 输出： 0 2
```

### Data classes (3.7+)

Python3提供data class装饰器来让我们更好的处理数据对象，而不用去实现 init() 和 repr() 方法。假设如下的代码:



```python
class Armor:
  def __init__(self, armor: float, description: str, level: int = 1):
      self.armor = armor
      self.level = level
      self.description = description

  def power(self) -> float:
      return self.armor * self.level

armor = Armor(5.2, "Common armor.", 2)
armor.power()
# 10.4
print(armor)
# <__main__.Armor object at 0x7fc4800e2cf8>
```


使用data class实现上面功能的代码，这么写:

```python
from dataclasses import dataclass

@dataclass
class Armor:
  armor: float
  description: str
  level: int = 1

def power(self) -> float:
    return self.armor * self.level
    
armor = Armor(5.2, "Common armor.", 2)
armor.power()
# 10.4
print(armor)
# Armor(armor=5.2, description='Common armor.', level=2)
```

### Implicit namespace packages (3.3+)

通常情况下，Python通过把代码打成包（在目录中加入init.py实现）来复用

在Python2里，每个目录都必须有init.py文件，以便其他模块调用目录下的python代码，在Python3里，通过 Implicit Namespace Packages可是不使用__init__.py文件



    sound/                          Top-level package
      __init__.py               Initialize the sound package
      formats/                  Subpackage for file format conversions
              wavread.py
              wavwrite.py
              aiffread.py
              aiffwrite.py
              auread.py
              auwrite.py
              ...
      effects/                  Subpackage for sound effects
              echo.py
              surround.py
              reverse.py
              ...
      filters/                  Subpackage for filters
              equalizer.py
              vocoder.py
              karaoke.py


# 02-10

neovim: 新时代的 vim，我在这个配置([https://github.com/PegasusWang/vim-config](https://link.zhihu.com/?target=https%3A//github.com/PegasusWang/vim-config))上自定义了自己的配置，使用起来性能和反应速度上远超原生的老古董 vim

meld/vimdiff: 文本比对工具。

tmux/tmuxp

- wemux: tmux 共享，[https://github.com/zolrath/wemux](https://link.zhihu.com/?target=https%3A//github.com/zolrath/wemux)
- sshfs: 本地挂在服务器文件夹
- tmate: [https://tmate.io](https://link.zhihu.com/?target=https%3A//tmate.io/) 终端共享工具，结对编程。很多现代化编辑器 vscode, atom 提供结对编程的插件。
- asciinema: 终端会话记录工具。[https://asciinema.org/](https://link.zhihu.com/?target=https%3A//asciinema.org/)



devdocs.io: 文档查询工具

gitx(mac):方便查看代码提交历史，便于了解整个代码仓库是怎样一步步构建的。[http://gitx.frim.nl/user_manual](https://link.zhihu.com/?target=http%3A//gitx.frim.nl/user_manual.html)

tig: text-mode interface for git. 喜欢命令行的可以尝试下。 [https://github.com/jonas/tig](https://link.zhihu.com/?target=https%3A//github.com/jonas/tig)

- EditorConfig: [http://editorconfig.org/](https://link.zhihu.com/?target=http%3A//editorconfig.org/) 用来统一编辑器配置。如果成员用不同的操作系统和编辑器，建议使用。尤其是对于 python 这种使用缩进的语言
- mac-setup: [https://github.com/sb2nov/mac-setup](https://link.zhihu.com/?target=https%3A//github.com/sb2nov/mac-setup) mac 下各种编程语言开发环境配置指引

[《使用vim+tmux+zsh+autojump高效工作》](https://link.zhihu.com/?target=http%3A//ningning.today/2016/11/09/tools/vim-tmux-zsh-autojump/)

- prospector: 集成了众多python代码检测工具
- mccabe: 圈复杂度检测工具。McCabe 是一种度量程序复杂度的方法，如果单个子程序复杂度过高，或许就需要拆分逻辑提高程序的易读性。
- bandit: 用于Python代码的安全性分析，openstack 的项目 [https://github.com/openstack/bandit](https://link.zhihu.com/?target=https%3A//github.com/openstack/bandit)
- rope，可以用来重构等，功能强大。笔者经常用rope自动帮我重新整理导入的包顺序。
- Pyreverse: 代码 UML 生成工具, 帮助我们理解继承关系 ([https://pythonhosted.org/theape/documentation/developer/explorations/explore_graphs/explore_pyreverse.html](https://link.zhihu.com/?target=https%3A//pythonhosted.org/theape/documentation/developer/explorations/explore_graphs/explore_pyreverse.html))
- Epydoc: Automatic API Documentation Generation for Python
- 2to3/python-modernize: python2 转 python3 工具。目前 Instagram 已经全面迁移到 python3
- 编写2/3兼容代码：[http://python-future.org/compa](https://link.zhihu.com/?target=http%3A//python-future.org/compatible_idioms.html)
- pigar: 找出项目使用到的依赖库
- buildout: 项目构建工具
- pyenv/virtualenv/pipenv：多版本管理
- bitbucket: 类似 github，好处是支持免费的私有仓库
- cookiecutter: 一系列项目模板生成工具，懒人必备。[https://github.com/audreyr/cookiecutter](https://link.zhihu.com/?target=https%3A//github.com/audreyr/cookiecutter)。笔者之前内部项目就直接用 flask-cookiecutter 直接生成的。
- yeoman: [http://yeoman.io/generators/](https://link.zhihu.com/?target=http%3A//yeoman.io/generators/) 前端项目模板生成工具
- ant-design: 后端管理后台项目解决方案 [https://ant.design/docs/react/p](https://link.zhihu.com/?target=https%3A//ant.design/docs/react/practical-projects-cn)
- Api 工具
  - checklist: [http://python.apichecklist.com/](https://link.zhihu.com/?target=http%3A//python.apichecklist.com/)

日志、异常收集工具

- Sentry: 用来记录异常非常好用，能看到完善的栈信息，方便排错。
- Fluentd



管理及运维、监控工具(devops很火)

- Supervisor.进程管理
- Fabric.应用部署
- docker.最近比较火的容器技术。很多采用微服务架构的公司使用 docker 作为容器部署服务，或者构建一致的开发环境
- SaltStack和Ansible. 配置管理
- StatsDGraphite等web监控



调试工具

- ipdb/pdb: ipdb 支持自动补全，比原生的 pdb 要好用一些。
- pdbpp: [https://pypi.org/project/pdbpp/](https://link.zhihu.com/?target=https%3A//pypi.org/project/pdbpp/)
- [https://curl.trillworks.com/](https://link.zhihu.com/?target=https%3A//curl.trillworks.com/) 把 curl 命令参数转成 requests 代码。 [https://github.com/NickCarneiro/curlconverter/](https://link.zhihu.com/?target=https%3A//github.com/NickCarneiro/curlconverter/)。
- httpie
- postman
- [http://httpbin.org](https://link.zhihu.com/?target=http%3A//httpbin.org)
- curl/requests 互相转化: [https://github.com/oeegor/curlify](https://link.zhihu.com/?target=https%3A//github.com/oeegor/curlify) [https://github.com/spulec/uncurl](https://link.zhihu.com/?target=https%3A//github.com/spulec/uncurl)



抓包和下载工具

- mitmproxy: 用 python 实现的终端命令行抓包工具，可以将请求直接导出成 python 代码，笔者经常用来抓包和调试。
- charles



爬虫相关

- Scrapy: 知名的爬虫框架。生态比较丰富
- pyspider: 国人写的一个不错的爬虫框架
- requests
- lxml/BeautifulSoup/pyquery: 解析 html，xml 等。
- tornado: 异步的 http client 可以写爬虫
- redis/celery: 实现队列、异步爬虫。异步方案也比较多
- phantomjs/puppeteer: 用来处理动态网站。puppeteer 基于 nodejs



异步任务框架

- celery: python 社区一个流行的异步任务框架

压测工具

- locust: python实现的压测工具。[http://locust.io/](https://link.zhihu.com/?target=http%3A//locust.io/)， 有 web 界面
- ab
- wrk

Profiler

- pyflame: [https://github.com/uber/pyflame](https://link.zhihu.com/?target=https%3A//github.com/uber/pyflame)

数据库工具

- mycli: mysql 命令行补全等。[https://github.com/dbcli/mycli](https://link.zhihu.com/?target=https%3A//github.com/dbcli/mycli)
- MysqlWorkbench/Sequel Pro: mysql 客户端工具。
- Navicat Premium: 强大的数据库管理工具，收费
- **Medis**: redis client 工具
- **MongoChef**: Mongodb 客户端工具





<img src="https://i.loli.net/2020/02/10/UNFt5Hpwy6L1rWu.jpg" alt="UNFt5Hpwy6L1rWu" style="zoom:33%;" />
<img src="https://i.loli.net/2020/02/10/xECDNp5wjnl8vWQ.jpg" alt="xECDNp5wjnl8vWQ" style="zoom: 33%;" />

# 2020 年 1 月 15 日(星期三)

[Python技巧小贴士 - 51CTO.COM](https://zhuanlan.51cto.com/art/201910/604777.htm)

## 字符串一次性执行多个 replace

```Python
user_input = "This\nstring has\tsome whitespaces...\r\n" 
 
character_map = { 
    ord('\n') : ' ', 
    ord('\t') : ' ', 
    ord('\r') : None 
} 
user_input.translate(character_map)  # This string has some whitespaces...  
```

## 迭代器切片(Slice)

如果对迭代器进行切片操作，会返回一个「TypeError」，提示生成器对象没有下标，但是我们可以用一个简单的方案来解决这个问题：

```Python
import itertools 
 
s = itertools.islice(range(50), 10, 20)
for i in s:
    ...
```

## 跳过可迭代对象某些元素

比如跳过文件的注释代码

```python
import itertools 
 
for line in itertools.dropwhile(lambda line: line.startswith("//"), string_from_file.split("\n")): 
    print(line) 
```

## 实现上下文管理

```Python
from contextlib import contextmanager 
 
@contextmanager 
def tag(name): 
    print(f"<{name}>") 
    yield 
    print(f"</{name}>") 
 
with tag("h1"): 
    print("This is Title.") 
```

## 控制可以/不可以导入什么

在 Python 中，所有成员都会被导出(除非我们使用了「__all__」)：

```Python
def foo(): 
    pass 
 
def bar(): 
    pass 
 
__all__ = ["bar"] 
```

在上面这段代码中，我们知道只有「bar」函数被导出了。同样，我们可以让「__all__」为空，这样就不会导出任何东西，当从这个模块导入的时候，会造成「AttributeError」。

## 实现所有比较运算符的简单方法

为一个类实现所有的比较运算符(如 __lt__ , __le__ , __gt__ , __ge__)是很繁琐的。有更简单的方法可以做到这一点吗？这种时候，「functools.total_ordering」就是一个很好的帮手：

```Python
from functools import total_ordering 
 
@total_ordering 
class Number: 
    def __init__(self, value): 
        self.value = value 
 
    def __lt__(self, other): 
        return self.value < other.value 
 
    def __eq__(self, other): 
        return self.value == other.value 
 
print(Number(20) > Number(3)) 
print(Number(1) < Number(5)) 
print(Number(15) >= Number(15)) 
print(Number(10) <= Number(2)) 
```

用「total_ordering」装饰器简化实现对类实例排序的过程。我们只需要定义「__lt__」和「__eq__」就可以了

## 限制「CPU」和内存使用量

？
如果不是想优化程序对内存或 CPU 的使用率（how），而是想直接将其限制为某个确定的数字，Python 也有一个对应的库可以做到：

```python
import signal 
import resource 
import os 
 
# To Limit CPU time 
def time_exceeded(signo, frame): 
    print("CPU exceeded...") 
    raise SystemExit(1) 
 
def set_max_runtime(seconds): 
    # Install the signal handler and set a resource limit 
    soft, hard = resource.getrlimit(resource.RLIMIT_CPU) 
    resource.setrlimit(resource.RLIMIT_CPU, (seconds, hard)) 
    signal.signal(signal.SIGXCPU, time_exceeded) 
 
# To limit memory usage 
def set_max_memory(size): 
    soft, hard = resource.getrlimit(resource.RLIMIT_AS) 
    resource.setrlimit(resource.RLIMIT_AS, (size, hard)) 
```



