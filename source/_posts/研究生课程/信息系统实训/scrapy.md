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
   
      ```shell
      scrapy shell 'http://www.gqzzw.com/type/bjsh' -s USER_AGENT='Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36'
      ```
      
      
      
      <img src="https://i.loli.net/2020/03/12/H84sqghj3rNXpWv.jpg" alt="H84sqghj3rNXpWv" style="zoom:50%;" />

## 学习

### 启动所有爬虫 代码启动

[Scrapy教程10- 动态配置爬虫 — scrapy-cookbook 0.2.2 文档](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-10.html)

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import pretty_errors
import scrapy
from scrapy.crawler import CrawlerProcess
from scrapy.utils.project import get_project_settings

from helloScrapy.spiders.gqzzw import GqzzwSpider

pretty_errors.activate()
lastPage = input('lastPage: ')
zzname = input('zzname: ')
process = CrawlerProcess(get_project_settings())
# 传入参数
process.crawl(GqzzwSpider, lastPage=lastPage, zzname=zzname)
process.start()  # the script will block here until the crawling is finished

```





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



### 选择器  css xpath

```python
# 二者等价
response.xpath('//div/a/text()').extract()
response.css('div a::text').extract()

response.xpath('//@class').extract()  # 选取所有名为 class 的属性
response.xpath('//article/div[1]').extract()
response.xpath('//article/div[last()]').extract()
response.xpath('//article/div[last()-1]').extract()
response.xpath('//article/div[@lang]').extract() # 包含 lang 属性
response.xpath("//article/div[@lang='eng']").extract() # lang 属性为 eng
response.xpath('/div/*').extract() # 所有子节点
response.xpath('//*').extract() # 所有元素
response.xpath('//title[@*]').extract() # 所有带属性的 title 元素
response.xpath('//div/a | //div/p').extract()# 所有 div 的 a 和 p

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



#### 获取HTML 注释

```python
from lxml import etree
 
html_str = """
<div id="box1">this from blog.csdn.net/lncxydjq , DO NOT COPY!
  <div id="box2">*****
    <!--can u get me, bitch?-->
  </div>
</div>
"""
 
html = etree.HTML(html_str)
 
print html.xpath('//div[@id="box1"]/div/node()')[1]
print type(html.xpath('//div[@id="box1"]/div/node()')[1])
print html.xpath('//div[@id="box1"]/div/node()')[1].text
 
"""output:
<!--can u get me, bitch?-->
<type 'lxml.etree._Comment'>
can u get me, bitch?
"""
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





## 课上教的

### **1** 延迟获取

#### **1.1** DOWNLOAD_DELAY

settings.py

设置DOWNLOAD_DELAY

数值越大，延迟越大。

#### 1.3 滚动加载

像那种页面滚动到下方，才新加载数据的网页，可以通过selenium执行脚本来实现。

```python
# 首先导入包
from selenium.common.exceptions import TimeoutException
# 然后输入代码
try:
  spider.browser.get(request.url)
  spider.browser.execute_script('window.scrollTo(0, document.body.scrollHeight)')
except TimeoutException as e:
  print('超时')
  spider.browser.execute_script('window.stop()')
time.sleep(2)
return HtmlResponse(url=spider.browser.current_url, body=spider.browser.page_source, encoding="utf-8", request=request)
```

滚动到页面底部的操作可以通过javascript代码'window.scrollTo(0, document.body.scrollHeight)'，来实现，所以要执行该脚本，方法是execute_script

### **2** settings.py

参考网址：[scrapy的配置文件settings - 龙云飞谷 - 博客园](https://www.cnblogs.com/longyunfeigu/p/9494408.html)

```python
#==>第一部分：基本配置<===
#1、项目名称，默认的USER_AGENT由它来构成，也作为日志记录的日志名
BOT_NAME = 'Amazon'

#2、爬虫应用路径
SPIDER_MODULES = ['Amazon.spiders']
NEWSPIDER_MODULE = 'Amazon.spiders'

#3、客户端User-Agent请求头
#USER_AGENT = 'Amazon (+http://www.yourdomain.com)'

