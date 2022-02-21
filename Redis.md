# 一、Nosql概述

## 为什么使用Nosql

> 1、单机Mysql时代

90年代,一个网站的访问量一般不会太大，单个数据库完全够用。随着用户增多，网站出现以下问题

![在这里插入图片描述](G:\Typora 文档\Redis图片\2020082010365930.png#pic_center)

1. 数据量增加到一定程度，单机数据库就放不下了
2. 数据的索引（B+ Tree）,一个机器内存也存放不下
3. 访问量变大后（读写混合），一台服务器承受不住。
   

> 2、Memcached(缓存) + Mysql + 垂直拆分（读写分离）

网站80%的情况都是在读，每次都要去查询数据库的话就十分的麻烦！所以说我们希望减轻数据库的压力，我们可以使用缓存来保证效率！

![image-20210910101850600](G:\Typora 文档\Redis图片\image-20210910101850600.png)

优化过程经历了以下几个过程：

* 优化数据库的数据结构和索引(难度大)

* 文件缓存，通过IO流获取比每次都访问数据库效率略高，但是流量爆炸式增长时候，IO流也承受不了

* MemCache,当时最热门的技术，通过在数据库和数据库访问层之间加上一层缓存，第一次访问时查询数据库，将结果保存到缓存，后续的查询先检查缓存，若有直接拿去使用，效率显著提升。

> 3、分库分表 + 水平拆分 + Mysql集群

![在这里插入图片描述](G:\Typora 文档\Redis图片\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70#pic_center)

> 4、如今最近的年代

 如今信息量井喷式增长，各种各样的数据出现（用户定位数据，图片数据等），大数据的背景下关系型数据库（RDBMS）无法满足大量数据要求。Nosql数据库就能轻松解决这些问题。

> 目前一个基本的互联网项目

![在这里插入图片描述](G:\Typora 文档\Redis图片\1)

> 为什么要用NoSQL ？

用户的个人信息，社交网络，地理位置。用户自己产生的数据，用户日志等等爆发式增长！
这时候我们就需要使用NoSQL数据库的，Nosql可以很好的处理以上的情况！

## 什么是Nosql

NoSQL = Not Only SQL（不仅仅是SQL）

Not Only Structured Query Language

关系型数据库：列+行，同一个表下数据的结构是一样的。

非关系型数据库：数据存储没有固定的格式，并且可以进行横向扩展。

NoSQL泛指非关系型数据库，随着web2.0互联网的诞生，传统的关系型数据库很难对付web2.0时代！尤其是超大规模的高并发的社区，暴露出来很多难以克服的问题，NoSQL在当今大数据环境下发展的十分迅速，Redis是发展最快的。

### Nosql特点

1. 方便扩展（数据之间没有关系，很好扩展！）
2. 大数据量高性能（Redis一秒可以写8万次，读11万次，NoSQL的缓存记录级，是一种细粒度的缓存，性能会比较高！）
3. 数据类型是多样型的！（不需要事先设计数据库，随取随用）
4. 传统的 RDBMS 和 NoSQL

 传统的 RDBMS(关系型数据库)

```java
传统的 RDBMS(关系型数据库)
- 结构化组织
- SQL
- 数据和关系都存在单独的表中 row col
- 操作，数据定义语言
- 严格的一致性
- 基础的事务
- ...
```

```java
Nosql
- 不仅仅是数据
- 没有固定的查询语言
- 键值对存储，列存储，文档存储，图形数据库（社交关系）
- 最终一致性
- CAP定理和BASE
- 高性能，高可用，高扩展
- ...
```

> 了解：3V + 3高

大数据时代的3V ：主要是**描述问题**的

1. 海量Velume
2. 多样Variety
3. 实时Velocity

大数据时代的3高 ： 主要是**对程序的要求**

1. 高并发
2. 高可扩
3. 高性能

真正在公司中的实践：NoSQL + RDBMS 一起使用才是最强的。

## 阿里巴巴演进分析

![1](G:\Typora 文档\Redis图片\2)

![在这里插入图片描述](G:\Typora 文档\Redis图片\3)

```java 
# 商品信息
- 一般存放在关系型数据库：Mysql,阿里巴巴使用的Mysql都是经过内部改动的。

# 商品描述、评论(文字居多)
- 文档型数据库：MongoDB

# 图片
- 分布式文件系统 FastDFS
- 淘宝：TFS
- Google: GFS
- Hadoop: HDFS
- 阿里云: oss

# 商品关键字 用于搜索
- 搜索引擎：solr,elasticsearch
- 阿里：Isearch 多隆

# 商品热门的波段信息
- 内存数据库：Redis，Memcache

# 商品交易，外部支付接口
- 第三方应用
```

## Nosql的四大分类

> KV键值对

新浪：Redis
美团：Redis + Tair
阿里、百度：Redis + Memcache

> 文档型数据库（bson数据格式）：

MongoDB(掌握)

1. 基于分布式文件存储的数据库。C++编写，用于处理大量文档。

2. MongoDB是RDBMS和NoSQL的中间产品。MongoDB是非关系型数据库中功能最丰富的，NoSQL中最像

   关系型数据库的数据库。

ConthDB

> 列存储数据库
>

HBase(大数据必学)
分布式文件系统

> 图关系数据库

| **分类**                | **Examples举例**                                   | **典型应用场景**                                             | **数据模型**                                    | **优点**                                                     | **缺点**                                                     |
| ----------------------- | -------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **键值对（key-value）** | Tokyo Cabinet/Tyrant, Redis, Voldemort, Oracle BDB | 内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等等。 | Key 指向 Value 的键值对，通常用hash table来实现 | 查找速度快                                                   | 数据无结构化，通常只被当作字符串或者二进制数据               |
| **列存储数据库**        | Cassandra, HBase, Riak                             | 分布式的文件系统                                             | 以列簇式存储，将同一列数据存在一起              | 查找速度快，可扩展性强，更容易进行分布式扩展                 | 功能相对局限                                                 |
| **文档型数据库**        | CouchDB, MongoDb                                   | Web应用（与Key-Value类似，Value是结构化的，不同的是数据库能够了解Value的内容） | Key-Value对应的键值对，Value为结构化数据        | 数据结构要求不严格，表结构可变，不需要像关系型数据库一样需要预先定义表结构 | 查询性能不高，而且缺乏统一的查询语法。                       |
| **图形(Graph)数据库**   | Neo4J, InfoGrid, Infinite Graph                    | 社交网络，推荐系统等。专注于构建关系图谱                     | 图结构                                          | 利用图结构相关算法。比如最短路径寻址，N度关系查找等          | 很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群 |

# Redis入门

**Redis （Remote Dictionary Server），远程字典服务**

开源、使用C语言编写，支持网络、基于内存可持久化的日志型，Key-Value数据库，提供多种语言的API，可以用多种语言调用 ，NoSQL技术之一，也被称之为结构化数据库之一

读的速度是11w，写的速度是8w


> Redis能干嘛

- 内存存储，持久化，内存是断电即失的，持久化很重要， 持久化有两种机制（RBD，AOF）
- 效率高，可以用于高速缓存
- 发布订阅系统
- 地图信息分析
- 计数器，（浏览量）
- 。。。

> 特性

- 多样的数据类型
- 持久化
- 集群
- 事务
- 。。

## 基础知识

Redis是单线程的，Redis是基于内存操作的，CPU不是Redis的瓶颈，根据机器的内存和网络带宽的，可以用单线程实现

每秒10w+的QPS（每秒查询率*QPS*），完全不比使用key-value的Memecache差

> 单线程Redis

* 误区1，高性能服务器一定是多线程
* 误区2，多线程一定比单线程效率高
  多线程要涉及CPU的上下文切换
* 核心：Redis是将所有数据全部放到内存中去操作，效率就是高，CPU上下文切换是耗时的操作，对于系统来说没有上下文切换，系统效率就是最高的，多次读写都是在一个cpu上的，在内存情况下效率就是最高的

## Redis五大数据类型

- String
- List
- Set
- Hash
- Zset

> Redis官方介绍

- Redis是内存中的数据结构存储系统，可以作用**数据库、缓存、消息中间MQ(消息队列)**

- 支持多种数据类型
  - String字符串
  - hash散列
  - list列表
  - set 集合
  - sorted sets有序集合
  - bimemaps
  - hyperloglog
  - geospatial

- Redis内置了主从复制replication、LUAscripting脚本，LRU 驱动事件，transactions事务，和不同级别的磁盘持久化persistence
- 通过Redis哨兵Sentinel，和自动分区Cluster，提供高可用性high availability

### Redis-Key

- keys * 查询全部key
- select 3 切换数据库3
- dbsize 查看数据库大小
- flushdb 清空当前库
- flushall 清空所有数据
- exists 判断某个key是否存在
- move 移除key
- expire 设置过期时间，可以用作单点登录
- ttl 查看过期时间
- type 查看key的类型
- config set requirepass XXX 设置密码
  - config set requirepass “” 取消密码
- auth xxx 登录

### String

- append 追加value
  - 追加不存在的key会set key
- strlen 查看value长度
- incr 自增1，可以用作增加浏览量increase
- decr decrease 递减

- incrby 能设置自增量的自增
- getrange 截取范围，下标从0开始:(0,-1表示查看全部)

```
127.0.0.1:6379> set getrange hellossy
OK
127.0.0.1:6379> get getrange
"hellossy"
127.0.0.1:6379> getrange getrange 0 -1
"hellossy"
```

- setrange 修改范围的值

```
127.0.0.1:6379> set k3 hellodw
OK
127.0.0.1:6379> setrange k3 2 xx
(integer) 7
127.0.0.1:6379> get k3
"hexxodw"
```

- setex set wth expire 设置key时顺便设置生存时间

- setnx （ set if not exist ）参数不存在就设置，在分布式锁经常使用，如果存在就创建失败

```
127.0.0.1:6379> setnx cunzai dw
(integer) 1
127.0.0.1:6379> setnx cunzai ssy
(integer) 0
```

- mset 批量设置,空格间隔

```
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3
OK
127.0.0.1:6379> keys *
1) "k3"
2) "k2"
3) "k1"
127.0.0.1:6379>
```



- mget

- msetnx 原子性操作，批量设置(要么一起成功，要么一起失败)

- 对象操作

  ```
  127.0.0.1:6379> mset user:1:name dongwei user:1:age 18
  OK
  127.0.0.1:6379> keys *
  1) "user:1:age"
  2) "user:1:name"
  ```

  

  - 设置一个对象，值为json字符串来保存一个对象
  - user:{id}:{filed}

- getset 先get再set，规则就按getset来，无论key存不存在，都按getset来

```
127.0.0.1:6379> getset db redis
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db dongwei
"redis"
127.0.0.1:6379> get db
"dongwei"
```

### List

在Redis中可以将List作为、栈、队列、阻塞队列

- 实际上是一个链表，left、right 都可以插入值
- 如果key不存在，创建新的链表
- 如果key存在，新增value
- 移除所有值，空链表
- 在两边插入效率最高，对中间元素操作，效率会降低

**所有的List命令都是以L开头的。**

- lpush 左边插入
- lrange 显示list 范围range
- rpush 右边插入
- Rpop right pop
- Lpop left pop
- Llen listlength
- lrem remove指定下标

- ltrim trim截取
- Rpoplpush pop再push到别的list
- lset 设置指定下表值
- exists 存在
- Linsert 将某个具体的value插入到某个元素的前面或后面

```
#=======================================================================

# lpush、lrange、Rpush
127.0.0.1:6379> lpush list one  #插入队列左边
(integer) 1
127.0.0.1:6379> lpush list two
(integer) 2
127.0.0.1:6379> lpush list three
(integer) 3
127.0.0.1:6379> lrange list 0 -1      #获取全部的值从0到-1，这是作为栈的最顶上的为index 0，
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> rpush list Right
(integer) 4
127.0.0.1:6379> lrange list 0 -1   #插入队列右边
1) "three"
2) "two"
3) "one"
4) "Right"



########################################################################

#Rpop、Lpop   移出元素      lindex 获取下标对应值
127.0.0.1:6379> rpop list
"Right"
127.0.0.1:6379> lindex list 2
"one"

########################################################################

#  返回llen 长度\    lrem   移除
127.0.0.1:6379> llen list
(integer) 3


127.0.0.1:6379> lrem list 2 three    #lrem key count value  移除几个，从左边删除的
(integer) 1

########################################################################

#ltrim   截取范围，其余的删除
127.0.0.1:6379> rpush list l1
(integer) 1
127.0.0.1:6379> rpush list l2
(integer) 2
127.0.0.1:6379> rpush list l3
(integer) 3
127.0.0.1:6379> rpush list l4
(integer) 4
127.0.0.1:6379> ltrim list 2 3
OK
127.0.0.1:6379> lrange list 0 -1
1) "l3"
2) "l4"
127.0.0.1:6379>
########################################################################

#rpoplpush  从一个list 的最右边的一个值pop出来push进别的list中
127.0.0.1:6379> rpush list 4 5 6
(integer) 6
127.0.0.1:6379> lrange list 
(error) ERR wrong number of arguments for 'lrange' command
127.0.0.1:6379> lrange list 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
127.0.0.1:6379> rpoplpush list destination
"6"

########################################################################

#linsert   在之前或之后插入
127.0.0.1:6379> linsert list before 3 inserttest  #插入在3之前
(integer) 6
127.0.0.1:6379> lrange list 0 -1
1) "value"
2) "2"
3) "inserttest"
4) "3"
5) "4"
6) "5"
127.0.0.1:6379> linsert list after 4 inserttest2  #插入在4之后
(integer) 7
127.0.0.1:6379> lrange list 0 -1
1) "value"
2) "2"
3) "inserttest"
4) "3"
5) "4"
6) "inserttest2"
7) "5"
127.0.0.1:6379> 

```

### Set

集合中的值不能重复，set是无需不重复原则

string和list的元素都是value，set中是member

- sadd
- smembers 查看集合的全部值
- sismember 判断是否存在
- scard 查看一共有几个元素
- srem 移除某一个member

- srandmember 随机抽取几个member
- spop 随机删除几个member
- smove 移动member到另一个set，可以适用于共同关注功能实现
- sdiff difference set 差集 ，结果来自first_key为基准

- sinter intersection set 交集
- sunion union联合 并集
  - 将用户放到set中，共同关注、共同爱好、推荐好友

```
127.0.0.1:6379> sadd k1 a
(integer) 1
127.0.0.1:6379> sadd k1 b
(integer) 1
127.0.0.1:6379> sadd k1 c
(integer) 1
127.0.0.1:6379> sadd k2 c
(integer) 1
127.0.0.1:6379> sadd k2 d
(integer) 1
127.0.0.1:6379> sadd k2 e
(integer) 1
127.0.0.1:6379> sdiff k1 k2
1) "a"
2) "b"
127.0.0.1:6379> sinter k1 k2
1) "c"
127.0.0.1:6379> sdiff k2 k1
1) "d"
2) "e"
127.0.0.1:6379> sunion k1 k2
1) "b"
2) "e"
3) "c"
4) "a"
5) "d"
```

### Hash

map集合，key - map 本质和string类型没有太大区别，还是一个简单的key - value，像是增加了一层

- hset hset key field value
- hget
- hmset
- hmget
- hmget

- Hmget
- Hdel 删除
- hgetall
- hlen
- hexists 判断hash中指定字段是否存在

- hkeys 获取key所有的field
- hvals 获取所有的value
- hincrby 递增，参数设置为负数就是递减
- hsetnx 是否存在，不存在就创建设置，存在就不设置

### ZSet

在set的基础上增加了一个值

可以作为一个排行榜功能，进场刷新，或者任务等级排序之类的，都可以做

```
127.0.0.1:6379> zadd k1 1
(error) ERR wrong number of arguments for 'zadd' command
127.0.0.1:6379> zadd k1 1 one
(integer) 1
127.0.0.1:6379> zadd k1 2 two 3 three
(integer) 2
127.0.0.1:6379> zrange k1 0 -1
1) "one"
2) "two"
3) "three"
127.0.0.1:6379> zadd salary 2500 xiaoming
(integer) 1
127.0.0.1:6379> zadd salary 5000 xiaohong
(integer) 1
127.0.0.1:6379> zadd salary 6000 xiaozhang
(integer) 1
127.0.0.1:6379> zrangebyscore salary -inf +inf
1) "xiaoming"
2) "xiaohong"
3) "xiaozhang"
127.0.0.1:6379> zrangebyscore salary -inf +inf withscores
1) "xiaoming"
2) "2500"
3) "xiaohong"
4) "5000"
5) "xiaozhang"
6) "6000"
```

![image-20210914112917260](G:\Typora 文档\image-20210914112917260.png)

## 三种特殊结构类型

- geospatial
- hyperloglog
- bitmaps

### geospatial

可以推算地理位置的信息，两地之间的距离，方圆几公里之内的人

需要注意：

- 地球南北两极无法直接添加，
- 一般会下载城市数据，直接通过Java程序一次性导入
- 有效经纬度范围从-180到180，超出范围时会返回错误，
- key由（纬度，经度，名称）构成

查看官网，一共有六个相关命令

![image-20201121151049547](G:\Typora 文档\Redis图片\cc6822bd1dd72a093e473b845255785c.png)

```bash
127.0.0.1:6379> geoadd china:city 116.40 39.90  beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23  shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqin 114.05  22.52 shenzhen
(integer) 2
127.0.0.1:6379> geoadd china:city 120.16 30.24 hangzhou 108.96 34.26 xian
(integer) 2
```

真的被坑死了~~~~~~~~

```bash
127.0.0.1:6379> geopos china:city beijing
1) 1) "116.39999896287918"
   2) "39.900000091670925"
```

```bash
127.0.0.1:6379> geodist china:city shanghai beijing
"1067378.7564"
127.0.0.1:6379> geodist china:city chongqin shanghai
"1447673.6920"
127.0.0.1:6379> geodist china:city shanghai shenzhen
"1215922.3698"
127.0.0.1:6379> geodist china:city shanghai beijing km
"1067.3788"
```

- 我附近的人功能，通过半径来查询，获取附近的人的定位地址

- georadius命令来实现，longitude纬度、latitude经度、radius半径 单位，输入查询位置的经纬度就能从key集合中找到在半径范围的元素

```bash
127.0.0.1:6379> georadius china:city 110 30 10000 km
1) "chongqin"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "shanghai"
6) "beijing"
```

后面跟了几个参数，withdist、withcoord、withhash、asc、desc、count

- Withdist：返回元素位置的同时，把与中心之间相差的距离一同返回
- withcoord ： 将元素经纬度一同返回

- withhash ： 返回经过geohash编码的有序集合分值，主要用于底层调试，作用不大
- asc ： 根据中心的位置，从近到远的方式返回位置元素
- desc：从远到近的方式返回元素

- count ：获取前n个匹配元素，对于提高效率是有效的

```bash
127.0.0.1:6379> georadius china:city 110 30 1000 km withdist withcoord
1) 1) "chongqin"
   2) "341.9374"
   3) 1) "106.49999767541885"
      2) "29.529999579006592"
2) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167"
      2) "34.2599996441893"
3) 1) "shenzhen"
   2) "924.6408"
   3) 1) "114.04999762773514"
      2) "22.520000087950386"
4) 1) "hangzhou"
   2) "977.5143"
   3) 1) "120.16000002622604"
      2) "30.240000322949022"
127.0.0.1:6379> georadius china:city 110 30 1000 km withdist withcoord count 3
1) 1) "chongqin"
   2) "341.9374"
   3) 1) "106.49999767541885"
      2) "29.529999579006592"
2) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167"
      2) "34.2599996441893"
3) 1) "shenzhen"
   2) "924.6408"
   3) 1) "114.04999762773514"
      2) "22.520000087950386"
```

```bash
127.0.0.1:6379> georadiusbymember china:city shanghai 400 km
1) "hangzhou"
2) "shanghai"
```

```bash
127.0.0.1:6379> geohash china:city beijing shanghai  # 把经纬度转换为以为的字符，两个字符串越接近则越近
1) "wx4fbxxfke0"
2) "wtw3sj5zbj0"
```

> GEO底层的实现原理就是Zset

```bash
127.0.0.1:6379> zrange china:city 0 -1
1) "chongqin"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "shanghai"
6) "beijing"
```

```bash
127.0.0.1:6379> zrem china:city shanghai
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "chongqin"
2) "xian"
3) "shenzhen"
4) "hangzhou"
5) "beijing"
```

### hyperloglog

是一种概率数据结构，计数唯一事物，从技术上讲估计一个集合的基数，通常计数唯一项需要使用成比例的内存，因为需要记录使用过的元素，以免多次记录，但是hyperloglog的算法可以用内存换精度，虽然有误差，但是误差小于1%，算法的神奇之处在于只需要很小的内存，最大也不超过12k，类似集合的功能，能记录2^64的计数，从内存的角度来说，hyperloglog是首选


> 能用在网页的UV

- 传统方式，set保存用户的id，用户可能是uuid，这样可能占用巨大的内存
- 但是只是需要计数功能并不需要保存用户的id

```bash
127.0.0.1:6379> pfadd mykey a b c d e f g h i j k
(integer) 1
127.0.0.1:6379> pfcount mykey
(integer) 11
127.0.0.1:6379> pfadd mykey2 a b c x y z a d g e q
(integer) 1
127.0.0.1:6379> pfcount mykey2
(integer) 10
127.0.0.1:6379> pfmerge mykey3 mykey myley2
OK
127.0.0.1:6379> pfcount mykey3
(integer) 11
```

### Bitmaps

统计用户信息，活跃与不活跃，登录未登录只有两种状态的数据，可以使用BitMaps

可以用作打卡功能实现，到达一定数目之后进行统计，判断预期数目与统计得出的数目是否达到预期

使用`bitmap`来记录周一到周日的打卡情况（0表示没有打卡，1表示打卡）

```bash
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 1
(integer) 0
127.0.0.1:6379> setbit sign 5 0
(integer) 0
127.0.0.1:6379> setbit sign 6 0
(integer) 0
```

查看某一天是否有打卡`getbit`

```bash
127.0.0.1:6379> getbit sign 3
(integer) 1
127.0.0.1:6379> getbit sign 6
(integer) 0
```

`bitcount` 统计key offset 为1的个数

```bash
127.0.0.1:6379> bitcount sign
(integer) 3
```

# 事务transition

Redis事务的本质是一组命令的集合，一次执行多个指令，事务中所有命令都被序列化，其他客户端提交的命令请求不会插入到事务执行命令序列中

顺序、排他、一次性

**单条命令是原子性执行的，但事务不保证原子性，且没有回滚，事务中任意命令执行失败，其余命令仍会被执行**

redis执行顺序

- 开启事务（multi）
- 命令入队（。。。）
- 执行事务（exec）
- 取消事务（discard）
- 监视、加锁（watch）
- 取消监视、解锁（unwatch）

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> exec
1) OK
2) OK
3) "v2"
4) OK
```

取消事务

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> discard
OK
127.0.0.1:6379> get k4
(nil)
```

