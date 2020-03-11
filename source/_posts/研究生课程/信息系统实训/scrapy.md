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

# 运行contract检查（没明白，不过好像也没用）
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



## 参考资料

[Scrapy框架的使用之Selector的用法 - 掘金](https://juejin.im/post/5aec1bb9f265da0b9526f855)

介绍了 css xpath re 的用法
