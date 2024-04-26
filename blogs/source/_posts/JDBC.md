---
title: JDBC
date: 2024-04-26 10:22:40
auther: 小狼
summary: Mybatis前置知识。整理数据库通信接口JDBC及一些持久层框架如Hibernate、JPA。
tags:
  - 持久层
  - JDBC
---

# JDBC

## 概述

* JDBC（Java Database Connectivity）是Java语言中用于与数据库通信的一种API，它允许Java应用程序通过标准的数据库访问方法与各种关系型数据库进行交互，例如MySQL、Oracle、SQL Server等。
* JDBC为访问不同的数据库提供了统一的接口，使开发人员能够编写Java程序以执行SQL查询、更新数据库记录。为使用者屏蔽了细节问题。

本质：

* 官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口
* 各个数据库厂商去实现这套接口，提供数据库驱动jar包。**（数据库驱动提供JDBC的实现类）**
* 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类。

优点：

* **跨数据库兼容性**：各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发。可随时替换底层数据库，访问数据库的Java代码基本不变。
* **安全性**：JDBC提供了一种安全的数据库访问机制，可以防止SQL注入等安全问题。通过使用预编译语句和参数化查询等技术，可以有效地防止恶意用户对数据库进行攻击。

## 基本流程

```java
public class JDBCDemo {

    public static void main(String[] args) throws Exception {
        //1. 注册驱动
        // Class.forName("com.mysql.jdbc.Driver");
        //2. 获取连接
        String url = "jdbc:mysql://127.0.0.1:3306/db1";
        String username = "root";
        String password = "1234";
        Connection conn = DriverManager.getConnection(url, username, password);
        //3. 定义sql
        String sql = "update account set money = 2000 where id = 1";
        //4. 获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        //5. 执行sql
        int count = stmt.executeUpdate(sql);//受影响的行数
        //6. 处理结果
        System.out.println(count);
        //7. 释放资源
        stmt.close();
        conn.close();
    }
}
```

## API

### DriverManager

DriverManager（驱动管理类）作用：

* 注册驱动

![registerDriver](JDBC\registerDriver.png)

我们使用的注册驱动代码为

```java
Class.forName("com.mysql.jdbc.Driver");
```

这是因为MySQL提供的Driver类源码调用了DriverManager.registerDriver。

<img src="JDBC\driver.png" alt="driver" style="zoom:67%;" />

在该类中的静态代码块中已经执行了 `DriverManager` 对象的 `registerDriver()` 方法进行驱动的注册了，那么我们只需要加载 `Driver` 类，该静态代码块就会执行。而`Class.forName("com.mysql.jdbc.Driver");` 就可以加载 `Driver` 类。

> ==提示：==
>
> * MySQL 5之后的驱动包，可以省略注册驱动的步骤
> * 自动加载jar包中META-INF/services/java.sql.Driver文件中的驱动类

### Connection

Connection（数据库连接对象）作用：获取执行 SQL 的对象；管理事务

#### 获取执行对象

普通执行SQL对象--statement

```sql
//3. 定义sql
String sql = "update account set money = 3000 where id = 1";
//4. 获取执行sql的对象 Statement
Statement stmt = conn.createStatement();
//5. 执行sql
int count = stmt.executeUpdate(sql);
```

预编译SQL的执行SQL对象：防止SQL注入

* SQL注入是通过操作输入来修改事先定义好的SQL语句，用以达到执行代码对服务器进行攻击的方法。

* 例如普通statement如果使用字符串拼接构造SQL  `String sql = "select * from stu where name='" + name + "'";`  在name中传入`"lucy" or name="郭麒麟"`，此时就会改变原有的SQL，查询到两条语句。也就是SQL注入漏洞