> 编译时异常会导致所有命令都不会成功

```bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> getset k4 v4
QUEUED
127.0.0.1:6379> setget k5 v5
(error) ERR unknown command 'setget'
127.0.0.1:6379> set k6 v6
QUEUED
127.0.0.1:6379> exec
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k3
(nil)
```

> 运行时报错，如果事务队列中存在语法性，那么执行命令的时候，其他命令是可以正常进行的，错误是抛出异常

```bash
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> incr k1
QUEUED
127.0.0.1:6379> set k2 v3
QUEUED
127.0.0.1:6379> get k2
QUEUED
127.0.0.1:6379> exec
1) (error) ERR value is not an integer or out of range
2) OK
3) "v3"
```

- 当有多个线程在操控redis的时候
- 被watch监视的key值如果发生改变，正在进行的事务将会失败
- **每次加锁后都要进行解锁，再加锁去重新获取最新的值**

# Jedis

官方推荐的操作`Redis`的中间件，`SpringBoot`已经有整合`RedisTemplate`

- 导入包
- 建立连接
- 操作
- 可关闭连接，或者直接就使用jedis连接池

自定义模板需要序列化：

问题：

首先我们定义一个pojo类：

其中使用到了lombok是一款在java开发中简洁化代码十分有用的插件工具，这篇博客对较为常用的几种注解进行记录，分享学习心得。

