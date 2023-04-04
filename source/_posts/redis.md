---
title: redis
date: 2021-01-04
banner_img: /img/basic/redis-binner.jpg
index_img: /img/basic/redis-index.jpg
tags: 
 - redis
categories:
 - java
 - middle-component
 - redis
---
# Redis

### 引言

在Web应用发展的初期，那时关系型数据库受到了较为广泛的关注和应用，原因是因为那时候Web站点基本上访问和并发不高、交互也较少。而在后来，随着访问量的提升，使用关系型数据库的Web站点多多少少都开始在性能上出现了一些瓶颈，而瓶颈的源头一般是在磁盘的I/O上。而随着互联网技术的进一步发展，各种类型的应用层出不穷，这导致在当今云计算、大数据盛行的时代，对性能有了更多的需求，主要体现在以下四个方面：

1. 低延迟的读写速度：应用快速地反应能极大地提升用户的满意度
2. 支撑海量的数据和流量：对于搜索这样大型应用而言，需要利用PB级别的数据和能应对百万级的流量
3. 大规模集群的管理：系统管理员希望分布式应用能更简单的部署和管理
4. 庞大运营成本的考量：IT部门希望在硬件成本、软件成本和人力成本能够有大幅度地降低

为了克服这一问题，NoSQL应运而生，它同时具备了高性能、可扩展性强、高可用等优点，受到广泛开发人员和仓库管理人员的青睐。

### Redis是什么

Redis是现在最受欢迎的NoSQL数据库之一，Redis是一个使用ANSI C编写的开源、包含多种数据结构、支持网络、基于内存、可选持久性的键值对存储数据库，其具备如下特性：

- 基于内存运行，性能高效
- 支持分布式，理论上可以无限扩展
- key-value存储系统
- 开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API

相比于其他数据库类型，Redis具备的特点是：

- C/S通讯模型
- 单进程单线程模型
- 丰富的数据类型
- 操作具有原子性
- 持久化
- 高并发读写
- 支持lua脚本

哪些大厂在使用Redis？

- github
- twitter
- 微博
- Stack Overflow
- 阿里巴巴
- 百度
- 美团
- 搜狐

#### Redis的应用场景有哪些？

