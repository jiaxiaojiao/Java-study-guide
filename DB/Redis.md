# Redis

> 本质上是一个key-value 的NoSQL非关系型数据库，内存数据库，整个数据库系统加载在内存当中进行操作，定期通过异步操作把数据库数据flush到硬盘上进行保存。

## 优点和缺点

- 优点：
    - 纯内存操作，性能快。
    - 支持多种数据结构，单个value的最大限制是1GB
    - 支持事务，操作都是原子性的（对数据的更改要不全部执行，要不全部不执行）
    - 丰富的特性，可用于缓存，消息，按Key设置过期时间，过期自动删除。

- 缺点：
    - 数据库容量收物理内存限制
    - 不能用作海量数据的高性能读写

## Redis 的数据类型

> 数据类型： String， List， Set，Sorted Set ,Hashes
```text
String  :  set,get,decr,incr,mget 
List  : hget,hset,hgetall
Set  : lpush,rpush,lpop,rpop,lrange
Sorted Set  : sadd,spop,smembers,sunion 
Hashes  : zadd,zrange,zrem,zcard

```
这些数据类型都支持push/pop、add/remove及取交集并集和差集及更丰富的操作。

## Redis 适合的场景
[Redis的常见应用场景](Redis-Context-of-Use.md)

## Redis数据淘汰机制
    在redis中允许用户设置最大使用内存大小server.maxmemory。
    内存大小有限，需要保存有效的数据。

redis内存数据集大小上升到一定大小时，就会实施数据淘汰策略/回收策略。Redis提供了6中数据淘汰策略。
1. volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰。
2. volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰 。
3. volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰 。
4. allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰 。
5. allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰 。
6. no-enviction（驱逐）：禁止驱逐数据。

## Redis的并发竞争问题如何解决？
Redis为单进程单线程模式，采用队列模式将并发访问变为串行访问。Redis本身没有锁的概念，Redis对于多个客户端连接并不存在竞争，但是在Jedis客户端对Redis进行并发访问时会发生连接超时、数据转换错误、阻塞、客户端关闭连接等问题，这些问题均是由于客户端连接混乱造成。对此有2种解决方法：
1. 客户端角度，为保证每个客户端间正常有序与Redis进行通信，对连接进行池化，同时对客户端读写Redis操作采用内部锁synchronized。 
2. 服务器角度，利用setnx实现锁。

注：对于第一种，需要应用程序自己处理资源的同步，可以使用的方法比较通俗，可以使用synchronized也可以使用lock；第二种需要用到Redis的setnx命令，但是需要注意一些问题。

## Redis的存储策略/存储机制

Redis存储机制分为两种，Snapshot AOF。无论哪种机制，Redis都是讲数据存储在内存中。

Snapshot工作原理：是将数据先存储在内存，然后当数据累计达到某些设定的伐值的时候，就会触发一次DUMP操作，将变化的数据一次性写入数据文件RDB文件。性能高。

AOF工作原理： 是将数据先存储在内存，但是在存储的时候会使用调用fsync来完成对本次写操作的日志记录，这个日志揭露文件其实是一个基于Redis的网络交互协议的文本文件。采用日志追加的方式来持久化数据。实时存储或准实时模式，频率高效率偏低。安全性高。

## redis容灾机制/容灾策略

基本的redis的容灾策略为： 

采用master-slave方式

为了得到好的读写性能，master不做任何的持久化 

slave同时开启Snapshot和AOF来进行持久化，保证数据的安全性 

当master挂掉后，修改slave为master 

恢复原master数据，修改原先master为slave，启动slave 

若master与slave都挂掉后，调用命令通过aof和snapshot进行恢复 

恢复时要先确保恢复文件都正确了，才能启动主库；也可以先启动slave，将master与slave对调 

开源方案codishttp://navyaijm.blog.51cto.com/4647068/1637688

## 哨兵的作用

监控：监控主从是否正常

通知：出现问题时，可以通知相关人员

故障迁移：自动主从切换

统一的配置管理：连接者询问sentinel取得主从的地址 

Raft算法核心: 可视图

## 其他
    redis在业务上可以实现哪些功能
    redis基本操作命令，设置过期时间。
    Redis的值超过了Long类型的最大值怎么办
    10G的文件里面存储有序的数字，内存4G，如何找到指定的数字，不适用MapReduce方式。














