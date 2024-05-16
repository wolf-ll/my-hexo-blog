---
title: redis
date: 2024-05-11 14:14:39
auther: 小狼
summary: Redis数据库基础与原理
categories: 后端
tags:
  - 数据库
  - Redis
---

# Redis

使用阿里云hit账号数据库，密码首字母大写

Redis诞生于2009年全称是**Re**mote  **D**ictionary **S**erver 远程词典服务器，是一个基于内存的键值型NoSQL数据库。

**特征**：

- 键值（key-value）型，value支持多种不同数据结构，功能丰富
- **单线程**，每个命令具备原子性
- 低延迟，速度快（**基于内存**、IO多路复用、良好的编码）。
- 支持数据持久化
- 支持主从集群、分片集群
- 支持多语言客户端

## 认识NoSQL

Redis是一种键值型的NoSql数据库，这里有两个关键字：

- 键值型

- NoSql

其中**键值型**，是指Redis中存储的数据都是以key、value对的形式存储，而value的形式多种多样，可以是字符串、数值、甚至json

**NoSql**可以翻译做Not Only Sql（不仅仅是SQL），或者是No Sql（非Sql的）数据库。也称为**非关系型数据库**。

sql vs nosql：

* 传统关系型数据库是结构化数据，每一张表都有严格的约束信息：**字段名、字段数据类型、字段约束**等等信息，插入的数据必须遵守这些约束。而NoSql则对数据库格式没有严格约束，往往**形式松散，自由**。可以是键值型，也可以是文档型或者图格式。

* 传统数据库的表与表之间往往存在关联，例如外键。而非关系型数据库不存在关联关系，要维护关系要么靠代码中的业务逻辑，要么靠数据之间的耦合：

  ```json
  {
    id: 1,
    name: "张三",
    orders: [
      {
         id: 1,
         item: {
  	 id: 10, title: "荣耀6", price: 4999
         }
      },
      {
         id: 2,
         item: {
  	 id: 20, title: "小米11", price: 3999
         }
      }
    ]
  }
  ```

* 传统关系型数据库会基于Sql语句做查询，语法有统一标准；而不同的非关系数据库查询语法差异极大，五花八门各种各样。

  <img src="redis\AzaHOTF.png" style="zoom:67%;" >

* 传统关系型数据库能满足事务ACID的原则。而非关系型数据库往往不支持事务，或者不能严格保证ACID的特性，只能实现**基本的一致性**。

除了上述四点以外，在存储方式、扩展性、查询性能上关系型与非关系型也都有着显著差异，总结如下：

<img src="redis\1.png" style="zoom: 67%;" >

- 存储方式
  - 关系型数据库基于磁盘进行存储，会有大量的**磁盘IO**，对性能有一定影响
  - 非关系型数据库，他们的操作更多的是**依赖于内存来操作，内存的读写速度会非常快**，性能自然会好一些

* 扩展性
  * 关系型数据库集群模式一般是主从，主从数据一致，起到数据备份的作用，称为垂直扩展。
  * 非关系型数据库可以**将数据拆分，存储在不同机器上，可以保存海量数据，解决内存大小有限的问题。称为水平扩展**。
  * 关系型数据库因为表之间存在关联关系，如果做水平扩展会给数据查询带来很多麻烦

## 初识redis

### linux启动

#### 默认启动

安装完成后，在任意目录输入redis-server命令即可启动Redis

<img src="redis\v7xWsqC.png" style="zoom:67%;" >

这种启动属于`前台启动`，会阻塞整个会话窗口，窗口关闭或者按下`CTRL + C`则Redis停止。不推荐使用。

#### 指定配置启动

如果要让Redis以`后台`方式启动，则必须修改Redis配置文件，就在我们之前解压的redis安装包下（`/usr/local/src/redis-6.2.6`），名字叫redis.conf

我们先将这个配置文件备份一份：

```
cp redis.conf redis.conf.bck
```

**然后修改redis.conf文件中的一些配置：**