使用lombok注解，目的和作用就在于不用再去写经常反复去写的（如Getter，Setter，Constructor等）一些代码了。

首先，用到的几个注解：

@Data
使用这个注解，就不用再去手写Getter,Setter,equals,canEqual,hasCode,toString等方法了，注解后在编译时会自动加进去。
@AllArgsConstructor
使用后添加一个构造函数，该构造函数含有所有已声明字段属性参数
@NoArgsConstructor
使用后创建一个无参构造函数

```JAVA
@Component
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User  {
    private String name;
    private int age;
}
```

之后我们创建一个test类进行测试：

```java
@Test
    public void test() throws JsonProcessingException {
        User user = new User("dongwei",18);
        redisTemplate.opsForValue().set("user",user);
        System.out.println(redisTemplate.opsForValue().get("user"));
    }
```

这个结果会报错：

```java
org.springframework.data.redis.serializer.SerializationException: Cannot serialize; nested exception is org.springframework.core.serializer.support.SerializationFailedException: Failed to serialize object using DefaultSerializer; nested exception is java.lang.IllegalArgumentException: DefaultSerializer requires a Serializable payload but received an object of type [com.dong.com.dong.pojo.User]

Caused by: org.springframework.core.serializer.support.SerializationFailedException: Failed to serialize object using DefaultSerializer; nested exception is java.lang.IllegalArgumentException: DefaultSerializer requires a Serializable payload but received an object of type [com.dong.com.dong.pojo.User]

Caused by: java.lang.IllegalArgumentException: DefaultSerializer requires a Serializable payload but received an object of type [com.dong.com.dong.pojo.User]
```

