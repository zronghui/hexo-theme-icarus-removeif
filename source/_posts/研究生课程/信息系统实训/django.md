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

## 2.3 模板使用中的注意事项

[Django 模板进阶 - Django 教程 - 自强学堂](https://code.ziqiangxuetang.com/django/django-template2.html)

==, !=, >=, <=, >, < 这些比较都可以在模板中使用，比较符号前后必须有至少一个空格！

and, or, not, in, not in 也可以在模板中使用



```html
{% if isLastPage %}
	<button class="turn_page disable" type="button">
{% else %}
	<button class="turn_page" type="button">
{% endif %}

# 一般来说不用加引号，它会自动转成字符串，但是在字符串中有空格的情况下，需要加上引号
<a href="{{hit.book_url}}"
# 怎么不自动加引号？
{{hit.book_name|safe}}
   
我们还可以通过{%autoescape off%}的方式关闭整段代码的自动转义，比如下面这样：
{% autoescape off %}
Hello {{ name }}
{% endautoescape %}
```

**获取当前用户：**

```
{{ request.user }}
```

**获取当前网址：**

```
{{ request.path }}
```

**获取当前 GET 参数：**

```
{{ request.GET.urlencode }}
```





[Django——模板层 - King'home - 博客园](https://www.cnblogs.com/king-home/p/11004487.html)

### 二、常用的模板层语法

只需要记两种特殊符号：

变量相关的用{{ }}，逻辑相关的用{%%}。

变量：当模版引擎遇到一个变量，它将计算这个变量，然后用结果替换掉它本身。 变量的命名包括任何字母数字以及下划线 ("_")的组合。 变量名称中不能有空格或标点符号。

点（.）在模板语言中有特殊的含义。当模版系统遇到点(".")，它将以这样的顺序查询：

字典查询（Dictionary lookup） 属性或方法查询（Attribute or method lookup） 数字索引查询（Numeric index lookup）

### 三、Filters（过滤器）

在*Django*的模板语言中通过使用过滤器来改变变量的显示

过滤器的语法： {{ value|filter_name:参数 }}

使用管道符"|"来应用过滤器。

例如：{{ name|lower }}会将name变量应用lower过滤器之后再显示它的值。lower在这里的作用是将文本全都变成小写

**注意事项：**

1. 过滤器支持“链式”操作。即一个过滤器的输出作为另一个过滤器的输入。
2. 过滤器可以接受参数，例如：{{ sss|truncatewords:30 }}，这将显示sss的前30个词。
3. 过滤器参数包含空格的话，必须用引号包裹起来。比如使用逗号和空格去连接一个列表中的元素，如：{{ list|join:', ' }}
4. **'|'左右没有空格没有空格没有空格**

**default:如果一个变量是false或者为空，使用给定的默认值。 否则，使用变量的值。**

```python
{{ value|default:"nothing"}}
# 如果没有传值或者只为空就显示nothing
```

**length：返回值的长度，作用于字符串和列表**

```python
{{ value|length }}
# 返回value的长度，如 value=['a', 'b', 'c', 'd']的话，就显示4.
```

**filesizeformat:返回value的长度，如 value=['a', 'b', 'c', 'd']的话，就显示4.**

```python
{{ value|filesizeformat }}
```

**slice 切片**

```python
{{value|slice:"2:-1"}}
```

**date: 时间格式化**

```python
{{ value|date:"Y-m-d H:i:s"}}
```

**safe**

*Django*的模板中会对HTML标签和JS等语法标签进行自动转义，原因显而易见，这样是为了安全。但是有的时候我们可能不希望这些HTML元素被转义，比如我们做一个内容管理系统，后台添加的文章中是经过修饰的，这些修饰可能是通过一个类似于FCKeditor编辑加注了HTML修饰符的文本，如果自动转义的话显示的就是保护HTML标签的源文件。为了在*Django*中关闭HTML的自动转义有两种方式，如果是一个单独的变量我们可以通过过滤器“|safe”的方式告诉*Django*这段代码是安全的不必转义。

```pyhton
value = "<a href='#'>点我</a>"
{{ value|safe}}
```

**truncatechars**

如果字符串字符多于指定的字符数量，那么会被截断。截断的字符串将以可翻译的省略号序列（“...”）结尾。

参数：截断的字符数

```pyth
{{ value|truncatechars:9}}
```

**truncatewords：**

在一定数量的字后截断字符串。

```python
{{ value|truncatewords:9}}
```

**cut: 移除**

移除value中所有与给出的变量相同的字符串

```python
{{ value|cut:' ' }}
```

**join：使用字符串连接列表，例如Python的str.join(list)**

#### 其他过滤器

| 过滤器             | 描述                                                     | 示例                                                         |
| ------------------ | -------------------------------------------------------- | ------------------------------------------------------------ |
| upper              | 以大写方式输出                                           | {{ user.name \| upper }}                                     |
| add                | 给value加上一个数值                                      | {{ user.age \| add:”5” }}                                    |
| addslashes         | 单引号加上转义号                                         |                                                              |
| capfirst           | 第一个字母大写                                           | {{ ‘good’\| capfirst }} 返回”Good”                           |
| center             | 输出指定长度的字符串，把变量居中                         | {{ “abcd”\| center:”50” }}                                   |
| cut                | 删除指定字符串                                           | {{ “You are not a Englishman” \| cut:”not” }}                |
| date               | 格式化日期                                               |                                                              |
| default            | 如果值不存在，则使用默认值代替                           | {{ value \| default:”(N/A)” }}                               |
| default_if_none    | 如果值为None, 则使用默认值代替                           |                                                              |
| dictsort           | 按某字段排序，变量必须是一个dictionary                   | {% for moment in moments \| dictsort:”id” %}                 |
| dictsortreversed   | 按某字段倒序排序，变量必须是dictionary                   |                                                              |
| divisibleby        | 判断是否可以被数字整除                                   | `{{ 224 | divisibleby:2 }} 返回 True`                        |
| escape             | 按HTML转义，比如将”<”转换为”&lt”                         |                                                              |
| filesizeformat     | 增加数字的可读性，转换结果为13KB,89MB,3Bytes等           | `{{ 1024 | filesizeformat }} 返回 1.0KB`                     |
| first              | 返回列表的第1个元素，变量必须是一个列表                  |                                                              |
| floatformat        | 转换为指定精度的小数，默认保留1位小数                    | {{ 3.1415926 \| floatformat:3 }} 返回 3.142 四舍五入         |
| get_digit          | 从个位数开始截取指定位置的数字                           | {{ 123456 \| get_digit:’1’}}                                 |
| join               | 用指定分隔符连接列表                                     | {{ [‘abc’,’45’] \| join:’*’ }} 返回 abc*45                   |
| length             | 返回列表中元素的个数或字符串长度                         |                                                              |
| length_is          | 检查列表，字符串长度是否符合指定的值                     | {{ ‘hello’\| length_is:’3’ }}                                |
| linebreaks         | 用或 标签包裹变量                                        | {{ “Hi\n\nDavid”\|linebreaks }} 返回HiDavid                  |
| linebreaksbr       | 用 标签代替换行符                                        |                                                              |
| linenumbers        | 为变量中的每一行加上行号                                 |                                                              |
| ljust              | 输出指定长度的字符串，变量左对齐                         | {{‘ab’\|ljust:5}}返回 ‘ab ’                                  |
| lower              | 字符串变小写                                             |                                                              |
| make_list          | 将字符串转换为列表                                       |                                                              |
| pluralize          | 根据数字确定是否输出英文复数符号                         |                                                              |
| random             | 返回列表的随机一项                                       |                                                              |
| removetags         | 删除字符串中指定的HTML标记                               | {{value \| removetags: “h1 h2”}}                             |
| rjust              | 输出指定长度的字符串，变量右对齐                         |                                                              |
| slice              | 切片操作， 返回列表                                      | {{[3,9,1] \| slice:’:2’}} 返回 [3,9] `{{ 'asdikfjhihgie' | slice:':5' }} 返回 ‘asdik’` |
| slugify            | 在字符串中留下减号和下划线，其它符号删除，空格用减号替换 | `{{ '5-2=3and5 2=3' | slugify }} 返回 5-23and5-23`           |
| stringformat       | 字符串格式化，语法同python                               |                                                              |
| time               | 返回日期的时间部分                                       |                                                              |
| timesince          | 以“到现在为止过了多长时间”显示时间变量                   | 结果可能为 45days, 3 hours                                   |
| timeuntil          | 以“从现在开始到时间变量”还有多长时间显示时间变量         |                                                              |
| title              | 每个单词首字母大写                                       |                                                              |
| truncatewords      | 将字符串转换为省略表达方式                               | `{{ 'This is a pen' | truncatewords:2 }}返回``This is ...`   |
| truncatewords_html | 同上，但保留其中的HTML标签                               | `{{ 'This is a pen' | truncatewords:2 }}返回``This is ...`   |
| urlencode          | 将字符串中的特殊字符转换为url兼容表达方式                | {{ ‘http://www.aaa.com/foo?a=b&b=c’ \| urlencode}}           |
| urlize             | 将变量字符串中的url由纯文本变为链接                      |                                                              |
| wordcount          | 返回变量字符串中的单词数                                 |                                                              |
| yesno              | 将布尔变量转换为字符串yes, no 或maybe                    | `{{ True | yesno }}{{ False | yesno }}{{ None | yesno }} ``返回 ``yes``no ``maybe` |

### 四、标签（tags）

**for循环：普通循环**

```html
<ul>
{% for user in user_list %}
    <li>{{ user.name }}</li>
{% endfor %}
</ul>
```

| Variable              | Description                          |
| :-------------------- | :----------------------------------- |
| `forloop.counter`     | 当前循环的索引值（从1开始）          |
| `forloop.counter0`    | 当前循环的索引值（从0开始）          |
| `forloop.revcounter`  | 当前循环的倒序索引值（从1开始）      |
| `forloop.revcounter0` | 当前循环的倒序索引值（从0开始）      |
| `forloop.first`       | 当前循环是不是第一次循环（布尔值）   |
| `forloop.last`        | 当前循环是不是最后一次循环（布尔值） |
| `forloop.parentloop`  | 本层循环的外层循环                   |

**for ... empty**

```html
<ul>
{% for user in user_list %}
    <li>{{ user.name }}</li>
{% empty %}
    <li>空空如也</li>
{% endfor %}
</ul>
```

**if 判断**

```html
{% if user_list %}
  用户人数：{{ user_list|length }}
{% elif black_list %}
  黑名单数：{{ black_list|length }}
{% else %}
  没有用户
{% endif %}
```

**if ...else**

```html
{% if user_list|length > 5 %}
  七座豪华SUV
{% else %}
    黄包车
{% endif %}
```

**with**

定义一个中间变量，多用于给一个复杂的变量起别名。

注意等号左右不要加空格。

```html
{% with total=business.employees.count %}
    {{ total }} employee{{ total|pluralize }}
{% endwith %}
# 或
{% with business.employees.count as total %}
    {{ total }} employee{{ total|pluralize }}
{% endwith %}
```

**csrf_token**

这个标签用于跨站请求伪造保护。

在页面的form表单里面写上{% csrf_token %}

### 注意事项：

1. *Django*的模板语言不支持连续判断，即不支持以下写法：

```
{% if a > b > c %}
...
{% endif %}
```

1. *Django*的模板语言中属性的优先级大于方法

```
def xx(request):
    d = {"a": 1, "b": 2, "c": 3, "items": "100"}
    return render(request, "xx.html", {"data": d})
```

如上，我们在使用render方法渲染一个页面的时候，传的字典d有一个key是items并且还有默认的 d.items() 方法，此时在模板语言中:

```
{{ data.items }}
```

默认会取d的items key的值。

### 五、自定义标签

#### 自定义过滤器

**必须要做的三件事：**

1. 在应用名下新建一个名为templates文件夹（必须叫这个名字）
2. 在该新建的文件夹内新建一个任意名称的py文件
3. 在该文件中需要固定写下面代码

```python
from django import template
register = template.Library()


# 自定义过滤器
@register.filter(name='XBB')
def index(a,b):
	return a+b


# 自定义标签：和自定义filter类似，只不过接收更灵活的参数。
@register.simple_tag
def plus(a,b,c):
	return a+b+c

# 自定义inclusion_tag：多用于返回html代码片段
@register.inclusion_tag('login.html',name='login')
def login(n):
	# l = []
	# for i in range(n):
	#     l.append('第%s项'%i)
	l = [ '第%s项'%i for i in range(n)]
	return {'l':l}
	# login.html
	<ul>
		{% for foo in l %}
		<li>{{ foo }}</li>
		{% endfor %}
	</ul>
	# 调用
	{% login 5 %}
```

1. 在使用自定义simple_tag和filter的html文件中导入之前创建的 my_tags.py

```html
{% load my_tags %}
```

1. 使用simple_tag和filter（如何调用）

```
{% load my_tag %}
{{ 666|XBB:8 }}

{% plus 1 2 3 %}



{% login 5 %}
```

### 六、模板导入和继承

**母板：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="x-ua-compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Title</title>
  {% block page-css %}
  
  {% endblock %}
</head>
<body>

<h1>这是母板的标题</h1>

{% block page-main %}

{% endblock %}
<h1>母板底部内容</h1>
{% block page-js %}

{% endblock %}
</body>
</html>
```

**继承母板：在子页面中在页面最上方使用下面的语法来继承母板。**

```
{% extends 'layouts.html' %}
```

**块(block):**

通过在母板中使用`{% block xxx %}`来定义"块"。

在子页面中通过定义母板中的block名来对应替换母板中相应的内容

```html
{% block page-main %}
  <p>世情薄</p>
  <p>人情恶</p>
  <p>雨送黄昏花易落</p>
{% endblock %}
```

**组件：**

可以将常用的页面内容如导航条，页尾信息等组件保存在单独的文件中，然后在需要使用的地方按如下语法导入即可。

```html
{% include 'navbar.html' %}
```

### 七、静态文件相关

静态文件配置

```html
{% load static %}

<link rel='stylesheet' href="{% static 'css/mycss.css'%}">  # 第一种方式
<link rel='stylesheet' href="{% get_static_prefix %}css/mycss.css">  # 第二种方式
```

某个文件多处被用到可以存为一个变量

```html
{% load static %}
{% static "images/hi.jpg" as myphoto %}
<img src="{{ myphoto }}"></img>
```

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



## 7.static

[static test · zronghui/helloDjango@fc19af7](https://github.com/zronghui/helloDjango/commit/fc19af71caf28b5d0130e621bd5766c42ee3c047?diff=unified)

article/templates/staticTest.html

```html
{% load static %}
<img src="{% static "template.jpg" %}" alt="My image">
```

helloDjango/settings.py

```python
STATICFILES_DIRS = [os.path.join(BASE_DIR, "static")]
```



## 8.去除模板中的硬编码 URL

[编写你的第一个 Django 应用，第 3 部分 | Django 文档 | Django](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/#removing-hardcoded-urls-in-templates)

## 9.自定义Error页面



[django系列六：自定义Error页面 - 知乎](https://zhuanlan.zhihu.com/p/31433527)

[No-Github/404-I-remember: 收集各种网站的404页面](https://github.com/No-Github/404-I-remember)

[500 Error Page HTML Templates](https://freefrontend.com/500-error-page-html-templates/)
[39 HTML 404 Page Templates](https://freefrontend.com/html-css-404-page-templates/)
[25 HTML Funny 404 Pages](https://freefrontend.com/html-funny-404-pages/)

### 404

采用[404 - 先知社区](https://xz.aliyun.com/404)页面源代码

代码：

[404 · zronghui/helloDjango@cb21c95](https://github.com/zronghui/helloDjango/commit/cb21c95ff8c38385318b094e4a8f0c49228ef455)

article/templates/error_404.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset=utf-8 />
<title>页面找不到了(´･ω･`)</title>
    <link rel="stylesheet" href="https://xz.aliyun.com/static/css/IE.css">
</head>
<body>
  <div class="demo">
    <p><span>4</span><span>0</span><span>4</span></p>
    <p>页面找不到了(´･ω･`)</p>
      <div style="text-align: center">
          您可以 <a href="javascript:history.go(-1);">返回上一页</a>
          或者 <a href="/">网站首页</a></div>
  </div>
</body>
</html>
```

article/views.py

```python
def error_404(request, exception):
    return render(request, 'error_404.html')
```

helloDjango/settings.py 

```python
DEBUG = False
```

helloDjango/urls.py 

```python
handler404 = error_404
```

### 500

article/templates/error_404.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset=utf-8/>
    <title>500</title>
    <style type="text/css">
        html {
            box-sizing: border-box;
        }

        *,
        *::before,
        *::after {
            box-sizing: inherit;
        }

        body * {
            margin: 0;
            padding: 0;
        }

        body {
            font: normal 100%/1.15 "Merriweather", serif;
            background-color: #7ed0f2;
            color: #fff;
        }

        .wrapper {
            position: relative;
            max-width: 1298px;
            height: auto;
            margin: 2em auto 0 auto;
        }

        /* https://www.flaticon.com/authors/vectors-market */
        /* https://www.flaticon.com/authors/icomoon */
        .box {
            max-width: 70%;
            min-height: auto;
            margin: 0 auto;
            padding: 1em 1em;
            text-align: center;
            background: url("https://www.dropbox.com/s/xq0841cp3icnuqd/flame.png?raw=1") no-repeat top 10em center/128px 128px,
            transparent url("https://www.dropbox.com/s/w7qqrcvhlx3pwnf/desktop-pc.png?raw=1") no-repeat top 13em center/128px 128px;
        }

        h1, p:not(:last-of-type) {
            text-shadow: 0 0 6px #216f79;
        }

        h1 {
            margin: 0 0 1rem 0;
            font-size: 8em;
        }

        p {
            margin-bottom: 0.5em;
            font-size: 3em;
        }

        p:first-of-type {
            margin-top: 4em;
        }

        p > a {
            border-bottom: 1px dashed #216f79;
            font-style: italic;
            text-decoration: none;
            color: #216f79;
        }

        p > a:hover {
            text-shadow: 0 0 6px #216f79;
        }

        p img {
            vertical-align: bottom;
        }
    </style>
</head>
<body>

<div class="wrapper">
    <div class="box">
        <h1>500</h1>
        <p>Sorry, it's me, not you.</p>
        <p>&#58;&#40;</p>
        <p><a href="/">Let me try again!</a></p>
    </div>
</div>

</body>
</html> 
```

article/views.py

```python
def error_500(request):
    return render(request, 'error_500.html')
```

helloDjango/settings.py 

```python
DEBUG = False
```

helloDjango/urls.py 

```python
handler500 = error_500
```

## 10. 登录

[django实现用户登陆访问限制@login_required-苦咖啡's运维之路-51CTO博客](https://blog.51cto.com/alsww/1732435)

### 创建users

```python
python manage.py shell 

from django.contrib.auth.models import User
user = User.objects.create_user('tempUser', 'user@email.com', 'password')
```

以失败告终

## 11.分页

[RSS和分页 - Django 搭建简易博客教程 - 极客学院Wiki](https://wiki.jikexueyuan.com/project/django-set-up-blog/rss-paging.html)

主要参考：

[分页 | Django 文档 | Django](https://docs.djangoproject.com/zh-hans/3.0/topics/pagination/)

代码详见：

[pagination · zronghui/helloDjango@a9eac83](https://github.com/zronghui/helloDjango/commit/a9eac8311beb2810b519ff7be88b201b0bed9b65)

article/templates/list.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>paginator test</title>
</head>
<body>

{% for contact in page_obj %}

    {# Each "contact" is a Contact model object. #}
    {{ contact }}
    ...
{% endfor %}

<div class="pagination">
    <span class="step-links">
        {% if page_obj.has_previous %}
            <a href="?page=1">&laquo; first</a>
            <a href="?page={{ page_obj.previous_page_number }}">previous</a>
        {% endif %}

        <span class="current">
            Page {{ page_obj.number }} of {{ page_obj.paginator.num_pages }}.
        </span>

        {% if page_obj.has_next %}
            <a href="?page={{ page_obj.next_page_number }}">next</a>
            <a href="?page={{ page_obj.paginator.num_pages }}">last &raquo;</a>
        {% endif %}
    </span>
</div>

</body>
</html>
```

article/views.py

```python
from django.core.paginator import Paginator

def listing(request):
    contact_list = list(range(100))
    paginator = Paginator(contact_list, 2)  # Show 25 contacts per page.
    page_number = request.GET.get('page')
    page_obj = paginator.get_page(page_number)
    return render(request, 'list.html', context={'page_obj': page_obj})
```

helloDjango/urls.py

```python
path('list', listing)
```

## 12.日志

参考：[Django的日志配置 | 卡瓦邦噶！](https://www.kawabangga.com/posts/1878)

### 理解Logger

首先要理解logging的工作，这里面主要有四个东西：格式器formatter，过滤器filter，处理器handler，日志实例logger。

```
                         formatter
logger ----> handler ---------------->  files, emails
                         filter
```

### 配置方式

Python中可以使用多种格式配置logging，比如.conf， .ini等。

在Django中，我们是把有关logging的配置写到settings里面。相应的配置及解释如下（仅供参考）。

**注意**：

发送邮件没有生效

真正生效的 logging 级别配置是最后的 loggers



settings.py

```python

# 管理员邮箱
ADMINS = (
    ('zronghui', '*****@qq.com'),
)

# 非空链接，却发生404错误，发送通知MANAGERS
SEND_BROKEN_LINK_EMAILS = True
MANAGERS = ADMINS

# Email设置
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.163.com'  # QQ邮箱SMTP服务器(邮箱需要开通SMTP服务)
EMAIL_PORT = 25  # QQ邮箱SMTP服务端口
EMAIL_HOST_USER = '*****@qq.com'  # 我的邮箱帐号
EMAIL_HOST_PASSWORD = '*****'  # 授权码
EMAIL_SUBJECT_PREFIX = 'website'  # 为邮件标题的前缀,默认是'[django]'
EMAIL_USE_TLS = True  # 开启安全链接
DEFAULT_FROM_EMAIL = SERVER_EMAIL = EMAIL_HOST_USER  # 设置发件人

# logging日志配置
LOGGING = {
    'version': 1,
    'disable_existing_loggers': True,
    'formatters': {  # 日志格式
        'standard': {
            'format': '%(asctime)s [%(threadName)s:%(thread)d] [%(name)s:%(lineno)d] [%(module)s:%(funcName)s] [%(levelname)s]- %(message)s'}
    },
    'filters': {  # 过滤器
        'require_debug_false': {
            '()': 'django.utils.log.RequireDebugFalse',
        }
    },
    'handlers': {  # 处理器
        'null': {
            'level': 'DEBUG',
            'class': 'logging.NullHandler',
        },
        'mail_admins': {  # 发送邮件通知管理员
            'level': 'ERROR',
            'class': 'django.utils.log.AdminEmailHandler',
            # 'filters': ['require_debug_false'],  # 仅当 DEBUG = False 时才发送邮件
            'include_html': True,
        },
        'debug': {  # 记录到日志文件(需要创建对应的目录，否则会出错)
            'level': 'INFO',
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': os.path.join(BASE_DIR, "log", 'debug.log'),  # 日志输出文件
            'maxBytes': 1024 * 1024 * 5,  # 文件大小
            'backupCount': 5,  # 备份份数
            'formatter': 'standard',  # 使用哪种formatters日志格式
        },
        'console': {  # 输出到控制台
            'level': 'INFO',
            'class': 'logging.StreamHandler',
            'formatter': 'standard',
        },
    },
    # 真正生效的 logging 级别配置
    'loggers': {  # logging管理器
        'django': {
            'handlers': ['debug', 'console'],
            'level': 'INFO',
            # 'level': 'DEBUG',
            'propagate': False
        },
        'django.request': {
            'handlers': ['mail_admins'],
            'level': 'ERROR',
            'propagate': True,
        },
        # 对于不在 ALLOWED_HOSTS 中的请求不发送报错邮件
        'django.security.DisallowedHost': {
            'handlers': ['null'],
            'propagate': False,
        },
    }
}
```



Manage.py

```python
logDir = os.path.join(os.path.dirname(__file__), 'log')
logFile = os.path.join(logDir, 'debug.log')
print(logFile)
if not os.path.exists(logDir):
  os.mkdir(logDir)
  if not os.path.exists(logFile):
    os.system(f'touch {logFile}')
```



## 13. swagger

```shell
pip install djangorestframework
pip install django-rest-swagger
```

django-rest-swagger 根据 djangorestframework 自动生成 swagger 文档

官方教程翻译：[主页 - Django REST framework中文站点](https://q1mi.github.io/Django-REST-framework-documentation/)

手把手教程：[m-haziq/django-rest-swagger-docs: Beginners approach to Django Rest Swagger](https://github.com/m-haziq/django-rest-swagger-docs)

根据 swagger 配置文件生成 Django 项目（或模块？）：[praekelt/swagger-django-generator: Convert Swagger specifications into Django or aiohttp code](https://github.com/praekelt/swagger-django-generator)

根据 swagger 配置文件生成相应 swagger URL



[aiohttp-swagger3 — aiohttp-swagger3 0.4.2 documentation](https://aiohttp-swagger3.readthedocs.io/en/latest/)
[hh-h/aiohttp-swagger3: Library for swagger documentation browsing and validating aiohttp requests using swagger specification 3.0](https://github.com/hh-h/aiohttp-swagger3)















```shell
pip install elasticsearch-dsl==5.4.0
```



## tricks

xadmin 是对 Django admin 的美化，但是只能用于 Django 1.* ，[django-admin-bootstrap](https://github.com/douglasmiranda/django-admin-bootstrap)Support Django **1.11** and **2.1**


收集静态文件 python manage.py collectstatic 这一句话就会把以前放在app下static中的静态文件全部拷贝到 settings.py 中设置的 STATIC_ROOT 文件夹中

path('polls/', include('polls.urls')) 

函数 include() 允许引用其它 URLconfs。每当 Django 遇到 include() 时，它会截断与此项匹配的 URL 的部分，并将剩余的字符串发送到 URLconf 以供进一步处理。因为投票应用有它自己的 URLconf( polls/urls.py )，他们能够被放在 "/polls/" ， "/fun_polls/" ，"/content/polls/"，或者其他任何路径下，这个应用都能够正常工作。

python manage.py shell 命令再次打开 Python 交互式命令行

**重定向类、模板类：**

```python
# project/urls.py
from django.urls import path
from django.views.generic.base import RedirectView

from application.views import NewView

class SubclassedRedirectView(RedirectView):
    pattern_name = 'new-view'

urlpatterns = [
    path("old-path/", SubclassedRedirectView.as_view()),
    path("old-path/", RedirectView.as_view(pattern_name='new-view')),
    path("new-path/", NewView.as_view(), name='new-view'),
]

# application/views.py
from django.views.generic.base import TemplateView

class HomeView(TemplateView):
    template_name = 'home.html'
```

**装饰器**

```python
1. require_POST

# 有些视图可以处理多个方法，比如:
def multi_method_view(request):
    if request.method == 'GET':
        return HttpResponse('Method was a GET.')
    elif request.method == 'POST':
        return HttpResponse('Method was a POST.')
# 假设您只想响应一个 POST
def guard_clause_view(request):
    if request.method != 'POST':
        raise Http404()
    return HttpResponse('Method was a POST.')
# 我们可以使用 require post decorator，并让 Django 为我们检查该方法。
from django.view.decorators.http import require_POST

@require_POST
def the_view(request):
    return HttpResponse('Method was a POST.')
  

@login_required # 需要登录
@user_passes_test(lambda user: user.is_staff) # 登录的是员工
```

**mixin**

```python
# application/views.py
from django.contrib.auth.mixins import LoginRequiredMixin, UserPassesTestMixin
from django.views.generic.base import TemplateView

class HomeView(LoginRequiredMixin, TemplateView):
    template_name = 'home.html'

class StaffProtectedView(UserPassesTestMixin, TemplateView):
    template_name = 'staff_eyes_only.html'

    def test_func(self):
        return self.request.user.is_staff
# 可以看到，LoginRequiredMixin UserPassesTestMixin 功能与@login_required @user_passes_test差不多，只是使用方式的区别
```



# useful package

## awesome

[wsvincent/awesome-django: A curated list of awesome things related to Django](https://github.com/wsvincent/awesome-django)
[haiiiiiyun/awesome-django-cn: Django 优秀资源大全。](https://github.com/haiiiiiyun/awesome-django-cn)

### Django 调用流程图

[meshy/django-schema-graph: An interactive graph of your Django model structure](https://github.com/meshy/django-schema-graph)

### Django 快速生成项目代码

[pydanny/cookiecutter-django: Cookiecutter Django is a framework for jumpstarting production-ready Django projects quickly.](https://github.com/pydanny/cookiecutter-django)

```shell
cookiecutter https://github.com/pydanny/cookiecutter-django
自动克隆仓库，并让用户回答问题
use_whitenoise [n]: n
use_celery [n]: y
use_mailhog [n]: n
use_sentry [n]: y
use_pycharm [n]: y
windows [n]: n
use_docker [n]: n
use_heroku [n]: y
use_compressor [n]: y
Select postgresql_version:
……

```

### django 模块化搜索

solr es 等多个搜索引擎接口统一

[django-haystack/django-haystack: Modular search for Django](https://github.com/django-haystack/django-haystack)
[Getting Started with Haystack — Haystack 2.5.0 documentation](https://django-haystack.readthedocs.io/en/master/tutorial.html)



[Getting Started with Haystack — Haystack 2.5.0 documentation 中文文档教程](https://s0django-haystack0readthedocs0io.icopy.site/en/v2.6.0/tutorial.html)
[django-haystack全文检索详细教程_Python_越努力越幸运—liupu-CSDN博客](https://blog.csdn.net/AC_hell/article/details/52875927)
[Haystack入门教程 - 简书](https://www.jianshu.com/p/fa1d29456d80)
[Django：haystack全文检索详细教程 - 秋寻草 - 博客园](https://www.cnblogs.com/gcgc/p/10762416.html)
[卧槽，简单的Django ElasticSearch Haystack我竟然调了那么久。。。 - 知乎](https://zhuanlan.zhihu.com/p/36963164)
[Django Haystack 全文检索与关键词高亮_Django博客教程_追梦人物的博客](https://www.zmrenwu.com/courses/django-blog-tutorial/materials/27/)

## 参考链接

[Django 官方中文文档](https://docs.djangoproject.com/zh-hans/3.0/topics/)



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

[django系列四：静态文件 - 知乎](https://zhuanlan.zhihu.com/p/31208172)

[Python Elasticsearch DSL 使用笔记(一) | Finger's Blog](http://fingerchou.com/2017/08/12/elasticsearch-dsl-with-python-usage-1/)

