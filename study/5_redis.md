## Redis简介

redis是一个开源的、使用C语言编写的、支持网络交互的、可基于内存也可持久化的Key-Value数据库。

官方网址：https://redis.io/。

Redis是一种非关系型数据库，NOSQL(not only sql)不仅仅是sql，对所有非关系型数据库的一种通称。

## NOSQL和RDBMS的区别

**RDBMS**

1） 高度组织化结构化数据。 user---userid username age sex .....

2） 结构化查询语言（SQL） sql语句

3） 数据和关系都存储在单独的表中。

4） 数据操纵语言DML，数据定义语言DDL

5） 严格的一致性. 事务 .

6） 基于事务

**NoSQL**

1） 代表着不仅仅是SQL

2） 没有声明性查询语言

3） 键 - 值对存储。

4） 非结构化和不可预知的数据 字符串 对象 队列 集合.

5） 高性能，高可用性和可伸缩性。 适合搭建集群。 mysql搭建集群非常复杂。主从模式

## Redis的特点

优点：

1） 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。

2） 丰富的数据类型 – 支持**string**（字符串），**hash**（哈希），**list**（列表），**set**（集合）及**zset**(sorted set：有序集合)数据类型操作。

3） 原子 – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。

4） 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。Redis 支持 master\slave 主\从机制、sentinel 哨兵模式、cluster 集群模式，保证了 Redis 运行的稳定和高可用性。

5） 支持持久化 – 可以将内存中的数据持久化在磁盘中，当宕机或者故障重启时，可以再次加载进如 Redis，从而不会或减少数据的丢失。

缺点：

1） 数据库容量受到物理内存的限制，不能用作海量数据的高性能读写。

2） 局限在较小数据量的高性能操作和运算上。

## `Redis`的应用场景：

- 缓存（数据查询、短连接、新闻内容、商品内容等等）
- 聊天室的在线好友列表
- 任务队列（秒杀、抢购、12306等等）
- 应用排行榜
- 网站访问统计
- 数据过期处理（可以精确到毫秒）
- 分布式集群架构中的`session`分离

## 安装

### Windows下安装Redis

#### 第一种：将Redis的压缩包解压到系统硬盘上（路径不要包含中文）。

##### 使用命令行进入安装目录

```
C:\development>cd Redis-x64-3.0.504

C:\development\Redis-x64-3.0.504>redis-server --service-install redis.windows.conf
[9056] 15 Sep 09:07:46.747 # Granting read/write access to 'NT AUTHORITY\NetworkService' on: "C:\development\Redis-x64-3.0.504" "C:\development\Redis-x64-3.0.504\"
[9056] 15 Sep 09:07:46.748 # Redis successfully installed as a service.

C:\development\Redis-x64-3.0.504>
```

#### 第二种：Redis-x64-3.2.100.msi

傻瓜式安装，选择文件安装路径。（其中有两步中，添加环境变量和外部访问，自己使用可选择不勾选）

### 启动服务

打开系统服务，找到Redis的服务项并启动。

### 服务命令

```
卸载服务：redis-server --service-uninstall
开启服务：redis-server --service-start
停止服务：redis-server --service-stop
```

### 访问Redis

在命令行中进入Redis安装目录，执行`redis-cli`命令，进入交互界面。

配置文件 redis.windows-service.conf 在安装路径 D:\Program Files\Redis

设置密码 requirepass 123456

### Linux下安装Redis

#### yum安装

1） 检查yum源。

```
yum search redis
```

2） 安装yum源。（第一步搜索不到时才需要）

```
yum install epel-release
```

3） yum安装。

```
yum install redis
```

#### 启动和关闭

#### 启动redis

```
service redis start
```

#### 停止redis

```
service redis stop
```

#### 查看redis运行状态

```
service redis status
```

#### 查看redis进程

```
ps -ef | grep redis
```

#### 配置

Redis的配置文件位于`/etc/redis.conf`。

#### 远程访问

在配置文件中，找到`bind 127.0.0.1`。使用#号将其注释，或绑定远程IP。

#### 设置端口

在配置文件中，找到`port 6379`。Redis的默认端口为6379，可根据需要修改。

#### 设置密码

在配置文件中，找到`protected`，将值设置为no。

在配置文件中，找到`requirepass`，其后设置密码。

## 连接工具

QuickRedis-2.7.1-win-ia32.exe

安装使用，可视化操作redis。

## 常用命令

### 一、redis启动

