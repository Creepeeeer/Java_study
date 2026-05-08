# 简介

JDBC就是使用Java语言操作关系型数据库的一套API

**本质**

官方定义了一套操作所有关系型数据库的规则，即接口

各个数据库厂商去实现这套接口，提供数据库驱动jar包

我们可以使用这套接口（JDBC）编程，真正执行代码的是驱动jar包中的实现类

```java
//注册驱动
Class.forName("com.mysql.cj.jdbc.Driver");
//获取连接
String url="jdbc:mysql://127.0.0.1:3306/test";
String username="root";
String password="060622";
Connection connection= DriverManager.getConnection(url,username,password);
//获取执行sql对象的statement
Statement statement=connection.createStatement();
//定义sql
String sql="update student set name='creep' where id=1";
//执行sql
int line_cnt=statement.executeUpdate(sql);
System.out.println(line_cnt);
//释放资源
statement.close();
connection.close();
```



# API

## DriverManager

### 注册驱动

```java
Class.forName("com.mysql.cj.jdbc.Driver");
//Driver类源码
static {
    try {
        DriverManager.registerDriver(new Driver());//注册驱动
    } catch (SQLException var1) {
        throw new RuntimeException("Can't register driver!");
    }
}
```

- MYSQL 5 之后的驱动包，可以省略注册驱动的步骤
- 自动加载jar包中META-INF/services/java.sql.Driver文件中的驱动类



### 获取连接

```sql
static Connection    getConnection(String url,String user,String password)
```

- url:连接路径

```java
语法：jdbc::mysql://ip地址(域名):端口号/数据库名称?参数键值对&参数键值对2...
e.g jdbc::mysql://127.0.0.1:3306/db1
- 如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql://数据库名称？参数键值对
- 配置useSSL=false 参数，禁用安全连接方式，解决警告提示
```

- user:用户名
- password:密码





## Connection

### 获取执行SQL的对象

- 普通执行SQL对象

```java
Statement createStatement();
```

- 预编译SQL的执行SQL对象：防止SQL注入

```java
PreparedStatement prepareStatement(sql);
```

- 执行存储过程的对象

```java
CallableStatement prepareCall(sql);
```



### 管理事务

- MYSQL事务管理

```java
开启事务：begin;/start transaction;
提交事务：commit;
回滚事务: rollback;
MYSQL默认自动提交事务
```

- JDBC事务管理，Connection接口中定义了三个对应的方法

  ```
  开启事务:setAutoCommit(boolean autoCommit):true为自动提交，false为手动提交事务，即为开启事务
  提交事务:commit();
  回滚事务:rollback();
  
  try{
      //开启事务
      connection.setAutoCommit(false);
      //执行sql
      int count1=statement.executeUpdate(sql1);
      System.out.println(count1);
      int count2=statement.executeUpdate(sql2);
      System.out.println(count2);
  
      //提交事务
      connection.commit();
  } catch (Exception e) {
      //回滚事务
      connection.rollback();
  }
  ```

  





## Statement

**作用：执行SQL语句**

```java
int executeUpdate(sql):执行DML，DDL语句
返回值:(1)DML语句影响的行数 (2)DDL语句执行后，执行成功也可能返回0
ResultSet executeQuery(sql):执行DQL语句
返回值：resultSet结果集对象
```





## ResultSet

封装了DQL查询语句的结果

```java
ResultSet statement.executeQuery(sql);执行DQL语句，返回ResultSet对象;
```

### 获取查询结果

```java
boolean next():(1)将光标从当前位置向前移动一行 (2)判断当前行是否为有效行
返回值：
    - true 有效行，当前行有数据
    - false 无效行，当前行没有数据
    
xxx getXxx(参数):获取数据
    xxx:数据类型;如: int getInt(参数);String getSrting(参数)
    参数:
		int:列的编号，从1开始
        String:列的名称
```

```java
        String sql="select * from student;";
        ResultSet resultSet=statement.executeQuery(sql);
        List list=new ArrayList<Student>();
        while(resultSet.next()){
            Student student =new Student();
            int id=resultSet.getInt(1);//这里表示列号，下标从1开始
            String name =resultSet.getString(2);
            String num=resultSet.getString(3);
            //也可以这么写
//            int id=resultSet.getInt("id");
//            String name =resultSet.getString("name");
//            String num=resultSet.getString("num");
            student.setId(id);
            student.setName(name);
            student.setNum(num);
            list.add(student);
        }
        System.out.println(list);
        //释放资源
        resultSet.close();
        statement.close();
        connection.close();
```





## PreparedStatement

预编译SQL语句并执行，预防SQL注入问题

SQL注入：SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行攻击的方法

```java
e.g
假如一张表存储了账户和密码
登录页面通过select * from tb where id='输入的账户' and password='输入的password'
假如输入的密码为'or'1'='1
将会变成password=''or'1'='1'恒为1查询到整张表
```



### 获取PreparedStatement对象

```java
//sql语句中的参数值，使用？占位符替代
String sql="select * from user where username =? and password =?"

//通过Connection对象获取，并传入对应的SQL语句
PreparedStatement pstmt=conn.prepareStatement(sql);
```

### 设置参数值

```java
PreparedStatement对象：setXxx(参数1,参数2):给？赋值
Xxx:数据类型；如setInt(参数1，参数2)
参数:
	参数1:?的位置编号，从1开始
    参数2:？的值
```

### 执行SQL

```java
executeUpdate();/executeQuery();不需要再传递sql
```



```java
e.g
String sql="select * from tb_user where username=? and password=?";
PreparedStatement preparedStatement=connection.prepareStatement(sql);
preparedStatement.setString(1,"creep");
preparedStatement.setString(2,"123456");
ResultSet resultSet=preparedStatement.executeQuery();
if(resultSet.next()){
    System.out.println("登陆成功");
}
else{
    System.out.println("登陆失败");
}
resultSet.close();
preparedStatement.close();  
connection.close();
```



### PreparedStatement原理

#### 预编译功能

设置url的参数

```java
useServerPrepStmts=true
String url="jdbc:mysql://127.0.0.1:3306/test?useServerPrepStmts=true";
```

#### 配置MYSQL执行日志

```java
log-output=FILE
general-log=1
general_log_file="D:\mysql.log"
slow_query_log_file="D:\mysql_slow.log"
long_query_time=2
```

#### 原理

- 在获取PreparedStatement对象时，将sql语句发送给mysql服务器，进行检查，编译(这些步骤很耗时)
- 执行时就不用再进行这些步骤了，速度更快
- 如果sql模板一样，则只需要机械能依次检查，编译



## 数据库连接池

- 数据库连接池是个容器，负责分配，管理数据库连接(Connection)
- 它允许应用程序重复使用一个现有的数据库连接，而不是再重新创建一个
- 释放空间时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏

### 连接池实现

标准接口DataSource

官方提供的数据库连接池标准接口，由第三方组织实现此接口

功能：获取连接

```java
Connection getConnection()
```

```java
//1.导入jar包

//2.定义配置文件
//druid.properties

//3.加载配置文件

//4.获取连接池对象
Properties prop=new Properties();
prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));
//获取连接池对象
DataSource dataSource=DruidDataSourceFactory.createDataSource(prop);
//获取数据库连接Connection
Connection connection= dataSource.getConnection();
System.out.println(connection);
```

