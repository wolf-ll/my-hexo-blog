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

### 字符串函数

<img src="MySQL\image-20240507140710177.png" alt="image-20240507140710177" style="zoom: 67%;" />

### 数值函数

<img src="MySQL\image-20240507141021752.png" alt="image-20240507141021752" style="zoom: 67%;" />

通过数据库的函数，生成一个六位数的随机验证码。

思路： 获取随机数可以通过rand()函数，但是获取出来的随机数是在0-1之间的，所以可以在其基础上乘以1000000，然后舍弃小数部分，如果长度不足6位，补0

```sql
select lpad(round(rand()*1000000 , 0), 6, '0'); 
```

### 日期函数

<img src="MySQL\image-20240507141208167.png" alt="image-20240507141208167" style="zoom:67%;" />

### 流程函数

<img src="MySQL\image-20240507141452210.png" alt="image-20240507141452210" style="zoom:67%;" />

需求: 查询emp表的员工姓名和工作地址 (北京/上海 ----> 一线城市 , 其他 ----> 二线城市)

```sql
select
    name,
    ( case workaddress when '北京' then '一线城市' when '上海' then '一线城市' else
    '二线城市' end ) as '工作地址'
from emp;
```

## 约束

概念：约束是作用于表中字段上的规则，用于限制存储在表中的数据。

目的：保证数据库中数据的正确、有效性和完整性。

### 分类

<img src="MySQL\image-20240507143454084.png" alt="image-20240507143454084" style="zoom:80%;" />

注意：约束是作用于表中字段上的，可以在创建表/修改表的时候添加约束。

示例：

```sql
CREATE TABLE tb_user(
    id int AUTO_INCREMENT PRIMARY KEY COMMENT 'ID唯一标识',
    name varchar(10) NOT NULL UNIQUE COMMENT '姓名' ,
    age int check (age > 0 && age <= 120) COMMENT '年龄' ,
    status char(1) default '1' COMMENT '状态',
    gender char(1) COMMENT '性别'
);
```

<img src="MySQL\image-20240507143712836.png" alt="image-20240507143712836" style="zoom:80%;" />

### 外键约束

外键：用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性

```sql
CREATE TABLE 表名(
    字段名 数据类型,
    ...
    [CONSTRAINT] [外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名)
);

ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表 (主表列名);

alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id);

ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```

<img src="MySQL\image-20240507144123244.png" style="zoom:80%;" >

#### 删除/更新行为

添加了外键之后，再删除父表数据时产生的约束行为，我们就称为删除/更新行为。具体的删除/更新行为有以下几种:

<img src="MySQL\image-20240507144346915.png" style="zoom:80%;" >

语法：

```sql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段) REFERENCES
主表名 (主表字段名) ON UPDATE CASCADE ON DELETE CASCADE;
```

## 多表查询

执行多表查询，使用逗号分隔多张表，如： select * from emp , dept;  得到两表的**笛卡尔积组合结果**

<img src="MySQL\image-20240507145729379.png" style="zoom:80%;" >

在SQL语句中，如何来去除无效的笛卡尔积呢？ 我们可以给多表查询加上连接查询的条件即可。

select * from emp , dept where emp.dept_id = dept.id;

这种情况下，由于id为17的员工，没有dept_id字段值，所以在多表查询时，根据连接查询的条件没有查询到，也就不会显示在结果中。

### 分类

* 连接查询
  * 内连接：相当于查询A、B**交集部分**数据--分为隐式内连接和显示内连接
  * 外连接：
  * 左外连接：查询左表所有数据，以及两张表交集部分数据
  * 右外连接：查询右表所有数据，以及两张表交集部分数据
  * 自连接：当前表与自身的连接查询，自连接必须使用表别名

```sql
# 隐式内连接
SELECT 字段列表 FROM 表1 , 表2 WHERE 条件 ... ;
# 显示内连接
SELECT 字段列表 FROM 表1 [ INNER ] JOIN 表2 ON 连接条件 ... ;
# eg:
select emp.name , dept.name from emp , dept where emp.dept_id = dept.id ;
select e.name, d.name from emp e inner join dept d on e.dept_id = d.id;

# 左外连接
SELECT 字段列表 FROM 表1 LEFT [ OUTER ] JOIN 表2 ON 条件 ... ; 
# 右外连接
SELECT 字段列表 FROM 表1 RIGHT [ OUTER ] JOIN 表2 ON 条件 ... ; 

# 自连接
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ... ;
# eg： 查询员工 及其 所属领导的名字
select a.name , b.name from emp a , emp b where a.managerid = b.id; 1
```

### 联合查询

对于union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集。

```sql
SELECT 字段列表 FROM 表A ...
UNION [ ALL ]
SELECT 字段列表 FROM 表B ....;
```

* 对于联合查询的多张表的**列数必须保持一致，字段类型也需要保持一致**。

* **union all 会将全部的数据直接合并在一起，union 会对合并之后的数据去重。**

### 子查询

SQL语句中嵌套SELECT语句，称为嵌套查询，又称子查询。

```sql
SELECT * FROM t1 WHERE column1 = ( SELECT column1 FROM t2 );
```

#### 标量子查询

子查询返回的结果是单个值（数字、字符串、日期等），最简单的形式，这种子查询称为标量子查询。

eg：查询在 "方东白" 入职之后的员工信息

```sql
select * from emp where entrydate > (select entrydate from emp where name = '方东白');
```

#### 列子查询

子查询返回的结果是一列（可以是多行），这种子查询称为列子查询。

<img src="MySQL\image-20240507152223041.png" style="zoom:80%;" >

 eg：查询比 财务部 所有人工资都高的员工信息

```sql
select * from emp where salary > all ( select salary from emp where dept_id =
(select id from dept where name = '财务部') );
```

#### 行子查询

子查询返回的结果是一行（可以是多列），这种子查询称为行子查询。

eg：查询与 "张无忌" 的薪资及直属领导相同的员工信息 ;

```sql
select * from emp where (salary,managerid) = (select salary, managerid from emp
where name = '张无忌');
```

#### 表子查询

子查询返回的结果是多行多列，这种子查询称为表子查询。

eg：查询与 "鹿杖客" , "宋远桥" 的职位和薪资相同的员工信息

```sql
select * from emp where (job,salary) in ( select job, salary from emp where name =
'鹿杖客' or name = '宋远桥' );
```

eg：查询入职日期是 "2006-01-01" 之后的员工信息 , 及其部门信息

```sql
select e.*, d.* from (select * from emp where entrydate > '2006-01-01') e left
join dept d on e.dept_id = d.id ;
```

## 事务

**事务 是一组操作的集合，它是一个不可分割的工作单位**，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

我们只需要在业务逻辑执行之前开启事务，执行完毕后提交事务。如果执行过程中报错，则回滚事务，把数据恢复到事务开始之前的状态。注意： 默认MySQL的事务是自动提交的，也就是说，当执行完一条DML语句时，MySQL会立即隐式的提交事务。

### 控制事务

```sql
# 设置事务提交方式
SELECT @@autocommit ;
SET @@autocommit = 0 ;

# 开启事务
START TRANSACTION 或 BEGIN ;
# 提交事务
COMMIT;
# 回滚事务
ROLLBACK;
```

上述的这种方式，我们是修改了事务的自动提交行为, 把默认的自动提交修改为了手动提交, 此时我们执行的DML语句都不会提交, 需要手动的执行commit进行提交。

### 事务四大特性

原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败。

