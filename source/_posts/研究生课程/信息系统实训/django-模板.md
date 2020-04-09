---
title: django 模板
toc: true
recommend: 1
uniqueId: '2020-04-09 04:06:21/"django 模板".html'
date: 2020-04-09 12:06:21
thumbnail:
categories:
- 研究生课程
- 信息系统实训
tags:
keywords:
---

[TOC]

<!--more-->



[Django——模板层 - King'home - 博客园](https://www.cnblogs.com/king-home/p/11004487.html)

### 二、常用的模板层语法

```
只需要记两种特殊符号：

变量相关的用{{}}，逻辑相关的用{%%}。

变量：当模版引擎遇到一个变量，它将计算这个变量，然后用结果替换掉它本身。 变量的命名包括任何字母数字以及下划线 ("_")的组合。 变量名称中不能有空格或标点符号。

点（.）在模板语言中有特殊的含义。当模版系统遇到点(".")，它将以这样的顺序查询：

字典查询（Dictionary lookup） 属性或方法查询（Attribute or method lookup） 数字索引查询（Numeric index lookup）
```

### 三、Filters（过滤器）

```
在*Django*的模板语言中通过使用过滤器来改变变量的显示

过滤器的语法： {{ value|filter_name:参数 }}

使用管道符"|"来应用过滤器。

例如：{{ name|lower }}会将name变量应用lower过滤器之后再显示它的值。lower在这里的作用是将文本全都变成小写
```

**注意事项：**

```
1. 过滤器支持“链式”操作。即一个过滤器的输出作为另一个过滤器的输入。
2. 过滤器可以接受参数，例如：{{ sss|truncatewords:30 }}，这将显示sss的前30个词。
3. 过滤器参数包含空格的话，必须用引号包裹起来。比如使用逗号和空格去连接一个列表中的元素，如：{{ list|join:', ' }}
4. **'|'左右没有空格没有空格没有空格**
```

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

```
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
```

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

在页面的form表单里面写上

```
{% csrf_token %}
```



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

