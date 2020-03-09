---
title: django
toc: true
recommend: 1
uniqueId: '2020-03-07 01:23:57/"django".html'
date: 2020-03-07 09:23:57
thumbnail:
categories:
- 研究生课程
- 信息系统实训
tags:
keywords:
---

[TOC]

<!--more-->

## 1. hello world

```shell
pip install django

django-admin.py startproject helloDjango
cd helloDjango
python manage.py startapp article
# python manage.py runserver
python manage.py runserver 0.0.0.0:80 &
# 最后面的”&”，这符号表示在后台运行该进程。这里的IP地址如果用公网IP
# 会运行不了，而用0.0.0.0则外网和127.0.0.1都能够访问。
# python manage.py migrate
```

helloDjango/settings.py

```python
DEBUG = True

# Application definition
INSTALLED_APPS = [
    'django.contrib.admin',
    ...
    'article',  #这里填写的是app的名称
]
```

helloDjango/urls.py

```python
from article.views import *

urlpatterns = [
    path('admin/', admin.site.urls),
    path('hello/', hello),
    path('hello/<int:num>', hello_num),
]
```

article/views.py

```python
from django.http import HttpResponse, Http404
from django.shortcuts import render


# Create your views here.

def hello(request):
    return HttpResponse("hello world")


def hello_num(request, num):
    try:
        num = int(num)
        return HttpResponse(f"hello world {num}")
    except:
        return Http404()
```



## 2.1 模板

helloDjango/settings.py

```python
import os

TEMPLATE_DIRS = [os.path.join(os.path.dirname(__file__), 'templates')]
```

helloDjango/urls.py

```
path('template', template),
```

Article/views.py

```python
def template(request):
    context = {'name': 'tim',
               'itemList': ['item1', 'item2', 'item3']
               }
    return render(request, '1.html', context=context)
```

Article/templates/1.html

```html
<html>

<head><title> hello django </title>
</head>

<body>
<h1> hello django </h1>

<p>Hello {{ name }}</p>

<ul>
    {% for item in itemList %}
        <li>{{ item }}</li>
    {% endfor %}
</ul>

{% if status %}
    <p>I like Python!</p>
{% else %}
    <p>I like Django!</p>
{% endif %}

{% ifequal a b %}
{% endifequal %}

{# 过滤器 #}
{{ name | lower }}
</body>
</html>
```



## 2.2 模板继承

2.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>2</title>
</head>
<body>

{% block blockName %}
  无用的文字
{% endblock %}

<p> 2.html out of block </p>

</body>
</html>
```

3.html

```html
{% extends '2.html' %}
{% block blockName %}
    <p> 3.html in block </p>
{% endblock %}

并不会显示
```

省略 views.py 中的函数和 urls.py 中的 URL 映射说明代码



3.html 的显示结果

```
3.html in block
2.html out of block
```

因此，2.html 中 block 内的内容不会显示，还有 3.html 中 block 外的内容也不会显示

## 3.django admin 管理后台

python manage.py createsuperuser 输入 user password 即可创建超级用户



## 4.models

### Django Model

- 每一个`Django Model`都继承自`django.db.models.Model`
- 在`Model`当中每一个属性`attribute`都代表一个database field
- 通过`Django Model API`可以执行数据库的增删改查, 而不需要写一些数据库的查询语句

### 设置数据库

Django项目建成后, 默认设置了使用SQLite数据库, 在my_blog/my_blog/setting.py中可以查看和修改数据库设置:

```
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        }
    }
```

还可以设置其他数据库, 如`MySQL, PostgreSQL`, 现在为了简单, 使用默认数据库设置

### 创建models

在my_blog/article/models.py下编写如下程序:

```python
    from django.db import models

    # Create your models here.
    class Article(models.Model) :
        title = models.CharField(max_length = 100)  #博客题目
        category = models.CharField(max_length = 50, blank = True)  #博客标签
        date_time = models.DateTimeField(auto_now_add = True)  #博客日期
        content = models.TextField(blank = True, null = True)  #博客文章正文

        def __unicode__(self) :
            return self.title

        class Meta:  #按时间下降排序
            ordering = ['-date_time']
