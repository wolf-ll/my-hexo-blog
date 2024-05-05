---
title: MySQL
date: 2024-04-26 17:21:17
auther: 小狼
summary: MySQL数据库查漏补缺，持续更新
categories: 后端
tags:
  - 数据库
  - MySQL
---

# MySQL

## 数据库相关概念

<img src="Mysql\image-20240426154008919.png" alt="image-20240426154008919" style="zoom: 80%;" />

> **数据库** : 数据库(DataBase 简称 DB)就是信息的集合或者说数据库是由数据库管理系统管理的数据的集合。
>
> **数据库管理系统** : 数据库管理系统(Database Management System 简称 DBMS)是一种操纵和管理数据库的大型软件，通常用于建立、使用和维护数据库。
>
> **数据库系统** : 数据库系统(Data Base System，简称 DBS)通常由软件、数据库和数据管理员(DBA)组成。
>
> **数据库管理员** : 数据库管理员(Database Administrator, 简称 DBA)负责全面管理和控制数据库系统。

### 非关系型数据库

* NoSQL（Not Only SQL 的缩写）泛指非关系型的数据库，主要针对的是键值、文档以及图形类型数据存储。并且，NoSQL 数据库天生支持分布式，数据冗余和数据分片等特性，旨在提供可扩展的高可用高性能数据存储解决方案。

* NoSQL 数据库可以存储关系型数据—它们与关系型数据库的存储方式不同。

* NoSQL 数据库代表：HBase、Cassandra、MongoDB、Redis。

<img src="MySQL\sql-nosql-tushi.png" alt="img"  />

NoSQL 数据库非常适合许多现代应用程序，例如移动、Web 和游戏等应用程序，它们需要灵活、可扩展、高性能和功能强大的数据库以提供卓越的用户体验。

**灵活性：** NoSQL 数据库通常提供灵活的架构，以实现更快速、更多的迭代开发。灵活的数据模型使 NoSQL 数据库成为半结构化和非结构化数据的理想之选。

**可扩展性：** NoSQL 数据库通常被设计为**通过使用分布式硬件集群来横向扩展，而不是通过添加昂贵和强大的服务器来纵向扩展。**

**高性能：** NoSQL 数据库**针对特定的数据模型和访问模式进行了优化**，这与尝试使用关系数据库完成类似功能相比可实现更高的性能。

**强大的功能：** NoSQL 数据库提供功能强大的 API 和数据类型，专门针对其各自的数据模型而构建。

### 关系型数据库

* 关系型数据库（RDB，Relational Database）就是一种**建立在关系模型基础上，由多张相互连接的二维表组成的数据库。**关系模型表明了数据库中所存储的数据之间的联系（一对一、一对多、多对多）。
* 关系型数据库中，我们的数据都被存放在了各种二维表中（比如用户表），表中的每一行就存放着一条数据（比如一个用户的信息）。
* 大部分关系型数据库都使用 SQL 来操作数据库中的数据。并且，大部分关系型数据库都支持事务的四大特性(ACID)。

目前主流的**关系型数据库管理系统**的市场占有率排名如下：

<img src="Mysql\image-20240426154125989.png" alt="image-20240426154125989"  />

* Oracle：大型的收费数据库，Oracle公司产品，价格昂贵。
* MySQL：开源免费的中小型数据库，后来Sun公司收购了MySQL，而Oracle又收购了Sun公司。目前Oracle推出了收费版本的MySQL，也提供了免费的社区版本。
* SQL Server：Microsoft 公司推出的收费的中型数据库，C#、.net等语言常用。
* PostgreSQL：开源免费的中小型数据库。
* DB2：IBM公司的大型收费数据库产品。
* SQLLite：嵌入式的微型数据库。Android内置的数据库以及微信本地的聊天记录存储采用的就是该数据库。
* MariaDB：开源免费的中小型数据库。是MySQL数据库的另外一个分支、另外一个衍生产品，与MySQL数据库有很好的兼容性。

