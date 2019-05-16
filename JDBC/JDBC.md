# JDBC 

## 通过JDBC访问数据库
1. 加载JDBC驱动器。classpath，WEB-INF/lib
2. 加载JDBC驱动。 Class.forName() 把类加载到JVM中。
3. 建立数据库连接。DriverManager.getConnection(url, username, password);
4. 建立Statement对象 或 PreparedStatement对象。
5. 执行SQL语句。
6. 访问结果集 ResultSet对象
7. 依次将ResultSet，Statement， Connection对象关闭，释放占有的资源。

JDBC驱动在底层 通常都是通过网络IO实现SQL命令与数据传输的。