# Redis的存储机制/持久化

Redis存储机制分为两种，Snapshot AOF。

无论哪种机制，Redis都是讲数据存储在内存中。

## 工作原理: 

- Snapshot：是将数据先存储在内存，然后当数据累计达到某些设定的伐值的时候，就会触发一次DUMP操作，将变化的数据一次性写入数据文件RDB文件。性能高。

- AOF： 是将数据先存储在内存，但是在存储的时候会使用调用fsync来完成对本次写操作的日志记录，这个日志揭露文件其实是一个基于Redis的网络交互协议的文本文件。采用日志追加的方式来持久化数据。实时存储或准实时模式，频率高效率偏低。安全性高。


## Redis容灾机制/容灾策略

基本的redis的容灾策略为： 采用master-slave方式 。

1. 为了得到好的读写性能，master不做任何的持久化 

2. slave同时开启Snapshot和AOF来进行持久化，保证数据的安全性 

3. 当master挂掉后，修改slave为master 

4. 恢复原master数据，修改原先master为slave，启动slave 

5. 若master与slave都挂掉后，调用命令通过aof和snapshot进行恢复 

6. 恢复时要先确保恢复文件都正确了，才能启动主库；也可以先启动slave，将master与slave对调 