一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。

隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。

持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

上述就是事务的四大特性，简称ACID。**只有保证了事务的持久性、原子性、隔离性之后，一致性才能得到保障。也就是说 A、I、D 是手段，C 是目的。**

### 并发事务问题

在典型的应用程序中，多个事务并发运行，经常会操作相同的数据来完成各自的任务（多个用户对同一数据进行操作）。并发虽然是必须的，但可能会导致以下的问题。

#### 脏读（Dirty read）

 赃读：一个事务读到另外一个事务还没有提交的数据。（A写数据，没提交就被B读）

> 一个事务读取数据并且对数据进行了修改，这个修改对其他事务来说是可见的，即使当前事务没有提交。这时另外一个事务读取了这个还未提交的数据，但第一个事务突然回滚，导致数据并没有被提交到数据库，那第二个事务读取到的就是脏数据，这也就是脏读的由来。

<img src="https://javaguide.cn/assets/concurrency-consistency-issues-dirty-reading-C1rL9lNt.png" alt="脏读" style="zoom:80%;" />

#### 丢失修改（Lost to modify）

（AB都读数据，然后同时写，造成写覆盖）

> 在一个事务读取一个数据时，另外一个事务也访问了该数据，那么在第一个事务中修改了这个数据后，第二个事务也修改了这个数据。这样第一个事务内的修改结果就被丢失，因此称为丢失修改。例如：事务 1 读取某表中的数据 A=20，事务 2 也读取 A=20，事务 1 先修改 A=A-1，事务 2 后来也修改 A=A-1，最终结果 A=19，事务 1 的修改被丢失。

<img src="https://javaguide.cn/assets/concurrency-consistency-issues-missing-modifications-D4pIxvwj.png" alt="丢失修改" style="zoom:80%;" />

#### 不可重复读（Unrepeatable read）

不可重复读：一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读。（B在一个事务内多次读数据，A在这个区间写数据，导致B读到不同数据）

> 指在一个事务内多次读同一数据。在这个事务还没有结束时，另一个事务也访问该数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改导致第一个事务两次读取的数据可能不太一样。这就发生了在一个事务内两次读到的数据是不一样的情况，因此称为不可重复读。

例如：事务 1 读取某表中的数据 A=20，事务 2 也读取 A=20，事务 1 修改 A=A-1，事务 2 再次读取 A =19，此时读取的结果和第一次读取的结果不同。

<img src="https://javaguide.cn/assets/concurrency-consistency-issues-unrepeatable-read-RYuQTZvh.png" alt="不可重复读" style="zoom:80%;" />

#### 幻读（Phantom read）

 幻读：一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在，好像出现了 "幻影"。

（B在一个事务内多次读数据，A在这个区间内新增数据，导致B前后读到新增的数据）

> 幻读与不可重复读类似。它发生在一个事务读取了几行数据，接着另一个并发事务插入了一些数据时。在随后的查询中，第一个事务就会发现多了一些原本不存在的记录，就好像发生了幻觉一样，所以称为幻读。

例如：事务 2 读取某个范围的数据，事务 1 在这个范围插入了新的数据，事务 2 再次读取这个范围的数据发现相比于第一次读取的结果多了新的数据。

<img src="https://javaguide.cn/assets/concurrency-consistency-issues-phantom-read-D-ETycCp.png" alt="幻读" style="zoom:80%;" />

- **不可重复读的重点是内容修改或者记录减少**比如多次读取一条记录发现其中某些记录的值被修改；
- **幻读的重点在于记录新增**比如多次执行同一条查询语句（DQL）时，发现查到的记录增加了。

幻读其实可以看作是不可重复读的一种特殊情况，单独把幻读区分出来的原因主要是**解决幻读和不可重复读的方案不一样**。

举个例子：执行 `delete` 和 `update` 操作的时候，可以直接对记录加锁，保证事务安全。而执行 `insert` 操作的时候，由于记录锁（Record Lock）只能锁住已经存在的记录，为了避免插入新记录，需要依赖间隙锁（Gap Lock）。也就是说执行 `insert` 操作的时候需要依赖 Next-Key Lock（Record Lock+Gap Lock） 进行加锁来保证不出现幻读。

### 事务隔离级别

为了解决并发事务所引发的问题，在数据库中引入了事务隔离级别。主要有以下几种

* **READ-UNCOMMITTED(读取未提交)** ：最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读。

* **READ-COMMITTED(读取已提交)** ：允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生。

* **REPEATABLE-READ(可重复读)** ：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生。

* **SERIALIZABLE(可串行化)** ：最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。

|     隔离级别     | 脏读 | 不可重复读 | 幻读 |
| :--------------: | :--: | :--------: | :--: |
| READ-UNCOMMITTED |  √   |     √      |  √   |
|  READ-COMMITTED  |  ×   |     √      |  √   |
| REPEATABLE-READ  |  ×   |     ×      |  √   |
|   SERIALIZABLE   |  ×   |     ×      |  ×   |

```sql
# 查看事务隔离级别
SELECT @@TRANSACTION_ISOLATION;
# 设置事务隔离级别  事务隔离级别越高，数据越安全，但是性能越低。
SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL { READ UNCOMMITTED |
READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
```

## 存储引擎

### MySQL体系结构

<img src="MySQL\体系结构.jpg" style="zoom:80%;" >

**1). 连接层**

最上层是一些**客户端和链接服务**，包含本地sock 通信和大多数基于客户端/服务端工具实现的类似于TCP/IP的通信。主要完成一些类似于**连接处理、授权认证、及相关的安全方案**。在该层上引入了**线程池**的概念，为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限。

**2). 服务层**

第二层架构主要完成大多数的**核心服务功能**，如**SQL接口**，并完成**缓存的查询，SQL的分析和优化，部分内置函数的执行**。所有跨存储引擎的功能也在这一层实现，如 **过程、函数**等。在该层，服务器会解析查询并创建相应的内部解析树，并对其完成相应的优化如确定表的查询的顺序，是否利用索引等，最后生成相应的执行操作。如果是select语句，服务器还会查询内部的缓存，如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。

**3). 引擎层**

存储引擎层， **存储引擎真正的负责了MySQL中数据的存储和提取**，服务器通过API和存储引擎进行通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需要，来选取合适的存储引擎。数据库中的**索引是在存储引擎层实现**的。

**4). 存储层**

数据存储层， 主要是将数据(如: redolog、undolog、数据、索引、二进制日志、错误日志、查询日志、慢查询日志等)**存储在文件系统**之上，并完成**与存储引擎的交互**。

存储引擎架构：

和其他数据库相比，MySQL有点与众不同，它的架构可以在多种不同场景中应用并发挥良好作用。主要体现在存储引擎上，**插件式的存储引擎架构，将查询处理和其他的系统任务以及数据的存储提取分离**。这种架构可以根据业务的需求和实际需要选择合适的存储引擎。

### 存储引擎介绍

**存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式 。**存储引擎是**基于表的**，而不是基于库的，所以存储引擎也可被称为**表类型**。我们可以在创建表的时候，来指定选择的存储引擎，如果没有指定将自动选择默认的存储引擎(InnoDB)。

```sql
show engines;	# 查询当前数据库支持的存储引擎

# 建表时指定存储引擎
CREATE TABLE 表名(
	字段1 字段1类型 [ COMMENT 字段1注释 ] ,
	......
	字段n 字段n类型 [COMMENT 字段n注释 ]
) ENGINE = INNODB [ COMMENT 表注释 ] ;
```