```properties
# 允许访问的地址，默认是127.0.0.1，会导致只能在本地访问。修改为0.0.0.0则可以在任意IP访问，生产环境不要设置为0.0.0.0
bind 0.0.0.0
# 守护进程，修改为yes后即可后台运行
daemonize yes 
# 密码，设置后访问Redis必须输入密码
requirepass 123321
```

Redis的其它常见配置：

```properties
# 监听的端口
port 6379
# 工作目录，默认是当前目录，也就是运行redis-server时的命令，日志、持久化等文件会保存在这个目录
dir .
# 数据库数量，设置为1，代表只使用1个库，默认有16个库，编号0~15
databases 1
# 设置redis能够使用的最大内存
maxmemory 512mb
# 日志文件，默认为空，不记录日志，可以指定日志文件名
logfile "redis.log"
```

**启动Redis：**

```sh
# 进入redis安装目录 
cd /usr/local/src/redis-6.2.6
# 启动
redis-server redis.conf
```

停止服务：

```sh
# 利用redis-cli来执行 shutdown 命令，即可停止 Redis 服务，
# 因为之前配置了密码，因此需要通过 -u 来指定密码
redis-cli -u 123321 shutdown
```

<img src="redis\image-20240512130026010.png" style="zoom:67%;" >

#### 开机自启

我们也可以通过配置来实现开机自启。

首先，新建一个系统服务文件：

```sh
vi /etc/systemd/system/redis.service
```

内容如下：

```conf
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /usr/local/src/redis-6.2.6/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

**然后重载系统服务：**

```sh
systemctl daemon-reload
```

现在，我们可以用下面这组命令来操作redis了：

```sh
# 启动
systemctl start redis
# 停止
systemctl stop redis
# 重启
systemctl restart redis
# 查看状态
systemctl status redis
```

**执行下面的命令，可以让redis开机自启：**

```sh
systemctl enable redis
```

### Redis桌面客户端

#### 命令行客户端

Redis安装完成后就自带了命令行客户端：redis-cli，使用方式如下：

```sh
redis-cli [options] [commonds]
```

其中常见的options有：

- `-h 127.0.0.1`：指定要连接的redis节点的**IP地址**，默认是127.0.0.1
- `-p 6379`：指定要连接的redis节点的端口，默认是6379
- `-a 123321`：指定redis的访问密码 

其中的commonds就是Redis的操作命令，例如：

- `ping`：与redis服务端做心跳测试，服务端正常会返回`pong`

**不指定commond时，会进入`redis-cli`的交互控制台**

<img src="redis\OYYWPNo.png" style="zoom:80%;" >

#### 图形化客户端

RedisDesktopManager：https://github.com/lework/RedisDesktopManager-Windows/releases

Redis默认有20个仓库，编号从0至19.  通过配置文件可以设置仓库数量，但是不超过16，并且不能自定义仓库名称。

<img src="redis\image-20240512133415507.png" style="zoom:67%;" >

## Redis常见命令

Redis是典型的key-value数据库，key一般是字符串，而value包含很多不同的数据类型：

<img src="redis\8tli2o9.png" style="zoom:67%;" >

官方文档与数据类型分组：https://redis.io/docs/latest/commands/

<img src="redis\image-20240512133753203.png" style="zoom:67%;" >

### keys

```shell
keys *	查看当前库所有key
 
exits key  判断某个key是否存在
 
type key  查看key是什么类型
 
del key	删除指定的key数据
 
unlink key	根据value选择非阻塞删除（仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作）
 
expire key 10  为给定的key设置过期时间（10s）
 
ttl key	查看还有多少秒过期：-1表示永不过期，-2表示已经过期
 
select	切换数据库
 
dbsize	查看当前数据库的key数量
 
flushdb	清空当前库
 
flushall  通杀全部库
```

### String

String类型，也就是字符串类型，是Redis中最简单的存储类型。其value是字符串，不过根据字符串的格式不同，又可以分为3类：

- string：普通字符串
- int：整数类型，可以做自增、自减操作
- float：浮点类型，可以做自增、自减操作

不管是哪种格式，底层都是字节数组形式存储，只不过是编码方式不同。字符串类型的最大空间不能超过512m.

```shell
set <key><value>  添加或修改键值对（key存在时，set覆盖旧值）
 