就是没有进行序列化：

进行序列化的两种方式：

第一种继承Seriable接口

```java
public class User implements Serializable {
    private String name;
    private int age;
}
```

第二种使用Jason格式进行传输：

```
public void test() throws JsonProcessingException {
        User user = new User("dongwei",18);
        String jsonUser = new ObjectMapper().writeValueAsString(user);//使用json进行传输
        redisTemplate.opsForValue().set("user",user);
        System.out.println(redisTemplate.opsForValue().get("user"));
    }
```

但是由于默认的序列化是GDK，显示效果依旧不好

```mysql
127.0.0.1:6379> keys *
1) "\xac\xed\x00\x05t\x00\x04user"
```

系统默认的序列化:底层还是通过调用JDK的IO操作ObjectInputStream和ObjectOutputStream类实现POJO的序列化和反序列化

```java
	/**
	 * Sets the default serializer to use for this template. All serializers (except the
	 * {@link #setStringSerializer(RedisSerializer)}) are initialized to this value unless explicitly set. Defaults to
	 * {@link JdkSerializationRedisSerializer}.
	 *
	 * @param serializer default serializer to use
	 */
	public void setDefaultSerializer(RedisSerializer<?> serializer) {
		this.defaultSerializer = serializer;//JdkSerializationRedisSerializer
	}
```