本地启动：redis-cli

远程启动：redis-cli -h host -p port -a password

**Redis 连接命令**

**1 AUTH password**

验证密码是否正确

**2 ECHO message**

打印字符串

**3 PING**

查看服务是否运行

**4 QUIT**

关闭当前连接

**5 SELECT index**

切换到指定的数据库

### 二、redis keys命令

**1、DEL key**

DUMP key

序列化给定的key并返回序列化的值

**2、EXISTS key**

检查给定的key是否存在

**3、EXPIRE key seconds**

为key设置过期时间

**4、EXPIRE key timestamp**

用时间戳的方式给key设置过期时间

**5、PEXPIRE key milliseconds**

设置key的过期时间以毫秒计

**6、KEYS pattern**

查找所有符合给定模式的key

**7、MOVE key db**

将当前数据库的key移动到数据库db当中

**8、PERSIST key**

移除key的过期时间，key将持久保存

**9、PTTL key**

以毫秒为单位返回key的剩余过期时间

**10、TTL key**

以秒为单位，返回给定key的剩余生存时间

**11、RANDOMKEY**

从当前数据库中随机返回一个key

**12、RENAME key newkey**

修改key的名称

**13、RENAMENX key newkey**

仅当newkey不存在时，将key改名为newkey

**14、TYPE key**

返回key所存储的值的类型

### 三、redis字符串命令

**1、SET key value**

**2、GET key**

**3、GETRANGE key start end**

返回key中字符串值的子字符

**4、GETSET key value**

将给定key的值设为value，并返回key的旧值

**5、GETBIT KEY OFFSET**

对key所储存的字符串值，获取指定偏移量上的位

**6、MGET KEY1 KEY2**

获取一个或者多个给定key的值

**7、SETBIT KEY OFFSET VALUE**

对key所是存储的字符串值，设置或清除指定偏移量上的位

**8、SETEX key seconds value**

将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。

**9、SETNX key value**

只有在 key 不存在时设置 key 的值。

**10、SETRANGE key offset value**

用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。

**11、STRLEN key**

返回 key 所储存的字符串值的长度。

**12、MSET key value [key value ...]**

同时设置一个或多个 key-value 对。

**13、MSETNX key value [key value ...]**

同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。

**14、PSETEX key milliseconds value**

这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。

**15、INCR key**

将 key 中储存的数字值增一。

**16、INCRBY key increment**

将 key 所储存的值加上给定的增量值（increment）

**17、INCRBYFLOAT key increment**

将 key 所储存的值加上给定的浮点增量值（increment）

**18、DECR key**

将 key 中储存的数字值减一

**19、DECRBY key decrement**

key 所储存的值减去给定的减量值（decrement）

**20、APPEND key value**

如果 key 已经存在并且是一个字符串， APPEND 命令将 指定value 追加到改 key 原来的值（value）的末尾。

### 四、Redis hash 命令

**1 HDEL key field1 [field2]**

删除一个或多个哈希表字段

**2 HEXISTS key field**

查看哈希表 key 中，指定的字段是否存在。

**3 HGET key field**

获取存储在哈希表中指定字段的值。

**4 HGETALL key**

获取在哈希表中指定 key 的所有字段和值

**5 HINCRBY key field increment**

为哈希表 key 中的指定字段的整数值加上增量 increment 。

**6 HINCRBYFLOAT key field increment**

为哈希表 key 中的指定字段的浮点数值加上增量 increment 。

**7 HKEYS key**

获取所有哈希表中的字段

**8 HLEN key**

获取哈希表中字段的数量

**9 HMGET key field1 [field2]**

获取所有给定字段的值

**10 HMSET key field1 value1 [field2 value2 ]**

同时将多个 field-value (域-值)对设置到哈希表 key 中。

**11 HSET key field value**

将哈希表 key 中的字段 field 的值设为 value 。

**12 HSETNX key field value**

只有在字段 field 不存在时，设置哈希表字段的值。

**13 HVALS key**

获取哈希表中所有值

**14 HSCAN key cursor [MATCH pattern] [COUNT count]**

迭代哈希表中的键值对。

### 五、Redis 列表命令

**1 BLPOP key1 [key2 ] timeout**

移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。

**2 BRPOP key1 [key2 ] timeout**

移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。

**3 BRPOPLPUSH source destination timeout**

从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。

**4 LINDEX key index**

通过索引获取列表中的元素

**5 LINSERT key BEFORE|AFTER pivot value**

