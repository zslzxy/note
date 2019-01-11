[toc]
# Redis学习笔记

### redis参考资料
http://redisdoc.com/ --- redis的各种命令快捷键。

https://redis.io/ --- redis的官网。

http://www.redis.cn/ --- redis的中文官网。


### 一、Redis常见问题？
#### 1、什么是Redis？
Redis是一个互联网时代下的高性能的NoSQL数据库，用于存储 key-value 类型的数据，主要适用于缓存与消息的发布订阅操作。

#### 2、Redis的特点？
Redis是单线程的，本质上就是一个Key-value类型的数据库，**将整个数据库放在内存中进行使用**，对性能的提升非常明显。  
Redis能够存储复杂多样的数据类型，常用的有String、hash、list、set、zset五种数据类型。  
Redis能够实现消息队列服务以及类似于Zookeeper的注册中心。  
Redis能够给数据设置过期时间，过期进行清除。  
Redis的缺点是物理内存的限制，只适用于少量的高性能的数据操作。


### 二、大数据时代的数据
#### 1、特征，3V && 3高
##### 1）3V：
- 海量(Volume)
- 多样(Variety)
- 实时(Velocity)

##### 2）3高
- 高并发
- 高可扩
- 高性能

### 三：大数据时代的数据库遵循原则
#### 1、传统的关系型数据库遵循的是ACID原则：
- 原子性
- 一致性
- 独立性
- 持久性

#### 2、非关系型数据库（NoSQL）遵循的是CAP原则：
- C : 强一致性(数据必须是具有一致性的，因为redis主要是帮数据库减负，必须数据的一致性)
- A : 高可用性
- P : 分区容错性

**注意：** 任何分布式的数据库都只能够三选二，不可能实现三者全包

#### 3、BASE理论
Base就是为了解决关系数据库强一致性引起的可用性降低而提出的理论。主要是注重在某个时间段让系统实现 高可用、软状态、最终一致的需求。