get <key>  查询对应键值
 
append <key><value>  将给定的value追加到原值的末尾
 
strlen <key>  获得值的长度
 
setnx <key><value>  只有key不存在时，设置key值
 
incr <key>  将key中储存的数字值增1，只能对数字值操作，如果为空，新增值为1

incrby <key> xx 让一个整型的key自增并指定步长，例如：incrby num 2 让num值自增2

incrbyfloat <Key> xx 让一个浮点型的key自增并指定步长

decr <key>  将key中储存的数字值建减1，只能对数字值操作，如果为空，新增值为-1
 
incrby / decrvy <key><步长>  将key中储存的数字值增减，自定义步长
 
mset <key1><value1><key2><value2>……  批量添加一个或多个 key-value 对
 
mget <key1><key2><key3>……  批量获取一个或多个value
 
msetnax  <key1><value1><key2><value2>……  同时设置一个或多个key-value对，当且仅当所有给定key都不存在
 
getrange <key><起始位置><结束位置>  获得值的范围，类似java中的substring，前包，后包
 
setrange <key><起始位置><value>  用<value>覆写<key>所存储的字符串值，从起始位置开始（索引从0开始）
 
setex <key><过期时间><value>  设置键值的同时，设置过期时间（单位：秒）
 
getset <key><value>  以新换旧，设置了新值的同时获得旧值
```

#### key结构

Redis没有类似MySQL中的Table的概念，我们该如何区分**不同类型的key**呢？

例如，需要存储用户、商品信息到redis，有一个用户id是1，有一个商品id恰好也是1，此时如果使用id作为key，那就会冲突了，该怎么办？

我们可以通过给key添加前缀加以区分：Redis的key允许**有多个单词形成层级结构，多个单词之间用':'隔开**，格式如下：

```shell
	项目名:业务名:类型:id
```

这个格式并非固定，也可以根据自己的需求来删除或添加词条。这样我们就可以把不同类型的数据区分开了。从而**避免了key的冲突问题。**

例如我们的项目名称叫 heima，有user和product两种不同类型的数据，我们可以这样定义key：

- user相关的key：**heima:user:1**

- product相关的key：**heima:product:1**

如果Value是一个Java对象，例如一个User对象，则可以**将对象序列化为JSON字符串后存储：**

| **KEY**         | **VALUE**                                  |
| --------------- | ------------------------------------------ |
| heima:user:1    | {"id":1,  "name": "Jack", "age": 21}       |
| heima:product:1 | {"id":1,  "name": "小米11", "price": 4999} |

<img src="redis\image-20240512143035672.png" style="zoom:80%;" >

并且，在Redis的桌面客户端中，还会以相同前缀作为层级结构，让数据看起来层次分明，关系清晰：

<img src="redis\image-20240512143224071.png" style="zoom:80%;" >

### Hash

Hash类型，也叫散列，其value是一个无序字典，类似于Java中的HashMap结构。

String结构是将对象序列化为JSON字符串后存储，当需要修改对象某个字段时很不方便；Hash结构可以将对象中的每个字段独立存储，可以针对单个字段做CRUD。

<img src="redis\VF2EPt0.png">

```sh
hset <key><field><value>  给<key>集合中的<filed>键赋值<value>，可以添加也可以修改
 
hget <key1><field>  从<key1>集合<field>取出value
 
hmset <key1><field1><value1><field2><value2>……  批量设置hash的值

hmget <key1><field1><field2>...  批量获取hash的field的值
 
hexits <key1><filed>  查看哈希表key中，给定域field是否存在
 
hkeys <key>  列出该hash集合key的所有field
 
hvals <key>  列出该hash集合的所有value
 
hincrby <key><field><increment>  为哈希表key中的域field的值加上增量（自增自减）并指定步长
 
hsetnx <key><field><value>  将哈希表key中的域field的值设置为value，当且仅当域field不存在