在列表的元素前或者后插入元素

**6 LLEN key**

获取列表长度

**7 LPOP key**

移出并获取列表的第一个元素

**8 LPUSH key value1 [value2]**

将一个或多个值插入到列表头部

**9 LPUSHX key value**

将一个值插入到已存在的列表头部

**10 LRANGE key start stop**

获取列表指定范围内的元素

**11 LREM key count value**

移除列表元素

**12 LSET key index value**

通过索引设置列表元素的值

**13 LTRIM key start stop**

对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。

**14 RPOP key**

移除并获取列表最后一个元素

**15 RPOPLPUSH source destination**

移除列表的最后一个元素，并将该元素添加到另一个列表并返回

**16 RPUSH key value1 [value2]**

在列表中添加一个或多个值

**17 RPUSHX key value**

为已存在的列表添加值

### 六、Redis 集合命令

**1 SADD key member1 [member2]**

向集合添加一个或多个成员

**2 SCARD key**

获取集合的成员数

**3 SDIFF key1 [key2]**

返回给定所有集合的差集

**4 SDIFFSTORE destination key1 [key2]**

返回给定所有集合的差集并存储在 destination 中

**5 SINTER key1 [key2]**

返回给定所有集合的交集

**6 SINTERSTORE destination key1 [key2]**

返回给定所有集合的交集并存储在 destination 中

**7 SISMEMBER key member**

判断 member 元素是否是集合 key 的成员

**8 SMEMBERS key**

返回集合中的所有成员

**9 SMOVE source destination member**

将 member 元素从 source 集合移动到 destination 集合

**10 SPOP key**

移除并返回集合中的一个随机元素

**11 SRANDMEMBER key [count]**

返回集合中一个或多个随机数

**12 SREM key member1 [member2]**

移除集合中一个或多个成员

**13 SUNION key1 [key2]**

返回所有给定集合的并集

**14 SUNIONSTORE destination key1 [key2]**

所有给定集合的并集存储在 destination 集合中

**15 SSCAN key cursor [MATCH pattern] [COUNT count]**

迭代集合中的元素

### 七、Redis 有序集合命令

**1 ZADD key score1 member1 [score2 member2]**

向有序集合添加一个或多个成员，或者更新已存在成员的分数

**2 ZCARD key**

获取有序集合的成员数

**3 ZCOUNT key min max**

计算在有序集合中指定区间分数的成员数

**4 ZINCRBY key increment member**

有序集合中对指定成员的分数加上增量 increment

**5 ZINTERSTORE destination numkeys key [key ...]**

计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中

**6 ZLEXCOUNT key min max**

在有序集合中计算指定字典区间内成员数量

**7 ZRANGE key start stop [WITHSCORES]**

通过索引区间返回有序集合成指定区间内的成员

**8 ZRANGEBYLEX key min max [LIMIT offset count]**

通过字典区间返回有序集合的成员

**9 ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT]**

通过分数返回有序集合指定区间内的成员

**10 ZRANK key member**

返回有序集合中指定成员的索引

**11 ZREM key member [member ...]**

移除有序集合中的一个或多个成员

**12 ZREMRANGEBYLEX key min max**

移除有序集合中给定的字典区间的所有成员

**13 ZREMRANGEBYRANK key start stop**

移除有序集合中给定的排名区间的所有成员

**14 ZREMRANGEBYSCORE key min max**

移除有序集合中给定的分数区间的所有成员

**15 ZREVRANGE key start stop [WITHSCORES]**

返回有序集中指定区间内的成员，通过索引，分数从高到底

**16 ZREVRANGEBYSCORE key max min [WITHSCORES]**

返回有序集中指定分数区间内的成员，分数从高到低排序

**17 ZREVRANK key member**

返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序

**18 ZSCORE key member**

返回有序集中，成员的分数值

**19 ZUNIONSTORE destination numkeys key [key ...]**

计算给定的一个或多个有序集的并集，并存储在新的 key 中

**20 ZSCAN key cursor [MATCH pattern] [COUNT count]**

迭代有序集合中的元素（包括元素成员和元素分值）

### 八、Redis 事务命令

**1 DISCARD**

取消事务，放弃执行事务块内的所有命令。

**2 EXEC**

执行所有事务块内的命令。

**3 MULTI**

标记一个事务块的开始。

**4 UNWATCH**

取消 WATCH 命令对所有 key 的监视。

**5 WATCH key [key ...]**

监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。

九、Redis 脚本命令

