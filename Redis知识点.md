# Redis知识点

## 一、前景知识

1、常识：磁盘上寻址级别是ms，带宽G/M；内存寻址级别是ns，带宽很大；寻址相差10w倍。操作系统是以最少4k单位从磁盘中取数。

2、数据库将数据存在磁盘中，并维护索引，内存使用B+树。当一个查询sql进来，当where条件命中索引，那么B+树先将索引读取到内存中，查找出在哪块数据中存在，然后将这块数据加载到内存中，即可以找到对应的数据。

![image-20200621153642917](C:\Users\GJH\AppData\Roaming\Typora\typora-user-images\image-20200621153642917.png)

> 面试题：数据库的表很大，性能会不会下降？
>
> 答：如果表有索引，增删改操作会变慢，原因是需要维护索引；查询数据分为两种情况，1、一个或并发量少的情况，查询依然很快；2、并发大的情况下会受磁盘的带宽影响速度，原因是将多个4k数据加载到内存中，会影响到速度。

3、SAP HANA内存级别的关系型数据库。价格贵，容量有限。

## 二、缓存数据库：memcache redis

1、数据库引擎查询网址：https://db-engines.com/en/

2、redis官网：https://redis.io/；中文网：http://redis.cn/

3、redis是什么？

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如 [字符串（strings）](http://redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://redis.cn/topics/lru-cache.html)，[事务（transactions）](http://redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

4、memcache与redis的区别：

1）memcache的value没有类型的概念，redis是有的；

2）memcache会将所有的数据返回给客服端；（类型不是很重要）redis对每种类型都有自己的方法（如lpop、index等）去需要的数据即可（计算向数据移动）

## 三、redis基础知识

### 1、redis安装过程

1）创建目录mkdir xxx

2）进入目录cd xxx

3）下载redis安装包wget xxx（网址）；若没有wget命令，则安装yum install wget

4）安装tar xf redis-xxx

5）进入目录 cd redis-xxx（可以阅读readme.md）

6）安装编译器yum install gcc（如果已安装，可跳过）

7）编译make（如果出错，可以使用make distclean）；编译完成后可以再src目录下执行./redis-server（手动启动）

8）配置成服务，安装make install PREFIX=/opt/redis5（目录）

9）配置环境变量vi /etc/profile；在文件最后export  REDIS_HOME=/opt/redis5；export PATH=$PATH:$REDIS_HOME/bin；退出执行加载文件source /etc/profile （echo $PATH查看配置文件）

10）进入cd utils，执行./install_server.sh（配置redis信息，可默认；后期可以使用service redis_6379 start[stop/status]）

### 2、



