hgetall <key> 获取一个hash类型的key中的所有的field和value
```

<img src="redis\image-20240512144509760.png" style="zoom:80%;" >

### List

Redis中的List类型与Java中的LinkedList类似，可以看做是一个双向链表结构。既可以支持正向检索和也可以支持反向检索。

特征也与LinkedList类似：

- **有序**
- 元素可以重复
- 插入和删除快
- 查询速度一般

常用来存储一个有序数据，例如：朋友圈点赞列表，评论列表等。

```sh
lpush / rpush <key><value1><value2><value3>……  从左边/右边插入一个或多个值
 
lpop / rpop <key>  从左边/右边移除并返回第一个值，没有则返回null
 
rpoplpush <key1><key2>  从<key1>列表右边移除一个值，插到<key2>列表左边
 
lrange <key><start><stop>  按照索引下标获得元素（从左到右） eg：lrange mylist 0 -1  0左边第一个，-1右边第一个（0 -1 表示获取所有）
 
lindex <key><index>  按照索引下标获得元素（从左到右）
 
llen <key>  获得列表长度
 
linsert <key> before <value><newvalue>  在<value>后面插入<newvalue>插入值
 
lrem <key><n><value>  从左起删除n个vlaue（从左到右）
 
lset <key><index><value>  将列表key下标为index的值替换成value
```

示例：

```sh
LPUSH users 1 2 3
RPUSH users 4 5 6	结果是users中包含 3 2 1 4 5 6
LRANGE users 1 2	结果是["1","4"]
```

### Set

Redis的Set结构与Java中的HashSet类似，可以看做是一个value为null的HashMap。因为也是一个hash表，因此具备与HashSet类似的特征：

* 无序
* **元素不可重复**
* 查找快
* 支持交集.并集.差集等功能

```sh
sadd <key><value1><value2>……	将一个或多个元素加入到集合key中，已经存在的元素将被忽略

srem <key><value1><value2>……	删除集合中的指定元素
 
smembers <key>	取出该集合的所有值
 
sismember <key><value>	判断集合<key>是否包含该<value>值，有1，没有0
 
scard <key>	返回该集合的元素个数
 
spop <key>	随机从该集合中吐出一个值
 
srandmember <key><n>	随机从该集合中取出n个值，不会从集合中删除
 
smove <source><destination>value	把集合中的一个值从一个集合移动到另一额集合
 
sinter<key1><key2>	返回两个集合的交集元素
 
sunion <key1><key2>	返回两个集合的并集元素
 
sdiff <key1><key2>	返回两个集合的差集元素
```

具体命令：

```sh
127.0.0.1:6379> sadd s1 a b c
(integer) 3
127.0.0.1:6379> smembers s1
1) "c"
2) "b"
3) "a"
127.0.0.1:6379> srem s1 a
(integer) 1
    
127.0.0.1:6379> SISMEMBER s1 a
(integer) 0
    
127.0.0.1:6379> SISMEMBER s1 b
(integer) 1
    
127.0.0.1:6379> SCARD s1
(integer) 2
```

### SortedSet（Zset）

Redis的SortedSet是一个**可排序的set集合**，与Java中的TreeSet有些类似，但底层数据结构却差别很大。SortedSet中的每一个元素都带有一个score属性，可以基于**score属性对元素排序**，底层的实现是**一个跳表（SkipList）加 hash表**。

SortedSet具备下列特性：

- 可排序
- 元素不重复
- 查询速度快

因为SortedSet的可排序特性，经常被用来实现排行榜这样的功能。

```
zadd <key><score1><value1><score2><value2>……  将一个或多个member元素及其score值加入到有序集key中，若已存在则覆盖score值
 
zrange <key><start><stop> [WITHSCORES]  返回有序集key中，下标在strart到stop之间的元素（带WITHSCORES，可以让分数一起返回）
 
zrangebyscore key min max [withscores][limit offset count]  返回有序集key中，所有score值介于min和max之间的成员(从小到大)
 
zrevrangebyscore key max min [withscores][limit offet count]  同上，从大到小排序
 