### 关系型 vs 非关系型

<img src="MySQL\image-20240426163636208.png" alt="image-20240426163636208"  />

## SQL

全称 Structured Query Language，结构化查询语言。操作关系型数据库的编程语言，定义了一套操作关系型数据库统一**标准** 。

1). SQL语句可以单行或多行书写，以分号结尾。

2). SQL语句可以使用空格/缩进来增强语句的可读性。

3). MySQL数据库的SQL语句不区分大小写，关键字建议使用大写。

4). 注释：单行注释：-- 注释内容 或 # 注释内容		多行注释：/* 注释内容 */

SQL语句，根据其功能，主要分为四类：DDL、DML、DQL、DCL

<img src="Mysql\image-20240426170912537.png" alt="image-20240426170912537" style="zoom: 80%;" />

### DDL 数据定义语言

```sql
--------------数据库操作---------------------
show databases;
select database();	# 当前数据库
create database [ if not exists ] 数据库名 [ default charset 字符集 ] [ collate 排序规则 ];
drop database [ if exists ] 数据库名 ;
use 数据库名 ;
-----------------表操作---------------------
show tables;
desc 表名 ; #查看表结构
show create table 表名 ;	# 查询指定表的建表语句
create table emp(
    id int comment '编号',
    workno varchar(10) comment '工号',
    name varchar(10) comment '姓名',
    gender char(1) comment '性别',
    age tinyint unsigned comment '年龄',
    idcard char(18) comment '身份证号',
    entrydate date comment '入职时间'
) comment '员工表';
ALTER TABLE 表名 ADD 字段名 类型 (长度) [ COMMENT 注释 ] [ 约束 ];	# 添加字段
ALTER TABLE emp ADD nickname varchar(20) COMMENT '昵称';
ALTER TABLE 表名 MODIFY 字段名 新数据类型 (长度);	# 修改字段类型
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型 (长度) [ COMMENT 注释 ] [ 约束 ];
ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT '昵称'; 1
ALTER TABLE 表名 DROP 字段名; 	# 删除字段
ALTER TABLE 表名 RENAME TO 新表名;	# 修改表名
DROP TABLE [ IF EXISTS ] 表名;	# 删除表
TRUNCATE TABLE 表名;	# 删除指定表, 并重新创建表
```

#### 数值类型

