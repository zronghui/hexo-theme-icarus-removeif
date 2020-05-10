---
title: elastic search
toc: true
recommend: 1
uniqueId: '2020-03-05 05:58:51/"elastic search".html'
date: 2020-03-05 13:58:51
thumbnail:
categories:
- 研究生课程
- 信息系统实训
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

### centos 

[用RPM安装Elasticsearch到Linux系统服务 - 简书](https://www.jianshu.com/p/3b0650b5c7bb)

[Install Elasticsearch with RPM | Elasticsearch Reference [6.8] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/rpm.html)

先安装 Java8

```shell
yum install java-1.8.0-openjdk
java -version
export JAVA_HOME='/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java'
echo JAVA_HOME
```



vim /etc/yum.repos.d/elasticsearch.repo

```shell
[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

```shell
yum makecache
yum install elasticsearch

sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable elasticsearch.service
# 启动或停止
sudo systemctl start elasticsearch.service
# sudo systemctl stop elasticsearch.service
# 检查Elasticsearch是否正在运行
curl -XGET 'localhost:9200'

export PATH=/usr/share/elasticsearch/bin:$PATH
elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.8.8/elasticsearch-analysis-ik-6.8.8.zip
pip3 install 'elasticsearch>=6.0.0,<7.0.0'
# 重启 es，加载 ik plugin
sudo systemctl stop elasticsearch.service
sudo systemctl start elasticsearch.service

```







打开 [localhost:9200](http://localhost:9200/)

<img src="https://i.loli.net/2020/03/05/Psf5OvknTA8lo1S.png" alt="Psf5OvknTA8lo1S" style="zoom: 25%;" />



## 学习

### 基本概念

Relational DB: ->Databases->Tables->Rows->Columns 

ElasticSearch: ->Indices->Types->Documents->Fields

## 使用

[Field datatypes | Elasticsearch Reference [7.6] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html#_core_datatypes)

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



## 建立索引 插入数据 查询数据 

[ElasticSearch+python完成类似mysql的不分词查询 - 知乎](https://zhuanlan.zhihu.com/p/51242233)

### 建立索引

```json
PUT /index_test          // index_test 对应mysql database_name
{
  "mappings":{
    "doc_type":{        // doc_type对应 对应mysql table_name
      "properties": {
          "field1": {"type": "keyword"},
          "field2": {"type": "keyword"},
          "field3": {"type": "keyword"}
      }
    }
  }
}

例如：

PUT /fb2m
{
  "mappings":{
    "kb_fact":{
      "properties": {
        "subject": {"type": "keyword"},
        "predicate": {"type": "keyword"},
        "object": {"type": "keyword"}
      }
    }
  }
}
```

### 批量插入数据

```python
import copy
import elasticsearch
from elasticsearch import helpers
import json
# 你的Elasticsearch所对应的IP地址和端口，注意，不是Kibanake可视化界面的地址
es = elasticsearch.Elasticsearch([{'host':'xx.xx.x.xxx','port':xxxx}])

print("============== index ================")
count = 0
i = 0
j = 0
num=0
actions = []
max_count = 2000
with open('dataset/index_data','r',encoding='utf-8') as f:
    for line in f:
        j += 1
        triple = line.strip().split(" ||| ")
        try:
            triple_dict = {'field1':triple[0],'field2':triple[1],'field3':triple[2]}
            # 如果数据量小可以用index的方法一条条插入
            # 这里index，doc_type就等于上一步建立索引所用的名称
            #es.index(index='index_test',doc_type='doc_type',body=triple_dict)
            action={
            "_index":"index_test",
            "_type":"doc_type",
            "_id":i,
            "_source":triple_dict
            }
            i += 1
            count += 1
            actions.append(action)
        except:
            print(" !!! "+ str(j) +" th row insert faied: "+line)
            continue
        if count>=max_count:
            helpers.bulk(es, actions)
            actions=[]
            count=0
            num+=1
            print("Insert "+str(num*max_count)+" records.")
helpers.bulk(es,actions)
print('finish~~~')
```

### 查询数据

[ES 21 - Elasticsearch的高级检索语法 (包括term、prefix、wildcard、fuzzy、boost等) - 瘦风 - 博客园](https://www.cnblogs.com/shoufeng/p/11103913.html#3--wildcard-query---%E9%80%9A%E9%85%8D%E7%AC%A6%E6%A3%80%E7%B4%A2)

详见博客，↑

1 term query - 索引词检索
1.1 term query - 不分词检索
1.2 terms query - in检索
2 prefix query - 前缀检索
3 wildcard query - 通配符检索
4 regexp query - 正则检索
5 fuzzy query - 纠错检索
6 boost评分权重 - 控制文档的优先级别
7 dis_max的用法 - best fields策略
7.1 dis_max的提出
7.2 使用示例
8 exist query - 存在检索, 已过期
9 复杂检索的使用范例
9.1 多条件过滤 - 包含
9.2 多条件拼接 - 包含+范围+排序
9.3 定制检索结果的排序规则



```json
1.不分词查询所有：
select * from doc_type

GET /index_test/doc_type/_search
{
  "query": {
    "match_all": {
    }
  }
}
2.不分词匹配一个字段：
select * from doc_type where filed1=value1

GET /index_test/doc_type/_search
{
  "query": {
    "term": { "filed1" :"value1 }
  }
}
3.不分词同时匹配两个字段：

mysql：select * from doc_type where filed1=value1 and filed2=value2

ES可以用bool过滤器实现组合检索,一个bool过滤器由三部分组成：

{
   "bool" : {
      "must" :     [],  //与 AND 等价
      "should" :   [],  //与 OR 等价
      "must_not" : [],  //与 NOT 等价
   }
}
所有对应代码如下：

GET /index_test/doc_type/_search
{
  "query": {
    "bool": {
      "must": [
        { "term": { "filed1" :"value1 }},
        { "term": {  "filed2" :"value2" }}
      ]
    }
  }
}

多字段（Multi-filed）查询
{
    "query": {
        "multi_match" : {
            "query" : "elasticsearch guide",
            "fields": ["title", "summary"]
        }
    }
}

需要提高某一个字段的分值。在下面的例子中，我们把 summary 字段的分数提高三倍
{
    "query": {
        "multi_match" : {
            "query" : "elasticsearch guide",
            "fields": ["title", "summary^3"]
        }
    },
    "_source": ["title", "summary", "publish_date"]
}

模糊（Fuzzy）查询
{
    "query": {
        "multi_match" : {
            "query" : "comprihensiv guide",
            "fields": ["title", "summary"],
            "fuzziness": "AUTO"
        }
    },
    "_source": ["title", "summary", "publish_date"],
    "size": 1
}
```

```python
Python代码检索：

# 上面DSL代码query部分复制下来
dsl = {
 "query": {
    "bool": {
      "must": [
        { "term": { "filed1" :"value1"}},
        { "term": {  "filed2" :"value2"}}
      ]
    }
  }
}
es_result = es.search(index='index_test', doc_type='doc_type', body=dsl)
# es返回的是一个dict
result = es_result['hits']['hits']
print(result)


```



### 更多查询

详见：[19 个很有用的 ElasticSearch 查询语句](https://n3xtchen.github.io/n3xtchen/elasticsearch/2017/07/05/elasticsearch-23-useful-query-example)

7. 正则（Regexp）查询
8. 短语匹配(Match Phrase)查询
9. 短语前缀（Match Phrase Prefix）查询
10. 查询字符串（Query String）
11. 简单查询字符串（Simple Query String）
12. 词条（Term）/多词条（Terms）查询
13. 词条（Term）查询 - 排序（Sorted）
14. 范围查询
15. 过滤(Filtered)查询
16. 多重过滤（Multiple Filters）
17. 作用分值: 域值（Field Value）因子
18. 作用分值: 衰变（Decay）函数
19. 函数分值: 脚本评分

### Query with highlight

[Highlighting - elasticsearch中文文档](http://doc.codingdict.com/elasticsearch/106/)

```
{
    "query" : { "match" : { "content" : "中国" }},
    "highlight" : {
        "pre_tags" : ["<tag1>", "<tag2>"],
        "post_tags" : ["</tag1>", "</tag2>"],
        "fields" : {
            "content" : {}
        }
    }
}
```

```json
{
    "took": 14,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "failed": 0
    },
    "hits": {
        "total": 2,
        "max_score": 2,
        "hits": [
            {
                "_index": "index",
                "_type": "fulltext",
                "_id": "4",
                "_score": 2,
                "_source": {
                    "content": "中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首"
                },
                "highlight": {
                    "content": [
                        "<tag1>中国</tag1>驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首 "
                    ]
                }
            },
            {
                "_index": "index",
                "_type": "fulltext",
                "_id": "3",
                "_score": 2,
                "_source": {
                    "content": "中韩渔警冲突调查：韩警平均每天扣1艘中国渔船"
                },
                "highlight": {
                    "content": [
                        "均每天扣1艘<tag1>中国</tag1>渔船 "
                    ]
                }
            }
        ]
    }
}
```





### Elastic 6.x 版只允许每个Index 包含一个Type，7.x 版将会彻底 ... 

干！

## ‘黎明前的黑暗’ 分词报错 

干！

[6.7.2，”黎明前的黑暗“ 分词报错 startOffset must be non-negative, and endOffset must be >= startOffset · Issue #680 · medcl/elasticsearch-analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik/issues/680)



## elasticsearch-dump

[taskrabbit/elasticsearch-dump: Import and export tools for elasticsearch](https://github.com/taskrabbit/elasticsearch-dump)

use a supported version of node. v8 or higher: 要求 npm 版本 >= 8

导出为 json，或把一个节点的数据复制到另外一个节点

## 问题

#### 1.网页端搜索引擎请求失败，elasticsearch Failed to establish a new connection

curl 127.0.0.1:9200

无法连接



systemctl restart elasticsearch.service 后 systemctl status elasticsearch.service -l 可以看到服务启动失败

解决艰辛过程：

systemctl 命令使用

```shell
1.启动、关闭、重启 nfs服务
systemctl start/stop/restart nfs-server.service

2.设置开机自启动
systemctl enable nfs-server.service

3.停止开机自启动
systemctl disable nfs-server.service

4.查看服务当前状态
systemctl status nfs-server.service
systemctl status nfs-server.service -l # 查看全部

5.查看所有已启动的服务
systemctl list-units --type=service
```



systemctl status elasticsearch.service -l 可以看到 There is insufficient memory for the Java Runtime Environment to continue Solution

vim /etc/elasticsearch/jvm.options

```shell
# -Xms1g
# -Xmx1g
-Xms500m
-Xmx500m
```

设小了可以启动，但是一搜索就又崩了，最后又改成默认的 1g

去 阿里云控制台 查看 mem 使用情况，一直挺高的，但是在服务器内部，使用 ps -aux | sort -k4nr | head 查看内存占用，并没有内存占用特别高的进程



最后，没辙了，**重启服务器**，问题解决，去 阿里云控制台 查看 mem 使用情况，内存占用正常





## 相关社区、教程

[Python Elasticsearch Client — Elasticsearch 7.5.1 documentation](https://elasticsearch-py.readthedocs.io/en/master/index.html)

[Introduction | Elasticsearch权威指南（中文版）](https://es.xiaoleilu.com/index.html)
[全文搜索引擎 Elasticsearch 入门教程 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html)
[Elastic 中文社区](https://elasticsearch.cn/)

## 参考链接

[Elasticsearch 基本介绍及其与 Python 的对接实现 | 静觅](https://cuiqingcai.com/6214.html)

[19 个很有用的 ElasticSearch 查询语句](https://n3xtchen.github.io/n3xtchen/elasticsearch/2017/07/05/elasticsearch-23-useful-query-example)

[elasticsearch-analysis-ik/README.md at master · medcl/elasticsearch-analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik/blob/master/README.md)



[Elasticsearch - Mapping - Tutorialspoint](https://www.tutorialspoint.com/elasticsearch/elasticsearch_mapping.htm)



[Python Elasticsearch DSL 查询、过滤、聚合操作实例 - 掘金](https://juejin.im/post/5d346f9551882549a70ad729)
[Python3 操作 elasticsearch - 简书](https://www.jianshu.com/p/462007422e65)



[干货 | Elasticsearch、Kibana数据导出实战 - 掘金](https://juejin.im/post/5d5a9583e51d456206115a06)

[Field datatypes | Elasticsearch Reference [7.6] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html#_core_datatypes)
