# 一、	在linux下安装redis #

1）
安装redis编译的c环境，yum install gcc-c++

2）将redis-2.6.16.tar.gz上传到Linux系统中

3）解压到/usr/local下  tar -xvf redis-2.6.16.tar.gz -C /usr/local

4）进入redis-2.6.16目录 使用make命令编译redis

5）在redis-2.6.16目录中 使用make PREFIX=/usr/local/redis install命令安装redis到/usr/local/redis中

6）拷贝redis-2.6.16中的redis.conf到安装目录redis中

7）启动redis 在bin下执行命令redis-server redis.conf（后台启动）

8）如需远程连接redis，需配置redis端口6379在linux防火墙中开发/sbin/iptables -I INPUT -p tcp --dport 6379 -j ACCEPT/etc/rc.d/init.d/iptables save

9）启动redis 在bin下执行命令redis-cli开启客户端

快捷命令： ps -er | grep redis 查看redis服务是否启动及其ID
可以用  kill -9 ID 杀死进程，关闭redis-server
若是前台模式启动，可以用Ctrl 加 C进行server的关闭

# 二、	Jedis的使用 #
1)	导包 ![](https://i.imgur.com/TKaEHcU.png)

2)	 ![](https://i.imgur.com/Ktyadcs.png)

# 三、	reis数据类型——字符串 #
1.设置值   set name zhangsan

2.取值     get name

3.取值再设置值 getset name lisi (返回zhangsan)

4.get name (返回李四)

5.incr num (数字字符串增加1)

6.decr num(数字字符串减1)

7.incrby num 5 (数字字符串增加5)

8.decrby num 5 (数字字符串减5)

9.append num 3(拼凑字符串)


# 四、	Redis数据类型——hash #
1.设置值  hset hash name zhangsan (hset key filed value)

2.取值    hget hash name (hget key filed)

3.设置多值  hmset hash name zhangsan age 25 addr hunan(hmset key filed value filed value filed value)  相当于hmset key 加上一个大map

4.取多值  hmget hash name age addr (hmset hash key filed filed filed)

5.删除某个filed : hdel hash name (hdel key filed)

6.删除整个hashkey : del hash (del key)

7.设置key中的field值增加increment ： hincrby hash age 5(hincrby key filed number)其中number可以为负数表示减少

8.判断指定的key中filed是否存在： hexists hash name (hexists key field)

9.获取key包含filed的数量：hlen hash (hlen key)

10.获得所有的key ： hkeys hash (hkeys key)

11.获得所有的value ： hvals hash (hvals key)

12.获取key中所有的filed-value ： hgetall hash (hgetall key)

# 五、	Redis数据类型——list #
1.设置值（从头部压栈）：lpush mylist a b c (lpush key x x x)

2.设置值（从尾部压栈）：rpush mylist 1 2 3 (rpush key x x x)

3.查看列表（0表示头部即正数第一个元素，-1表示尾部即倒数第一个元素，-2表示倒数第二个元素）：lrange mylist 0 -1 (lrange key start end) 0到-1表示遍历所有元素

4.取值（从头部弹栈）： lpop mylist (lpop key)

5.取值（从尾部弹栈）： rpop mylist (rpop key)

6.获取列表中个数：llen mylist (llen key)

7.设置值（从头部压栈，若key不存在，则失败）：lpushx mylist a (lpushx key x )

8.删除元素（删除count 个 值为 value的元素）：从头删除lrem mylist 2 3(lrem key count value) 从尾部删除 lrem mylist -2 3  count大于0从头删小于0从尾删 如果等于0 则删除所有的 等于value的值

9.设置值（指定角标）：lset mylist 2 xxx (lset key index value)

10.插入值（指定元素后）：linsert mylist after|before a yyy 在list中a的前边后者后面插入yyy

11.将链表中尾部元素弹出加到头部：rpoplpush mylist mylist2(rpoplpush resource destination) 将mylist尾部元素弹出加到mylist2的头部。  相当于循环


# 六、	Redis数据类型——set（无序，不允许出现重复元素） #
1.添加成员：sadd set a b c (sadd key values)

2.获取所有成员：smembers set (smembers key) 获取set中的所有值成员。

3.删除成员：srem set a (srem set members) 删除set中指定成员。

4.判断set中是否存在某成员：sismember set a (sismember key member)

5.集合的差集运算：sdiff set1 set2 (sdiff key1 key2)返回key1与key2的差集，结果与key的顺序有关，返回的应该是key1中有的但是key2中没有的。即key1-key2

6.集合的交集运算：sinter set1 set2 (sinter key1 key2)返回key1和key2的交集。

7.集合的并集运算：sunion set1 set2 (sunion key1 key2)返回key1和key2的并集，即它们两的全部值去重。

8.集合中成员数量：scard set (scard key)返回集合中成员数量

9.随机返回set中成员：srandmember set(srandmember key)随机返回key中某个成员

10.将key1.key2相差的成员储存在某个位置：sdiffstore key3 key1 key2 (sdiffstore destination key1 key2) 将key1 key2的sdiff结果存储在destination中

11.将key1.key2交集的成员储存在某个位置：sinterstore key3 key1 key2 (sinterstore destination key1 key2) 将key1 key2的sinter结果存储在destination中

12 将key1.key2并集的成员储存在某个位置：sunionstore key3 key1 key2 (sunionstore destination key1 key2) 将key1 key2的sunion结果存储在destination中


# 七、	Redis数据类型——sortedset（有顺序的，有权重的set） #
1.添加元素：zadd sortedset 70 zhangsan 80 lisi 90 wangwu (zadd key score member score2 member2 score3 member3)

2.获取元素的权重：zscore sortedset zhangsan (zscore key member)

3.获取元素中的成员数量：zcard sortedset (zcard key)返回成员数量

4.删除元素：zrem sorted zhangsan lisi (zrem key members)

5.范围查询①：zrange sortedset 0 -1 withscores (zrange key start end [withscores])加上withscores还会返回权重，若不加的话则只返回member。默认排名范围从小到大。

6.范围查询②：zrevrange sortedset 0 -1 withscores (zrevrange key start end [withscores])会按权重大小从大到小的顺序返回，加上withscores会连带权重值一起返回。

7.范围删除①：zremrangebyrank sortedset 0 2(zremrangebyrank key start end) 按照排名范围删除元素 （默认排名是从小到大）

8.范围删除②：zremrangebyscore sortedset 70 80 (zremrangebyscore key start end)按照分数范围删除元素。



# 八、	Redis中keys的通用操作 #
1.匹配操作： keys * (keys pattern) 或者 keys my* 将会返回与该key匹配的所有keys。

2.删除操作：del key1 key2 删除指定的key，可同时删除多个。

3.判断key是否存在：exists key 若该key存在返回1，不存在返回0。

4.为当前的key重命名：rename mytest mytest1 (rename key key1)将key重命名为key1。

5.设置key的过期时间：expire key 1000设置过期时间，单位是秒

6.获取key所剩的时间：ttl key 返回剩余秒数，若返回为-1，设表示没有设置超时时间，若返回为-2，则说明该key超时不存在。

7.获取key的类型：type key 以字符串的方式返回key的类型。有string、list、set、hash、zset五种类型，不存在返回none。


# 九、	Redis特性 #
1.移库

一个redis实例最多可提供16个数据库，下标从0到15，默认使用0号库。可通过select num 选择连接某个数据库。还可以通过 move key num 将key从当前库移至num库下。

2.订阅与发布

subscribe channel (订阅频道)，例如subscribe cctv 订阅cctv频道

psubscribe channe* (批量订阅频道)，例如psubscribe cctv ccav espn

publish channel content (在指定的频道中发布消息). 例如publish cctv
today is a good day ，所有订阅cctv的连接将收到消息。

3.redis事务

multi：用于标志事务的开启，其后执行的命令将存放在命令队列，相当于关系型数据库中的start transaction

exec：提交事务，相当于关系型数据库中的commit

discard：回滚事务，关系型数据库中的rollback

注意：redis中的事务即使队列中某一句执行失败，其余操作也不会受影响。这点不同于关系型数据库中的事务。


# 十、	Redis持久化 #
Redis的高性能是因为数据都存储在了内存中，为了使Redis在重启之后仍能保证数据不丢失，需要将数据从内存中同步到硬盘中，这一过程就是持久化，Redis支持两种持久化方式，第一种是RDB方式，还有一种是AOF方式，可以单独使用其中一种或两种同时使用（Redis默认是RDB方式）。

1.RDB持久化（默认支持，无需配置）

该机制是指在指定的时间间隔内将内存中的数据集快照写入磁盘。

优势：简单，用于存储大数据集

劣势：有可能会丢失数据（在两次快照中间，如果服务器挂掉，则会丢失数据）

快照参数配置：

   save 900 1 # 每900秒（15分钟）至少有1个key发生变化，则dump内存快照。

   save 300 10 # 每300秒（5分钟）至少有10个key发生变化，则dump内存快照。

save 60 10000 # 每60秒（1分钟）至少有10000个key发生变化，则dump内存快照。


2.AOF持久化

该机制以日志的形式记录服务器所处理的每一个写操作，在Redis服务器启动之初会读取该文件来重新构建数据库，以保证启动后数据库中的数据是完整的。

优势：数据持久化及时

劣势：运行效率慢

参数配置：

      在redis.conf中将appendonly no 改为 appendonly yes
      aof存储时机：appendfsync always #每次有数据修改时都会写入AOF文件
         appendfsync everysec  #每秒钟同步一次，该策略为AOF缺省（默认）策略
         appendfsync no   #从不同步，高效但是不会持久化。

3.无持久化

我们可以通过配置的方式禁用Redis服务器的持久化功能，这样我们就可以将Redis视为一个功能加强版的memcached了。