| 类型        | 大小   | 有符号(SIGNED)范围                                       | 无符号(UNSIGNED)范围                                         | 描述                |
| ----------- | ------ | -------------------------------------------------------- | ------------------------------------------------------------ | ------------------- |
| TINYINT     | 1byte  | (-128,127)                                               | (0,255)                                                      | 小整数值            |
| SMALLINT    | 2bytes | (-32768,32767)                                           | (0,65535)                                                    | 大整数值            |
| MEDIUMINT   | 3bytes | (-8388608,8388607)                                       | (0,16777215)                                                 | 大整数值            |
| INT/INTEGER | 4bytes | (-2147483648  2147483647)                                | (0,4294967295)                                               | 大整数值            |
| BIGINT      | 8bytes | (-2^63,2^63-1)                                           | (0,2^64-1)                                                   | 极大整数值          |
| FLOAT       | 4bytes | (-3.402823466 E+38,  3.402823466351 E+38)                | 0和(1.175494351 E-38,3.402823466 E+38)                       | 单精度浮点数值      |
| DOUBLE      | 8bytes | (-1.7976931348623157  E+308,  1.7976931348623157  E+308) | 0和  (2.2250738585072014  E-308,  1.7976931348623157  E+308) | 双精度浮点数值      |
| DECIMAL     |        | 依赖于M(精度)和D(标度）的值                              | 依赖于M(精度)和D(标度)的值                                   | 小数值(精确定 点数) |

#### 字符串类型

<img src="MySQL\image-20240505120222157.png" alt="image-20240505120222157"  />

**char是定长字符串**，指定长度多长，就占用多少个字符，和字段值的长度无关 。而varchar是变长字符串，指定的长度为最大占用长度 。相对来说，char性能更好。

#### 日期时间类型

<img src="MySQL\image-20240505124939337.png" />

### DML 数据操作语言

DML英文全称是Data Manipulation Language(数据操作语言)，用来对数据库中表的数据记录进行增、删、改操作。

```sql
INSERT INTO 表名 (字段名1, 字段名2, ...) VALUES (值1, 值2, ...);
INSERT INTO 表名 VALUES (值1, 值2, ...), (值1, 值2, ...), (值1, 值2, ...) ; 	# 批量添加
insert into employee(id,workno,name,gender,age,idcard,entrydate) values(1,'1','Itcast','男',10,'123456789012345678','2000-01-01');
UPDATE 表名 SET 字段名1 = 值1 , 字段名2 = 值2 , .... [ WHERE 条件 ] ;
update employee set name = '小昭' , gender = '女' where id = 1; 
DELETE FROM 表名 [ WHERE 条件 ] ;
delete from employee where gender = '女';	# 删除一条数据
delete from employee;	# 删除所有员工数据
```

### DQL 数据查询语言

DQL英文全称是Data Query Language(数据查询语言)，数据查询语言，用来查询数据库中表的记录。

```sql
SELECT	字段列表	FROM	表名列表
WHERE	条件列表	GROUP BY	分组字段列表	HAVING	分组后条件列表	ORDER BY	排序字段列表	LIMIT	分页参数

SELECT 字段1 [ (AS) 别名1 ] , 字段2 [ (AS) 别名2 ] ... FROM 表名;		# 别名，as可以省略
SELECT DISTINCT 字段列表 FROM 表名;	# 去除重复记录

# 查询年龄小于45的员工 , 并根据工作地址分组 , 获取员工数量大于等于3的工作地址
select workaddress, count(*) address_count from emp where age < 45 group by workaddress having address_count >= 3;

SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数 ;
```

*  where与having区别
  * 执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果进行过滤。
  * 判断条件不同：where不能对聚合函数进行判断，而having可以。
  * **分组之后，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。**

#### 执行顺序

<img src="MySQL\image-20240505180928668.png" alt="image-20240505180928668" style="zoom:80%;" />

### DCL 数据控制语言

DCL英文全称是**Data Control Language**(数据控制语言)，用来管理数据库用户、控制数据库的访问权限。

```sql
select * from mysql.user; 	# 查询用户
```

其中 Host代表当前用户访问的主机, 如果为localhost, 仅代表只能够在当前本机访问，是不可以远程访问的。 User代表的是访问该数据库的用户名。在MySQL中**需要通过Host和User来唯一标识一个用户。**

<img src="MySQL\image-20240505181311834.png" />

```sql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';		# 创建用户
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码' ;	# 修改密码
DROP USER '用户名'@'主机名' ;		# 删除用户	主机名可以使用 % 通配。
```

#### 权限控制

<img src="MySQL\image-20240505181611707.png" alt="image-20240505181611707" style="zoom: 67%;" />

```sql
SHOW GRANTS FOR '用户名'@'主机名' ;	# 查询权限
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';	# 授予权限
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';	# 撤销权限

show grants for 'heima'@'%';
grant all on itcast.* to 'heima'@'%';	# 授予 'heima'@'%' 用户itcast数据库所有表的所有操作权限
revoke all on itcast.* from 'heima'@'%';
```

*  多个权限之间，使用逗号分隔

* 授权时， 数据库名和表名可以使用 * 进行通配，代表所有。

## 函数



## 参考

黑马程序员MySQL：https://www.bilibili.com/video/BV1Kr4y1i7ru/?vd_source=8f6745987f6d9c4a333570852e433d6c

JavaGuide：https://javaguide.cn/database/sql/sql-syntax-summary.html#%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5

