## 面试总结

> 面试中的知识点总结

## [项目经验](Resume.md)
- 项目介绍
- 整体架构和使用到的中间件
- 解决过的比较复杂的问题

## 面试题
[20190527 国网外包笔试题](Questions-1.md)

## Java基础知识
- 栈和队列
- 接口和抽象类
- int和Integer的
- 自动拆箱/装箱
- 常量池
- == 和equals
- 重载和重写
- String 和 StringBuffer ，StringBuilder的区别

## 集合框架
- ArrayList
- LinkedList
- HashMap
- LinkedHashMap
- ConcurrentHashMap
- 1.7版本和1.8版本的HashMap的区别
- 1.7版本和1.8版本的ConcurrentHashMap的区别
- HashMap能不能排序，HashMap的长度为什么事2的幂次方。

## 多线程
- 创建线程的几种方式？wait sleep分别是谁的方法，区别，线程间的通信方式。
- 死锁，怎么排查死锁？
- 创建线程池的几种方式，线程池有什么好处？
- 线程继承和接口的区别，接口有什么好处？
- Synchronized  lock reentrantLock，区别，用法，原理。
- CountDownLatch 与 CyclicBarrier 用法
- volatile关键字的作用和原理
- 乐观锁和悲观锁
- 对公平锁，非公平锁，可冲入锁，自旋锁，读写锁的理解。
- CAS是什么，底层原理
- ArrayBlockingQueue ， LinkedBlockingQueue， SynchronousQueue等堵塞队列的理解。
- ThreadPoolExecutor的传入参数及内部工作原理。
- 分布式环境下，怎么保证线程安全。
## JVM相关问题
- JVM内存机制
- 介绍下垃圾回收机制，垃圾回收机制有哪些算法，各自的特点。
- 聊聊GC， Major GC ，FullGC区别，垃圾收集器有哪些，他们的区别？
- OutOfMemoryError  怎么处理
- JVM调优有哪些参数
- 自己写一个类叫 java.lang.String 类加载过程，双亲委派模型。
## 框架相关问题
- SSM 
- ORM
- Spring使用了哪些设计模式？
- Spring注入bean的方式
- 对Spring IOC 和 Spring AOP的理解
- Spring事务隔离级别和传播机制
- Mybatis的缓存机制（一级缓存和二级缓存）
- Mybatis的mapper文件中 # $ 的区别
- Spring MVC的流程
- Spring 和 Spring Boot的区别
- 对SpringBoot的理解
- RPC框架有哪些，他们的区别？
- Dubbo的使用和理解
- Spring Cloud的使用和组件，谈谈你的理解。
## 消息中间件
- MQ 消息队列
- Kafka
- 如何保证消息中间件的高可用？
- 如何保证消息中间件重复发送消息？
- 消息队列积压了大量的消息，你该怎么处理？
- 如何保证消费者消费消息是有顺序的？
- 让你开发一个消息中间件，你会怎么架构？
## Redis
- Redis 默认访问端口。
- Redis 数据类型
- Redis的持久化机制？
- Redis的过期策略？
- 怎么保证Redis的高可用？
- 什么是缓存穿透，如何避免？
- 什么是缓存雪崩，如何避免？
- 如何保证缓存与数据的双写一致性？
- Redis单线程模型原理，为什么能支撑高并发？
- Redis哨兵架构的理解和底层原理？
## 数据库
- MySQL Oracle 默认访问端口
- SQL优化
- 什么情况下索引会失效？
- 数据库的存储引擎，比如：MySQL的MyISAM和InnoDB的区别？
- 索引的最左原则
- 索引的底层原理
- 分库分表，分库分表方案

## 其他
- 分布式事务是怎么解决的？
- 分布式Session方案
- 设计一个秒杀场景
- 怎么防止表单多次提交
- Linux的基本操作命令
- ElasticSearch的使用和原理
- zookeeper的使用和原理
- zookeeper 默认访问端口
- JDK源码，看源码
- 个人博客
- 开源项目

## HR面试