```

其中`__unicode__(self)` 函数Article对象要怎么表示自己, 一般系统默认使用`` 来表示对象, 通过这个函数可以告诉系统使用title字段来表示这个对象

- `CharField` 用于存储字符串, max_length设置最大长度
- `TextField` 用于存储大量文本
- `DateTimeField` 用于存储时间, auto_now_add设置True表示自动设置对象增加时间

### 同步数据库

```shell
python manage.py migrate #命令行运行该命令
```

因为我们已经执行过该命令会出现如下提示

```
    Operations to perform:
      Apply all migrations: admin, contenttypes, sessions, auth
    Running migrations:
      No migrations to apply.
      Your models have changes that are not yet reflected in a migration, and so won't be applied.
      Run 'manage.py makemigrations' to make new migrations, and then re-run 'manage.py migrate' to apply them.
```

那么现在需要执行下面的命令

```shell
    $ python manage.py makemigrations
    #得到如下提示
    Migrations for 'article':
      0001_initial.py:
        - Create model Article
```

现在重新运行以下命令

```shell
    $ python manage.py migrate
    #出现如下提示表示操作成功
    Operations to perform:
      Apply all migrations: auth, sessions, admin, article, contenttypes
    Running migrations:
      Applying article.0001_initial... OK
```

> migrate命令按照app顺序建立或者更新数据库, 将`models.py`与数据库同步

现在我们进入Django中的交互式shell来进行数据库的增删改查等操作

```shell
    $ python manage.py shell
    Python 3.4.2 (v3.4.2:ab2c023a9432, Oct  5 2014, 20:42:22)
    [GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    (InteractiveConsole)
    >>>
```

> 这里进入Django的shell和python内置的shell是非常类似的

```python
    >>> from article.models import Article
    >>> #create数据库增加操作
    >>> Article.objects.create(title = 'Hello World', category = 'Python', content = '我们来做一个简单的数据库增加操作')

    >>> Article.objects.create(title = 'Django Blog学习', category = 'Python', content = 'Django简单博客教程')

    >>> #all和get的数据库查看操作
    >>> Article.objects.all()  #查看全部对象, 返回一个列表, 无对象返回空list
    [, ]
    >>> Article.objects.get(id = 1)  #返回符合条件的对象

    >>> #update数据库修改操作
    >>> first = Article.objects.get(id = 1)  #获取id = 1的对象
    >>> first.title
    'Hello World'
    >>> first.date_time
    datetime.datetime(2014, 12, 26, 13, 56, 48, 727425, tzinfo=)
    >>> first.content
    '我们来做一个简单的数据库增加操作'
    >>> first.category
    'Python'
    >>> first.content = 'Hello World, How are you'
    >>> first.content  #再次查看是否修改成功, 修改操作就是点语法
    'Hello World, How are you'

    >>> #delete数据库删除操作
    >>> first.delete()
    >>> Article.objects.all()  #此时可以看到只有一个对象了, 另一个对象已经被成功删除
    []

    Blog.objects.all()  # 选择全部对象
    Blog.objects.filter(caption='blogname')  # 使用 filter() 按博客题目过滤
    Blog.objects.filter(caption='blogname', id="1") # 也可以多个条件
    #上面是精确匹配 也可以包含性查询
    Blog.objects.filter(caption__contains='blogname')

    Blog.objects.get(caption='blogname') # 获取单个对象 如果查询没有返回结果也会抛出异常

    #数据排序
    Blog.objects.order_by("caption")
    Blog.objects.order_by("-caption")  # 倒序

    #如果需要以多个字段为标准进行排序（第二个字段会在第一个字段的值相同的情况下被使用到），使用多个参数就可以了
    Blog.objects.order_by("caption", "id")

    #连锁查询
    Blog.objects.filter(caption__contains='blogname').order_by("-id")

    #限制返回的数据
    Blog.objects.filter(caption__contains='blogname')[0]
    Blog.objects.filter(caption__contains='blogname')[0:3]  # 可以进行类似于列表的操作
```

## 5.测试

article/tests.py

```python
from django.test import TestCase


# Create your tests here.
# 类名无所谓
class ATestClass(TestCase):
    # 测试方法以 test 开头
    def test_aTestMethod(self):
        self.assertEquals([1, 2, 3], [1, 2, 3])
        
```

```shell
python manage.py test article
```

## 6.Django 部署

整个部署的链路是 Nginx -> uWSGI -> Python Web程序，通常还会提到supervisord工具。

supervisor 是一个进程管理工具。任何人都不能保证程序不异常退出，不别被人误杀，所以一个典型的工程做法就是使用supervisor看守着你的进程，一旦异常退出它会立马进程重新启动起来。

# 阿里云 centos 部署 Django

```shell
yum install nginx
# 安装 python3.8 见 _posts/阿里云/centos.md
pip3 install django

# 升级 sqlite3
# wget http://www.sqlite.org/2019/sqlite-autoconf-3280000.tar.gz
rsync -azvhP sqlite-autoconf-3280000.tar.gz root@47.93.53.47:/root/
tar -zxvf sqlite-autoconf-3280000.tar.gz;cd sqlite-autoconf-3280000;
./configure;make && sudo make install
# 替换默认 sqlite
sudo mv -v /usr/bin/sqlite3 /usr/bin/sqlite3.7.17
sudo cp -v sqlite3 /usr/bin
echo 'export LD_LIBRARY_PATH="/usr/local/lib"'>>~/.zshrc
sqlite3 --version
source ~/.zshrc

git clone git@github.com:zronghui/helloDjango.git
python3 manage.py migrate
python3 manage.py runserver 0.0.0.0:8000
```









## 分页

[RSS和分页 - Django 搭建简易博客教程 - 极客学院Wiki](https://wiki.jikexueyuan.com/project/django-set-up-blog/rss-paging.html)



```shell
pip install elasticsearch-dsl==5.4.0
```



## 注意

1. xadmin 是对 Django admin 的美化，但是只能用于 Django 1.* ，[django-admin-bootstrap](https://github.com/douglasmiranda/django-admin-bootstrap)Support Django **1.11** and **2.1**
2. 收集静态文件 python manage.py collectstatic 这一句话就会把以前放在app下static中的静态文件全部拷贝到 settings.py 中设置的 STATIC_ROOT 文件夹中

## 参考链接

[Django搭建简易博客教程_Django开发中文手册[PDF]下载-极客学院Wiki](https://wiki.jikexueyuan.com/project/django-set-up-blog/)

[Django路由控制URL详解 - 知乎](https://zhuanlan.zhihu.com/p/74726087)

[Django基础(19): Django Admin管理后台详解(上) - 知乎](https://zhuanlan.zhihu.com/p/47962034)

[Django Admin 管理工具 | 菜鸟教程](https://www.runoob.com/django/django-admin-manage-tool.html)

[Introduction · Django 中文文档 1.8](https://wizardforcel.gitbooks.io/django-chinese-docs-18/content/)

[如何在阿里云上部署 Django 应用程序 - 阿里云新手学堂](https://www.alibabacloud.com/zh/getting-started/projects/how-to-deploy-django-application-on-alibaba-cloud)
[阿里云centos部署Django+nginx+uwsgi服务，安装mezzanine为例-云栖社区-阿里云](https://yq.aliyun.com/articles/590911)

[Django2.2以上针对Sqlite3版本不匹配 | 沐雨露の山头 | 懒若新生](https://mudew.com/20190417/django-22-does-not-match-the-sqlite3-version-above/)

[Django运行访问项目出现的问题:DisallowedHost at / Invalid HTTP_HOST header_Python_will5451的博客-CSDN博客](https://blog.csdn.net/will5451/article/details/53861092)

[Django 部署基础 - Django 教程 - 自强学堂](https://code.ziqiangxuetang.com/django/django-deploy-base.html)
[Django 部署(Nginx) - Django 教程 - 自强学堂](https://code.ziqiangxuetang.com/django/django-nginx-deploy.html)

[管理静态文件（比如图片、JavaScript、CSS） | Django 文档 | Django](https://docs.djangoproject.com/zh-hans/3.0/howto/static-files/)

[Python Elasticsearch DSL 使用笔记(一) | Finger's Blog](http://fingerchou.com/2017/08/12/elasticsearch-dsl-with-python-usage-1/)