下面就开始操作了，自定义一下：



# Redis.conf详解

redis启动的时候就是通过配置文件启动的

## 1.单位

```bash
# Note on units: when memory size is needed, it is possible to specify
# it in the usual form of 1k 5GB 4M and so forth:
#
# 1k => 1000 bytes
# 1kb => 1024 bytes
# 1m => 1000000 bytes
# 1mb => 1024*1024 bytes
# 1g => 1000000000 bytes
# 1gb => 1024*1024*1024 bytes
#
# units are case insensitive so 1GB 1Gb 1gB are all the same.
```

配置文件unit对单位大小写不敏感

## 2.包含

```bash
# Include one or more other config files here.  This is useful if you
# have a standard template that goes to all Redis servers but also need
# to customize a few per-server settings.  Include files can include
# other files, so use this wisely.
#
# Notice option "include" won't be rewritten by command "CONFIG REWRITE"
# from admin or Redis Sentinel. Since Redis always uses the last processed
# line as value of a configuration directive, you'd better put includes
# at the beginning of this file to avoid overwriting config change at runtime.
#
# If instead you are interested in using includes to override configuration
# options, it is better to use include as the last line.
#
# include .\path\to\local.conf
# include c:\path\to\other.conf
```

可以将多个文件进行整合

## 3.网络

```bash
# By default, if no "bind" configuration directive is specified, Redis listens
# for connections from all the network interfaces available on the server.
# It is possible to listen to just one or multiple selected interfaces using
# the "bind" configuration directive, followed by one or more IP addresses.
#
# Examples:
#
# bind 192.168.1.100 10.0.0.1
# bind 127.0.0.1 ::1
#
# ~~~ WARNING ~~~ If the computer running Redis is directly exposed to the
# internet, binding to all the interfaces is dangerous and will expose the
# instance to everybody on the internet. So by default we uncomment the
# following bind directive, that will force Redis to listen only into
# the IPv4 lookback interface address (this means Redis will be able to
# accept connections only from clients running into the same computer it
# is running).
#
# IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES
# JUST COMMENT THE FOLLOWING LINE.
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bind 127.0.0.1 #绑定的ip

# Protected mode is a layer of security protection, in order to avoid that
# Redis instances left open on the internet are accessed and exploited.
#
# When protected mode is on and if:
#
# 1) The server is not binding explicitly to a set of addresses using the
#    "bind" directive.
# 2) No password is configured.
#
# The server only accepts connections from clients connecting from the
# IPv4 and IPv6 loopback addresses 127.0.0.1 and ::1, and from Unix domain
# sockets.
#
# By default protected mode is enabled. You should disable it only if
# you are sure you want clients from other hosts to connect to Redis
# even if no authentication is configured, nor a specific set of interfaces
# are explicitly listed using the "bind" directive.
protected-mode yes #保护模式

# Accept connections on the specified port, default is 6379 (IANA #815344).
# If port 0 is specified Redis will not listen on a TCP socket.
port 6379 #端口设置

```

## 4.通用

```bash
daemonize no  #以守护线程的方式运行，默认是no，我们需要自己开启为yes

pidfile /var/run/redis_6379.pid #如果以后台的方式运行，我们就需要指定一个pid文件

# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably) 生产环境
# warning (only very important / critical messages are logged)
loglevel notice

logfile ""  #日志文件的位置名

databases 16  #数据库的数量，默认是16个

always-show-loho yes  #是否总是显示logo
```

## 5.快照 rdb操作设置

持久化，在规定的时间内，执行了多少次操作，则会持久化到文件中有两种文件格式：① .rdb ②.aof

redis是内存数据库，如果没有持久化，那么数据断电即失。



```bash
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""

save 900 1 #如果在900s内，，如果至少有一个1 key进行了修改，我们就进行持久化操作
save 300 10
save 60 10000


stop-writes-on-bgsave-error yes #持久化如果出错，是否还需要继续工作

# Compress string objects using LZF when dump .rdb databases?
# For default that's set to 'yes' as it's almost always a win.
# If you want to save some CPU in the saving child set it to 'no' but
# the dataset will likely be bigger if you have compressible values or keys.

rdbcompression yes #是否压缩rdb文件，需要消耗一些cpu资源


rdbchecksum yes  #保存rdb文件的时候，进行错误的检查校验

dir ./ # rdb文件保存的目录

```



## 6.安全

```bash
# Require clients to issue AUTH <PASSWORD> before processing any other
# commands.  This might be useful in environments in which you do not trust
# others with access to the host running redis-server.
#
# This should stay commented out for backward compatibility and because most
# people do not need auth (e.g. they run their own servers).
#
# Warning: since Redis is pretty fast an outside user can try up to
# 150k passwords per second against a good box. This means that you should
# use a very strong password otherwise it will be very easy to break.
#
# 
requirepass foobared #可以设置密码 默认是没有的

```

常用的设置密码的方式

```bash
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) ""
127.0.0.1:6379> config set requirepass "123456"
```

## 7.限制



