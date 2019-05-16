# Hibernate 

> 开放源代码的对象关系映射框架。
<br> 对JDBC进行了非常轻量级的对象封装。
<br> 将POJO与数据库表建立映射关系。
<br> 全自动的orm框架，自动生成SQL语句，自动执行。

## 为什么要用Hibernate/JDBC的缺点
- 编程过程很繁琐，使用try和catch比较多
- jdbc没有做数据的缓存
- 没有做到面向对象编程，
- SQL语句的跨平台性很差。

## JDBC的优点：效率高

## Hibernate优点
- 完全面向对象编程
- hibernate缓存，一级缓存 二级缓存 查询缓存
- 编程的时候比较简单
- 跨平台性很强
- 使用场合是企业内部的系统。

## Hibernate缺点
- 效率较低
- 表中的数据如果在千万级别，不适合
- 如果表与表之间关系比较复杂，不适合

## Hibernate 的API接口
Hibernate的API一共有6个。
分别是 Session，SessionFactory，Transaction，Query，Criteria 和 Configuration。通过这些接口，可以对持久化对象进行存取、事务控制。

- Configuration：负责配置并启动hibernate，创建SessionFactory　　
- SessionFactory：负责初始化hibernate，创建session对象
    - 1、hibernate中的配置文件、映射文件、持久化类的信息都在sessionFactory中
    - 2、sessionFactory中存放的信息都是共享的信息
    - 3、sessionFactory本身就是线程安全的
    - 4、一个hibernate框架sessionFactory只有一个
    - 5、sessionFactory是一个重量级别的类
- Session：负责被持久化对象CRUD操作
    - 1、得到了一个session，相当于打开了一次数据库的连接
    - 2、在hibernate中，对数据的crud操作都是由session来完成的
- Transaction：负责事物相关的操作
    - hibernate中的事务默认不是自动提交的
    - 设置了connection的setAutoCommit为false
    - 只有产生了连接，才能进行事务的操作。所以只有有了session以后，才能有transaction
- Query：负责执行各种数据库查询