### 存储引擎特点

#### InnoDB

InnoDB是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL 5.5 之后，InnoDB是默认的MySQL 存储引擎。

* DML操作遵循ACID模型，支持**事务**；InnoDB 默认使用的 REPEATABLE-READ（可重读）隔离级别是可以解决幻读问题发生的（基于 MVCC 和 Next-Key Lock）。
* **行级锁，提高并发访问性能；**-- InnoDB 支持行级锁(row-level locking)和表级锁,默认为行级锁。
* 支持外键FOREIGN KEY约束，保证数据的完整性和正确性；
* 使用 InnoDB 的数据库在异常崩溃后，数据库重新启动的时候会保证数据库恢复到崩溃前的状态。这个恢复的过程依赖于 `redo log`。

文件：xxx.ibd：xxx代表的是表名，innoDB引擎的每张表都会对应这样一个表空间文件，存储该表的表结构（frm-早期的 、sdi-新版的）、数据和索引。

##### 逻辑存储结构

<img src="MySQL\image-20240507165406280.png">

* 表空间 : InnoDB存储引擎逻辑结构的最高层，ibd文件其实就是**表空间文件**，在表空间中可以包含多个Segment段。
* 段 : 表空间是由各个段组成的， 常见的段有**数据段、索引段、回滚段**等。InnoDB中对于段的管理，都是引擎自身完成，不需要人为对其控制，一个段中包含多个区。
* 区 : 区是表空间的**单元结构**，每个区的大小为1M。 默认情况下， InnoDB存储引擎页大小为16K， 即一个区中一共有64个连续的页。
* 页 : 页是组成区的最小单元，**页也是InnoDB 存储引擎磁盘管理的最小单元**，每个页的大小默认为 16KB。为了保证页的连续性，InnoDB 存储引擎每次从磁盘申请 4-5 个区。
* 行 : InnoDB 存储引擎是面向行的，也就是说**数据是按行进行存放的**，在每一行中除了定义表时所指定的字段以外，还包含两个隐藏字段(后面会详细介绍)。

#### MyISAM

MyISAM是MySQL早期的默认存储引擎。**不支持事务，不支持外键；支持表锁，不支持行锁。最大的缺陷就是崩溃后无法安全恢复。**

xxx.sdi：存储表结构信息		xxx.MYD: 存储数据		xxx.MYI: 存储索引

#### Memory

Memory引擎的表数据是存储在内存中的，由于受到硬件问题、或断电问题的影响，只能将这些表作临时表或缓存使用。

内存存放； hash索引（默认）

#### 对比

<img src="MySQL\image-20240507171837044.png" alt="image-20240507171837044" style="zoom:80%;" />

### 存储引擎选择

在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合。

* InnoDB: 是Mysql的默认存储引擎，支持事务、外键。如果应用**对事务的完整性有比较高的要求，在并发条件下要求数据的一致性**，数据操作除了插入和查询之外，还包含很多的更新、删除操作，那么InnoDB存储引擎是比较合适的选择。
* MyISAM ： 如果应用是**以读操作和插入操作为主**，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不是很高，那么选择这个存储引擎是非常合适的。
* MEMORY：将所有数据保存在内存中，访问速度快，通常用于**临时表及缓存**。MEMORY的缺陷就是**对表的大小有限制，太大的表无法缓存在内存**中，而且无法保障数据的安全性。

## 索引

索引（index）是帮助MySQL高效获取数据的数据结构(有序)。在数据之外，数据库系统还维护着**满足特定查找算法**的数据结构，这些数据结构**以某种方式引用（指向）数据**， 这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引。

<img src="MySQL\image-20240507172545744.png" alt="image-20240507172545744" style="zoom:80%;" />

大多数情况下，索引查询都是比全表扫描要快的。但是如果数据库的数据量不大，那么使用索引也不一定能够带来很大提升。

### 索引结构

MySQL的索引是在存储引擎层实现的，不同的存储引擎有不同的索引结构，主要包含以下几种：

<img src="MySQL\image-20240507172740796.png" style="zoom:80%;" >

不同的存储引擎对于索引结构的支持情况：

<img src="MySQL\image-20240507173002960.png" style="zoom:80%;" >

我们平常所说的索引，如果没有特别指明，都是指B+树结构组织的索引。

#### 二叉查找树

如果选择二叉树作为索引结构，会存在以下缺点：

* 顺序插入时，会形成一个链表，查询性能大大降低。

* 大数据量情况下，层级较深，检索速度慢。

此时大家可能会想到，我们可以选择红黑树，红黑树是一颗自平衡二叉树，那这样即使是顺序插入数据，最终形成的数据结构也是一颗平衡的二叉树。但是，即使如此，由于红黑树也是一颗二叉树，所以也会存在一个缺点：大数据量情况下，层级较深，检索速度慢。

在MySQL的索引结构中，并没有选择二叉树或者红黑树，而选择的是B+Tree。在详解B+Tree之前，先来介绍一下B-Tree。

#### B-Tree

B-Tree，B树是一种**多路平衡查找树**，相对于二叉树，B树每个节点可以有多个分支，即多叉。以一颗最大度数（max-degree）为5(5阶)的b-tree为例，那这个B树每个节点最多存储4个key，5个指针：

<img src="MySQL\image-20240507222040839.png">

插入一组数据： 100 65 169 368 900 556 780 35 215 1200 234 888 158 90 1000 88 120 268 250 。然后观察一些数据插入过程中，节点的变化情况。

<img src="MySQL\image-20240507223005789.png" style="zoom:80%;" >



特点：

* 5阶的B树，每一个节点最多存储4个key，对应5个指针。

* 一旦节点存储的key数量到达5，就会裂变，中间元素向上分裂。

* 在B树中，非叶子节点和叶子节点都会存放数据。

一棵m阶B树(balanced tree of order m)是一棵平衡的m路搜索树。它或者是空树，或者是满足下列性质的树：

* 根结点至少有两个子女；
* 每个非根节点所包含的关键字个数 j 满足：┌m/2┐ - 1 <= j <= m - 1；
* 除根结点以外的所有结点（不包括叶子结点）的度数正好是关键字总数加1，故内部子树个数 k 满足：┌m/2┐ <= k <= m ；
* 所有的叶子结点都位于同一层。

#### B+Tree

B+Tree是B-Tree的变种，我们以一颗最大度数（max-degree）为4（4阶）的b+tree为例，来看一下其结构示意图：

<img src="MySQL\image-20240507223154029.png" alt="image-20240507223154029" style="zoom:80%;" />

与B树对比：

- B 树的所有节点既存放键(key) 也存放数据(data)，而 B+树只有叶子节点存放 key 和 data，其他内节点只存放 key。
- B 树的叶子节点都是独立的;**B+树的叶子节点有一条引用链指向与它相邻的叶子节点。**
- B 树的检索的过程相当于对范围内的每个节点的关键字做二分查找，可能还没有到达叶子节点，检索就结束了。而 B+树的检索效率就很稳定了，任何查找都是从根节点到叶子节点的过程，叶子节点的顺序检索很明显。
- 在 B 树中进行范围查询时，首先找到要查找的下限，然后对 B 树进行中序遍历，直到找到查找的上限；而 **B+树的范围查询，只需要对链表进行遍历即可。**

综上，B+树与 B 树相比，具备更少的 IO 次数、更稳定的查询效率和更适于范围查询这些优势。

