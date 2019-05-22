# MyBatis 

基于Java的持久层框架，

提供的持久层框架包括 SQL Maps 和 Data Access Objects (DAO)。

消除了几乎所有JDBC代码和参数的手工设置以及结果集的检索。

使用简单的XML 或注解用于配置和原始映射，将接口和Java的POJOs普通的Java对象映射成数据库中的记录。

####  1. #{}和${}的区别是什么？
```text
#{}是预编译处理，${}是字符串替换。
Mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；
Mybatis在处理${}时，就是把${}替换成变量的值。
使用#{}可以有效的防止SQL注入，提高系统安全性。
```

#### 2. 分页

#### 3. 设置缓存

#### 4. mybatis 跟 hibernate、 jdbc 的区别 优缺点