Redis 的应用场景包括：缓存系统（“热点”数据：高频读、低频写）、计数器、消息队列系统、排行榜、社交网络和实时系统。

 ![img](https://i.loli.net/2021/01/19/H8lWRphECVtYGk2.png)

### Redis的数据类型及主要特性

Redis提供的数据类型主要分为5种自有类型和一种自定义类型，这5种自有类型包括：String类型、哈希类型、列表类型、集合类型和顺序集合类型。

![img](https://i.loli.net/2021/01/19/dzZCyliWpQwqRhT.png)

#### String类型：

它是一个二进制安全的字符串，意味着它不仅能够存储字符串、还能存储图片、视频等多种类型, 最大长度支持512M。

对每种数据类型，Redis都提供了丰富的操作命令，如：

- GET/MGET
- SET/SETEX/MSET/MSETNX
- INCR/DECR
- GETSET
- DEL

#### 哈希类型：

该类型是由field和关联的value组成的map。其中，field和value都是字符串类型的。

Hash的操作命令如下：

- HGET/HMGET/HGETALL
- HSET/HMSET/HSETNX
- HEXISTS/HLEN
- HKEYS/HDEL
- HVALS

#### 列表类型：

该类型是一个插入顺序排序的字符串元素集合, 基于双链表实现。

List的操作命令如下：

- LPUSH/LPUSHX/LPOP/RPUSH/RPUSHX/RPOP/LINSERT/LSET
- LINDEX/LRANGE
- LLEN/LTRIM

#### 集合类型：

Set类型是一种无顺序集合, 它和List类型最大的区别是：集合中的元素没有顺序, 且元素是唯一的。

Set类型的底层是通过哈希表实现的，其操作命令为：

- SADD/SPOP/SMOVE/SCARD
- SINTER/SDIFF/SDIFFSTORE/SUNION

Set类型主要应用于：在某些场景，如社交场景中，通过交集、并集和差集运算，通过Set类型可以非常方便地查找共同好友、共同关注和共同偏好等社交关系。

#### 顺序集合类型：

ZSet是一种有序集合类型，每个元素都会关联一个double类型的分数权值，通过这个权值来为集合中的成员进行从小到大的排序。与Set类型一样，其底层也是通过哈希表实现的。

ZSet命令：

- ZADD/ZPOP/ZMOVE/ZCARD/ZCOUNT
- ZINTER/ZDIFF/ZDIFFSTORE/ZUNION

### Redis的数据结构

Redis的数据结构如下图所示：

![img](https://i.loli.net/2021/01/19/dY6Qa2FX4nyPj7C.png)

关于上表中的部分释义：

1. 压缩列表是列表键和哈希键的底层实现之一。当一个列表键只包含少量列表项，并且每个列表项要么就是小整数，要么就是长度比较短的字符串，Redis就会使用压缩列表来做列表键的底层实现
2. 整数集合是集合键的底层实现之一，当一个集合只包含整数值元素，并且这个集合的元素数量不多时，Redis就会使用整数集合作为集合键的底层实现

如下是定义一个Struct数据结构的例子：

![img](https://lunasaw.github.io/luna-image-bed/img/139239-20191126141036706-1247302576.png)

 

#### 简单动态字符串SDS (Simple Dynamic String)

基于C语言中传统字符串的缺陷，Redis自己构建了一种名为简单动态字符串的抽象类型，简称SDS，其结构如下：

![img](https://lunasaw.github.io/luna-image-bed/img/139239-20191126141052157-436992972.png)

SDS几乎贯穿了Redis的所有数据结构，应用十分广泛。

#### SDS的特点

和C字符串相比，SDS的特点如下：

![img](https://lunasaw.github.io/luna-image-bed/img/139239-20191126141103927-1852926069.png)

　　1. 常数复杂度获取字符串长度

　　　　Redis中利用SDS字符串的len属性可以直接获取到所保存的字符串的长
　　　　度，直接将获取字符串长度所需的复杂度从C字符串的O(N)降低到了O(1)。

　　2. 减少修改字符串时导致的内存重新分配次数

　　　　通过C字符串的特性，我们知道对于一个包含了N个字符的C字符串来说，其底层实现总是N+1个字符长的数组（额外一个空字符结尾）

　　　　那么如果这个时候需要对字符串进行修改，程序就需要提前对这个C字符串数组进行一次内存重分配（可能是扩展或者释放）　

　　　　而内存重分配就意味着是一个耗时的操作。

Redis巧妙的使用了SDS避免了C字符串的缺陷。在SDS中，buf数组的长度不一定就是字符串的字符数量加一，buf数组里面可以包含未使用的字节，而这些未使用的字节由free属性记录。

与此同时，SDS采用了**空间预分配**的策略，避免C字符串每一次修改时都需要进行内存重分配的耗时操作，将内存重分配从原来的每修改N次就分配N次——>降低到了修改N次最多分配N次。

如下是Redis对SDS的简单定义：

![img](https://lunasaw.github.io/luna-image-bed/img/139239-20191126141239967-123973180.png) 

![img](https://tva1.sinaimg.cn/large/008i3skNly1gvqmb7qezrj60bb0bwdgx02.jpg)

### Redis特性1：事务

- 命令序列化，按顺序执行
- 原子性
- 三阶段: 开始事务 - 命令入队 - 执行事务
- 命令：MULTI/EXEC/DISCARD

### Redis特性2：发布订阅(Pub/Sub)

- Pub/sub是一种消息通讯模式
- Pub发送消息, Sub接受消息
- Redis客户端可以订阅任意数量的频道
- “fire and forgot”, 发送即遗忘
- 命令：Publish/Subscribe/Psubscribe/UnSub

　　![img](https://i.loli.net/2021/01/19/u5pyEk47B2dKDHC.png)

### Redis特性3：Stream

- Redis 5.0新增
- 等待消费
- 消费组(组内竞争)
- 消费历史数据
- FIFO

![img](https://i.loli.net/2021/01/19/5RsvUW9X81aLdVl.png)

 

 

 

以上就是Redis的基本概念，下面我们将介绍在开发过程中可能会踩到的“坑”。

### Redis常见问题解析：击穿

概念：在Redis获取某一key时, 由于key不存在, 而必须向DB发起一次请求的行为, 称为“Redis击穿”。

![img](https://i.loli.net/2021/01/19/KXzoAb5Wf8xjMpB.png)

引发击穿的原因：

- 第一次访问
- 恶意访问不存在的key
- Key过期

合理的规避方案：

- 服务器启动时, 提前写入
- 规范key的命名, 通过中间件拦截
- 对某些高频访问的Key，设置合理的TTL或永不过期

### Redis常见问题解析：雪崩

概念：Redis缓存层由于某种原因宕机后，所有的请求会涌向存储层，短时间内的高并发请求可能会导致存储层挂机，称之为“Redis雪崩”。

合理的规避方案：

- 使用Redis集群
- 限流

### Redis在产品开发中的应用实践

为此，我很高兴的为大家介绍，葡萄城架构师Jim将在2019-11-27 14：00 为大家带来一场公开课，其中 Jim除了为大家讲解Redis的基础，同时也会实际演示他所在的项目组使用Redis时碰到的问题以及解决方案，对于刚接触Redis的同学来说，更具参考意义和学习价值，欢迎大家届时参加，公开课地址：https://live.vhall.com/661463644。

- 后端采用nodeJS
- 使用Azure的Redis服务
- Redis的使用场景

　　　　- token缓存, 用于令牌验证

　　　　- IP白名单

碰到的问题

- “网络抖动”或者Redis服务异常导致Redis访问超时
- Redis客户端驱动稳定性问题

　　　　- 连接池 “Broken connection” 问题

　　　　- JS的Promise引出的Redis重置问题

下面我们来简单了解一下Redis的进阶知识。

### 进阶之Redis协议简介

Redis客户端通讯协议：RESP(Redis Serialization Protocol)，其特点是：

- 简单
- 解析速度快
- 可读性好

Redis集群内部通讯协议：RECP(Redis Cluster Protocol ) ，其特点是：

- 每一个node两个tcp 连接
- 一个负责client-server通讯(P: 6379)
- 一个负责node之间通讯(P: 10000 + 6379)

 ![img](https://i.loli.net/2021/01/19/1kqa8y9d3guoKNx.png)

Redis协议支持的数据类型：

- 简单字符(首字节: “+”)

   　　“+OK\r\n”

- 错误(首字节: “-”)

   　　“-error msg\r\n”

- 数字(首字节: “:”)

   　　“:123\r\n”

- 批量字符(首字节: “$”)

   　　“&hello\r\nWhoa re you\r\n”

- 数组(首字节: “*”)

   　　“*0\r\n”

      　　“*-1\r\n”

### 除了Redis，还有什么NoSQL型数据库

市面上类似于Redis，同样是NoSQL型的数据库有很多，如下图所示，除了Redis，还有MemCache、Cassadra和Mongo。下面，我们就分别对这几个数据库做一下简要的介绍：

![img](https://tva1.sinaimg.cn/large/008i3skNly1gvqmard27zj608w0aswez02.jpg)

 

**Memcache****：**这是一个和Redis非常相似的数据库，但是它的数据类型没有Redis丰富。Memcache由LiveJournal的Brad Fitzpatrick开发，作为一套分布式的高速缓存系统，被许多网站使用以提升网站的访问速度，对于一些大型的、需要频繁访问数据库的网站访问速度的提升效果十分显著。

**Apache Cassandra****：**（社区内一般简称为C*）这是一套开源分布式NoSQL数据库系统。它最初由Facebook开发，用于储存收件箱等简单格式数据，集Google BigTable的数据模型与Amazon Dynamo的完全分布式架构于一身。Facebook于2008将 Cassandra 开源，由于其良好的可扩展性和性能，被 Apple、Comcast、Instagram、Spotify、eBay、Rackspace、Netflix等知名网站所采用，成为了一种流行的分布式结构化数据存储方案。

**MongoDB**：是一个基于分布式文件存储、面向文档的NoSQL数据库，由C++编写，旨在为WEB应用提供可扩展的高性能数据存储解决方案。MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系型数据库的，它支持的数据结构非常松散，是一种类似json的BSON格式。