zincrby <key><increment><value>  让sorted set中的指定元素自增，步长为指定的increment值
 
zrem <key><value>  删除该集合下，指定元素value
 
zcount <key><min><max>  统计该集合，分数区间内的元素个数
 
zrank <key><value>  返回该值在集合中的排名，从0开始

zscore <key><value> 获取sorted set中的指定元素的score值

zcard <key> 获取sorted set中的元素个数

ZDIFF.ZINTER.ZUNION：求差集.交集.并集
```

注意：所有的排名默认都是升序，如果要降序则在命令的Z后面添加REV即可，例如：

- **升序**获取sorted set 中的指定元素的排名：ZRANK key member
- **降序**获取sorted set 中的指定元素的排名：ZREVRANK key memeber

#### 案例

将班级的下列学生得分存入Redis的SortedSet中：

Jack 85, Lucy 89, Rose 82, Tom 95, Jerry 78, Amy 92, Miles 76

并实现下列功能：

- 删除Tom同学
- 获取Amy同学的分数
- 获取Rose同学的排名
- 查询80分以下有几个学生
- 给Amy同学加2分
- 查出成绩前3名的同学
- 查出成绩80分以下的所有同学

```sh
ZADD studs 85 Jack 89 Lucy 82 Rose 95 tom 78 Jerry 92 Amy 76 Miles
```



<img src="redis\image-20240512152116142.png" style="zoom:80%;" >

```sh
zrem studs tom
zrank studs Rose	排名2
zrevrank studs Rose	排名3（倒数4）
zcount studs 0 80	结果2人
zincrby studs 2 Amy	结果94.0分
zrange studs 0 2	结果["Miles","Rose","Jerry"]
zrangebyscore studs 0 80	结果["Miles","Jerry"]
```

### Bitmaps

```
setbit <key><offset><value>  设置Bitmaps中某个偏移量的值（0或1）（offset：偏移量从0开始）
 
getbit <key><offset>  获取Bitmaps中某个偏移量的值
 
bitcount <key>[start end]  统计字符串从start字节到end字节比特值为1的量
 
bitop and(or/not/xor) <destkey> [key] 复合操作，可以做多个bitmaps的交集(and)、并集(or)、非(not)、异或(xor)操作并将结果保存在destkey中
```

### HyperLogLog

```
pfadd <key><element>[element……]  添加指定元素到HyperLogLog中
 
pfcount <key> [key……]  计算HLL的近似基数，可以计算多个HLL（比如用HLL存储每天的UV，计算一周的UV可以使用7天的UV合并计算即可）
 
pfmerge <destkey><sourcekey>[sourcekey……]  将一个或多个HLL合并后的结果存储在另一个HLL中（比如每月活跃用户可以使用每天的活页用户来合并计算可得）
```

### Geospatial

```
geoadd <key><longitude><latitude><member>[longitude lattitude member……] 添加地理位置（经度、维度、名称）
 
geopos <key><member>[member……]  获得指定地区的坐标值
 
geodist <key><member1><member2> [m|km|ft|mi]  获取两个位置之间的直线距离
 
georadius <key><longitude><latitude>radius m|km|ft|mi	以给定的经纬度为中心，找出某一半径内的元素
```

## Redis的Java客户端

在Redis官网中提供了各种语言的客户端，地址：https://redis.io/docs/clients/

其中Java客户端也包含很多：

<img src="redis\image-20240512153303012.png"  >

标记为*的就是推荐使用的java客户端，包括：

- Jedis和Lettuce：这两个主要是提供了Redis命令对应的API，方便我们操作Redis，而**SpringDataRedis又对这两种做了抽象和封装**，因此我们后期会直接以SpringDataRedis来学习。
- Redisson：是**在Redis基础上实现了分布式的可伸缩的java数据结构**，例如Map、Queue等，而且支持跨进程的同步机制：Lock、Semaphore等待，比较适合用来实现特殊的功能需求。

### Jedis客户端

Jedis的官网地址： https://github.com/redis/jedis

#### 引入依赖

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>5.0.0</version>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

#### 建立连接

```java
public class redisTest {
    private Jedis jedis;
    @BeforeEach
    void setUp() {
        // 建立连接
        jedis = new Jedis("ip", 6379);
        jedis.auth("name", "pwd");
        // 选择库
        jedis.select(0);
    }
}
```

#### 测试连接

```java
@Test
void testString() {
    // 存入数据
    String result = jedis.set("name", "虎哥");
    System.out.println("result = " + result);
    // 获取数据
    String name = jedis.get("name");
    System.out.println("name = " + name);
}