![image](https://note.youdao.com/yws/api/personal/file/F3081821005849B3B0F6A9B875BB2C06?method=download&shareKey=3328266543ecf71da39586b9a4b18f48)


#### 4、RDBMS vs NoSQL的区别
##### RDBMS
- 高度组织化结构化数据
- 结构化查询语言（SQL）
- 数据和关系都存储在单独的表中。
- 数据操纵语言，数据定义语言
- 严格的一致性
- 基础事务

##### NoSQL
- 代表着不仅仅是SQL
- 没有声明性查询语言
- 没有预定义的模式
- 键 - 值对存储，列存储，文档存储，图形数据库
- 最终一致性，而非ACID属性
- 非结构化和不可预知的数据
- CAP定理
- 高性能，高可用性和可伸缩性


### 四、redis的常用命令
- select 2      	切换为数据库2 

- dbsize        	查看数据库的key的数量
- keys *        	显示所以的key
- flushdb       	清空当前数据库
- flushall       	清空所有数据库
- exists key  	查询某个key是否存在
- move k1 2 	将 k1 移动到 2 号库
- expire k1 15     将 k1 的过期时间设置为 15 秒钟
- ttl k1 		查看 k1 的状态，-1表示永久存在，-2表示已过期，其他数字表示还剩多上时间过期
- del k1   	删除 k1
- type k1		查看 k1 的类型


### 五、Redis数据类型
#### 1、String---字符串
##### 1）常用方法：
- set k1

- get k1
- del k1
- append k1 12345   在 k1 的value后面添加 12345
- strlen k1    得到 k1 的value的长度  。
- INCR k3    k3 的值每次增加 1（注意：k3 只能是数字）
- INCRBY k3 4    k3 的值每次增加 4（注意：k3 只能是数字）
- DECR k3   k3 的值每次减少 1（注意：k3 只能是数字）
- DECRBY k3 4    k3 的值每次减少 4（注意：k3 只能是数字）。
- GETRANGE k1 0 2   得到 k1 下标为 0 到 2 的数据
- SETRANGE k1 0 2 kkk   覆盖 k1 下标为 0 到 2 的数据为 kkk
- SETEX k4 10 vv   创建 k4 的时候设置过期时间
- SETNX kk aa   如果 kk 这个key不存在的话，就创建这个key，并赋值；如果存在的话就不赋值  。
- MSET k11 v11 k12 v12   设置多个值，看k11为v11，k12为v12
- MGET k11 k12   得到 k11，k12的值 


#### 2、hash---哈希，类似java里面的Map
##### 特征：
KV模式不变，但是Value是一个键值对。

##### 常用方法：
- hset user id 1   创建一个key为 user，value为 id 1的数据类型

- hget user id   得到user中的id的值
- hmset user id 1 name zsl password 123456  设置一个对象到user这个key中
- hmget user id name   得到user中的id与name的值
- hgetall user  得到user中的所有value
- HDEL user name   删除user中的name以及name属性的值
- hlen user   得到user中的value中的key的长度
- HEXISTS user id  查看user中是否有id这个属性，如果有的话，返回1，如果没有的话，返回0
- HKEYS user  得到user中所有的 key
- HVALS user   得到user中所有的value
- hsetnx user age 12   如果user中没有age这个属性的话，才添加这个属性

#### 3、list---列表
##### 特征：
单值多value。
- 它是一个字符串链表，left、right都可以插入添加；

- 如果键不存在，创建新的链表；
- 如果键已存在，新增内容；
- 如果值全移除，对应的键也就消失了。
- 链表的操作无论是头和尾效率都极高，但假如是对中间元素进行操作，效率就很惨淡了。

##### 1）常用方法：
- LPUSH list01 1 2 3 4   从左边放入到 list01 中

- RPUSH list01 1 2 3 4   从右边放入到 list01 中
- LRANGE list02 0 -1     从左边读取 list02 的值，0 -1 是下标，指的是读取list中的所有数据
- lpop list01   将 list01 的左边的第一个数据读取出来
- rpop list01   将 list01 的右边的第一个数据读取出来
- LINDEX list02 2  将 list02 的数据根据下标(索引)来进行查找
- lrem list01 2 3  从左到右将 list01 中的 2 个 3 删除掉
- LTRIM list01 0 2   将 list01 中的下表为 0 到 2 的内容截取出来重新赋值给 list01
- lset list01 1 x  将list01中的下标为 1 的数据置换为 x
- LINSERT list01 before x java   在list01中的值为 x 之前的位置添加字符串 java

#### 4、set---集合
##### 特征：
set集合不允许重复且无序的，当重复添加的时候，也只会保存一个值。
##### 常用方法：
- SADD set01 1  2 3     添加set01集合，数据为 1,2,3

- SMEMBERS set01     查看 set01集合的所有数据
- SISMEMBER set01 3   查看set01集合中是否有 3 这个值，如果有，返回 1，如果没有，返回 0。
- SCARD set01   获取集合中有多少个元素
- SREM set01 2   删除 set01 中的 value为2的元素
- SPOP set01  将 set01 中的数据随机单个出栈。
- SMOVE set02 set03 c  将 set02中的value为 c 的值迁移到 set03集合的后面
- sdiff set02 set03   两个集合的差集
- sinter set02 set03  两个集合的交集
- sunion set02 set03  两个集合的并集(注意，并集是经过去重以后得来的)

#### 5、zset---有序集合


### 六、Redis配置文件redis.conf
#### 1、配置缓存过期策略
**默认为永不过期：**
![image](https://note.youdao.com/yws/api/personal/file/40AA8ECA7AB64786A499229FC724B46E?method=download&shareKey=a19ea3ab998a945bed0565f6463bf5f7)

**其他缓存策略：**  
![image](https://note.youdao.com/yws/api/personal/file/617680A135044E5382E13AF17BE389FE?method=download&shareKey=a31f2f919b3118180c764c6d1ec8e679)


#### 2、redis.conf的常用参数说明
redis.conf 配置项说明如下：
1. Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
  daemonize no
2. 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
  pidfile /var/run/redis.pid
3. 指定Redis监听端口，默认端口为6379，作者在自己的一篇博文中解释了为什么选用6379作为默认端口，因为6379在手机按键上MERZ对应的号码，而MERZ取自意大利歌女Alessia Merz的名字
  port 6379
4. 绑定的主机地址
  bind 127.0.0.1
5.当 客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能
  timeout 300
6. 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
  loglevel verbose
7. 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null
  logfile stdout
8. 设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id
  databases 16
9. 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
  save <seconds> <changes>
  Redis默认配置文件中提供了三个条件：
  save 900 1
  save 300 10
  save 60 10000
  分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有10个更改以及60秒内有10000个更改。
10. 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大
  rdbcompression yes
11. 指定本地数据库文件名，默认值为dump.rdb
  dbfilename dump.rdb
12. 指定本地数据库存放目录
  dir ./
13. 设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步
  slaveof <masterip> <masterport>
14. 当master服务设置了密码保护时，slav服务连接master的密码
  masterauth <master-password>
15. 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH <password>命令提供密码，默认关闭
  requirepass foobared
16. 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息
  maxclients 128
17. 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区
  maxmemory <bytes>
18. 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no
  appendonly no
19. 指定更新日志文件名，默认为appendonly.aof
   appendfilename appendonly.aof
20. 指定更新日志条件，共有3个可选值： 
  no：表示等操作系统进行数据缓存同步到磁盘（快） 
  always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全） 
  everysec：表示每秒同步一次（折衷，默认值）
  appendfsync everysec
21. 指定是否启用虚拟内存机制，默认值为no，简单的介绍一下，VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析Redis的VM机制）
   vm-enabled no
22. 虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享
   vm-swap-file /tmp/redis.swap
23. 将所有大于vm-max-memory的数据存入虚拟内存,无论vm-max-memory设置多小,所有索引数据都是内存存储的(Redis的索引数据 就是keys),也就是说,当vm-max-memory设置为0的时候,其实是所有value都存在于磁盘。默认值为0
   vm-max-memory 0
24. Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的 数据大小来设定的，作者建议如果存储很多小对象，page大小最好设置为32或者64bytes；如果存储很大大对象，则可以使用更大的page，如果不 确定，就使用默认值
   vm-page-size 32
25. 设置swap文件中的page数量，由于页表（一种表示页面空闲或使用的bitmap）是在放在内存中的，，在磁盘上每8个pages将消耗1byte的内存。
   vm-pages 134217728
26. 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4
   vm-max-threads 4
27. 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启
  glueoutputbuf yes
28. 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法
  hash-max-zipmap-entries 64
  hash-max-zipmap-value 512
29. 指定是否激活重置哈希，默认为开启（后面在介绍Redis的哈希算法时具体介绍）
  activerehashing yes
30. 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件
  include /path/to/local.conf




### 七、redis的持久化
#### 1、RDB(Redis Database)
##### 1）简介：
在指定的时间间隔内将内存中的数据集快照写入磁盘。也即是通常说将内存的内容写入到Snapshot快照，当需要恢复数据的时候，直接从Snapshot快照中将数据恢复到内存中。
##### 2）特点：
a）redis会单独创建( Fork )一个子进程来进行持久化，会将数据写入到一个临时文件中，待到持久化过程结束以后，就将这个临时文件替换之前的持久化文件。整个过程中，主进程是不进行任何IO操作的，且对于数据恢复的完整性不是那么敏感，所以RDB方式比AOF更加高效。  
b）RDB的缺点是最后一次持久化的数据可能会被丢失。
##### 3）Fork：
fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等）
数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程，用于RDB的操作。
##### 4）RDB：
RDB保存的就是 dump.rdb 文件。
##### 5）redis默认配置设置:

```
a）900 秒内 更新一次。
b）300 秒内 更新10次。
c）60   秒内 更新10000次。
d）输入命令 save(直接保存，其他业务暂时停止) 或者 bgsave(后台异步保存)
```

##### 6）redis的dump.rdb文件如何恢复：
将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可。
#### 2、AOF（Append Only File）
##### 1）简介：
以日志的形式来记录每一个写的操作，将redis执行过的所有写的指令记录下来，文件只能追加，不能修改文件。redis启动之初会读取该文件重新构建数据，换而言之，redis重启的话就是将日志文件的内容的写的指令从前到后执行一次已完成数据的恢复。
##### 2）开启AOF功能：
**将 appendonly no 改为 appendonly yes**  
![image](https://note.youdao.com/yws/api/personal/file/3B00E0F232B04C49AC93D8B659F293D0?method=download&shareKey=a0ea7ab1b1bba9b1cc3ba140df9e4d78)

##### 3）AOF文件的恢复：
aof文件被破坏：redis-check-aof --fix进行修复aof文件，然后重启redis恢复数据。
##### 4）AOF的Rewrite：
特点：AOF采用的是文件追加的方式，文件会越来越大。为了避免这种情况，新增了重写机制。当aof文件到达伐值以后，redis回启东aof文件的内容压缩，只保留恢复数据的最小指令值。


### 八、redis事务
#### 1、简介
可以一次性执行多个命令，本质是一组命令的集合，。一个事务中的所有命令都会序列化，按顺序的执行而不会被其他命令插入，不允许加、塞。  

自己的理解：在一个队列中，一次性、顺序性、排他性的批处理一系列命令。

#### 2、Redis事务命令
##### 事务正常执行：


开启事务：输入命令 MULTI   
----中间是输入redis的其他命令----  
结束事务：输入命令 EXEC  
事务中途放弃：  
开启事务：输入命令 MULTI   
----中间是输入redis的其他命令----   
放弃事务：输入命令 DISCARD  ---   实现的功能是 事务不执行，所有的命令不执行。   



#### 3、事务的注意
事务中有一条事务出错，部分命令会导致全体连坐，相当于编译时异常，都不会执行，比如将 set 写成 seet；部分命令不会导致全体连坐，只会冤头债主，相当于运行时异常，其他命令仍然会执行，比如 INCR 命令。

#### 4、事务的乐观锁、悲观锁
##### 乐观锁(Optimistic Lock)：
乐观锁的概念就是每次自己拿数据的时候，别人都不会对自己的数据进行修改，所以对数据不会上锁；但是在更新数据的时候会判断一下这个期间是否有人来更新过数据，避免中途有人来操作过这个数据，实现这个判断的功能一般是使用 版本号控制等机制来实现。
##### 乐观锁机制：
提交的数据的版本号必须大于当前数据的版本才能够提交数据。
##### 乐观锁使用方法：

```
watch k1
MULTI
.....
EXEC
```
 
**注意：**
使用watch来监控事务，如果数据没有被更改，就可以事务的提交，不然就会失败。

##### 悲观锁(Pessimistic Lock)：
悲观锁的概念就是在自己拿数据的时候，害怕别人来对数据进行了其他的操作，所以每次拿数据的时候就会上锁，这样别人只能在所释放以后才能够重新拿到数据。传统的关系型数据库会用到悲观锁。


### 九.redis的安装
1.redis软件的安装：yum install gcc

2.解压redis文件：tar -zxvf redis-3.0.4.tar.gz

3、自定义安装位置，即在 /usr/local/redis/redis-3.0.4 下面执行    
make PREFIX=/usr/local/redis && make install  PREFIX=/usr/local/redis   

5.进入redis的解压目录，复制 redis.conf文件到根目录下面的 /myredis/文件夹下面，每次启动调用这个文件夹里面的redis配置文件   

6.修改redis.conf配置文件：

```
bind 192.168.21.133
daemonize yes
appendonly yes
```

7.启动redis服务器：在主目录（终端就行），输入 
```
./redis-server /myredis/redis6379.conf
```
开启服务器。

8.启动redis的客户端：
```
redis-cli –p 6379 -h 192.168.21.133
```
客户端连接服务器，然后 ping 一下就行。


### 十.redis普通集群
##### 注意：
每个redis第一次启动都是主服务器，需要进行下面的设置才能成为 主从复制 的redis集群
   
##### 步骤：
1.复制 /myredis/文件目录下的redis.conf文件，需要多少台机器，就复制多少次，修改里面的 port，修改conf文件的名字，然后通过不同的配置文件启动不同的redis服务器。  

如： 

```
redis-server /myredis/redis6379.conf 
redis-server /myredis/redis6380.conf
```


2.配置仆从redis服务器：slaveof 127.0.0.1 6379    
意思是将这个redis服务器设置为端口号为6379的redis服务器的仆从服务器


3.如何取消主从配置：slaveof  no one  


4.如何查看这个redis的主从信息：info replication  

### 十一、搭建redis-cluster集群：
1、 上传redis-3.0.4 与 redis-3.0.4.gem两个文件。  
先安装单机版redis，让其能够被外界访问，复制六个作为redis集群。

2、安装依赖：

```
yum install ruby
yum install rubygems
```

3、输入  gem install redis-3.0.4.gem

4、进入安装的这个目录，  cd redis-3.0.4/src

5、复制 redis-trib.rb 文件到与redis集群文件夹的同级目录。

6、启动redis的所有redis，编写脚本文件来启动6个redis服务。

7、在 redis-trib.rb 文件所在的目录里输入命令：
> ./redis-trib.rb create --replicas 1 192.168.21.140:7001 192.168.21.140:7002 192.168.21.140:7003 192.168.21.140:7004 192.168.21.140:7005 192.168.21.140:7006
