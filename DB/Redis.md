# Redis

## Redis常见面试题
- [redis 简介](Redis.md#Redis简介)
- [为什么要用 redis/为什么要用缓存]()
- [为什么要用 redis 而不用 map/guava 做缓存?]()
- [redis 和 memcached 的区别]()
- [redis 常见数据结构以及使用场景分析]()
    - [String]()
    - [Hash]()
    - [List]()
    - [Set]()
    - [Sorted Set]()
- [redis 设置过期时间]()
- [redis 内存淘汰机制(MySQL里有2000w数据，Redis中只存20w的数据，如何保证Redis中的数据都是热点数据?)]()
- [redis 持久化机制(怎么保证 redis 挂掉之后再重启数据可以进行恢复)]()
- [redis 事务]()
- [缓存雪崩和缓存穿透问题解决方案]()
- [如何解决 Redis 的并发竞争 Key 问题]()
- [如何保证缓存与数据库双写时的数据一致性?]()


## Redis简介
> 本质上是一个key-value 的NoSQL非关系型数据库，内存数据库，整个数据库系统加载在内存当中进行操作，定期通过异步操作把数据库数据flush到硬盘上进行保存。

最新版本： 

    Redis 5.0.5     Released Wed May 15 17:57:41 CEST 2019

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 

它支持多种类型的数据结构，如 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询。 

Redis 内置了 复制（replication），LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction），事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel）和自动 分区（Cluster）提供高可用性（high availability）。

## 安装Redis

> 默认端口6379

- [Linux下安装Redis](Redis-InstallL.md)

- [Windows下安装Redis](Redis-InstallW.md)

## 1. 优点和缺点

- 优点：
    - 纯内存操作，性能快。
    - 支持多种数据结构，单个value的最大限制是1GB
    - 支持事务，操作都是原子性的（对数据的更改要不全部执行，要不全部不执行）
    - 丰富的特性，可用于缓存，消息，按Key设置过期时间，过期自动删除。

- 缺点：
    - 数据库容量收物理内存限制
    - 不能用作海量数据的高性能读写

## 2. Redis的数据类型

数据类型： 字符串（String）， 列表（List）， 集合（Set），有序集合（Sorted Set） ,散列（Hashes） 。

这些数据类型都支持push/pop、add/remove及取交集并集和差集及更丰富的操作。

- 字符串 String 
    - set
    - get
    - decr
    - incr
    - mget 
- 列表 List
    - hget
    - hset
    - hgetall
- 集合 Set 
    - lpush
    - rpush
    - lpop
    - rpop
    - lrange
- 有序集合 Sorted Set
    - sadd
    - spop
    - smembers
    - sunion 
- 散列 Hashes 
    - zadd
    - zrange
    - zrem
    - zcard

- 设置过期时间

    语法：redis.expire(key, expiration)
    
## 3. 命令
- Key 键
- String 字符串
- Hash 哈希表
- List 列表
- Set 集合
- SortedSet 有序集合
- Pub/Sub 发布/订阅
- Transaction 事务
- Script 脚本
- Connection 连接
- Server 服务器

## 4. Redis数据淘汰机制

在redis中允许用户设置最大使用内存大小server.maxmemory。内存大小有限，需要保存有效的数据。

redis内存数据集大小上升到一定大小时，就会实施数据淘汰策略/回收策略。

Redis提供了6中数据淘汰策略。
1. volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰。
2. volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰 。
3. volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰 。
4. allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰 。
5. allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰 。
6. no-enviction（驱逐）：禁止驱逐数据。

## 拓展
- [Redis 的常见应用场景](Redis-Context-of-Use.md)
- [Redis 的并发竞争问题](Redis-Concurrent.md)
- [Redis 的存储策略/存储机制/持久化](Redis-Persistence.md)
- [Redis 的哨兵机制](Redis-Sentinel.md)
- [Redis 的集群部署](Redis-Cluster.md)
- [RedLock 分布式锁](Redis-RedLock.md)
- [Java使用RedisTemplate](Redis-Template.md)