MySQL中优化之后的B+Tree：在原B+Tree的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的B+Tree，提高区间访问的性能，利于排序。

<img src="MySQL\image-20240507235942534.png">

在 MySQL 中，MyISAM 引擎和 InnoDB 引擎都是使用 B+Tree 作为索引结构，但是，两者的实现方式不太一样。（下面的内容整理自《Java 工程师修炼之道》）

> MyISAM 引擎中，B+Tree 叶节点的 data 域存放的是数据记录的地址。在索引检索的时候，首先按照 B+Tree 搜索算法搜索索引，如果指定的 Key 存在，则取出其 data 域的值，然后以 data 域的值为地址读取相应的数据记录。这被称为“**非聚簇索引（非聚集索引）**”。
>
> InnoDB 引擎中，其**数据文件本身就是索引文件**。相比 MyISAM，索引文件和数据文件是分离的，其表数据文件本身就是按 B+Tree 组织的一个索引结构，树的叶节点 data 域保存了完整的数据记录。这个索引的 key 是数据表的主键，因此 InnoDB 表数据文件本身就是主索引。这被称为“**聚簇索引（聚集索引）**”，而其余的索引都作为 **辅助索引** ，辅助索引的 data 域存储相应记录主键的值而不是地址，这也是和 MyISAM 不同的地方。在根据主索引搜索时，直接找到 key 所在的节点即可取出数据；在根据辅助索引查找时，则需要先取出主键的值，再走一遍主索引。 因此，在设计表的时候，不建议使用过长的字段作为主键，也不建议使用非单调的字段作为主键，这样会造成主索引频繁分裂。

#### Hash

哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。

<img src="MySQL\image-20240508002143227.png">

如果两个(或多个)键值，映射到一个相同的槽位上，他们就产生了hash冲突（也称为hash碰撞），可以通过链表来解决。

A. Hash索引只能用于对等比较(=，in)，**不支持范围查询（between，>，< ，...）**

B. **无法利用索引完成排序操作**

C. 查询效率高，通常(不存在hash冲突的情况)只需要一次检索就可以了，效率通常要高于B+tree索引

> MySQL 的 InnoDB 存储引擎不直接支持常规的哈希索引，但是，InnoDB 存储引擎中存在一种特殊的“自适应哈希索引”（Adaptive Hash Index），自适应哈希索引并不是传统意义上的纯哈希索引，而是结合了 B+Tree 和哈希索引的特点，以便更好地适应实际应用中的数据访问模式和性能需求。自适应哈希索引的每个哈希桶实际上是一个小型的 B+Tree 结构。这个 B+Tree 结构可以存储多个键值对，而不仅仅是一个键。这有助于减少哈希冲突链的长度，提高了索引的效率。

**为什么InnoDB存储引擎选择使用B+tree索引结构?**

A. 相对于二叉树，层级更少，搜索效率高；

B. 对于B-tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致**一页中存储的键值减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低；**

C. 相对Hash索引，B+tree支持范围匹配及排序操作；

### 索引分类

#### 按照数据结构维度划分

- BTree 索引：MySQL 里默认和最常用的索引类型。只有叶子节点存储 value，非叶子节点只有指针和 key。存储引擎 MyISAM 和 InnoDB 实现 BTree 索引都是使用 B+Tree，但二者实现方式不一样（前面已经介绍了）。
- 哈希索引：类似键值对的形式，一次即可定位。
- RTree 索引：一般不会使用，仅支持 geometry 数据类型，优势在于范围查找，效率较低，通常使用搜索引擎如 ElasticSearch 代替。
- 全文索引：对文本的内容进行分词，进行搜索。目前只有 `CHAR`、`VARCHAR` ，`TEXT` 列上可以创建全文索引。一般不会使用，效率较低，通常使用搜索引擎如 ElasticSearch 代替。

#### 按照底层存储方式角度划分

- 聚簇索引（聚集索引）：索引结构和数据一起存放的索引，**InnoDB 中的主键索引就属于聚簇索引。**
- 聚集索引选取规则:
  - 如果存在主键，主键索引就是聚集索引。如果不存在主键，将使用第一个唯一（UNIQUE）索引作为聚集索引。
  - 如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引。
- 非聚簇索引（非聚集索引）：索引结构和数据分开存放的索引，索引结构的叶子节点关联的是对应的主键。**二级索引(辅助索引)就属于非聚簇索引**。MySQL 的 MyISAM 引擎，不管主键还是非主键，使用的都是非聚簇索引。
  - 聚集索引的叶子节点下挂的是这一行的数据 。
  - 二级索引的叶子节点下挂的是该字段值对应的主键值。

接下来，我们来分析一下，当我们执行如下的SQL语句时，具体的查找过程是什么样子的。

<img src="MySQL\image-20240508163939785.png" alt="image-20240508163939785" style="zoom:80%;" />

* 由于是根据name字段进行查询，所以先根据name='Arm'到name字段的二级索引中进行匹配查找。但是在二级索引中只能查找到 Arm 对应的主键值 10。
*  由于查询返回的数据是*，所以此时，还需要根据主键值10，到聚集索引中查找10对应的记录，最终找到10对应的行row。
*  最终拿到这一行的数据，直接返回即可。

> 回表查询： 这种先到二级索引中查找数据，找到主键值，然后再到聚集索引中根据主键值，获取数据的方式，就称之为回表查询。

#### 按照应用维度划分

- 主键索引：针对于表中主键创建的索引。默认自动创建。加速查询 + 列值唯一（不可以有 NULL）+ 表中只有一个。
- 普通索引：仅加速查询。快速定位特定数据。又叫常规索引
- 唯一索引：加速查询 + 列值唯一（可以有 NULL）。避免同一个表中某数据列中的值重复，唯一索引可以有多个。
- 覆盖索引：一个索引包含（或者说覆盖）所有需要查询的字段的值。
- 联合索引：多列值组成一个索引，专门用于组合搜索，其效率大于索引合并。
- 全文索引：对文本的内容进行分词，进行搜索。目前只有 `CHAR`、`VARCHAR` ，`TEXT` 列上可以创建全文索引。一般不会使用，效率较低，通常使用搜索引擎如 ElasticSearch 代替。

#### MySQL 8.x 中实现的索引新特性

- 隐藏索引：也称为不可见索引，不会被优化器使用，但是仍然需要维护，通常会软删除和灰度发布的场景中使用。主键不能设置为隐藏（包括显式设置或隐式设置）。
- 降序索引：之前的版本就支持通过 desc 来指定索引为降序，但实际上创建的仍然是常规的升序索引。直到 MySQL 8.x 版本才开始真正支持降序索引。另外，在 MySQL 8.x 版本中，不再对 GROUP BY 语句进行隐式排序。
- 函数索引：从 MySQL 8.0.13 版本开始支持在索引中使用函数或者表达式的值，也就是**在索引中可以包含函数或者表达式。**

### 索引语法

```sql
# 创建（唯一/全文）索引
CREATE [ UNIQUE | FULLTEXT ] INDEX index_name ON table_name (index_col_name,... ) ;
# 查看索引
SHOW INDEX FROM table_name ;
# 删除索引
DROP INDEX index_name ON table_name ;
```

### SQL性能分析

MySQL 客户端连接成功后，通过 show [session|global] status 命令可以提供服务器状态信息。通过如下指令，可以查看当前数据库的INSERT、UPDATE、DELETE、SELECT的访问频次。

