# **Redis**---REmote DIctionary Server（远程词典服务器）

![image-20210611143211604](C:\Users\安哥拉长毛兔\AppData\Roaming\Typora\typora-user-images\image-20210611143211604.png)

key-value 存储系统，跨平台的非关系型数据库。

Redis 通常被称为数据结构服务器，因为值（value）可以是字符串(String)、哈希(Hash)、列表(list)、集合(sets)和有序集合(sorted sets)等类型。



**特点：**

- Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- Redis支持数据的备份，即master-slave模式的数据备份。



## Nosql

关系型数据库：

非关系型数据库（Nosql）

#### kv键值对

* Redis、 Redis + Tair、 Redis + memchche

#### 文档型数据库

​	Bosn 格式和json一样，只是二进制存储

​	**MongoDB**(必须掌握)

#### 列存储数据库

​	**Hbase**、 分布式文件系统

#### 图关系数据库

​	不是存图，存放的是关系，比如朋友圈社交网络、广告推荐等

​	**No4j**



## Redis入门



## 五大基本数据类型

### 字符串 （Strings）

### 列表 （lists）

### 散列 （hashes）

### 集合 （sets）

### 有序集合 （zsets）



## 三种特殊数据类型



### Geospatial 地理位置

### Hyperloglog 基数统计

### Bitmap 位图





## 事务

Redis 事务： 一组命令的集合！一个事务中的所有命令都会被序列化，事务执行过程中会按照顺序执行！

**一次性、顺序性、排他性**



Redis 事务没有隔离级别的概念,所有的命令在事务中没有被直接执行，而是放入队列，只有发起执行命令时才会依次执行。

**Redis 单条命令是保证原子性的，但事务不保证原子性**



Redis事务：

* 开启事务（multi）
* 命令入队 (...)
* 执行事务 (exec)

取消事务 （discard） 



编译异常： 有错报错，都不会执行

运行异常：运行时发现语法错误，只有错误的命令不执行，其他命令正常执行



## 锁

**悲观锁**：很悲观，认为什么时候都会出问题，要加锁

**乐观锁**：很乐观，认为什么时候都不会出问题，更新数据的时候判断在次期间时候有人修改了数据

**Redis 使用 watch 进行加锁（乐观锁）**

事务执行失败先解锁 再重新监视  unwatch

watch 本身就是乐观锁

自旋锁：失败后，先解锁，再重新监视。



## Jedis







redis 使用客户端连接

redis-service.exe redis.windows.conf 

redis-cli -c -h ip -p 端口号



集群批量删除：redis-cli keys"*" |xargs redis-cli del（待确认）



## SpringBoot整合



springBoot操作数据 spring-data(和springboot 齐名的项目)

springBoot 2.x 后 ，jedis 被替换为 lettuce



Jedes: 采用直连，多个线程操作是不安全的，避免不安全则使用 jedis pool 连接池，更好像BIO

lettuce: 采用netty, 实例可以在多个线程中进行共享，不存在线程不安全情况，更像NIO



自定义 RedisTemplate

使用注解 @Configuration













## Redis 持久化

### RDB (Redis DateBase)

备份redis数据，恢复数据从文件加载

优点：

1、适合大规模的数据恢复

2、对数据的完整性要求不高

缺点：

1、需要一定的时间间隔 进程操作，如果redis意外宕机，最后一次修改的数据会丢失

2、fork进程，会占用一定的内存空间

### AOF (Append Only Mode)

文件追加操作记录，恢复时加载文件。

优点：

1、每一次修改都同步，文件完整性更好

2、默认每秒同步一次，可能对视一秒的数据

缺点：

1、相对于数据文件来说，aof远比rdb文件大，恢复数据比rdb慢

2、rof运行效率比rdb慢，redis默认使用rdb对数据进行持久化





## Redis 发布订阅

设计模式： 发布订阅和观察者模式的区别？

<img src="C:\Users\安哥拉长毛兔\AppData\Roaming\Typora\typora-user-images\image-20210619200313954.png" alt="image-20210619200313954" style="zoom:80%;" />



从表面上看：

- 观察者模式里，只有两个角色 —— 观察者 + 被观察者
- 而发布订阅模式里，却不仅仅只有发布者和订阅者两个角色，还有一个经常被我们忽略的 —— 经纪人Broker

往更深层次讲：

- 观察者和被观察者，是松耦合的关系
- 发布者和订阅者，则完全不存在耦合

从使用层面上讲：

- 观察者模式，多用于单个应用内部
- 发布订阅模式，则更多的是一种跨应用的模式(cross-application pattern)，比如我们常用的消息中间件





## Redis集群模式



### Redis 主从复制

主从复制，读写分离。 80% 的情况都是在读操作！ 

主服务器只负责写，从服务器负责读，最低配 一主二从。

 SLAVEOF 

slaveof no noe  把从机转换为主机





### 哨兵模式



### Jedis sharding集群（主备）



### Cluster集群模式





## Redis 缓存穿透和雪崩























