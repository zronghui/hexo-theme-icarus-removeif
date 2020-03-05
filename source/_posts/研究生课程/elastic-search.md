---
title: elastic search
toc: true
recommend: 1
uniqueId: '2020-03-05 05:58:51/"elastic search".html'
date: 2020-03-05 13:58:51
thumbnail:
categories:
- 研究生课程
tags:
keywords:
---

[TOC]

<!--more-->

## 安装

```shell
brew install elasticsearch
# 修改 PATH
export PATH=/usr/local/Cellar/elasticsearch/6.8.6/bin:$PATH

# 安装中文分词插件
elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.8.6/elasticsearch-analysis-ik-6.8.6.zip

# 启动
elasticsearch
pip3 install 'elasticsearch>=6.0.0,<7.0.0'
```

打开 [localhost:9200](http://localhost:9200/)

<img src="https://i.loli.net/2020/03/05/Psf5OvknTA8lo1S.png" alt="Psf5OvknTA8lo1S" style="zoom: 25%;" />



## 学习

### 基本概念

Field->type->document->index->node->cluster

字段   分组   一条数据    一张表  节点    集群

## 使用

```python
from elasticsearch import Elasticsearch
es = Elasticsearch()

# 创建 Index
result = es.indices.create(index='news', ignore=400)
print(result)
# {'acknowledged': True, 'shards_acknowledged': True, 'index': 'news'}

# 删除 Index
result = es.indices.delete(index='news', ignore=[400, 404])
print(result)
# {'acknowledged': True}

# 插入数据
data = {'title': '美国留给伊拉克的是个烂摊子吗', 'url': 'http://view.news.qq.com/zt2011/usa_iraq/index.htm'}
result = es.create(index='news', doc_type='politics', id=1, body=data)
print(result)
# {'_index': 'news', '_type': 'politics', '_id': '1', '_version': 1, 'result': 'created', '_shards': {'total': 2, 'successful': 1, 'failed': 0}, '_seq_no': 0, '_primary_term': 1}

# index 方法也可以插入数据，而且可以不指定 ID，会自动生成， index 也可以更新已经存在的数据
# es.index(index='news', doc_type='politics', body=data)

# 更新数据
data = {
    'title': '美国留给伊拉克的是个烂摊子吗',
    'url': 'http://view.news.qq.com/zt2011/usa_iraq/index.htm',
    'date': '2011-12-16'
}
result = es.update(index='news', doc_type='politics', body=data, id=1)
print(result)

# 删除数据
result = es.delete(index='news', doc_type='politics', id=1)
print(result)
# {'_index': 'news', '_type': 'politics', '_id': '1', '_version': 3, 'result': 'deleted', '_shards': {'total': 2, 'successful': 1, 'failed': 0}, '_seq_no': 2, '_primary_term': 1}

# 查询数据
dsl = {
    'query': {
        'match': {
            'title': '中国 领事馆'
        }
    }
}
es = Elasticsearch()
result = es.search(index='news', doc_type='politics', body=dsl)
print(json.dumps(result, indent=2, ensure_ascii=False))

# 以上任何执行若报错，返回：
# {'error': {...}, 'status': ...}, 如：
# {'error': {'root_cause': [{'type': 'resource_already_exists_exception', 'reason': 'index [news/QM6yz2W8QE-bflKhc5oThw] already exists', 'index_uuid': 'QM6yz2W8QE-bflKhc5oThw', 'index': 'news'}], 'type': 'resource_already_exists_exception', 'reason': 'index [news/QM6yz2W8QE-bflKhc5oThw] already exists', 'index_uuid': 'QM6yz2W8QE-bflKhc5oThw', 'index': 'news'}, 'status': 400}
```



查询返回结果

```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 5,
    "successful": 5,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": 2,
    "max_score": 2.546152,
    "hits": [
      {
        "_index": "news",
        "_type": "politics",
        "_id": "dk5G9mQBD9BuE5fdHOUm",
        "_score": 2.546152,
        "_source": {
          "title": "中国驻洛杉矶领事馆遭亚裔男子枪击，嫌犯已自首",
          "url": "http://news.ifeng.com/world/detail_2011_12/16/11372558_0.shtml",
          "date": "2011-12-18"
        }
      },
      {
        "_index": "news",
        "_type": "politics",
        "_id": "dU5G9mQBD9BuE5fdHOUj",
        "_score": 0.2876821,
        "_source": {
          "title": "中韩渔警冲突调查：韩警平均每天扣1艘中国渔船",
          "url": "https://news.qq.com/a/20111216/001044.htm",
          "date": "2011-12-17"
        }
      }
    ]
  }
}
```

## Hello world

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import json

import pretty_errors
from elasticsearch import Elasticsearch

es = Elasticsearch()
mapping = {
    'properties': {
        'title': {
            'type': 'text',
            'analyzer': 'ik_max_word',
            'search_analyzer': 'ik_max_word'
        }
    }
}
es.indices.delete(index='news', ignore=[400, 404])
es.indices.create(index='news', ignore=400)
result = es.indices.put_mapping(index='news', doc_type='politics', body=mapping)
print(result)
# {'acknowledged': True}

datas = [
    {
        'title': '美国留给伊拉克的是个烂摊子吗',
        'url': 'http://view.news.qq.com/zt2011/usa_iraq/index.htm',
        'date': '2011-12-16'
    },
    {
        'title': '公安部：各地校车将享最高路权',
        'url': 'http://www.chinanews.com/gn/2011/12-16/3536077.shtml',
        'date': '2011-12-16'
    },
    {
        'title': '中韩渔警冲突调查：韩警平均每天扣1艘中国渔船',
        'url': 'https://news.qq.com/a/20111216/001044.htm',
        'date': '2011-12-17'
    },
    {
        'title': '中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首',
        'url': 'http://news.ifeng.com/world/detail_2011_12/16/11372558_0.shtml',
        'date': '2011-12-18'
    }
]

for data in datas:
    debug = es.index(index='news', doc_type='politics', body=data)
es.indices.refresh(index="news")

result = es.search(index='news', doc_type='politics')

print(json.dumps(result, indent=2, ensure_ascii=False))

dsl = {
    'query': {
        'match': {
            'title': '中国'
        }
    }
}

es = Elasticsearch()
# result = es.search(index='news', doc_type='politics', q='中国 领事馆')
result = es.search(index='news', doc_type='politics', body=dsl)

print(json.dumps(result, indent=2, ensure_ascii=False))

# {'acknowledged': True}
# {
#   "took": 3,
#   "timed_out": false,
#   "_shards": {
#     "total": 5,
#     "successful": 5,
#     "skipped": 0,
#     "failed": 0
#   },
#   "hits": {
#     "total": 4,
#     "max_score": 1.0,
#     "hits": [
#       {
#         "_index": "news",
#         "_type": "politics",
#         "_id": "q4xHqnABU_ZbTbliQ7pZ",
#         "_score": 1.0,
#         "_source": {
#           "title": "中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首",
#           "url": "http://news.ifeng.com/world/detail_2011_12/16/11372558_0.shtml",
#           "date": "2011-12-18"
#         }
#       },
#       {
#         "_index": "news",
#         "_type": "politics",
#         "_id": "qoxHqnABU_ZbTbliQ7pQ",
#         "_score": 1.0,
#         "_source": {
#           "title": "中韩渔警冲突调查：韩警平均每天扣1艘中国渔船",
#           "url": "https://news.qq.com/a/20111216/001044.htm",
#           "date": "2011-12-17"
#         }
#       },
#       {
#         "_index": "news",
#         "_type": "politics",
#         "_id": "qIxHqnABU_ZbTbliQ7oy",
#         "_score": 1.0,
#         "_source": {
#           "title": "美国留给伊拉克的是个烂摊子吗",
#           "url": "http://view.news.qq.com/zt2011/usa_iraq/index.htm",
#           "date": "2011-12-16"
#         }
#       },
#       {
#         "_index": "news",
#         "_type": "politics",
#         "_id": "qYxHqnABU_ZbTbliQ7pJ",
#         "_score": 1.0,
#         "_source": {
#           "title": "公安部：各地校车将享最高路权",
#           "url": "http://www.chinanews.com/gn/2011/12-16/3536077.shtml",
#           "date": "2011-12-16"
#         }
#       }
#     ]
#   }
# }
# {
#   "took": 7,
#   "timed_out": false,
#   "_shards": {
#     "total": 5,
#     "successful": 5,
#     "skipped": 0,
#     "failed": 0
#   },
#   "hits": {
#     "total": 2,
#     "max_score": 0.2876821,
#     "hits": [
#       {
#         "_index": "news",
#         "_type": "politics",
#         "_id": "q4xHqnABU_ZbTbliQ7pZ",
#         "_score": 0.2876821,
#         "_source": {
#           "title": "中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首",
#           "url": "http://news.ifeng.com/world/detail_2011_12/16/11372558_0.shtml",
#           "date": "2011-12-18"
#         }
#       },
#       {
#         "_index": "news",
#         "_type": "politics",
#         "_id": "qoxHqnABU_ZbTbliQ7pQ",
#         "_score": 0.2876821,
#         "_source": {
#           "title": "中韩渔警冲突调查：韩警平均每天扣1艘中国渔船",
#           "url": "https://news.qq.com/a/20111216/001044.htm",
#           "date": "2011-12-17"
#         }
#       }
#     ]
#   }
# }
#
```



## 相关社区、教程

[Introduction | Elasticsearch权威指南（中文版）](https://es.xiaoleilu.com/index.html)
[全文搜索引擎 Elasticsearch 入门教程 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html)
[Elastic 中文社区](https://elasticsearch.cn/)

## 参考链接

[Elasticsearch 基本介绍及其与 Python 的对接实现 | 静觅](https://cuiqingcai.com/6214.html)