```sql
-- session 是查看当前会话 ;
-- global 是查询全局数据 ;
SHOW GLOBAL STATUS LIKE 'Com_______';
```

<img src="MySQL\image-20240508165617129.png">

#### 慢查询日志

慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句的日志。

MySQL的慢查询日志默认没有开启，我们可以查看一下系统变量 slow_query_log。

不建议开启，因为开启会带来一定的性能影响

<img src="MySQL\image-20240508165730121.png">

```sql
# 开启慢查询日志，只对当前数据库生效，并且重启数据库后失效
set global slow_query_log = 1;
# 设置阈值
set long_query_time = 3;

# 得到返回记录集最多的10 个SQL
./mysqldumpslow -s r -t 10 /data/mysql/logs/slow.log

# 得到访问次数最多的10 个SQL
./mysqldumpslow -s c -t 10 /data/mysql/logs/slow.log

# 得到按照时间排序的前10 条里面含有左连接的查询语句
./mysqldumpslow -s t -t 10 -g "left join" /data/mysql/logs/slow.log

# 另外建议在使用这些命令时结合| 和more 使用，否则有可能出现爆屏情况
./mysqldumpslow -s r -t 10 /data/mysql/logs/slow.log | more
```

#### explain

EXPLAIN 或者 DESC命令获取 MySQL 如何执行 SELECT 语句的信息，包括在 SELECT 语句执行过程中表如何连接和连接的顺序。

```sql
-- 直接在select语句之前加上关键字 explain / desc
EXPLAIN SELECT 字段列表 FROM 表名 WHERE 条件 ;
```

<img src="MySQL\image-20240508214501370.png" style="zoom:80%;" >

Explain 执行计划中各个字段的含义:

<img src="MySQL\image-20240508214613100.png" style="zoom:80%;" >

### 索引使用

#### 最左前缀法则

如果索引了多列（联合索引），要遵守最左前缀法则。**最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列。如果跳跃某一列，或者中间使用了范围查询，索引将会部分失效(后面的字段索引失效)。**

索引的底层是一颗B+树，那么联合索引的底层也就是一颗B+树，只不过联合索引的B+树节点中存储的是键值。由于构建一棵B+树只能根据一个值来确定索引关系，所以数据库依赖联合索引最左的字段来构建。

举例：创建一个（a,b）的联合索引，那么它的索引树就是下图的样子。

<img src="MySQL\image-20240508214330139.png" alt="image-20240508214330139" style="zoom:67%;" />

可以看到a的值是有顺序的，1，1，2，2，3，3，而b的值是没有顺序的1，2，1，4，1，2。但是我们又可发现a在等值的情况下，b值又是按顺序排列的，但是这种顺序是相对的。这是因**为MySQL创建联合索引的规则是首先会对联合索引的最左边第一个字段排序，在第一个字段的排序基础上，然后在对第二个字段进行排序。所以b=2这种查询条件没有办法利用索引。**

##### 一些要点

* 建立联合索引（a，b，c）时，实际相当于建立了（a），（b），（c）三个索引
* 全值匹配时，无论顺序abc还是cba，在查询时都使用到了联合索引。这是因为MySQL中有查询优化器explain，所以sql语句中字段的顺序不需要和联合索引定义的字段顺序相同，查询优化器会判断纠正这条SQL语句以什么样的顺序执行效率高，最后才能生成真正的执行计划。
  * explain对这种查询的结果显示为type=ref，表示非唯一性索引扫描，返回匹配某个单独值的所有行。
  * 注意这种情况下，最左侧必须是**等值匹配**，因为只有左边等值情况下，右边条件才有序
* 匹配最左边的1个或多个列时，也是ref，能够利用联合索引。
* 如果中间跳过几个字段，那么只能索引到有序的位置为止。
* 如果匹配到b或者bc这种条件（跳过最左边），explain也显示用了abc，**但type是index**
  * index：这种类型表示是**mysql会对整个该索引进行扫描**。要想用到这种类型的索引，对这个索引并无特别要求，只要是索引，或者某个复合索引的一部分，mysql都可能会采用index类型的方式扫描。但是呢，缺点是效率不高，mysql会从索引中的第一个数据一个个的查找到最后一个数据，直到找到符合判断条件的某个索引
  * 它虽然使用了联合索引，但是它是对整个索引树进行了扫描，正好匹配到该索引，与最左匹配原则无关，一般只要是某联合索引的一部分，但又不遵循最左匹配原则时，都可能会采用index类型的方式扫描，但它的效率远不如最左匹配原则的查询效率高

#### 索引失效情况

* **索引列运算**：不要在索引列上进行运算操作， 索引将失效。

```sql
#  当根据phone字段进行等值匹配查询时, 索引生效。
explain select * from tb_user where phone = '17799990015';
#  当根据phone字段进行函数运算操作之后，索引失效。
explain select * from tb_user where substring(phone,10,2) = '15';
```

* **字符串不加引号**：字符串类型字段使用时，不加引号，索引将失效。（如果字符串不加单引号，对于查询结果，没什么影响，但是数据库存在隐式类型转换，索引将失效。）

<img src="MySQL\image-20240508221636173.png">

* **模糊查询**：如果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，索引失效。

```sql
explain select * from tb_user where profession like '软件%';
#  下面的失效
explain select * from tb_user where profession like '%工程';
explain select * from tb_user where profession like '%工%';
```

*  **or连接条件**：用or分割开的条件， 如果or前的条件中的列有索引，而后面的列中没有索引，那么涉及的索引都不会被用到。

```sql
# 由于age没有索引，所以即使id、phone有索引，索引也会失效。当or连接的条件，左右两侧字段都有索引时，索引才会生效。
explain select * from tb_user where id = 10 or age = 23;
explain select * from tb_user where phone = '17799990017' or age = 23;
```

* **数据分布影响**：如果MySQL评估使用索引比全表更慢，则不使用索引。

> MySQL在查询时，会评估使用索引的效率与走全表扫描的效率，如果走全表扫描更快，则放弃索引，走全表扫描。 因为索引是用来索引少量数据的，**如果通过索引查询返回大批量的数据，则还不如走全表扫描来的快，此时索引就会失效。**

#### SQL提示

SQL提示，是优化数据库的一个重要手段，简单来说，就是在SQL语句中加入一些人为的提示来达到优化操作的目的。

```sql
# use index ： 建议MySQL使用哪一个索引完成此次查询（仅仅是建议，mysql内部还会再次进行评估）。
explain select * from tb_user use index(idx_user_pro) where profession = '软件工程';
# ignore index ： 忽略指定的索引。
explain select * from tb_user ignore index(idx_user_pro) where profession = '软件工程';
# force index ： 强制使用索引。
explain select * from tb_user force index(idx_user_pro) where profession = '软件工程';
```

### 覆盖索引

尽量使用覆盖索引，减少select *。 那么什么是覆盖索引呢？ **覆盖索引是指查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到 。**

> 在tb_user表中有一个联合索引 idx_user_pro_age_sta，该索引关联了三个字段profession、age、status，而这个索引也是一个二级索引，所以叶子节点下面挂的是这一行的主键id。 所以当我们查询返回的数据在 id、profession、age、status 之中，则直接走二级索引直接返回数据了。 **如果超出这个范围，就需要拿到主键id，再去扫描聚集索引，再获取额外的数据了**，这个过程就是回表。 而我们如果一直使用select * 查询返回所有字段值，很容易就会造成回表查询（除非是根据主键查询，此时只会扫描聚集索引）。

### 前缀索引