```bash
maxclients 10000 #设置能连接的redis的最大客户端的数量

maxmemory <bytes> #redis配置最大的内存容量

# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select among five behaviors:
#
# volatile-lru -> remove the key with an expire set using an LRU algorithm
# allkeys-lru -> remove any key according to the LRU algorithm
# volatile-random -> remove a random key with an expire set
# allkeys-random -> remove a random key, any key
# volatile-ttl -> remove the key with the nearest expire time (minor TTL)
# noeviction -> don't expire at all, just return an error on write operations

maxmemory-policy noeviction #内存达到上限之后的处理策略
1、volatile-lru：只对设置了过期时间的key进行LRU（默认值）
2、allkeys-lru ： 删除lru算法的key
3、volatile-random：随机删除即将过期key
4、allkeys-random：随机删除
5、volatile-ttl ： 删除即将过期的
6、noeviction ： 永不过期，返回错误

```



## 8.APPEND ONLY模式 aof配置



```bash
appendonly no  #默认是不开启aof模式的，默认是使用rdb方式持久化的，在大部分所有情况下，rdb完全够用！

# The name of the append only file (default: "appendonly.aof")
appendfilename "appendonly.aof"  #持久化的文件的名字

# appendfsync always  #每次修改都会sync，消耗性能
appendfsync everysec  #每秒执行一次，sync，可能会丢失这1s的数据
# appendfsync no      #不执行sync，这个时候操作系统自己同步数据，速度最快
```



# Redis持久化

Redis是内存数据库，如果不讲内存中的数据库状态保存到磁盘中，那么一旦出现服务器进程退出，服务器中的数据库状态也会消失。所以Redis提供了持久化操作。

## RDB（Redis DataBase）

RDB持久化是指在指定的时间间隔内将内存中的数据集快照写入磁盘，实际操作过程是fork一个子进程，先将数据集写入临时文件，写入成功后，再替换之前的文件，用二进制压缩存储。