* 可以使用`PreparedStatement`来实现数据库操作，它的特点是：使用占位符`?`表示SQL语句中的参数，通过`set`方法为SQL语句传入参数

  ```sql
  public static void queryPrepare(String name) throws SQLException {
      // 预编译SQL，SQL语句中的参数值，使用？占位符替代
      ptmt = conn.prepareStatement("select * from stu where name=?");
      ptmt.setString(1, name);
      // 打印预编译后的SQL语句
      System.out.println(ptmt.toString());
      // 调用这个方法时不需要传递SQL语句，因为获取SQL语句执行对象时已经对SQL语句进行预编译了。
      rs = ptmt.executeQuery();
      while (rs.next()) {
          StringBuilder sb = new StringBuilder("[id=");
          sb.append(rs.getLong("id"));
          sb.append(", name=" + rs.getString("name"));
          sb.append(", age=" + rs.getInt("age"));
          sb.append(", class_id=" + rs.getLong("class_id"));
          sb.append("]");
          System.out.println(sb.toString());
      }
  }
  ```


> ==小结：==
>
> * 在获取PreparedStatement对象时，将sql语句发送给mysql服务器进行检查，编译（这些步骤很耗时）
> * 执行时就不用再进行这些步骤了，速度更快
> * 如果sql模板一样，则只需要进行一次检查、编译

## 数据库连接池

### 简介

* 数据库连接池是个容器，负责分配、管理数据库连接(Connection)

* 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；

* 释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏
* 好处
  * 资源重用：在需要时分配和回收资源
  * 降低连接的建立和关闭开销，提升系统响应速度
  * 限制并发连接数，便于管理和维护，避免数据库连接遗漏

**其他八股见Java基础数据库连接池部分**

之前我们代码中使用连接是没有使用都创建一个Connection对象，使用完毕就会将其销毁。这样重复创建销毁的过程是特别耗费计算机的性能的及消耗时间的。

而数据库使用了数据库连接池后，就能达到Connection对象的复用，如下图

<img src="JDBC\数据库连接池.png" alt="数据库连接池" style="zoom:80%;" />

连接池是在一开始就创建好了一些连接（Connection）对象存储起来。用户需要连接数据库时，不需要自己创建连接，而只需要从连接池中获取一个连接进行使用，使用完毕后再将连接对象归还给连接池；这样就可以起到资源重用，也节省了频繁创建连接销毁连接所花费的时间，从而提升了系统响应的速度。

### Driud连接池

Druid（德鲁伊）连接池是阿里巴巴开源的数据库连接池项目。使用步骤：

> * 导入jar包 druid-1.1.12.jar
> * 定义配置文件
> * 加载配置文件
> * 获取数据库连接池对象
> * 获取连接

现在通过代码实现，首先需要先将druid的jar包放到项目下的lib下并添加为库文件

<img src="JDBC\Driud导入.png" alt="image-20210725212911980" style="zoom:80%;" />

项目结构如下：

<img src="JDBC\Driud结构.png" alt="image-20210725213210091" style="zoom:80%;" />

编写配置文件如下：

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql:///db1?useSSL=false&useServerPrepStmts=true
username=root
password=1234
# 初始化连接数量
initialSize=5
# 最大连接数
maxActive=10
# 最大等待时间
maxWait=3000
```

使用druid的代码如下：

```java
public class DruidDemo {

    public static void main(String[] args) throws Exception {
        //3. 加载配置文件
        Properties prop = new Properties();
        prop.load(new FileInputStream("jdbc-demo/src/druid.properties"));
        //4. 获取连接池对象
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        //5. 获取数据库连接 Connection
        Connection connection = dataSource.getConnection();
        System.out.println(connection); //获取到了连接后就可以继续做其他操作了
    }
}
```



待更新



## 参考

黑马程序员JDBC原理+实战：https://www.bilibili.com/video/BV1s3411K7jH/?spm_id_from=333.337.search-card.all.click&vd_source=bf952648bf410c0b9b23bf213e3d24ba

JDBC连接如何防止SQL注入：https://blog.csdn.net/u014454538/article/details/108952103