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

<img src="https://oss.javaguide.cn/github/javaguide/database/mongodb/sql-nosql-tushi.png" alt="img"  />

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

## 参考

黑马程序员MySQL：https://www.bilibili.com/video/BV1Kr4y1i7ru/?vd_source=8f6745987f6d9c4a333570852e433d6c

JavaGuide：https://javaguide.cn/database/sql/sql-syntax-summary.html#%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5