![img](http://img1.tuicool.com/NjYjYvF.png!web?_=6182478)

在指定的时间间隔内将内存中的数据集快照写入磁盘，也就是行话说的Snapshot快照，它恢复时是将快照文件直接读到内存里面。

> ①、优势
>
> 　　1.RDB是一个非常紧凑(compact)的文件，它保存了redis 在某个时间点上的数据集。这种文件非常适合用于进行备份和灾难恢复。
>
> 　　2.生成RDB文件的时候，redis主进程会fork()一个子进程来处理所有保存工作，主进程不需要进行任何磁盘IO操作。
>
> 　　3.RDB 在恢复大数据集时的速度比 AOF 的恢复速度要快。
>
> ②、劣势
>
> 　　1、RDB方式数据没办法做到实时持久化/秒级持久化。因为bgsave每次运行都要执行fork操作创建子进程，属于重量级操作，如果不采用压缩算法(内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑)，频繁执行成本过高(影响性能)
>
> 　　2、RDB文件使用特定二进制格式保存，Redis版本演进过程中有多个格式的RDB版本，存在老版本Redis服务无法兼容新版RDB格式的问题(版本不兼容)
>
> 　　3、在一定间隔时间做一次备份，所以如果redis意外down掉的话，就会丢失最后一次快照后的所有修改(数据有丢失)





> rdb触发机制

* 当快照满足下面条件的时候

```bash
save 900 1 #如果在900s内，，如果至少有一个1 key进行了修改，我们就进行持久化操作
save 300 10
save 60 10000
```

* 执行flushall命令
* 退出redis的时候

> 优点：

1. 适合大规模的数据恢复，只有有dump.rdb文件就可以直接恢复
2. 对数据的完整性要求不高

> 缺点

1. 需要一定的时间间隔进行操作，如果redis意外宕机之后，这个最后一次修改数据就没有了、
2. fork进程的时候，会占用一定的内存空间。

## AOF（Append Only File）

以日志的形式去记 录每个写操作，将Redis执行过的所有指令记录下来（读操作不记录），值追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

```bash
# By default Redis asynchronously dumps the dataset on disk. This mode is
# good enough in many applications, but an issue with the Redis process or
# a power outage may result into a few minutes of writes lost (depending on
# the configured save points).
#
# The Append Only File is an alternative persistence mode that provides
# much better durability. For instance using the default data fsync policy
# (see later in the config file) Redis can lose just one second of writes in a
# dramatic event like a server power outage, or a single write if something
# wrong with the Redis process itself happens, but the operating system is
# still running correctly.
#
# AOF and RDB persistence can be enabled at the same time without problems.
# If the AOF is enabled on startup Redis will load the AOF, that is the file
# with the better durability guarantees.
#
# Please check http://redis.io/topics/persistence for more information.

appendonly no

# The name of the append only file (default: "appendonly.aof")
appendfilename "appendonly.aof"

# The fsync() call tells the Operating System to actually write data on disk
# instead of waiting for more data in the output buffer. Some OS will really flush
# data on disk, some other OS will just try to do it ASAP.
#
# Redis supports three different modes:
#
# no: don't fsync, just let the OS flush the data when it wants. Faster.
# always: fsync after every write to the append only log. Slow, Safest.
# everysec: fsync only one time every second. Compromise.
#
# The default is "everysec", as that's usually the right compromise between
# speed and data safety. It's up to you to understand if you can relax this to
# "no" that will let the operating system flush the output buffer when
# it wants, for better performances (but if you can live with the idea of
# some data loss consider the default persistence mode that's snapshotting),
# or on the contrary, use "always" that's very slow but a bit safer than
# everysec.
#
# More details please check the following article:
# http://antirez.com/post/redis-persistence-demystified.html
#
# If unsure, use "everysec".

# appendfsync always
appendfsync everysec
# appendfsync no

# When the AOF fsync policy is set to always or everysec, and a background
# saving process (a background save or AOF log background rewriting) is
# performing a lot of I/O against the disk, in some Linux configurations
# Redis may block too long on the fsync() call. Note that there is no fix for
# this currently, as even performing fsync in a different thread will block
# our synchronous write(2) call.
#
# In order to mitigate this problem it's possible to use the following option
# that will prevent fsync() from being called in the main process while a
# BGSAVE or BGREWRITEAOF is in progress.
#
# This means that while another child is saving, the durability of Redis is
# the same as "appendfsync none". In practical terms, this means that it is
# possible to lose up to 30 seconds of log in the worst scenario (with the
# default Linux settings).
#
# If you have latency problems turn this to "yes". Otherwise leave it as
# "no" that is the safest pick from the point of view of durability.
no-appendfsync-on-rewrite no

# Automatic rewrite of the append only file.
# Redis is able to automatically rewrite the log file implicitly calling
# BGREWRITEAOF when the AOF log size grows by the specified percentage.
#
# This is how it works: Redis remembers the size of the AOF file after the
# latest rewrite (if no rewrite has happened since the restart, the size of
# the AOF at startup is used).
#
# This base size is compared to the current size. If the current size is
# bigger than the specified percentage, the rewrite is triggered. Also
# you need to specify a minimal size for the AOF file to be rewritten, this
# is useful to avoid rewriting the AOF file even if the percentage increase
# is reached but it is still pretty small.
#
# Specify a percentage of zero in order to disable the automatic AOF
# rewrite feature.

auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb   #如果aof文件大于64M，就会fork一个新的进程来将我们的文件进行重写

# An AOF file may be found to be truncated at the end during the Redis
# startup process, when the AOF data gets loaded back into memory.
# This may happen when the system where Redis is running
# crashes, especially when an ext4 filesystem is mounted without the
# data=ordered option (however this can't happen when Redis itself
# crashes or aborts but the operating system still works correctly).
#
# Redis can either exit with an error when this happens, or load as much
# data as possible (the default now) and start if the AOF file is found
# to be truncated at the end. The following option controls this behavior.
#
# If aof-load-truncated is set to yes, a truncated AOF file is loaded and
# the Redis server starts emitting a log to inform the user of the event.
# Otherwise if the option is set to no, the server aborts with an error
# and refuses to start. When the option is set to no, the user requires
# to fix the AOF file using the "redis-check-aof" utility before to restart
# the server.
#
# Note that if the AOF file will be found to be corrupted in the middle
# the server will still exit with an error. This option only applies when
# Redis will try to read more data from the AOF file but not enough bytes
# will be found.
aof-load-truncated yes
```

redis中默认是不开启的，我们需要手动进行配置，只需要将appendonly改为yes就直接可以开启了。

如果这个rof文件是有错位的，这个时候redis是启动不起来的，我们需要修复这个aof文件，redis提供了一个工具**`'redis-check-aof --fix'`**

> 优点

1. 每一次修改都同步，文件的完整性会更好
2. 每秒同步一次，可能会丢失一秒的数据
3. 从不同步效率更高

> 缺点

1. 相对数据文件来说，AOF远远大于RDB，修复的速度也要比RDB慢
2. AOF运行效率也要比RDB慢，所以我们redis默认配置就是RDB持久化



## 扩展

1. RDB持久化方式能够在指定的时间间隔内对你的数据进行快照存储
2. AOF持久化方式记录每次对服务器的写操作，当服务器重启的时候执行这些命令来恢复原始数据，AOF命令以Redis协议追加保存每次写的操作到文件末尾，redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。
3. 只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化。
4. 同时开启两种持久化方式
   * 在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集完整。
   * RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只是用AOF文件呢，作者建议是不要的，因为RDB更适合用于数据库备份（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，留作为一个万一的手段。
5. 性能建议
   * 因为RDB文件只用作后备用途，建议在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留 `save 900 1`这条规则。
   * 如果Enable AOF，好处是在最恶劣的环境情况下只会丢失不超过两秒的数据，启动脚本简单只load自己的AOF文件就可以了，代价一是带来持续的IO，二是AOF rewrite的最后将rewrite过程中产生的新数据写到新文件造成的堵塞几乎是不可能避免的，只要硬盘允许，应该尽可能减少AOF rewrite的频率，AOF重写的基础默认大小是64M，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。
   * 如果不Enable AOF，仅靠Master-Slave Repllcation实现高可用性也是可以的，能够省略掉一大笔IO，也减少了rewrite时带来的系统波动，代价是如果Master/Slave同时倒掉，也会丢失十几分钟的数据，启动脚本也要比较两个Master/Slave中的RDB文件，载入比较新的这种，微博目前使用的就是这种架构。



# Redis发布订阅

订阅发布消息图



![image-20210928084708277](G:\Typora 文档\Redis图片\image-20210928084708277.png)

下面展示了频道channel 1，以及订阅这个频道的三个客户端------client2  client5和client1之间的关系

![image-20210928085321618](G:\Typora 文档\image-20210928085321618.png)

当有新消息通过PUBLISH命令发送给频道channel 1时候，这个消息会被送给订阅它的三个客户端

![image-20210928085425443](G:\Typora 文档\image-20210928085425443.png)



> 命令

这些命令被广泛应用于构建及时通信，比如网络聊天和实时广播、实时提醒等

![image-20210928085628749](G:\Typora 文档\image-20210928085628749.png)

订阅端

```bash
127.0.0.1:6379> subscribe dongwei
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "dongwei"
3) (integer) 1
1) "message"
2) "dongwei"
3) "ssy"
```

发布端

```bash
127.0.0.1:6379> publish dongwei ssy
(integer) 1
```

>  原理

Redis是使用C实现的，通过分析Redis源码中的pubsub.c文件，了解发布和订阅机制的底层实现，加深对Redis的了解。

Redis通过PUBLISH、SUBSCRIBE和PSUBSCRIBE等命令实现发布和订阅功能。

通过SUBSCRIBE命令订阅某个频道后，Redis-server里维护了一个字典，字典的键就是一个个channel，而字典的值则是一个链表，链表保存了所有订阅这个channel的客户端，SUBSCRIBE命令的关键，就是将客户端添加到给定channel的订阅链表中。

通过PUBLISH命令向订阅者发送消息，redis-server会使用给定的频道作为键，在它所维护的channel字典中查找记录了订阅这个频道所有客户端的链表，遍历这个链表，将消息发布给所有订阅者。

pub/sub从字面上理解就是发布和订阅，在redis中，你可以设定对某一个key值进行消息发布及订阅，当一个key值上进行了消息发布后，所哟订阅它的客户端都会收到相应的消息，这一功能最明显的用法就是用作实时消息系统，比如普通的即时聊天，群聊功能等。

# Redis主从复制

> 概念

主从复制，是指一台redis服务器的数据，复制到其他的redis服务器，前者称为主节点（master/leader），后者称为从节点（slave/follower）；数据的复制都是单向的，只能从主节点到从节点，Master以写为主，Slave以读为主。

默认情况下，每台Redis服务器都是主节点，且一个主节点可以有多个从节点（或者没有从节点），但一个从节点只能有一个主节点。

> 主从复制的作用主要包括一下几个：

1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式
2. 故障恢复：当主节点出现问题之后，同时可以从节点提供服务，实现快速的故障恢复；实际上是一种服务冗余。
3. 负载均衡：在主从复制的基础上，配合读写分离，可以由主节点一共写服务，由从节点提供读服务，分担服务器负载；尤其是在写少读多的场景下，通过多个节点分担读负载，可以大大提升Redis服务器的并发量。
4. 高可用基石：除了上述作用之外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础。

**一般来说，要将Redis运用于工程项目中，只使用一台Redis是万万不能的（宕机）原因如下：**

1. 从结构上来说，单个Redis服务器会发生单点故障，并且一台服务器需要处理所有的请求负载，压力较大。
2. 从容量上讲：单个Redis服务器内存容量是有限的，就算一台Redis服务器内存容量是256G，也不能将虽有内存用作Redis存储内存，一般来说，单台Redis最大使用内存应该不超过20G。

## 环境配置

查看当前库的信息

```bash
127.0.0.1:6379> info replication
# Replication
role:master   #主机
connected_slaves:0
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

修改配置文件

1. 端口号修改
2. pid文件名称修改
3. Log文件名称修改
4. dump.rdb文件名称修改

## 一主二从

默认情况下，每台Redis服务器都是主节点：我们一般情况下只用配置从机就好了。

```bash
SLAVEOF HOST
```

## 细节

主机可以写，从机不能写只能读！主机中的所有信息和数据，都会自动被从机保存

如果是使用命令行来设置主从配置的话，这个时候从机如果重启了，就会变成主机，只要变为主机，立马就会从主机中获取值

> 复制原理 

master接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕后，master将传送整个数据文件到slave，并完成一次完全同步。

全量复制：slave服务在接收到数据库文件数据后，将其存盘并加载带内存中。

增量复制：Master继续将新的所有收集到的修改命令一次传送给slave，完成同步。

但是只要重新连接master，一次完全同步（全量复制）就会自动执行，



当主机挂掉，下面小弟如何篡位：

```bash
slaveof no one
```



## 哨兵（Sentinel）

Sentinel 其实也是一个 redis 的服务端程序，它也会定时执行 serverCron 函数，只是里面其他的程序用不到，用到的是对普通 redis 节点的监控以及故障转移模块。

Sentinel 初始化的时候会清空原来的命令表，写入自己独有的命令进去，所以普通 redis 节点支持的数据读写命令，对 Sentinel 来说都是找不到命令，因为它根本就没有初始化这些命令的执行器。

Sentinel 会定时的对自己监控的 master 执行 info 命令，获取最新的主从关系，还会定时的给所有的 redis 节点发送 ping 心跳检测命令，如果检测到某个 master 无法响应了，就会在给其他 Sentinel 发送消息，主观认为该 master 宕机，如果 Sentinel 集群认同该 master 下线的人数达到一个值，那么大家统一意见，下线该 master。

下线之前需要做的是找 Sentinel 集群中的某一个来执行下线操作，这个步骤叫领导者选举，选出来以后会从该 master 所有的 slave 节点中挑一个合适的作为新的 master，并让其他 slave 重新同步新的 master。

其实以上我们就简单的介绍了 Sentinel 是什么，本质上做了哪些事情，等下我们会结合源码细说其中的细节实现。这里我们再看下，如何配置并启动一个 Sentinel 监控。（生产环境建议配置大于三个）



# Redis缓存击穿和雪崩

> 问题

Redis缓存的使用，极大的提升了应用程序的性能和效率，特别是数据查询方面，但是同时，它也带来了一些问题，其中，最要害的问题，就是数据的一致性问题，从严格意义上讲，这个问题无解。如果对数据的一致性要求很高，那么久不能使用缓存。

还有一些经典的问题就是，缓存穿透，缓存雪崩和缓存击穿，目前业界都有比较主流的解决方案。

## 缓存穿透（查不到）

> 概念

缓存穿透的概念很简单，用户想要查询一个数据，发现redis内存数据库没有，也就是缓存没有命中，于是向持久层数据库查询。发现也没有，于是本次查询失败，当用户很多的时候，缓存都没有命中，于是都去请求持久层数据库，这会给数据库造成很大的压力，这时候就相当于出现了缓存穿透。

> 解决方案

**布隆过滤器**

布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃，从而避免了对底层存储系统的查询压力

![image-20210928215300786](G:\Typora 文档\Redis图片\image-20210928215300786.png)



> 缓存空对象

当存储层中不命中后，及时返回的空对象也将其缓存起来，同时设置一个过期时间，之后再访问这个数据将会从环迅中获取，保护了后端数据源

![image-20210928215607836](G:\Typora 文档\image-20210928215607836.png)

**但是这种方法会存在两个问题**

1. 如果空值被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键
2. 即时对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务可能受影响。



## 缓存击穿（差太多）

这里需要注意和缓存击穿的区别，缓存击穿，是指一个key非常热点，在不断的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就击破缓存，直接请求数据库，就像在一个屏障上凿开一个洞。

当某个key在过期瞬间，有大量的请求并发访问，这类数据一般是热点数据，由于缓存过期，会同时访问数据库来查询最新数据，并且会写缓存，会导致数据库瞬间压力过大。

> 解决方案

**设置热点数据永不过期**

从缓存层面来看，没有设置过期时间，所以不会出现热点key过期产生的问题

**加互斥锁**

分布式锁：使用分布式锁，保证对于每个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可，这种方式将高并发的压力转移到分布式锁，因此对分布式锁的考验很大。

## 缓存雪崩

缓存雪崩，是指在某一事件段，缓存集中过期失效，Redis宕机

产生雪崩的原因之一，比如在写文本的时候，马上就要到双十二零点了，很快就会迎来一波抢购，这波商品时间比较集中的放入缓存，假设缓存一个小时，那么到了凌晨一点钟的时候，这批商品的缓存就过期了。而对于这批药品的访问查询，都落到了数据库上，对于数据库而言，就会产生周期性的压力波峰。于是所有的请求都会达到存储层，存储层的调用会暴增，造成存储层也会挂掉的情况。

![image-20210929085727247](G:\Typora 文档\Redis图片\image-20210929085727247.png)



其实集中过期，倒不是非常知名的，比较知名的是缓存雪崩，是缓存服务器某个节点宕机或者断网，因为自然形成的缓存雪崩，一定是在某个时间段集中创建缓冲，这个时候，数据库也是可以顶住压力的，无非就是对数据库周期性的压力而已，而缓存服务节点的宕机，对数据库服务器造成的压力是不可预知的，很可能瞬间就把数据压垮。

> 解决方案

**Redis高可用**

这个思想的含义是，既然redis有可能挂掉，那么我多增设几台redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建的集群

**限流降级**

这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量，比如对某一个key只允许一个线程查询数据，其他线程等待

**数据预热**

数据加热的含义就是在正式部署之前，我可先把可能的数据先预先访问一遍，这样部分可能大量的访问数据就会加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间尽量均匀一些。