#4、是否遵循爬虫协议
# Obey robots.txt rules
ROBOTSTXT_OBEY = False

#5、是否支持cookie，cookiejar进行操作cookie，默认开启
#COOKIES_ENABLED = False

#6、Telnet用于查看当前爬虫的信息，操作爬虫等...使用telnet ip port ，然后通过命令操作
#TELNETCONSOLE_ENABLED = False
#TELNETCONSOLE_HOST = '127.0.0.1'
#TELNETCONSOLE_PORT = [6023,]

#7、Scrapy发送HTTP请求默认使用的请求头
#DEFAULT_REQUEST_HEADERS = {
#   'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
#   'Accept-Language': 'en',
#}



#===>第二部分：并发与延迟<===
#1、下载器总共最大处理的并发请求数,默认值16
#CONCURRENT_REQUESTS = 32

#2、每个域名能够被执行的最大并发请求数目，默认值8
#CONCURRENT_REQUESTS_PER_DOMAIN = 16

#3、能够被单个IP处理的并发请求数，默认值0，代表无限制，需要注意两点
#I、如果不为零，那CONCURRENT_REQUESTS_PER_DOMAIN将被忽略，即并发数的限制是按照每个IP来计算，而不是每个域名
#II、该设置也影响DOWNLOAD_DELAY，如果该值不为零，那么DOWNLOAD_DELAY下载延迟是限制每个IP而不是每个域
#CONCURRENT_REQUESTS_PER_IP = 16

#4、如果没有开启智能限速，这个值就代表一个规定死的值，代表对同一网址延迟请求的秒数
#DOWNLOAD_DELAY = 3



#===>第三部分：智能限速/自动节流：AutoThrottle extension<===
#一：介绍
from scrapy.contrib.throttle import AutoThrottle #http://scrapy.readthedocs.io/en/latest/topics/autothrottle.html#topics-autothrottle
设置目标：
1、比使用默认的下载延迟对站点更好
2、自动调整scrapy到最佳的爬取速度，所以用户无需自己调整下载延迟到最佳状态。用户只需要定义允许最大并发的请求，剩下的事情由该扩展组件自动完成


#二：如何实现？
在Scrapy中，下载延迟是通过计算建立TCP连接到接收到HTTP包头(header)之间的时间来测量的。
注意，由于Scrapy可能在忙着处理spider的回调函数或者无法下载，因此在合作的多任务环境下准确测量这些延迟是十分苦难的。 不过，这些延迟仍然是对Scrapy(甚至是服务器)繁忙程度的合理测量，而这扩展就是以此为前提进行编写的。


#三：限速算法
自动限速算法基于以下规则调整下载延迟
#1、spiders开始时的下载延迟是基于AUTOTHROTTLE_START_DELAY的值
#2、当收到一个response，对目标站点的下载延迟=收到响应的延迟时间/AUTOTHROTTLE_TARGET_CONCURRENCY
#3、下一次请求的下载延迟就被设置成：对目标站点下载延迟时间和过去的下载延迟时间的平均值
#4、没有达到200个response则不允许降低延迟
#5、下载延迟不能变的比DOWNLOAD_DELAY更低或者比AUTOTHROTTLE_MAX_DELAY更高

#四：配置使用
#开启True，默认False
AUTOTHROTTLE_ENABLED = True
#起始的延迟
AUTOTHROTTLE_START_DELAY = 5
#最小延迟
DOWNLOAD_DELAY = 3
#最大延迟
AUTOTHROTTLE_MAX_DELAY = 10
#每秒并发请求数的平均值，不能高于 CONCURRENT_REQUESTS_PER_DOMAIN或CONCURRENT_REQUESTS_PER_IP，调高了则吞吐量增大强奸目标站点，调低了则对目标站点更加”礼貌“
#每个特定的时间点，scrapy并发请求的数目都可能高于或低于该值，这是爬虫视图达到的建议值而不是硬限制
AUTOTHROTTLE_TARGET_CONCURRENCY = 16.0
#调试
AUTOTHROTTLE_DEBUG = True
CONCURRENT_REQUESTS_PER_DOMAIN = 16
CONCURRENT_REQUESTS_PER_IP = 16