@Test
void testHash() {
    // 插入hash数据
    jedis.hset("user:2", "name", "Jack");
    jedis.hset("user:2", "age", "21");

    // 获取
    Map<String, String> map = jedis.hgetAll("user:2");
    System.out.println(map);
}
```

释放资源

```java
@AfterEach
void tearDown() {
    if (jedis != null) {
        jedis.close();
    }
}
```

#### jedis连接池

Jedis本身是线程不安全的，并且频繁的创建和销毁连接会有性能损耗，因此我们推荐大家使用Jedis连接池代替Jedis的直连方式。

```java
public class JedisConnectionFactory {

    private static JedisPool jedisPool;

    static {
        // 配置连接池
        JedisPoolConfig poolConfig = new JedisPoolConfig();
        poolConfig.setMaxTotal(8);
        poolConfig.setMaxIdle(8);	// 最大空闲连接
        poolConfig.setMinIdle(0);	// 最小空闲连接
        poolConfig.setMaxWaitMillis(1000);	// 没有连接时，最多等待1000ms
        // 创建连接池对象，参数：连接池配置、服务端ip、服务端端口、超时时间、密码
        jedisPool = new JedisPool(poolConfig, "ip", 6379, 1000, "pwd");
    }

    public static Jedis getJedis(){
        return jedisPool.getResource();
    }
}
```

### SpringDataRedis客户端

SpringData是Spring中数据操作的模块，包含对各种数据库的集成，其中对Redis的集成模块就叫做SpringDataRedis，官网地址：https://spring.io/projects/spring-data-redis

- 提供了**对不同Redis客户端的整合（Lettuce和Jedis）**
- 提供了RedisTemplate统一API来操作Redis
- 支持Redis的发布订阅模型
- 支持Redis哨兵和Redis集群
- 支持基于Lettuce的响应式编程
- 支持基于JDK、JSON、字符串、Spring对象的数据序列化及反序列化
- 支持基于Redis的JDKCollection实现

SpringDataRedis中提供了RedisTemplate工具类，其中封装了各种对Redis的操作。并且将不同数据类型的操作API封装到了不同的类型中：

<img src="redis\UFlNIV0.png" style="zoom:80%;" >

#### 引入依赖

```xml
<dependencies>
    <!-- spring-data-redis-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    <!--common-pool-->
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-pool2</artifactId>
    </dependency>
    <!--Jackson依赖-->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>	