**1 EVAL script numkeys key [key ...] arg [arg ...]**

执行 Lua 脚本。

**2 EVALSHA sha1 numkeys key [key ...] arg [arg ...]**

执行 Lua 脚本。

**3 SCRIPT EXISTS script [script ...]**

查看指定的脚本是否已经被保存在缓存当中。

**4 SCRIPT FLUSH**

从脚本缓存中移除所有脚本。

**5 SCRIPT KILL**

杀死当前正在运行的 Lua 脚本。

**6 SCRIPT LOAD script**

将脚本 script 添加到脚本缓存中，但并不立即执行这个脚本。

十、Redis 服务器命令

**1 BGREWRITEAOF**

异步执行一个 AOF（AppendOnly File） 文件重写操作

**2 BGSAVE**

在后台异步保存当前数据库的数据到磁盘

**3 CLIENT KILL [ip:port] [ID client-id]**

关闭客户端连接

**4 CLIENT LIST**

获取连接到服务器的客户端连接列表

**5 CLIENT GETNAME**

获取连接的名称

**6 CLIENT PAUSE timeout**

在指定时间内终止运行来自客户端的命令

**7 CLIENT SETNAME connection-name**

设置当前连接的名称

**8 CLUSTER SLOTS**

获取集群节点的映射数组

**9 COMMAND**

获取 Redis 命令详情数组

**10 COMMAND COUNT**

获取 Redis 命令总数

**11 COMMAND GETKEYS**

获取给定命令的所有键

**12 TIME**

返回当前服务器时间

**13 COMMAND INFO command-name [command-name ...]**

获取指定 Redis 命令描述的数组

**14 CONFIG GET parameter**

获取指定配置参数的值

**15 CONFIG REWRITE**

对启动 Redis 服务器时所指定的 redis.conf 配置文件进行改写

**16 CONFIG SET parameter value**

修改 redis 配置参数，无需重启

**17 CONFIG RESETSTAT**

重置 INFO 命令中的某些统计数据

**18 DBSIZE**

返回当前数据库的 key 的数量

**19 DEBUG OBJECT key**

获取 key 的调试信息

**20 DEBUG SEGFAULT**

让 Redis 服务崩溃

**21 FLUSHALL**

删除所有数据库的所有key

**22 FLUSHDB**

删除当前数据库的所有key

**23 INFO [section]**

获取 Redis 服务器的各种信息和统计数值

**24 LASTSAVE**

返回最近一次 Redis 成功将数据保存到磁盘上的时间，以 UNIX 时间戳格式表示

**25 MONITOR**

实时打印出 Redis 服务器接收到的命令，调试用

**26 ROLE**

返回主从实例所属的角色

**27 SAVE**

同步保存数据到硬盘

**28 SHUTDOWN [NOSAVE] [SAVE]**

异步保存数据到硬盘，并关闭服务器

**29 SLAVEOF host port**

将当前服务器转变为指定服务器的从属服务器(slave server)

**30 SLOWLOG subcommand [argument]**

管理 redis 的慢日志

**31 SYNC**

用于复制功能(replication)的内部命令

## 持久化

Redis的持久化有两种方式：RDB和AOF，redis默认采用的是RDB的方式。

### RDB

在默认配置中，Redis将内存数据库快照保存在名字为dump.rdb的二进制文件中。
可以配置持久化策略：save N M，让redis在“N秒内至少有M个改动”才会触发一次rdb持久化操作。
例如redis的默认策略有：

```
save 900 1          -- 900秒内至少有一个改动
save 300 10         -- 300秒内至少有10个改动
save 60 10000	    -- 60秒内至少有10000个改动
```

还可以手动执行命令生成RDB快照，进入redis客户端执行命令save或bgsave可以生成dump.rdb文件，
每次命令执行都会将所有redis内存快照到一个新的rdb文件里，并覆盖原有rdb快照文件。

#### RDB方式优缺点

优点：方便快捷

应为dump.rdb文件是二进制文件，所以当redis服务崩溃恢复的时候，能很快的将文件数据恢复到内存之中。

缺点：性能差，丢失数据

RDB每次持久化需要将所有内存数据写入文件，然后替换原有文件，当内存数据量很大的时候，频繁的生成快照会很耗性能。如果将生成快照的策略设置的时间间隔很大，会导致redis宕机的时候丢失过的的数据。

### AOF(Append-Only File)