当字段类型为字符串（varchar，text，longtext等）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO， 影响查询效率。此时可以只将字符串的一部分前缀，建立索引，这样可以大大节约索引空间，从而提高索引效率。****

```sql
create index idx_xxxx on table_name(column(n)) ; 
# 为tb_user表的email字段，建立长度为5的前缀索引。
create index idx_email_5 on tb_user(email(5));
```

**前缀长度**：可以根据索引的选择性来决定，而选择性是指不重复的索引值（基数）和数据表的记录总数的比值，索引选择性越高则查询效率越高， 唯一索引的选择性是1，这是最好的索引选择性，性能也是最好的。

```
select count(distinct email) / count(*) from tb_user ;
# 前缀为5情况下索引选择性
select count(distinct substring(email,1,5)) / count(*) from tb_user ;
```

<img src="MySQL\image-20240509162814392.png" style="zoom:80%;" >

### 索引设计原则

1). 针对于数据量较大，且查询比较频繁的表建立索引。

2). 针对于常作为查询条件（where）、排序（order by）、分组（group by）操作的字段建立索引。

3). 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高。

4). 如果是字符串类型的字段，字段的长度较长，可以针对于字段的特点，建立前缀索引。

5). 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率。

6). 要控制索引的数量，索引并不是多多益善，**索引越多，维护索引结构的代价也就越大，会影响增删改的效率。**

7). 如果索引列不能存储NULL值，请在创建表时使用NOT NULL约束它。当优化器知道每列是否包含NULL值时，它可以更好地确定哪个索引最有效地用于查询。

## SQL优化

### 插入数据

如果我们需要一次性往数据库表中**插入多条记录**，可以从以下三个方面进行优化。

```sql
# 方案1，批量插入
Insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
# 方案2，手动控制事务
start transaction;
insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
insert into tb_test values(4,'Tom'),(5,'Cat'),(6,'Jerry');
insert into tb_test values(7,'Tom'),(8,'Cat'),(9,'Jerry');
commit;
# 方案3，主键顺序插入，性能要高于乱序插入
主键乱序插入 : 8 1 9 21 88 2 4 15 89 5 7 3
主键顺序插入 : 1 2 3 4 5 7 8 9 15 21 88 89
```

#### 大批量插入数据

如果一次性需要插入大批量数据(比如: 几百万的记录)，使用insert语句插入性能较低，此时可以使用MySQL数据库提供的load指令进行插入。

<img src="MySQL\image-20240510111007454.png">

```sql
# 可以执行如下指令，将数据脚本文件中的数据加载到表结构中：
-- 客户端连接服务端时，加上参数 -–local-infile
mysql –-local-infile -u root -p
-- 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
set global local_infile = 1;
-- 执行load指令将准备好的数据，加载到表结构中
load data local infile '/root/sql1.log' into table tb_user fields terminated by ',' lines terminated by '\n';
```

### 主键优化

**数据组织方式**：在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表(index organized table IOT)。

在InnoDB引擎中，数据行是记录在逻辑结构 page 页中的，而每一个页的大小是固定的，默认16K。那也就意味着， 一个页中所存储的行也是有限的，如果插入的数据行row在该页存储不下，将会存储到下一个页中，页与页之间会通过指针连接。

#### 页分裂

页可以为空，也可以填充一半，也可以填充100%。每个页包含了2-N行数据(如果一行数据过大，会行溢出)，根据主键排列。

主键顺序插入时，直接从磁盘中申请页，逐个写入即可。乱序插入时，假如1#，2#页已经写满，存放如下数据。

<img src="MySQL\image-20240510112438937.png">

此时插入id=50的记录时，

<img src="MySQL\image-20240510112619060.png" style="zoom:80%;" >

上述的这种现象，称之为 "页分裂"，是比较耗费性能的操作。

#### 页合并

目前表中已有数据的索引结构(叶子节点)如下

<img src="MySQL\image-20240510112833127.png">

<img src="MySQL\image-20240510112916942.png" style="zoom:80%;" >

<img src="MySQL\image-20240510113018365.png" alt="image-20240510113018365" style="zoom:80%;" />

这个里面所发生的合并页的这个现象，就称之为 "页合并"。

#### 索引设计优化

* 满足业务需求的情况下，尽量降低主键的长度。
* 插入数据时，尽量选择顺序插入，选择使用AUTO_INCREMENT自增主键。
* 尽量不要使用UUID做主键或者是其他自然主键，如身份证号。
* 业务操作时，避免对主键的修改。

### order by优化

MySQL的排序，有两种方式：

Using filesort : 通过表的索引或全表扫描，读取满足条件的数据行，然后**在排序缓冲区sort buffer中完成排序操作**，所有**不是通过索引直接返回排序结果的排序都叫 FileSort 排序**。

Using index : 通过**有序索引顺序扫描直接返回有序数据**，这种情况即为 using index，不需要额外排序，操作效率高。

对于以上的两种排序方式，Using index的性能高，而Using filesort的性能低，我们在优化排序操作时，尽量要优化为 Using index。

**order by优化原则:**

A. 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则。

B. 尽量使用覆盖索引。

C. 多字段排序, 一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）。

D. 如果不可避免的出现filesort，大数据量排序时，可以适当增大排序缓冲区大小sort_buffer_size(默认256k)。

### group by优化

A. 在分组操作时，可以通过索引来提高效率。

B. 分组操作时，索引的使用也是满足最左前缀法则的。

### limit优化

在数据量比较大时，如果进行limit分页查询，在查询时，越往后，分页查询效率越低。

<img src="MySQL\image-20240510114353842.png">

当在进行分页查询时，如果执行 limit 2000000,10 ，此时需要MySQL排序前2000010 记录，仅仅返回 2000000 - 2000010 的记录，其他记录丢弃，查询排序的代价非常大 。

优化思路: 一般分页查询时，通过创建 **覆盖索引** 能够比较好地提高性能，可以通过覆盖索引加子查询形式进行优化。

```sql
explain select * from tb_sku t , (select id from tb_sku order by id limit 2000000,10) a where t.id = a.id;
```

### count优化

在之前的测试中，我们发现，如果数据量很大，在执行count操作时，是非常耗时的。

MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行 count(*) 的时候会直接返回这个数，效率很高； 但是如果是带条件的count，MyISAM也慢。

InnoDB 引擎就麻烦了，它执行 count(*) 的时候，**需要把数据一行一行地从引擎里面读出来，然后累积计数。**

如果说要大幅度提升InnoDB表的count效率，主要的优化思路：**自己计数(可以借助于redis这样的数据库进行**,但是如果是带条件的count又比较麻烦了)。

<img src="MySQL\image-20240510114710602.png" style="zoom:80%;" >

按照效率排序的话，count(字段) < count(主键 id) < count(1) ≈ count(\*)，所以尽量使用 count(\*)。

### update优化

**InnoDB的行锁是针对索引加的锁，不是针对记录加的锁 ,并且该索引不能失效，否则会从行锁升级为表锁 。**

例如：

```sql
# 当我们在执行删除的SQL语句时，会锁定id为1这一行的数据，然后事务提交之后，行锁释放。
update course set name = 'javaEE' where id = 1 ; 
# 当我们开启多个事务，在执行上述的SQL时，我们发现行锁升级为了表锁。 导致该update语句的性能大大降低。
update course set name = 'SpringBoot' where name = 'PHP' ;
```

## 视图、存储过程、触发器

### 视图

