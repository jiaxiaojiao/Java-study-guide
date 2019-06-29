# Redis

## Redis常见面试题
- [redis 简介](#Redis简介)
- [为什么要用 redis/为什么要用缓存](#为什么要用Redis)
- [为什么要用 redis 而不用 map/guava 做缓存?](#为什么不用map做缓存)
- [redis 和 memcached 的区别](#Redis和Memcached的区别)
- [redis 常见数据结构以及使用场景分析](#Redis的数据类型)
    - String
    - Hash
    - List
    - Set
    - Sorted Set
- [redis 设置过期时间](#Redis设置过期时间)
- [redis 内存淘汰机制(MySQL里有2000w数据，Redis中只存20w的数据，如何保证Redis中的数据都是热点数据?)](#Redis数据淘汰机制)
- [redis 持久化机制/存储策略(怎么保证 redis 挂掉之后再重启数据可以进行恢复)](Redis-Persistence.md)
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

## 为什么要用Redis

**高性能**：从数据库访问数据比较慢（硬盘读取的），将数据存在Redis缓存中，下一次直接访问缓存，速度很快，因为是内存操作。如果数据库数据出现改变，同步改变缓存中响应的数据。

**高并发**：Redis缓存能承受的并发请求远大于数据库，减轻数据库压力。

## 为什么不用map做缓存

缓存分为本地缓存和分布式缓存。

本地缓存： 以Java为例，使用自带的 map 或者 guava 实现的是本地缓存。
    - 轻量以及快速，但数据存储有限。
    - 生命周期随着 jvm 的销毁而结束
    - 在多实例的情况下，每个实例都需要各自保存一份缓存，缓存不具有一致性。

分布式缓存： 使用 redis 或 memcached 之类的称为分布式缓存，
    - 持久化
    - 过期机制
    - 在多实例的情况下，各实例共用一份缓存数据，缓存具有一致性。
    - 缺点是需要保持 redis 或 memcached服务的高可用，整个程序架构上较为复杂

## Redis和Memcached的区别

1. 数据类型
    - Redis支持丰富的数据类型： 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets）
    - Memcached支持简单的数据类型： 文本型（String）、二进制类型（新版本加的）

2. 持久化
    - Redis支持持久化： 可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用。
    - Memcached支持持久化： 把数据全部存在内存中。

3. 集群模式
    - Redis原生集群： Redis cluster模式
    - Memcached没有原生集群模式。

4. 网络IO模型
    - Redis 单线程，多路IO复用模型。 
    - Memcached 多线程，非阻塞IO复用的网络模型。


## Redis的优点和缺点

- 优点：
    - 纯内存操作，性能快。
    - 支持多种数据结构，单个value的最大限制是1GB
    - 支持事务，操作都是原子性的（对数据的更改要不全部执行，要不全部不执行）
    - 丰富的特性，可用于缓存，消息，按Key设置过期时间，过期自动删除。

- 缺点：
    - 数据库容量收物理内存限制
    - 不能用作海量数据的高性能读写

## Redis的数据类型

数据类型： 字符串（String）， 列表（List）， 集合（Set），有序集合（Sorted Set） ,散列（Hashes） 。

这些数据类型都支持push/pop、add/remove及取交集并集和差集及更丰富的操作。

- 字符串 String 
    - 常用命令： set, get, decr, incr, mget
    - 描述： 常规key-value类型，value可以是String或数字。
    - 场景： 常规key-value缓存、微博数、粉丝数等。
    
- 散列 Hash
    - 常用命令： hget, hset, hgetall
    - 描述： 是一个String类型的field和value的映射表
    - 场景： 适合存储对象
    
- 列表 List 
    - 常用命令： lpush, rpush, lpop, rpop, lrange
    - 描述： 链表。可以实现双向链表，支持反向查找和遍历。
    - 场景： 微博关注列表，粉丝列表，消息列表，lrange命令实现分页查询。
    
- 集合 Set
    - 常用命令： sadd, spop, smembers, sunion
    - 描述： 类似于List的列表，特殊之处在于Set可以自动排序。可以基于 set 轻易实现交集、并集、差集的操作。
    - 场景： 微博关注人和粉丝，可以方便实现共同关注 共同粉丝等功能。

- 有序集合 Sorted Set ZSet
    - 常用命令： zadd, zrange, zrem, zcard
    - 描述： 和set相比，sorted set增加了一个权重参数score，使得集合中的元素能够按score进行有序排列。
    - 场景： 在直播系统中，实时排行信息包含直播间在线用户列表，各种礼物排行榜，弹幕消息（可以理解为按消息维度的消息排行榜）等信息，适合使用 Redis 中的 Sorted Set 结构进行存储。



### Redis设置过期时间
语法：redis.expire(key, expiration)

set key 的时候，都可以给一个 expire time。

Redis是怎么删除已经设置过期时间的Key的？
- 定期删除： redis默认是每隔 100ms 就**随机抽取**一些设置了过期时间的key，检查其是否过期，如果过期就删除。注意这里是随机抽取的。
- 惰性删除： 定期删除可能会导致很多过期 key 到了时间并没有被删除掉。所以就有了惰性删除。假如你的过期 key，靠定期删除没有被删除掉，还停留在内存里，除非你的系统去查一下那个 key，才会被redis给删除掉。


如果定期删除漏掉了很多过期 key，然后你也没及时去查，也就没走惰性删除，此时会怎么样？如果大量过期key堆积在内存里，导致redis内存块耗尽了。怎么解决这个问题呢？ **redis 内存淘汰机制。**

## Redis数据淘汰机制

在redis中允许用户设置最大使用内存大小server.maxmemory。内存大小有限，需要保存有效的数据。

redis内存数据集大小上升到一定大小时，就会实施数据淘汰策略/回收策略。

Redis提供了6中数据淘汰策略。
1. volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰。
2. volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰 。
3. volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰 。
4. allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰 。
5. allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰 。
6. no-enviction（驱逐）：禁止驱逐数据。

4.0版本后增加以下两种：

7. **volatile-lfu**：从已设置过期时间的数据集(server.db[i].expires)中挑选最不经常使用的数据淘汰
8. **allkeys-lfu**：当内存不足以容纳新写入数据时，在键空间中，移除最不经常使用的key


## 拓展
- [Redis 的常见应用场景](Redis-Context-of-Use.md)
- [Redis 的并发竞争问题](Redis-Concurrent.md)
- [Redis 的哨兵机制](Redis-Sentinel.md)
- [Redis 的集群部署](Redis-Cluster.md)
- [RedLock 分布式锁](Redis-RedLock.md)
- [Java使用RedisTemplate](Redis-Template.md)



---

未完

### redis 事务

Redis 通过 MULTI、EXEC、WATCH 等命令来实现事务(transaction)功能。事务提供了一种将多个命令请求打包，然后一次性、按顺序地执行多个命令的机制，并且在事务执行期间，服务器不会中断事务而改去执行其他客户端的命令请求，它会将事务中的所有命令都执行完毕，然后才去处理其他客户端的命令请求。

在传统的关系式数据库中，常常用 ACID 性质来检验事务功能的可靠性和安全性。在 Redis 中，事务总是具有原子性（Atomicity）、一致性（Consistency）和隔离性（Isolation），并且当 Redis 运行在某种特定的持久化模式下时，事务也具有持久性（Durability）。

### 缓存雪崩和缓存穿透问题解决方案

**缓存雪崩** 

简介：缓存同一时间大面积的失效，所以，后面的请求都会落到数据库上，造成数据库短时间内承受大量请求而崩掉。

解决办法（中华石杉老师在他的视频中提到过，视频地址在最后一个问题中有提到）：

- 事前：尽量保证整个 redis 集群的高可用性，发现机器宕机尽快补上。选择合适的内存淘汰策略。
- 事中：本地ehcache缓存 + hystrix限流&降级，避免MySQL崩掉
- 事后：利用 redis 持久化机制保存的数据尽快恢复缓存

![](http://my-blog-to-use.oss-cn-beijing.aliyuncs.com/18-9-25/6078367.jpg)


**缓存穿透** 

简介：一般是黑客故意去请求缓存中不存在的数据，导致所有的请求都落到数据库上，造成数据库短时间内承受大量请求而崩掉。

解决办法： 有很多种方法可以有效地解决缓存穿透问题，最常见的则是采用布隆过滤器，将所有可能存在的数据哈希到一个足够大的bitmap中，一个一定不存在的数据会被 这个bitmap拦截掉，从而避免了对底层存储系统的查询压力。另外也有一个更为简单粗暴的方法（我们采用的就是这种），如果一个查询返回的数据为空（不管是数 据不存在，还是系统故障），我们仍然把这个空结果进行缓存，但它的过期时间会很短，最长不超过五分钟。

参考：

- [https://blog.csdn.net/zeb_perfect/article/details/54135506](https://blog.csdn.net/zeb_perfect/article/details/54135506)

### 如何解决 Redis 的并发竞争 Key 问题

所谓 Redis 的并发竞争 Key 的问题也就是多个系统同时对一个 key 进行操作，但是最后执行的顺序和我们期望的顺序不同，这样也就导致了结果的不同！

推荐一种方案：分布式锁（zookeeper 和 redis 都可以实现分布式锁）。（如果不存在 Redis 的并发竞争 Key 问题，不要使用分布式锁，这样会影响性能）

基于zookeeper临时有序节点可以实现的分布式锁。大致思想为：每个客户端对某个方法加锁时，在zookeeper上的与该方法对应的指定节点的目录下，生成一个唯一的瞬时有序节点。 判断是否获取锁的方式很简单，只需要判断有序节点中序号最小的一个。 当释放锁的时候，只需将这个瞬时节点删除即可。同时，其可以避免服务宕机导致的锁无法释放，而产生的死锁问题。完成业务流程后，删除对应的子节点释放锁。

在实践中，当然是从以可靠性为主。所以首推Zookeeper。

参考：

- https://www.jianshu.com/p/8bddd381de06

### 如何保证缓存与数据库双写时的数据一致性?

你只要用缓存，就可能会涉及到缓存与数据库双存储双写，你只要是双写，就一定会有数据一致性的问题，那么你如何解决一致性问题？

一般来说，就是如果你的系统不是严格要求缓存+数据库必须一致性的话，缓存可以稍微的跟数据库偶尔有不一致的情况，最好不要做这个方案，读请求和写请求串行化，串到一个内存队列里去，这样就可以保证一定不会出现不一致的情况

串行化之后，就会导致系统的吞吐量会大幅度的降低，用比正常情况下多几倍的机器去支撑线上的一个请求。