为解决RDB方式丢失数据的问题，从1.1版本开始，redis增加了一种更加可靠的方式：AOF持久化方式。
在使用AOF方式时，redis每执行一次修改数据命令，都会将该命令追加到appendonly.aof文件中（先写入os cache，然后通过fsync刷盘）。

可以通过修改配置文件来使用AOF：

```
appendonly yes
```

redis重启的时候，会重放appendonly.aof中的命令恢复数据。

#### AOF策略

AOF可以配置三种刷盘策略：

```
appendfsync always：每次执行写命令都会刷盘，非常慢，也非常安全。
appendfsync everysec：每秒刷盘一次，兼顾性能和安全。
appendfsync no：将刷盘操作交给系统，很快，不安全。
```

推荐使用everysec，该策略下，最多会丢1秒的数据。

AOF重写:
应为appendonly.aof文件中存储的是执行命令，所以会产生很多没用的命令，因此，redis会定期根据最新的内存数据生成新的aof文件。
如下两个配置可以控制aop文件重写的频率：

```
auto‐aof‐rewrite‐min‐size 64mb：	-- aof文件至少达到了64m才会触发重写
auto‐aof‐rewrite‐percentage 100：	-- 距离上次重写增长了100%才会再次触发重写
```

AOF也可以手动触发重写：bgrewriteof
注意，AOF重写redis会fork出一个子进程去做(与bgsave命令类似)，不会对redis正常命令处理有太多
影响。

### RDB于AOF对比

| 命令       | RDB          | AOF          |
| ---------- | ------------ | ------------ |
| 启动优先级 | 低           | 高           |
| 体积       | 小           | 大           |
| 恢复速度   | 快           | 慢           |
| 数据安全性 | 容易丢失数据 | 根据策略决定 |

## 缓存

缓存的本质是空间换时间，缓存就是数据交换的缓冲区（cache）解决较快的处理速度与较慢的存储媒介之间的矛盾。例如CPU的一二三级缓存。

一级缓存：
内置在CPU内部并与CPU同速运行，可以有效的提高CPU的运行效率。一级缓存越大，CPU的运行效率越高，但受到CPU内部结构的限制，一级缓存的容量都很小。

二级缓存：
为了协调一级缓存和内存之间的速度。cpu调用缓存首先是一级缓存，当处理器的速度逐渐提升，会导致一级缓存就供不应求，这样就得提升到二级缓存了。二级缓存它比一级缓存的速度相对来说会慢，但是它比一级缓存的空间容量要大。主要就是做一级缓存和内存之间数据临时交换的地方用。

三级缓存：
为读取二级缓存后未命中的数据设计的—种缓存，在拥有三级缓存的CPU中，只有约5%的数据需要从内存中调用，这进一步提高了CPU的效率。其运作原理在于使用较快速的储存装置保留一份从慢速储存装置中所读取数据并进行拷贝，当有需要再从较慢的储存体中读写数据时，缓存(cache)能够使得读写的动作先在快速的装置上完成，如此会使系统的响应较为快速。

## SpringBoot使用Redis缓存

### 添加依赖

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-data-redis -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### 配置Redis

```yaml
spring  
  redis:
    ## 服务器IP
    host: 127.0.0.1
    ## 端口号
    port: 6379
    ## 数据库索引
    database: 0
    ## 数据库密码
    password: ''
    ## 超时时间
    timeout: 5000
    jedis:
      pool:
        ## 最大连接数
        max-active: 50
        ## 最大等待时间
        max-wait: 300
        ## 最大空闲连接
        max-idle: 20
        ## 最小空闲连接
        min-idle: 2
```

### 缓存注解

@EnableCaching：该注解主要用于开启基于注解的缓存支持，用在Application类上

@CacheEvict：该注解用于清理缓存

@CachePut：该注解用于设置缓存

@Caching：该注解可以对缓存清理、设置 等操作打包

@CacheConfig：该注解同样用于缓存设置

@Cacheable：该注解主要针对方法配置，能够根据方法的请求参数对其结果进行缓存，比如如果缓存在存在该值，则用缓存数据， 如果不在缓存，则存入缓存；

1） 启动类注解

开启基于注解的缓存。

```java
@SpringBootApplication
@EnableCaching
public class MyApplication {
    public void main(String[] args){...}
}
```

2） Service层注解

```java
@Cacheable(cacheNames="myEntitys", key="#id", unless = "#result==null")
public MyEntity get(Integer id){
    return myEntityMapper.getById(id);
}
```

@Cacheable注解