视图（View）是一种**虚拟存在**的表。视图中的数据并不在数据库中实际存在，**行和列数据来自定义视图的查询中使用的表**，并且是在使用视图时**动态生成**的。

通俗的讲，视图只保存了查询的**SQL逻辑**，不保存查询结果。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。

```sql
# 创建
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [CASCADED | LOCAL ] CHECK OPTION ]

# 查询
查看创建视图语句：SHOW CREATE VIEW 视图名称;
查看视图数据：SELECT * FROM 视图名称 ...... ;

# 修改
方式一：CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]
方式二：ALTER VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]

# 删除
DROP VIEW [IF EXISTS] 视图名称 [,视图名称] ...
```

#### 视图作用

1). 简单

视图不仅可以简化用户对数据的理解，也可以简化他们的操作。**那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件。**

2). 安全

数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图用户只**能查询和修改他们所能见到的数据**

3). 数据独立

视图可帮助用户**屏蔽真实表结构变化带来的影响**。

### 存储过程

存储过程是**事先经过编译并存储在数据库中的一段 SQL 语句的集合**，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。存储过程思想上很简单，就是**数据库 SQL 语言层面的代码封装与重用。**

* 封装，复用 ----------------> 可以把某一业务SQL封装在存储过程中，需要用到的时候直接调用即可。

* 可以接收参数，也可以返回数据 --------> 再存储过程中，可以传递参数，也可以接收返回值。

* **减少网络交互**，效率提升 -------------> 如果涉及到多条SQL，每执行一次都是一次网络传输。 而如果封装在存储过程中，我们只需要网络交互一次可能就可以了。

```sql
# 创建
CREATE PROCEDURE 存储过程名称 ([ 参数列表 ])
BEGIN
-- SQL语句
END ;

# 调用
CALL 名称 ([ 参数 ]);

# 查看
SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA = 'xxx'; -- 查询指定数据库的存储过程及状态信息
SHOW CREATE PROCEDURE 存储过程名称 ; -- 查询某个存储过程的定义

# 删除
DROP PROCEDURE [ IF EXISTS ] 存储过程名称;
```

#### 变量

在MySQL中变量分为三种类型: 系统变量、用户定义变量、局部变量。

**系统变量** 是MySQL服务器提供，不是用户定义的，属于服务器层面。分为全局变量（GLOBAL）、会话变量（SESSION）。

```sql
SHOW [ SESSION | GLOBAL ] VARIABLES ; -- 查看所有系统变量
SHOW [ SESSION | GLOBAL ] VARIABLES LIKE '......'; -- 可以通过LIKE模糊匹配方式查找变量
SELECT @@[SESSION | GLOBAL] 系统变量名; -- 查看指定变量的值

SET [ SESSION | GLOBAL ] 系统变量名 = 值 ;  -- 设置系统变量，如果没有指定SESSION/GLOBAL，默认是SESSION，会话变量。
SET @@[SESSION | GLOBAL]系统变量名 = 值 ;
```

**用户定义变量** 是用户根据需要自己定义的变量，**用户变量不用提前声明，在用的时候直接用 "@变量名" 使用就可以**。其作用域为当前连接。

```sql
# 赋值 方式1
SET @var_name = expr [, @var_name = expr] ... ;
SET @var_name := expr [, @var_name := expr] ... ;
# 方式2
SELECT @var_name := expr [, @var_name := expr] ... ;
SELECT 字段名 INTO @var_name FROM 表名;
# 使用
SELECT @var_name ; 1
```

**局部变量** 是根据需要定义的在局部生效的变量，访问之前，**需要DECLARE声明**。可用作存储过程内的**局部变量和输入参数**，局部变量的范围是在其内声明的BEGIN ... END块。

```sql
DECLARE 变量名 变量类型 [DEFAULT ... ] ;  -- 变量类型就是数据库字段类型：INT、BIGINT、CHAR、VARCHAR、DATE、TIME等。
SET 变量名 = 值 ;
SET 变量名 := 值 ;
SELECT 字段名 INTO 变量名 FROM 表名 ... ;
```

#### if案例

根据定义的分数score变量，判定当前分数对应的分数等级。

```sql
create procedure p3()
begin
    declare score int default 58;
    declare result varchar(10);
    if score >= 85 then
    	set result := '优秀';
    elseif score >= 60 then
    	set result := '及格';
    else
    	set result := '不及格';
    end if;
    select result;
end;
call p3();
```

#### 参数

IN ：该类参数作为输入，也就是需要调用时传入值 	默认

OUT ：该类参数作为输出，也就是该参数可以作为返回值

INOUT ：既可以作为输入参数，也可以作为输出参数

```sql
CREATE PROCEDURE 存储过程名称 ([ IN/OUT/INOUT 参数名 参数类型 ])
BEGIN
	-- SQL语句
END ;

# 案例
create procedure p4(in score int, out result varchar(10))
begin
	if score xxxx
end;
-- 定义用户变量 @result来接收返回的数据, 用户变量可以不用声明
call p4(18, @result);
select @result;
```

#### 循环

```sql
-- 含义： 当case_value的值为 when_value1时，执行statement_list1，当值为 when_value2时，
执行statement_list2， 否则就执行 statement_list
CASE case_value
    WHEN when_value1 THEN statement_list1
    [ WHEN when_value2 THEN statement_list2] ...
    [ ELSE statement_list ]
END CASE;
-- 含义： 当条件search_condition1成立时，执行statement_list1，当条件search_condition2成立时，执行statement_list2， 否则就执行 statement_list
CASE
    WHEN search_condition1 THEN statement_list1
    [WHEN search_condition2 THEN statement_list2] ...
    [ELSE statement_list]
END CASE;

-- 先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
WHILE 条件 DO
	SQL逻辑...
END WHILE;

-- 先执行一次逻辑，然后判定UNTIL条件是否满足，如果满足，则退出。如果不满足，则继续下一次循环
REPEAT
    SQL逻辑...
    UNTIL 条件
END REPEAT;

[begin_label:] LOOP
    SQL逻辑...
END LOOP [end_label];
LEAVE label; -- 退出指定标记的循环体
ITERATE label; -- 直接进入下一次循环
```

#### 游标

游标（CURSOR）是用来存储查询结果集的数据类型 , 在存储过程和函数中可以使用游标对结果集进行循环的处理。游标的使用包括游标的声明、OPEN、FETCH 和 CLOSE，其语法分别如下。

```sql
DECLARE 游标名称 CURSOR FOR 查询语句 ;
OPEN 游标名称 ; 		-- 打开游标
FETCH 游标名称 INTO 变量 [, 变量 ] ;	-- 获取游标记录
CLOSE 游标名称 ;
```

#### 条件处理程序

条件处理程序（Handler）可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤。

```sql
DECLARE handler_action HANDLER FOR condition_value [, condition_value]... statement ;

handler_action 的取值：
    CONTINUE: 继续执行当前程序
    EXIT: 终止执行当前程序
condition_value 的取值：
    SQLSTATE sqlstate_value: 状态码，如 02000
    SQLWARNING: 所有以01开头的SQLSTATE代码的简写
    NOT FOUND: 所有以02开头的SQLSTATE代码的简写
    SQLEXCEPTION: 所有没有被SQLWARNING 或 NOT FOUND捕获的SQLSTATE代码的简写
```

### 存储函数

存储函数是有返回值的存储过程，存储函数的参数只能是IN类型的。具体语法如下：