```

#### 配置redis

application.properties

```properties
# REDIS (RedisProperties)
# Redis数据库索引（默认为0）
spring.data.redis.database=0
# Redis服务器地址
spring.data.redis.host=ip
# Redis服务器连接端口
spring.data.redis.port=6379
# Redis服务器连接密码（默认为空）
spring.data.redis.password=pwd
# 连接池最大连接数（使用负值表示没有限制）
spring.data.redis.jedis.pool.max-active=8
# 连接池最大阻塞等待时间（使用负值表示没有限制）
spring.data.redis.jedis.pool.max-wait=1000
# 连接池中的最大空闲连接
spring.data.redis.jedis.pool.max-idle=8
# 连接池中的最小空闲连接
spring.data.redis.jedis.pool.min-idle=0
# 连接超时时间（毫秒）
spring.data.redis.timeout=5000
```

#### 注入RedisTemplate，测试

因为有了SpringBoot的自动装配，我们可以拿来就用：

```java
@SpringBootTest
class SpringRedisApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {
    }
    @Test
    void testString() {
        // 写入string
        redisTemplate.opsForValue().set("name", "胡歌");
        // 获取string数据
        Object name = redisTemplate.opsForValue().get("name");
        System.out.println("name = " + name);
    }
    @Test
    void testHash() {
        redisTemplate.opsForHash().put("user:2", "name", "Jack");
        Object name = redisTemplate.opsForHash().get("user:2", "name");
        System.out.println("name = "+name);
    }
}
```

#### 自定义序列化

**RedisTemplate默认采用JDK的序列化工具，可以接收任意Object作为值写入Redis，序列化为字节形式，在redis中可读性很差且内存占用大。**
修改默认的序列化方式为jackson

我们可以自定义RedisTemplate的序列化方式

```java
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory){
        // 创建RedisTemplate对象
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        // 设置连接工厂
        template.setConnectionFactory(connectionFactory);
        // 创建JSON序列化工具
        GenericJackson2JsonRedisSerializer jsonRedisSerializer = 
            							new GenericJackson2JsonRedisSerializer();
        // 设置Key的序列化
        template.setKeySerializer(RedisSerializer.string());
        template.setHashKeySerializer(RedisSerializer.string());
        // 设置Value的序列化
        template.setValueSerializer(jsonRedisSerializer);
        template.setHashValueSerializer(jsonRedisSerializer);
        // 返回
        return template;
    }
}
```

<img src="redis\image-20240512170814734.png" style="zoom:80%;" >

这里采用了JSON序列化来代替默认的JDK序列化方式。最终结果如图：

<img src="redis\image-20240512170944012.png">

整体可读性有了很大提升，并且能将Java对象自动的序列化为JSON字符串，并且查询时能自动把JSON反序列化为Java对象。不过，**其中记录了序列化时对应的class名称，目的是为了查询时实现自动反序列化。这会带来额外的内存开销。**

#### StringRedisTemplate

为了**节省内存空间**，我们可以**不使用JSON序列化器来处理value，而是统一使用String序列化器**，要求**只能存储String类型的key和value**。当需要存储Java对象时，**手动完成对象的序列化和反序列化。**

因为存入和读取时的序列化及反序列化都是我们自己实现的，SpringDataRedis就不会将class信息写入Redis了。

这种用法比较普遍，因此SpringDataRedis就提供了RedisTemplate的子类：StringRedisTemplate，它的key和value的序列化方式默认就是String方式。

```java

@SpringBootTest
class RedisStringTests {

    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    @Test
    void testString() {
        // 写入一条String数据
        stringRedisTemplate.opsForValue().set("verify:phone:13600527634", "124143");
        // 获取string数据
        Object name = stringRedisTemplate.opsForValue().get("name");
        System.out.println("name = " + name);
    }
    // JSON序列化工具
    private static final ObjectMapper mapper = new ObjectMapper();

    @Test
    void testSaveUser() throws JsonProcessingException {
        // 创建对象
        User user = new User("虎哥", 21);
        // 手动序列化
        String json = mapper.writeValueAsString(user);
        // 写入数据
        stringRedisTemplate.opsForValue().set("user:200", json);

        // 获取数据
        String jsonUser = stringRedisTemplate.opsForValue().get("user:200");
        // 手动反序列化
        User user1 = mapper.readValue(jsonUser, User.class);
        System.out.println("user1 = " + user1);
    }

    @Test
    void testHash() {
        stringRedisTemplate.opsForHash().put("user:400", "name", "虎哥");
        stringRedisTemplate.opsForHash().put("user:400", "age", "21");

        Map<Object, Object> entries = stringRedisTemplate.opsForHash().entries("user:400");
        System.out.println("entries = " + entries);
    }

    @Test
    void name() {
    }
}
```



## 参考

黑马程序员redis：https://www.bilibili.com/video/BV1cr4y1671t/?p=16&spm_id_from=pageDriver&vd_source=bf952648bf410c0b9b23bf213e3d24ba

redis常用命令：https://blog.csdn.net/weixin_49851451/article/details/134311296