用于类和方法，开启基于注解的缓存。

1） cacheNames/value：用来指定缓存组件的名字。

2） key：缓存数据时使用的 key，可以用它来指定。默认是使用方法参数的值。（这个 key 你可以使用 spEL 表达式来编写）。

3）keyGenerator：key 的生成器。 key 和 keyGenerator 二选一使用。

4）cacheManager：可以用来指定缓存管理器。从哪个缓存管理器里面获取缓存。

5）condition：可以用来指定符合条件的情况下才缓存。

6）unless：否定缓存。当 unless 指定的条件为 true ，方法的返回值就不会被缓存。当然你也可以获取到结果进行判断。（通过 #result 获取方法结果）。

7）sync：是否使用异步模式。

@CacheEvict注解

用于清理缓存。

```java
@CacheEvict(value="myEntitys", allEntries=true)
public int delete(Integer id) {
    return myEntityMapper.delete(id);
}
```

1） value：缓存位置名称，不能为空。
2） key：缓存的key，默认为空。
3） condition：触发条件，只有满足条件的情况才会清除缓存，默认为空，支持SpEL。
4） allEntries：true表示清除value中的全部缓存，默认为false。

### 配置过期时间

```java
import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.cache.CacheManager;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheConfiguration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.cache.RedisCacheWriter;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.*;

import java.time.Duration;
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

@Configuration
public class RedisCacheManagerConfig {
    /**
     * 模板配置以及序列化配置
     */
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();
        template.setConnectionFactory(factory);
        JdkSerializationRedisSerializer jdkSerializationRedisSerializer = new JdkSerializationRedisSerializer();
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        template.setKeySerializer(stringRedisSerializer);
        template.setValueSerializer(jdkSerializationRedisSerializer);
        template.setHashKeySerializer(stringRedisSerializer);
        template.setHashValueSerializer(jdkSerializationRedisSerializer);
        template.afterPropertiesSet();
        return template;
    }


    @Bean
    RedisCacheWriter writer(RedisTemplate<String, Object> redisTemplate) {
        return RedisCacheWriter.nonLockingRedisCacheWriter(Objects.requireNonNull(redisTemplate.getConnectionFactory()));
```

## 缓存雪崩、击穿和穿透

在应用中引入缓存，就会出现缓存异常的三个问题：雪崩、击穿和穿透。

### 缓存雪崩

为了保证数据的一致性，通常会给缓存设置过期时间，当缓存数据过期后，用户的访问数据将不在缓存中，业务层需要重新访问数据库生成缓存，并将数据更新到Redis中，后续请求就可以直接命中缓存。

但是，如果大量缓存数据在同一时间过期，或者Redis数据库宕机。此时再有大量用户请求，将无法从Redis中读取，全部都会直接访问数据库，从而导致数据库压力剧增，严重时会产生数据库宕机。这就是缓存雪崩。

雪崩的原因：

1） 大量数据同时过期

2） Redis宕机

解决方式：

1） 均匀设置过期时间：生成缓存设置过期时间时添加随机数，避免同一时间过期。

2） 互斥锁：Redis未命中时，锁定业务处理，保证只有一个请求来构建缓存。互斥锁通常还需要设置超时时间，防止无响应。

3） 双Key策略：缓存数据时，设置主key，备用key，主key设置过期时间，备用key不过期。主key未命中时，启用备用key，更新时同时更新。

4） 后台更新缓存：业务层不更新缓存，由后台纯种负责缓存更新。

5） 服务熔断或请求限流。

6） 构建Redis高可用集群。

### 缓存击穿

缓存击穿是指热点数据在某个时间过期的时候，此时对于热点数据又有大量的请求，因为缓存中不存在，又需要请求数据库，增加数据库压力。

解决方案：

1） 预先设置热门数据，提前存入缓存。

2） 实时监控热门数据，调整key过期时长。

3） 对于热点数据进行二级缓存，并对于不同级别的缓存设定不同的失效时间。

4） 设置分布式锁。

### 缓存穿透

用户请求的数据即不在缓存中，也不在数据库中，此时缓存永远都不会生效，请求都会作用到数据库上，进而导致数据库宕机。

解决方案：

1） 缓存空对象：实现简单，但需要额外内存空间。

2） 布隆过滤器：内存占用少，实现复杂，存在误判。布隆过滤器用于判断元素是否在集合中，采用hash实现。

3） 设置黑白名单：实时监控，排查请求数据，限制服务。