```sql
CREATE FUNCTION 存储函数名称 ([ 参数列表 ])
RETURNS type [characteristic ...]
BEGIN
    -- SQL语句
    RETURN ...;
END ;
```

characteristic说明：

* DETERMINISTIC：相同的输入参数总是产生相同的结果
* NO SQL ：不包含 SQL 语句。
* READS SQL DATA：包含读取数据的语句，但不包含写入数据的语句。

### 触发器

触发器是与表有关的数据库对象，指**在insert/update/delete之前(BEFORE)或之后(AFTER)，触发并执行触发器中定义的SQL语句集合**。触发器的这种特性可以协助应用在数据库端确保**数据的完整性, 日志记录 , 数据校验**等操作 。

使用别名OLD和NEW来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发。

<img src="MySQL\image-20240510163048010.png" style="zoom:67%;" >

```sql
# 创建触发器
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE
ON tbl_name FOR EACH ROW -- 行级触发器
BEGIN
	trigger_stmt ;
END;

SHOW TRIGGERS ;
DROP TRIGGER [schema_name.]trigger_name ; -- 如果没有指定 schema_name，默认为当前数据库 。

# 案例：通过触发器记录 tb_user 表的数据变更日志，将变更日志插入到日志表user_logs中, 包含增加,修改 , 删除 ;
create trigger tb_user_insert_trigger after insert on tb_user for each row
begin
    insert into user_logs(id, operation, operate_time, operate_id, operate_params)
    VALUES
    (null, 'insert', now(), new.id, concat('插入的数据内容为:
    id=',new.id,',name=',new.name, ', phone=', NEW.phone, ', email=', NEW.email, ',
    profession=', NEW.profession));
end;

# 修改数据 的触发器
create trigger tb_user_update_trigger after update on tb_user for each row
begin
    insert into user_logs(id, operation, operate_time, operate_id, operate_params)
    VALUES
    (null, 'update', now(), new.id,
    concat('更新之前的数据: id=',old.id,',name=',old.name, ', phone=',
    old.phone, ', email=', old.email, ', profession=', old.profession,
    ' | 更新之后的数据: id=',new.id,',name=',new.name, ', phone=',
    NEW.phone, ', email=', NEW.email, ', profession=', NEW.profession));
end;

# 删除
create trigger tb_user_delete_trigger after delete on tb_user for each row
begin
    insert into user_logs(id, operation, operate_time, operate_id, operate_params)
    VALUES
    (null, 'delete', now(), old.id,
    concat('删除之前的数据: id=',old.id,',name=',old.name, ', phone=',
    old.phone, ', email=', old.email, ', profession=', old.profession));
end;
```

## 锁

**锁是计算机协调多个进程或线程并发访问某一资源的机制。**在数据库中，除传统的计算资源（CPU、RAM、I/O）的争用以外，数据也是一种供许多用户共享的资源。如何**保证数据并发访问的一致性、有效性**是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。

MySQL中的锁，按照锁的粒度分，分为以下三类：

### 全局锁

* **全局锁**：锁定数据库中的所有表。全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML的写语句，DDL语句，已经更新操作的事务提交语句都将被阻塞。其典型的使用场景是**做全库的逻辑备份，对所有的表进行锁定，从而获取一致性视图，保证数据的完整性**。

  ```sql
  flush tables with read lock ;	-- 加全局锁
  mysqldump -uroot –p1234 itcast > itcast.sql	-- 数据备份
  unlock tables ;		-- 释放锁
  ```

  数据库中加全局锁，是一个比较重的操作，存在以下问题：

  * 如果在主库上备份，那么在备份期间都不能执行更新，业务基本上就得停摆。
  * 如果在从库上备份，那么在备份期间从库不能执行主库同步过来的二进制日志（binlog），会导致主从延迟。

  在InnoDB引擎中，我们可以在备份时加上参数 --single-transaction 参数来完成不加锁的一致性数据备份。

  ```sql
  mysqldump --single-transaction -uroot –p123456 itcast > itcast.sql
  ```

### 表级锁

* **表级锁**：每次操作锁住整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低。应用在MyISAM、InnoDB、BDB等存储引擎中。

  表级锁主要分为：表锁，元数据锁，意向锁

#### 表锁

对于表锁，分为两类：**表共享读锁（read lock）**--只阻塞写，**表独占写锁（write lock）**--阻塞读写

语法：**加锁：lock tables 表名... read/write。**	**释放锁：unlock tables** / 客户端断开连接 。

结论: 读锁不会阻塞其他客户端的读，但是会阻塞写。写锁既会阻塞其他客户端的读，又会阻塞其他客户端的写。

#### 元数据锁

meta data lock , 元数据锁，简写MDL。

MDL加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上。MDL锁主要作用是维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。**为了避免DML与DDL冲突，保证读写的正确性。**

这里的元数据，大家可以简单理解为就是一张表的表结构。 也就是说，**某一张表涉及到未提交的事务时，是不能够修改这张表的表结构**的。在MySQL5.5中引入了MDL，**当对一张表进行增删改查的时候，加MDL读锁(共享)；当对表结构进行变更操作的时候，加MDL写锁(排他)。**

常见的SQL操作时，所添加的元数据锁：

<img src="MySQL\image-20240510171058496.png" style="zoom:80%;" >

#### 意向锁

为了避免DML在执行时，加的行锁与表锁的冲突，在InnoDB中引入了意向锁，**使得表锁不用检查每行数据是否加锁，使用意向锁来减少表锁的检查。**

假如没有意向锁，客户端一对表加了行锁后，客户端二如何给表加表锁呢：

1.首先客户端一，开启一个事务，然后执行DML操作，在执行DML语句时，会对涉及到的行加行锁

2.当客户端二，想对这张表加表锁时，会检查当前表是否有对应的行锁，如果没有，则添加表锁，此时就会**从第一行数据，检查到最后一行数据，效率较低。**

有了意向锁之后 :

1.客户端一，在执行DML操作时，会对涉及的行加行锁，同时也会对**该表**加上意向锁。

2.而其他客户端，在对这张表加表锁的时候，会**根据该表上所加的意向锁来判定是否可以成功加表锁**，而不用逐行判断行锁情况了。

分类：

* **意向共享锁(IS):** 由语句**select ... lock in share mode**添加 。 与 **表锁共享锁 (read)兼容**，与表锁排他锁(write)互斥。

* **意向排他锁(IX):** 由insert、update、delete、select...**for update**添加 。与**表锁共享锁(read)及排他锁(write)都互斥**，**意向锁之间不会互斥。**

> 一旦事务提交了，意向共享锁、意向排他锁，都会自动释放。

可以通过以下SQL，查看意向锁及行锁的加锁情况：

```sql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```

### 行级锁

* **行级锁**：每次操作锁住对应的行数据。锁定粒度最小，发生锁冲突的概率最低，并发度最高。应用在InnoDB存储引擎中

InnoDB的数据是基于索引组织的，**行锁是通过对索引上的索引项加锁来实现的**，而不是对记录加的锁。对于行级锁，主要分为以下三类：

<img src = "MySQL\image-20240510173950346.png" style="zoom:80%;" >

## 参考

黑马程序员MySQL：https://www.bilibili.com/video/BV1Kr4y1i7ru/?vd_source=8f6745987f6d9c4a333570852e433d6c

JavaGuide：https://javaguide.cn/database/mysql/mysql-questions-01.html

最左前缀匹配原则：https://blog.csdn.net/yuanchangliang/article/details/107798724