#===>第四部分：爬取深度与爬取方式<===
#1、爬虫允许的最大深度，可以通过meta查看当前深度；0表示无深度
# DEPTH_LIMIT = 3

#2、爬取时，0表示深度优先Lifo(默认)；1表示广度优先FiFo

# 后进先出，深度优先
# DEPTH_PRIORITY = 0
# SCHEDULER_DISK_QUEUE = 'scrapy.squeue.PickleLifoDiskQueue'
# SCHEDULER_MEMORY_QUEUE = 'scrapy.squeue.LifoMemoryQueue'
# 先进先出，广度优先

# DEPTH_PRIORITY = 1
# SCHEDULER_DISK_QUEUE = 'scrapy.squeue.PickleFifoDiskQueue'
# SCHEDULER_MEMORY_QUEUE = 'scrapy.squeue.FifoMemoryQueue'


#3、调度器队列
# SCHEDULER = 'scrapy.core.scheduler.Scheduler'
# from scrapy.core.scheduler import Scheduler

#4、访问URL去重
# DUPEFILTER_CLASS = 'step8_king.duplication.RepeatUrl'



#===>第五部分：中间件、Pipelines、扩展<===
#1、Enable or disable spider middlewares
# See http://scrapy.readthedocs.org/en/latest/topics/spider-middleware.html
#SPIDER_MIDDLEWARES = {
#    'Amazon.middlewares.AmazonSpiderMiddleware': 543,
#}

#2、Enable or disable downloader middlewares
# See http://scrapy.readthedocs.org/en/latest/topics/downloader-middleware.html
DOWNLOADER_MIDDLEWARES = {
   # 'Amazon.middlewares.DownMiddleware1': 543,
}

#3、Enable or disable extensions
# See http://scrapy.readthedocs.org/en/latest/topics/extensions.html
#EXTENSIONS = {
#    'scrapy.extensions.telnet.TelnetConsole': None,
#}

#4、Configure item pipelines
# See http://scrapy.readthedocs.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
   # 'Amazon.pipelines.CustomPipeline': 200,
}



#===>第六部分：缓存<===
"""
1. 启用缓存
    目的用于将已经发送的请求或相应缓存下来，以便以后使用
    
    from scrapy.downloadermiddlewares.httpcache import HttpCacheMiddleware
    from scrapy.extensions.httpcache import DummyPolicy
    from scrapy.extensions.httpcache import FilesystemCacheStorage
"""
# 是否启用缓存策略
# HTTPCACHE_ENABLED = True

# 缓存策略：所有请求均缓存，下次在请求直接访问原来的缓存即可
# HTTPCACHE_POLICY = "scrapy.extensions.httpcache.DummyPolicy"
# 缓存策略：根据Http响应头：Cache-Control、Last-Modified 等进行缓存的策略
# HTTPCACHE_POLICY = "scrapy.extensions.httpcache.RFC2616Policy"

# 缓存超时时间
# HTTPCACHE_EXPIRATION_SECS = 0

# 缓存保存路径
# HTTPCACHE_DIR = 'httpcache'

# 缓存忽略的Http状态码
# HTTPCACHE_IGNORE_HTTP_CODES = []

# 缓存存储的插件
# HTTPCACHE_STORAGE = 'scrapy.extensions.httpcache.FilesystemCacheStorage'

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



## 防止封IP策略

