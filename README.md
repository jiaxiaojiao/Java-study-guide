# 知识点总结

## 服务器与容器
- [Tomcat](Tomcat/Tomcat.md)
> Tomcat服务器是一个免费的开放源代码的Web应用服务器。
- apache
- nginx

## IDE 集成开发环境
- Eclipse
- NetBeans
- Intellij IDEA

## 测试与日志
- junit
- log4j

## JavaEE Web 
> Java 企业应用的Web应用

- 框架
- 基本[MVC](MVC/MVC.md)
    - [Spring](Java/Spring/Spring.md)
    > Spring是轻量级的 [控制反转IoC](Java/Spring/Spring-IoC.md) 和 [面向切面AOP](./Spring/Spring-AOP.md) 的容器框架。
    - Spring MVC
    - Spring Boot
    - Spring Cloud
    - Struts
    - [Hibernate](Java/Hibernate/Hibernate.md) 
    > 开放源代码的对象关系映射框架。
    <br> Hibernate是一个开放源代码的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装，它将POJO与数据库表建立映射关系，是一个全自动的orm框架，hibernate可以自动生成SQL语句，自动执行，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。 Hibernate可以应用在任何使用JDBC的场合，既可以在Java的客户端程序使用，也可以在Servlet/JSP的Web应用中使用，最具革命意义的是，Hibernate可以在应用EJB的JaveEE架构中取代CMP，完成数据持久化的重任。
    - ibatis/[Mybatis](Java/Mybatis/Mybatis.md)
    > 持久层框架。
    <br> MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以对配置和原生Map使用简单的 XML 或注解，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。
- JPA Java持久层API
- EJB JavaEE服务器端组件模型
- 前端
    - [Javascript](JavaScript/JavaScript.md)
    - [jQuery](JavaScript/jQuery/jQuery.md)
- JSP/Servlet
- JDBC
- [缓存](Cache/cache.md)
- 编码与加解密
    - base64
    - MD5
    - 对称加密
    - 非对称加密

## I/O 与网络
- 基本IO
    - 文件IO
    - 序列化
    - 网络IO
        - socket
- 线程与并发
    - [线程](Java/Thread/Thread.md)
        - 锁
        - 线程安全
        - 共享与可见性 
    - NIO
        - 并发容器
        - Executor 框架
        - 并发
        - IO设计模式
- 网络开发
    - 数据结构
        - XML
        - JSON
    - 通信协议
        - [HTTP](HTTP/HTTP.md)/HTTPS
        - XMPP
        - Socket
    - 通信框架
        - MINA
        - netty
## 开源与类库
- apache commons
- Guava
- [Lombok 底层实现原理](Java/Spring/tools/lombok.md) 

## 项目管理
- Maven
- Git
- Gradle

## [Java](Java/Java.md) 基础知识
- [Java 关键字](Java/Keyword/keyword.md)
- [JDK 1.8的新特性](Java/JDK1.8/JDK1.8.md)
- 数据结构
    - 基本数据类型
    - 引用数据类型
- 基本语法
    - 运算符
    - 访问控制
    - 循环条件
    - [异常](Java/Throwable/Throwable.md)
    - 反射
- 面向对象
    - 对象
    - 枚举 enum
    - [集合类](Java/Collection/Collection.md)
    - 继承与多态
    - 泛型
    - 内部类
    - 接口
    - 注解
    - 异常与错误
- 常用类
- 正则表达式： Pattern 
- 内存与[JVM Java虚拟机](Java/JVM/JVM.md)
    - JVM参数
    - 内存分配机制
    - 垃圾回收机制 GC
    - 内存泄漏与监控

## 数据结构与算法
- [数据结构](Data-Structure/Data-Structure.md)
    - 线性结构： [链表](Data-Structure/linked-list.md) hash
    - 树形结构： 树 二叉树
    - 图
- 算法
    - 搜索算法
    - 排序算法
        - [冒泡排序](/Algorithm/Sorting-Algorithm/Bubble-Sort.md)

## 数据库 DB
- 关系型数据库
    > 关系型数据库的三范式/ 关系型数据库在设计表时必须要遵循的规范: 
    <br> 1.数据库中的每一列都是不可分割的基本数据项，同一列不能有多个值。
    <br> 2.数据库中的每行必须可以被唯一的区分。
    <br> 3.不能存在传递依赖。除主键外，其他字段必须依赖主键。
    <br><br> [数据库事务](/DB/DB-transaction.md)的四个基本特征/ACID特性: 事务是并发控制的单位，是用户定义的一个操作序列，这些操作要不都做，要不都不做。是一个不可分割的工作单元。
    <br> 原子性（Atomicity）
    <br> 一致性（Consistency）
    <br> 隔离性（Isolation）
    <br> 持久性（Durability）
    - [Oracle](/DB/Oracle.md)
    - [MySQL](/DB/MySQL.md)
- 非关系型数据库
    - [Redis](/DB/Redis.md)
    - [MongoDB](/DB/MongoDB.md)

## [设计模式](Design-Pattern/Design-Pattern.md)
- 原则
- 创建模式
- 结构模式
- 行为模式

## [分布式系统](Distributed-System/distributed-system.md)
- [RESTful](Java/RESTful/RESTful.md)
> REST一组架构约束条件和原则。满足的就是RESTful
- [Dubbo](Java/Dubbo/Dubbo.md)
- [Zookeeper](Distributed-System/Zookeeper/Zookeeper.md)

##  [软件开发流程](Process/Software-Development-Process.md)

## 操作系统
- Linux
    - [Linux 常用命令](Linux/Linux.md)

## 程序开发语言
- [程序开发语言综述](Language/Language.md)

