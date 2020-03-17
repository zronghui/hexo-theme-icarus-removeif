---
title: scrapy
toc: true
recommend: 1
uniqueId: '2020-03-10 03:26:04/"scrapy".html'
date: 2020-03-10 11:26:04
thumbnail:
categories:
- 研究生课程
- 信息系统实训
tags:
keywords:
---

[TOC]

<!--more-->

[Scrapy入门教程 — Scrapy 0.24.6 文档](https://scrapy-chs.readthedocs.io/zh_CN/0.24/intro/tutorial.html)

## helloScrapy

```shell 
pip install scrapy
scrapy startproject helloScrapy
scrapy genspider volmoe volmoe.com
scrapy genspider douban douban.com
```



### settings.py

300 是权重，决定多个 pipeline 执行的顺序

```python
# ROBOTSTXT_OBEY = True

ITEM_PIPELINES = {
   'helloScrapy.pipelines.HelloscrapyPipeline': 300,
}
```

### pipelines.py

```python
# -*- coding: utf-8 -*-

# Define your item pipelines here
#
# Don't forget to add your pipeline to the ITEM_PIPELINES setting
# See: https://docs.scrapy.org/en/latest/topics/item-pipeline.html
import json


class HelloscrapyPipeline(object):
    """
    处理爬虫所爬取到的数据
    """

    def __init__(self):
        """
        初始化操作，在爬虫运行过程中只执行一次
        """
        self.file = open('books.json', 'w', encoding='utf-8')

    def process_item(self, item, spider):
        # 现将item数据转为字典类型，再将其保存为json文件
        text = json.dumps(dict(item), ensure_ascii=False) + '\n'
        # 写入本地
        self.file.write(text)
        # 会将item打印到屏幕上，方便观察
        return item

    def close_spider(self, spider):
        """
        爬虫关闭时所执行的操作，在爬虫运行过程中只执行一次
        """
        self.file.close()

```



### items.py

```python
import scrapy


class BookItem(scrapy.Item):
    # define the fields for your item here like:
    # name = scrapy.Field()
    book_name = scrapy.Field()
    book_url = scrapy.Field()
    book_desc = scrapy.Field()
```



### Spider/volmoe.py

```python
# -*- coding: utf-8 -*-
import scrapy
from zhconv import convert

from helloScrapy.items import BookItem


class VolmoeSpider(scrapy.Spider):
    name = 'volmoe'
    allowed_domains = ['volmoe.com']
    # start_urls = [f'https://volmoe.com/l/all,all,all,sortpoint,all,all/{i}.htm' for i in range(1, 2)]
    start_urls = [f'https://volmoe.com/l/all,all,all,sortpoint,all,all/{i}.htm' for i in range(1, 473)]

    # 自动调用 parse ，解析 item URL
    def parse(self, response):
        itemUrlList = list(set(response.xpath('/html/body/div[7]/table') \
                               .xpath('//a/@href') \
                               .re(r'https://volmoe.com/c/\d+.htm')))
        for itemUrl in itemUrlList:
            yield scrapy.Request(url=itemUrl, callback=self.parse_item)

    # 手动调用 parse_item ，解析 item
    def parse_item(self, response):
        try:
            item = BookItem()
            item['book_url'] = response.url
            item['book_name'] = convert(response.xpath('//div/b/text()').extract()[0], 'zh-cn')
            item['book_desc'] = convert(response.xpath('//*[@id="desc_text"]/text()').extract()[0].strip(), 'zh-cn')
            yield item
        except Exception as e:
            print(e)
            return

```

### 注意

1. 爬取 列表页+详情页 用 rules 更方便，但是我用了，没有生效，所以手动指定函数处理

1. 直接在网页上的 xpath 可能会与爬虫请求到的结构不同，所以可以用 scrapy shell "url"， view(response), 在网页上使用 xpath finder , xpath helper 查看指定元素的 xpath

1. scrapy genspider mydomain mydomain.com，最后的 mydomain.com 不用加 http 和 com 后面的/, 如 ~~http://mydomain.com/~~

1. 借助 https://github.com/further-reading/scrapy-gui 测试 CSS ~~xpath~~

   <img src="https://i.loli.net/2020/03/11/b5X6KtGxNMn7OCv.png" alt="b5X6KtGxNMn7OCv" style="zoom:50%;" />
   
   5. 获得的 URL 没有域名时：url = response.urljoin(next)  ?
   
   6. 可以在一个爬虫中用 custom_settings <img src="https://i.loli.net/2020/03/12/YLS5ftcphlDMTzx.png" alt="YLS5ftcphlDMTzx" style="zoom: 25%;" />
   
   7. scrapy对request的URL去重 通过 yield scrapy.Request(url, self.parse, dont_filter=False) 里的 dont_filter（默认False） 实现
   
   8. 如果命令行里不想看到那么多输出的话，可以加个 -L WARNING 参数运行爬虫，如：scrapy crawl spider1 -L WARNING
   
   9. scrapy shell 设置 UA
   
      <img src="https://i.loli.net/2020/03/12/H84sqghj3rNXpWv.jpg" alt="H84sqghj3rNXpWv" style="zoom:50%;" />

## 学习

### 启动所有爬虫？？

### 启动一个爬虫

scrapy crawl volmoe

### 事例 spider

```python
import scrapy

from tutorial.items import DmozItem

class DmozSpider(scrapy.Spider):
    name = "dmoz"
    allowed_domains = ["dmoz.org"]
    start_urls = [
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/",
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Resources/"
    ]

    def parse(self, response):
        for sel in response.xpath('//ul/li'):
            item = DmozItem()
            item['title'] = sel.xpath('a/text()').extract()
            item['link'] = sel.xpath('a/@href').extract()
            item['desc'] = sel.xpath('text()').extract()
            yield item
```



### 保存爬取到的数据

最简单存储爬取的数据的方式是使用 [Feed exports](https://scrapy-chs.readthedocs.io/zh_CN/0.24/topics/feed-exports.html#topics-feed-exports):

```
scrapy crawl dmoz -o items.json
```

该命令将采用 [JSON](http://en.wikipedia.org/wiki/JSON) 格式对爬取的数据进行序列化，生成 `items.json` 文件。

在类似本篇教程里这样小规模的项目中，这种存储方式已经足够。 如果需要对爬取到的item做更多更为复杂的操作，您可以编写 [Item Pipeline](https://scrapy-chs.readthedocs.io/zh_CN/0.24/topics/item-pipeline.html#topics-item-pipeline) 。 



### 在Shell中尝试Selector选择器

为了介绍Selector的使用方法，接下来我们将要使用内置的 [Scrapy shell](https://scrapy-chs.readthedocs.io/zh_CN/0.24/topics/shell.html#topics-shell) 。Scrapy Shell需要您预装好IPython(一个扩展的Python终端)。

您需要进入项目的根目录，执行下列命令来启动shell:

```
scrapy shell "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/"
```

注解

当您在终端运行Scrapy时，请一定记得给url地址加上引号，否则包含参数的url(例如 `&` 字符)会导致Scrapy运行失败。

shell的输出类似:

```
[ ... Scrapy log here ... ]

2014-01-23 17:11:42-0400 [default] DEBUG: Crawled (200) <GET http://www.dmoz.org/Computers/Programming/Languages/Python/Books/> (referer: None)
[s] Available Scrapy objects:
[s]   crawler    <scrapy.crawler.Crawler object at 0x3636b50>
[s]   item       {}
[s]   request    <GET http://www.dmoz.org/Computers/Programming/Languages/Python/Books/>
[s]   response   <200 http://www.dmoz.org/Computers/Programming/Languages/Python/Books/>
[s]   settings   <scrapy.settings.Settings object at 0x3fadc50>
[s]   spider     <Spider 'default' at 0x3cebf50>
[s] Useful shortcuts:
[s]   shelp()           Shell help (print this help)
[s]   fetch(req_or_url) Fetch request (or URL) and update local objects
[s]   view(response)    View response in a browser

In [1]:
```

当shell载入后，您将得到一个包含response数据的本地 `response` 变量。输入 `response.body` 将输出response的包体， 输出 `response.headers` 可以看到response的包头。

更为重要的是，当输入 `response.selector` 时， 您将获取到一个可以用于查询返回数据的selector(选择器)， 以及映射到 `response.selector.xpath()` 、 `response.selector.css()` 的 快捷方法(shortcut): `response.xpath()` 和 `response.css()` 。

同时，shell根据response提前初始化了变量 `sel` 。该selector根据response的类型自动选择最合适的分析规则(XML vs HTML)。

让我们来试试:

```python
In [1]: response.xpath('//title')
Out[1]: [<Selector xpath='//title' data=u'<title>Open Directory - Computers: Progr'>]

In [2]: response.xpath('//title').extract()
Out[2]: [u'<title>Open Directory - Computers: Programming: Languages: Python: Books</title>']

In [3]: response.xpath('//title/text()')
Out[3]: [<Selector xpath='//title/text()' data=u'Open Directory - Computers: Programming:'>]

In [4]: response.xpath('//title/text()').extract()
Out[4]: [u'Open Directory - Computers: Programming: Languages: Python: Books']

In [5]: response.xpath('//title/text()').re('(\w+):')
Out[5]: [u'Computers', u'Programming', u'Languages', u'Python']
```

### 命令行工具

详见：[命令行工具(Command line tools) — Scrapy 0.24.6 文档](https://scrapy-chs.readthedocs.io/zh_CN/0.24/topics/commands.html)

```shell
scrapy startproject myproject

# 可以指定模板（怎么自定义模板？）
scrapy genspider [-t template] <name> <domain>
# 查看 basic 模板
scrapy genspider -d basic

scrapy crawl myspider

# 运行contract检查, 运行没问题就好
scrapy check [-l] <spider>

scrapy list

# 使用 EDITOR 中设定的编辑器编辑给定的spider
scrapy edit spider1

# 使用Scrapy下载器(downloader)下载给定的URL，并将获取到的内容送到标准输出（命令行？）
scrapy fetch <url>

# ** 在浏览器中打开给定的URL，并以Scrapy spider获取到的形式展现。 有些时候spider获取到的页面和普通用户看到的并不相同。 因此该命令可以用来检查spider所获取到的页面，并确认这是您所期望的。
scrapy view <url>

scrapy shell [url]

# 获取给定的URL并使用相应的spider分析处理。如果您提供 --callback 选项，则使用spider的该方法处理，否则使用 parse 。
scrapy parse <url> [options]

# 获取Scrapy的设定
scrapy settings [options]
$ scrapy settings --get BOT_NAME
scrapybot
$ scrapy settings --get DOWNLOAD_DELAY
0

# 在未创建项目的情况下，运行一个编写在Python文件中的spider。
scrapy runspider <spider_file.py>
$ scrapy runspider myspider.py
[ ... spider starts crawling ... ]

# 输出Scrapy版本。配合 -v 运行时，该命令同时输出Python, Twisted以及平台的信息，方便bug提交。
scrapy version [-v]

# 将项目部署到Scrapyd服务？
scrapy deploy [ <target:project> | -l <target> | -L ]

# 运行benchmark测试
scrapy bench


```

### items

```python
# 定义一个 item
class Product(scrapy.Item):
    name = scrapy.Field()
    price = scrapy.Field()
    stock = scrapy.Field()
    last_updated = scrapy.Field(serializer=str)
    

# item API和 dict API 非常相似

# 一、与Item配合
# 创建item
Product(name='PC', price=100)
# 获取字段的值
p.get('name')
p.get('name', 'default')
p['name']
'name' in product # True  name 字段是否有填充
'last_updated' in product.fields # False  last_updated 是否是声明的字段
# 设置字段的值
p['last_updated'] = 'today'
# 获取所有获取到的值
p.keys() # ['price', 'name']
p.items() # [('price', 1000), ('name', 'Desktop PC')]

# 其他任务
#   复制item:
>>> product2 = Product(product)
>>> product3 = product2.copy()
#   根据item创建字典(dict):
>>> dict(product) # create a dict from all populated values
{'price': 1000, 'name': 'Desktop PC'}
#   根据字典(dict)创建item:
>>> Product({'name': 'Laptop PC', 'price': 1500})

# 扩展Item
#   可以通过继承原始的Item来扩展item(添加更多的字段或者修改某些字段的元数据)
class DiscountedProduct(Product):
    discount_percent = scrapy.Field(serializer=str)
    discount_expiration_date = scrapy.Field()
# Item对象
# 字段(Field)对象

```



### 选择器

```python
# 二者等价
response.xpath('//div/a/text()').extract()
response.css('div a::text').extract()
# 二者等价
response.xpath('//a/@href').extract_first()
response.xpath('a::attr(href)').extract_first()

# 复杂点的用法  href 属性包含 'image'
response.xpath('//a[contains(@href, "iamge")]/@href').extract()
response.css('a[href*=image]::attr(href)').extract()

# xpath css 可以嵌套
response.xpath('...').css('...')

# re
response.xpath('//a[contains(@href, "image")]/text()').re(r'Name:\s*(.*)')
# re re_first

# 相对XPaths

divs = response.xpath('//div')

>>> for p in divs.xpath('//p'):  # this is wrong - gets all <p> from the whole document
# 下面是比较合适的处理方法(注意 .//p XPath的点前缀):

>>> for p in divs.xpath('.//p'):  # extracts all <p> inside
...     print p.extract()

# 另一种常见的情况将是提取所有直系 <p> 的结果:
>>> for p in divs.xpath('p'):
```

### spiders

#### 传递参数

```python
scrapy crawl myspider -a category=electronics

import scrapy

class MySpider(Spider):
    name = 'myspider'

    def __init__(self, category=None, *args, **kwargs):
        super(MySpider, self).__init__(*args, **kwargs)
        self.start_urls = ['http://www.example.com/categories/%s' % category]
        # ...
```

#### Spider 类

**name**

唯一标识 spider

**allowed_domains**

包含 spider 允许爬取的域名列表

**start_urls**

**start_requests() ?**

必须返回可迭代对象

逻辑：

1. 未指定 start_urls: start_requests 生效
2. 指定 start_urls: make_requests_from_url 生效

可以使用 start_requests 在启动时以 post 登录某个网站：

```python
def start_requests(self):
    return [scrapy.FormRequest("http://www.example.com/login",
                               formdata={'user': 'john', 'pass': 'secret'},
                               callback=self.logged_in)]

def logged_in(self, response):
    # here you would extract links to follow and return Requests for
    # each of them, with another callback
    pass
```

**make_requests_from_url(url) ?**

该方法接受一个URL并返回用于爬取的 Request 对象。 该方法在初始化request时被 start_requests() 调用，也被用于转化url为request。

**log(message[, level, component])**

log中自动带上该spider的 name 属性。 

```python
class MySpider(scrapy.Spider):
    name = 'example.com'
    allowed_domains = ['example.com']
    start_urls = [
        'http://www.example.com/1.html',
        'http://www.example.com/2.html',
        'http://www.example.com/3.html',
    ]

    def parse(self, response):
        self.log('A response from %s just arrived!' % response.url)
```

**parse 中返回多个 request 和 item**

```python
import scrapy
from myproject.items import MyItem

class MySpider(scrapy.Spider):
    name = 'example.com'
    allowed_domains = ['example.com']
    start_urls = [
        'http://www.example.com/1.html',
        'http://www.example.com/2.html',
        'http://www.example.com/3.html',
    ]

    def parse(self, response):
        sel = scrapy.Selector(response)
        for h3 in response.xpath('//h3').extract():
            yield MyItem(title=h3)

        for url in response.xpath('//a/@href').extract():
            yield scrapy.Request(url, callback=self.parse)
```



#### CrawlSpider

继承自 Spider，有一个新属性 rules 和一个可复写的方法 parse_start_url

Rule(link_extractor, callback=None, cb_kwargs=None, follow=None, process_links=None, process_request=None)

**当编写爬虫规则时，请避免使用 parse 作为回调函数**。 由于 CrawlSpider 使用 parse 方法来实现其逻辑，如果 您覆盖了 parse 方法，crawl spider 将会运行失败。

Follow 默认为 False

```python
import scrapy
from scrapy.contrib.spiders import CrawlSpider, Rule
from scrapy.contrib.linkextractors import LinkExtractor

class MySpider(CrawlSpider):
    name = 'example.com'
    allowed_domains = ['example.com']
    start_urls = ['http://www.example.com']

    rules = (
        # 提取匹配 'category.php' (但不匹配 'subsection.php') 的链接并跟进链接(没有callback意味着follow默认为True)
        Rule(LinkExtractor(allow=('category\.php', ), deny=('subsection\.php', ))),

        # 提取匹配 'item.php' 的链接并使用spider的parse_item方法进行分析
        Rule(LinkExtractor(allow=('item\.php', )), callback='parse_item'),
    )

    def parse_item(self, response):
        self.log('Hi, this is an item page! %s' % response.url)

        item = scrapy.Item()
        item['id'] = response.xpath('//td[@id="item_id"]/text()').re(r'ID: (\d+)')
        item['name'] = response.xpath('//td[@id="item_name"]/text()').extract()
        item['description'] = response.xpath('//td[@id="item_description"]/text()').extract()
        return item
```



#### XMLFeedSpider、CSVFeedSpider、SitemapSpider

```Python
XMLFeedSpider例子
该spider十分易用。下边是其中一个例子:

from scrapy import log
from scrapy.contrib.spiders import XMLFeedSpider
from myproject.items import TestItem

class MySpider(XMLFeedSpider):
    name = 'example.com'
    allowed_domains = ['example.com']
    start_urls = ['http://www.example.com/feed.xml']
    iterator = 'iternodes' # This is actually unnecessary, since it's the default value
    itertag = 'item'

    def parse_node(self, response, node):
        log.msg('Hi, this is a <%s> node!: %s' % (self.itertag, ''.join(node.extract())))

        item = TestItem()
        item['id'] = node.xpath('@id').extract()
        item['name'] = node.xpath('name').extract()
        item['description'] = node.xpath('description').extract()
        return item
        
CSVFeedSpider例子
下面的例子和之前的例子很像，但使用了 CSVFeedSpider:

from scrapy import log
from scrapy.contrib.spiders import CSVFeedSpider
from myproject.items import TestItem

class MySpider(CSVFeedSpider):
    name = 'example.com'
    allowed_domains = ['example.com']
    start_urls = ['http://www.example.com/feed.csv']
    delimiter = ';'
    headers = ['id', 'name', 'description']

    def parse_row(self, response, row):
        log.msg('Hi, this is a row!: %r' % row)

        item = TestItem()
        item['id'] = row['id']
        item['name'] = row['name']
        item['description'] = row['description']
        return item
        
        
        
SitemapSpider样例
简单的例子: 使用 parse 处理通过sitemap发现的所有url:

from scrapy.contrib.spiders import SitemapSpider

class MySpider(SitemapSpider):
    sitemap_urls = ['http://www.example.com/sitemap.xml']

    def parse(self, response):
        pass # ... scrape item here ...

```



### Item Pipeline

当Item在Spider中被收集之后，它将会被传递到Item Pipeline，一些组件会按照一定的顺序执行对Item的处理。以下是item pipeline的一些典型应用：

- 清理HTML数据
- 验证爬取的数据(检查item包含某些字段)
- 查重(并丢弃)
- 将爬取结果保存到数据库中

#### 编写自己的 item pipeline

**process_item**

返回 item 或 抛出 DropItem 异常

**open_spider**

当spider被开启时，这个方法被调用。

**close_spider**
当spider被关闭时，这个方法被调用

#### Item pipeline 样例

```python
# 验证价格，同时丢弃没有价格的item
from scrapy.exceptions import DropItem

class PricePipeline(object):

    vat_factor = 1.15

    def process_item(self, item, spider):
        if item['price']:
            if item['price_excludes_vat']:
                item['price'] = item['price'] * self.vat_factor
            return item
        else:
            raise DropItem("Missing price in %s" % item)
            
# 将item写入JSON文件
import json

class JsonWriterPipeline(object):

    def __init__(self):
        self.file = open('items.jl', 'wb')

    def process_item(self, item, spider):
        line = json.dumps(dict(item)) + "\n"
        self.file.write(line)
        return 
      
# 去重
from scrapy.exceptions import DropItem

class DuplicatesPipeline(object):

    def __init__(self):
        self.ids_seen = set()

    def process_item(self, item, spider):
        if item['id'] in self.ids_seen:
            raise DropItem("Duplicate item found: %s" % item)
        else:
            self.ids_seen.add(item['id'])
            return item
```

#### 代理ip

```python
class ProxyMiddleware(object):
    logger = logging.getLogger(__name__)

    def process_exception(self, request, exception, spider):
        self.logger.debug('Get Exception')
        request.meta['proxy'] = get_random_proxy() # 形如 https://127.0.0.1:9743
        return request

```



settings.py 启动 item pipeline 插件

从小到大的顺序执行

```python
ITEM_PIPELINES = {
    'myproject.pipelines.PricePipeline': 300,
    'myproject.pipelines.JsonWriterPipeline': 800,
}
```





### Jobs: 暂停，恢复爬虫

```shell
# 启用或恢复一个爬虫，都是
scrapy crawl douban -s JOBDIR=jobs/douban-1
```



### 爬虫优化

```python
# 增加并发
CONCURRENT_REQUESTS = 100
# 降低log级别
# LOG_LEVEL = 'INFO'
# 禁止cookies
COOKIES_ENABLED = False
# 禁止重试
RETRY_ENABLED = False
# 减小下载超时,有些网站就是慢
# DOWNLOAD_TIMEOUT = 15
# 禁止重定向
REDIRECT_ENABLED = False
```





## 参考资料

[Scrapy框架的使用之Selector的用法 - 掘金](https://juejin.im/post/5aec1bb9f265da0b9526f855)

介绍了 css xpath re 的用法



## 工具 -- Useful packages

[further-reading/scrapy-gui: A simple, Qt-Webengine powered web browser with built in functionality for basic scrapy webscraping support.](https://github.com/further-reading/scrapy-gui)

今天在豆瓣失败了，大多还是很好用的

```python
ipython
import scrapy_gui
scrapy_gui.open_browser()
```



<img src="https://i.loli.net/2020/03/11/b5X6KtGxNMn7OCv.png" alt="b5X6KtGxNMn7OCv" style="zoom:50%;" />



### gerapy

主要用来管理本地或远程主机的 scrapy 项目，替代 scrapyd 的命令行

```shell
pip3 install gerapy
gerapy init
gerapy init <workspace> # 留空 为 gerapy
cd gerapy
gerapy migrate
gerapy createsuperuser
gerapy runserver
```

[Gerapy/Gerapy: Distributed Crawler Management Framework Based on Scrapy, Scrapyd, Django and Vue.js](https://github.com/Gerapy/Gerapy)
[跟繁琐的命令行说拜拜！Gerapy分布式爬虫管理框架来袭！ - 天善智能：专注于商业智能BI和数据分析、大数据领域的垂直社区平台](https://ask.hellobi.com/blog/cuiqingcai/11195#articleHeader5)
[python爬虫之Gerapy安装部署_Python_baidu_32542573的博客-CSDN博客](https://blog.csdn.net/baidu_32542573/article/details/79722390)





scrapy crawl shudan -s JOBDIR=jobs/shudan-1