[yidao620c/core-scrapy: python-scrapy demo](https://github.com/yidao620c/core-scrapy#)

如果抓取太频繁了，就被被封IP

策略1：设置download_delay下载延迟，数字设置为5秒，越大越安全
策略2：禁止Cookie，某些网站会通过Cookie识别用户身份，禁用后使得服务器无法识别爬虫轨迹
策略3：使用user agent池。也就是每次发送的时候随机从池中选择不一样的浏览器头信息，防止暴露爬虫身份
策略4：使用IP池，这个需要大量的IP资源，貌似还达不到这个要求
策略5：分布式爬取，这个是针对大型爬虫系统的，对目前而言我们还用不到。



scrapy crawl shudan -s JOBDIR=jobs/shudan-1



## scrapy-cookbook

Contents:

- Scrapy教程01- 入门篇
  - [安装scrapy](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-01.html#scrapy)
  - [简单示例](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-01.html#)
  - [Scrapy特性一览](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-01.html#scrapy)
- Scrapy教程02- 完整示例
  - [创建Scrapy工程](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-02.html#scrapy)
  - [定义我们的Item](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-02.html#item)
  - [第一个Spider](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-02.html#spider)
  - [运行爬虫](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-02.html#)
  - [处理链接](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-02.html#)
  - [导出抓取数据](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-02.html#)
  - [保存数据到数据库](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-02.html#)
  - [下一步](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-02.html#)
- Scrapy教程03- Spider详解
  - [CrawlSpider](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-03.html#crawlspider)
  - [XMLFeedSpider](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-03.html#xmlfeedspider)
  - [CSVFeedSpider](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-03.html#csvfeedspider)
  - [SitemapSpider](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-03.html#sitemapspider)
- Scrapy教程04- Selector详解
  - [关于选择器](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-04.html#)
  - [使用选择器](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-04.html#)
  - [嵌套选择器](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-04.html#)
  - [使用正则表达式](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-04.html#)
  - [XPath相对路径](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-04.html#xpath)
  - [XPath建议](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-04.html#xpath)
- Scrapy教程05- Item详解
  - [定义Item](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-05.html#item)
  - [Item Fields](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-05.html#item-fields)
  - [Item使用示例](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-05.html#item)
  - [Item Loader](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-05.html#item-loader)
  - [输入/输出处理器](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-05.html#)
  - [自定义Item Loader](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-05.html#item-loader)
  - [在Field定义中声明输入/输出处理器](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-05.html#field)
  - [Item Loader上下文](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-05.html#item-loader)
  - [内置的处理器](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-05.html#)
- Scrapy教程06- Item Pipeline
  - [编写自己的Pipeline](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-06.html#pipeline)
  - [Item Pipeline示例](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-06.html#item-pipeline)
  - [激活一个Item Pipeline组件](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-06.html#item-pipeline)
  - [Feed exports](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-06.html#feed-exports)
  - [请求和响应](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-06.html#)
- Scrapy教程07- 内置服务
  - [发送email](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-07.html#email)
  - [同一个进程运行多个Spider](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-07.html#spider)
  - [分布式爬虫](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-07.html#)
  - [防止被封的策略](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-07.html#)
- Scrapy教程08- 文件与图片
  - [使用Files Pipeline](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-08.html#files-pipeline)
  - [使用Images Pipeline](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-08.html#images-pipeline)
  - [使用例子](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-08.html#)
  - [自定义媒体管道](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-08.html#)
- Scrapy教程09- 部署
  - [部署到Scrapyd](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-09.html#scrapyd)
  - [部署到Scrapy Cloud](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-09.html#scrapy-cloud)
- Scrapy教程10- 动态配置爬虫
  - [脚本运行Scrapy](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-10.html#scrapy)
  - [同一进程运行多个spider](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-10.html#spider)
  - [定义规则表](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-10.html#)
  - [定义文章Item](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-10.html#item)
  - [定义ArticleSpider](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-10.html#articlespider)
  - [编写pipeline存储到数据库中](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-10.html#pipeline)
  - [修改run.py启动脚本](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-10.html#run-py)
- Scrapy教程11- 模拟登录
  - [重写start_requests方法](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-11.html#start-requests)
  - [使用FormRequest](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-11.html#formrequest)
  - [重写_requests_to_follow](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-11.html#requests-to-follow)
  - [页面处理方法](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-11.html#)
  - [完整源码](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-11.html#)
- Scrapy教程12- 抓取动态网站
  - [scrapy-splash简介](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-12.html#scrapy-splash)
  - [安装docker](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-12.html#docker)
  - [安装Splash](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-12.html#splash)
  - [安装scrapy-splash](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-12.html#scrapy-splash)
  - [配置scrapy-splash](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-12.html#scrapy-splash)
  - [使用scrapy-splash](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-12.html#scrapy-splash)
  - [使用实例](https://scrapy-cookbook.readthedocs.io/zh_CN/latest/scrapy-12.html#)



