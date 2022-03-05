---
title: Redis数据库
categories: 数据库
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-03-06 20:30:00
top:
tags: Redis
typora-root-url: ..
---



# 1. redis介绍

## 1.1 Redis 是什么

REmote DIctionary Server(Redis) 是一个由 Salvatore Sanfilippo 写的 key-value 存储系统。它是一种非关系型数据库， 提供了一些丰富的数据结构，包括 lists、sets、ordered sets 以及 hashes ，当然还有和 Memcached 一样的 strings 结构。Redis 当然还包括了对这些数据结构的丰富操作。

Redis 常被称作是一款数据结构服务器（data structure server）。Redis 的键值可以包括字符串（strings）类型，同时它还包括哈希（hashes）、列表（lists）、集合（sets）和 有序集合（sorted sets）等数据类型。

对于这些数据类型，你可以执行原子操作。例如：对字符串进行附加操作（append）；递增哈希中的值；向列表中增加元素；计算集合的交集、并集与差集等。

## 1.2 Redis 的优点

- 性能极高 : Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型：Redis 支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子：Redis 的所有操作都是原子性的，同时 Redis 还支持对几个操作全并后的原子性执行。
- 丰富的特性：Redis 还支持 publish/subscribe, 通知, key 过期等等特性。

使用 Redis 只需要下载对应的软件包开箱即用，截止目前（2019.1）最新的版本有 5.* 可用

## 1.3 redis应用场景

- 用来做缓存(ehcache/memcached)——redis的所有数据是放在内存中的（内存数据库）
- 可以在某些特定应用场景下替代传统数据库——比如社交类的应用
- 在一些大型系统中，巧妙地实现一些特定的功能：session共享、购物车
- 只要你有丰富的想象力，redis可以用在可以给你无限的惊喜…….

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/Snipaste_2020-03-06_20-59-24.png)

## 1.4 redis对比mysql

Redis的数据存放在内存，所以速度快但是会受到内存空间限制。MySQL存放在硬盘，在速度上没有Redis快，但是存放的数据量要多的多。所以redis适合放一些频繁使用，比较热的数据，因为是放在内存中，读写速度都非常快，一般会应用在下面一些场景:排行榜、计数器、消息队列推送。

Redis是一个K-V数据库，同时还支持List/Hash/Set/Sorted Set等几个简单数据结构，所以只能以这些数据结构为基础实现功能。而MySQL这点就不必说了。

Redis是内存型数据库，不可能存储过大的数据；而mysql是存储在硬盘上的，可以支持更大规模的数据，成本更低。

目前大多数公司的存储都是mysql + redis，mysql作为主存储，redis作为辅助存储被用作缓存，加快访问读取的速度，提高性能。

## 1.5 推荐阅读

- [redis官方网站](https://redis.io/) 
- [redis中文官网](http://redis.cn/) 



# 2. Redis 安装

## 2.1 Linux

### 2.1.1 在线安装

直接输入命令 

```
sudo apt-get install redis-server
```

安装完成后，Redis服务器会自动启动。
使用

```
ps -aux|grep redis
```

命令可以看到服务器系统进程默认端口6379

配置文件为/etc/redis/redis.conf(在线安装推荐)或者 /usr/local/redis/redis.conf(手动安装)

**【调整默认配置】**

```
sudo vi /etc/redis/redis.conf
```

①开启Redis的远程连接：
注释掉绑定地址 ，不然只允许本机访问

`# bind 127.0.0.1`

②修改Redis的默认端口
`port 6379`

③关闭保护模式：

`redis.conf修改 88行为no`

④开启后台启动:

` redis.conf修改 136行为yes `

⑤添加Redis的访问账号：
Redis服务器默认是不需要密码的，假设设置密码为123456。

`config set requirepass 123456`

如此，便将密码设置成了123456
设置之后，可通过以下指令查看密码

```
config get requirepass
```

密码设置之后，当你退出再次连上redis的时候，就需要输入密码了，不然是无法操作的。这里有两种方式输入密码，一是连接的时候直接输入密码，而是连接上之后再输入密码，分别如下所示：

```
auth 123456
```

**配置完成后重新启动服务器**

sudo service redis-server   +下面的命令：

start 启动服务

restart 重启服务

stop 停止服务

```
sudo /etc/init.d/redis-server restar
sudo service redis-server restart
sudo redis-server /etc/redis/redis.conf
```

### 2.1.2 从源码开始安装

进入 root 目录，并下载 Redis 的程序包：

```
sudo su
wget http://download.redis.io/redis-stable.tar.gz
```

在目录下，解压安装包，生成新的目录 redis-4.0.14：

```
tar -xzvf redis-stable.tar.gz
```

进入解压之后的目录，进行编译：

```
cd redis-stable
make
```

说明：如果没有明显的错误，则表示编译成功。

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558538164013.png)

##### 查看重要文件

在 Redis 安装完成后，注意一些重要的文件，可用 **ls** 命令查看。

- 服务端：src/redis-server
- 客户端：src/redis-cli
- 默认配置文件：redis.conf

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558543779151.png)

##### 设置环境变量

为了今后能更方便地打开 Redis 服务器和客户端，可以将 src 目录下的 redis-server 和 redis-cli 添加进环境变量所属目录里面。

```
cp redis-server /usr/local/bin/
cp redis-cli /usr/local/bin/
```

添加完成后在任何目录下输入 `redis-server` 可启动服务器，输入 `redis-cli` 可启动客户端。

##### 运行测试

在前面的步骤设置完成后可以运行测试，确认 Redis 的功能是否正常：

```
cd /root/redis-2.8.4
make test
```

运行测试需要花费一定的时间。操作截图如下：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539004966.png)



### 2.1.3 Redis 启动

Redis 是一个服务端和客户端配合的程序，和 Mysql 类似，因此要使用 Redis 需要先启动服务端，客户端有多种形式，比如在 Java 中连接 Redis 服务器也是扮演了一个客户端的角色，本节实验中采用终端的形式来对 Redis 的各项操作进行练习。

####  启动 Redis-server

在命令行终端输入如下命令：

```
redis-server  
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539083727.png)

说明：从以上的截图中，可以发现启动的端口为缺省的 6379。用户可以在启动的时候，指定具体的配置文件，并在其中指定启动的端口。

保持此终端的运行，Ctrl+Shift+T 重开一个终端标签。

####  查看 Redis

在命令行终端执行如下命令查看 Redis：

```
ps -ef | grep redis
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539129020.png)

通过端口号检查 Redis 服务器状态：

```
netstat -nlt|grep 6379
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539149394.png)

####  启动 Redis-client

在命令行终端执行如下命令启动 Redis-client：

```
redis-cli
```

操作结果：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539282420.png)

至此，redis 启动完成。

在有的环境下，redis 交互环境可能出现中文乱码的情况，解决办法是用下列命令启动 redis 客户端：

```
redis-cli --raw
```



## 2.2 windows安装

点击: <https://github.com/microsoftarchive/redis/releases>

然后选择 msi 文件

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1568647099924.png)

下载完成后双击打开安装（安装过程中一定要添加到 path 环境）

### 启动REdis

在命令行终端执行如下命令启动 Redis-client：

```
redis-cli
```



# 3. 数据类型

Redis 不仅仅是简单的 key-value 存储器，同时也是一种 data structures server。传统的 key-value 是指支持使用一个 key 字符串来索引 value 字符串的存储，而 Redis 中，value 不仅仅支持字符串，还支持更多的复杂结构，包括列表、集合、哈希表等。

现在我们一一讲解：Redis keys 是采用二进制安全，这就意味着你可以使用任何二进制序列作为重点，比如："foo" 可以联系一个 JPEG 文件；空字符串也是一个有效的密钥。

Redis 字符串是二进制安全的，这意味着一个 Redis 字符串能包含任意类型的数据，例如： 一张经过 base64 编码的图片或者一个序列化的对象。通过这样的方式，Redis 的字符串可以支持任意形式的数据，但是对于过大的文件不适合存入 redis，一方面系统内存有限，另外一方面字符串类型的值最多能存储 512M 字节的内容。

**知识点** 

- Redis strings
- Redis Lists
- Redis Hashes
- Redis set

### 3.1 Redis strings

+ **set** 设置值

+ **get** 获取值

```
$ redis-cli
> set mykey somevalue
> get mykey
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539338731.png)

如上例所示，可以使用 set 和 get 命令来创建和检索 strings。

+ **mset** 设置多个值
+ **mget** 获取多个值
+ **keys *** 获取所有的键

Redis 可以运用 mset 和 mget 命令一次性完成多个 key-value 的对应关系，使用 mget 命令，Redis 返回一个 value 数组：

```
> mset a 10 b 20 c 30
> mget a b c
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539651556.png)



+ **append** 添加字符
+ **del**  删除
+ **incr/decr** 增加/减少

即使 string 是 Redis 的基本类型，也可以对其进行一些有趣的操作，例如加法器：

```
> set counter 100 # 初始化
> incr counter   # +1
> incr counter   # +1
> incrby counter 50 # +50 自定义计数
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539601807.png)

incr 命令让 the value 成为一个整数，运行一次 incr 便加一。incrby 命令便是一个加法运算。类似的命令如减法运算为： decr 和 decrby。



注意：set 命令将取代现有的任何已经存在的 key。set 命令还有一个提供附加参数的选项，我们能够让 set 命令只有在没有相同 key 的情况下成功，反之亦然，可以让 set 命令在有相同 key 值的情况下成功：

```
> set mykey newval nx
> set mykey newval xx
```

操作截图;

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539410969.png)

**set完整内容**

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1559386310906.png)

参数含义:

+ set : 设置一个键值对内容
+ key : 键名
+ value : 值内容
+ [EX seconds] : 设置指定的过期时间，以秒为单位。(可选)
+ [PX milliseconds] : 设置指定的过期时间，以毫秒为单位。(可选)
+ NX : 只有键key不存在的时候才会设置key的值
+ XX : 只有键key存在的时候才会设置key的值

### 3.2 Redis Lists

Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边），lpush 命令插入一个新的元素到头部，而 rpush 命令插入一个新元素到尾部。当这两个操作中的任一操作在一个空的 Key 上执行时就会创建一个新的列表。相似的，如果一个列表操作清空一个列表，那么对应的 key 将被从 key 空间删除。

+ **lpush/rpush** 队列左边或者右边增加内容
+ **lrange** 获取指定长度的内容

push 一类的命令的返回值为 list 的长度。这里有一些类表操作和结果的例子：

```
> rpush mylist A
> rpush mylist B
> lpush mylist first
> lrange mylist 0 -1
```

操作截图:

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539705684.png)

**注意**：lrange 需要两个索引，0 表示 list 开头第一个，-1 表示 list 的倒数第一个，即最后一个。-2 则是 list 的倒数第二个，以此类推。

这些命令都是可变的命令，也就是说你可以一次加入多个元素放入 list：

```
> rpush mylist 1 2 3 4 5 "foo bar"
> lrange mylist 0 -1
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539832387.png)

+ **ltrim** 对一个列表进行修剪(trim)，保留指定区间内的元素，不在指定区间之内的元素都将被删除。
+ **lpop/rpop** 从队列左边或者右边弹出内容

在 Redis 的命令操作中，还有一类重要的操作：pop，它可以弹出一个元素，简单的理解就是获取并删除第一个元素，和 push 类似的是它也支持双边的操作，可以从右边弹出一个元素也可以从左边弹出一个元素，对应的指令为 rpop 和 lpop：

```
> del mylist
> rpush mylist a b c
> rpop mylist
> lrange mylist 0 -1
> lpop mylist
> lrange mylist 0 -1
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558539989212.png)

+ **lpushx/rpushx** key存在时才插入数据, 不存在是不做任何处理

一个列表最多可以包含 4294967295（2 的 32 次方减一） 个元素，这意味着它可以容纳海量的信息，最终瓶颈一般都取决于服务器内存大小。

事实上，在高级的企业架构当中，会把缓存服务器分离开来，因为数据库服务器和缓存服务器的特点各异，比如对于数据库服务器应该用更快、更大的硬盘，而缓存专用服务器则偏向内存性能，一般都是 64GB 起步。

#### List 阻塞操作

理解阻塞操作对一些请求操作有很大的帮助，关于阻塞操作的作用，这里举一个例子。

假如你要去楼下买一个汉堡，一个汉堡需要花一定的时间才能做出来，非阻塞式的做法是去付完钱走人，过一段时间来看一下汉堡是否做好了，没好就先离开，过一会儿再来，而且要知道可能不止你一个人在买汉堡，在你离开的时候很可能别人会取走你的汉堡，这是很让人烦的事情。

阻塞式就不一样了，付完钱一直在那儿等着，不拿到汉堡不走人，并且后面来的人统统排队。

Redis 提供了阻塞式访问 brpop 和 blpop 命令。用户可以在获取数据不存在时阻塞请求队列，如果在时限内获得数据则立即返回，如果超时还没有数据则返回 null。

#### List 常见应用场景

分析 List 应用场景需要结合它的特点，List 元素是线性有序的，很容易就可以联想到聊天记录，你一言我一语都有先后，因此 List 很适合用来存储聊天记录等顺序结构的数据。



### 3.3 Redis 无序集合

Redis 集合（Set）是一个无序的字符串集合。你可以以 O(1) 的时间复杂度 (无论集合中有多少元素时间复杂度都是常量）完成添加、删除以及测试元素是否存在。

Redis 集合拥有令人满意的不允许包含相同成员的属性，多次添加相同的元素，最终在集合里只会有一个元素，这意味着它可以非常方便地对数据进行去重操作，一个 Redis 集合的非常有趣的事情是它支持一些服务端的命令从现有的集合出发去进行集合运算，因此你可以在非常短的时间内进行合并（unions）, 求交集（intersections）, 找出不同的元素（differences of sets）。

+ **sadd/srem** 添加/删除元素
+ **sismember** 判断是否为set的一个元素
+ **smembers** 返回该集合的所有成员 

```
> sadd myset 1 2 3
> smembers myset
```

sadd 命令产生一个无序集合，返回集合的元素个数。smembers 用于查看集合。

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540427157.png)

sismember 用于查看集合是否存在，匹配项包括集合名和元素个数。匹配成功返回 1，匹配失败返回 0。

```
> sismember myset 3
> sismember myset 30
> sismember mys 3
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540577228.png)

- **sdiff** 返回一个集合与其他集合的差异
- **sinter** 返回几个集合的交集
- **sunion** 返回几个集合的并集



### 3.4 Redis 有序集合

Redis 有序集合与普通集合非常相似，是一个没有重复元素的字符串集合。不同之处是有序集合的每一个成员都关联了一个权值，这个权值被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是权值可以是重复的。

使用有序集合你可以以非常快的速度（O(log(N))）添加、删除和更新元素。因为元素是有序的, 所以你也可以很快的根据权值（score）或者次序（position）来获取一个范围的元素。访问有序集合的中间元素也是非常快的，因此你能够使用有序集合作为一个没有重复成员的智能列表。在有序集合中，你可以很快捷的访问一切你需要的东西：有序的元素，快速的存在性测试，快速访问集合的中间元素！简而言之使用有序集合你可以完成许多对性能有极端要求的任务，而那些任务使用其它类型的数据库真的是很难完成的。

zadd 与 sadd 类似，但是在元素之前多了一个参数，这个参数便是用于排序的。形成一个有序的集合。

```
> zadd hackers 1940 "Alan Kay"
> zadd hackers 1957 "Sophie Wilson"
> zadd hackers 1953 "Richard Stallman"
> zadd hackers 1949 "Anita Borg"
> zadd hackers 1965 "Yukihiro Matsumoto"
> zadd hackers 1914 "Hedy Lamarr"
> zadd hackers 1916 "Claude Shannon"
> zadd hackers 1969 "Linus Torvalds"
> zadd hackers 1912 "Alan Turing"
```

查看集合：**zrange 是查看正序的集合**，**zrevrange 是查看反序的集合**。0 表示集合第一个元素，-1 表示集合的倒数第一个元素。

```
> zrange hackers 0 -1
> zrevrange hackers 0 -1
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540664912.png)

使用 withscores 参数返回记录值。

```
> zrange hackers 0 -1 withscores
```

操截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540712107.png)



### 3.5 Redis Hashes

Redis Hashes 是字符串字段和字符串值之间的映射，因此它们是展现对象的完美数据类型。例如一个有名、姓、年龄等等属性的用户：一个带有一些字段的 hash 仅仅需要一块很小的空间存储，因此你可以存储数以百万计的对象在一个小的 Redis 实例中。哈希主要用来表现对象，它们有能力存储很多对象，因此你可以将哈希用于许多其它的任务。

- **hset/hget** 设置/获取散列值
- **hmset/hgetall** 设置/获取多对散列值

```
> hmset user:1000 username antirez birthyear 1977 verified 1
> hget user:1000 username
> hget user:1000 birthyear
> hgetall user:1000
```

格式：`hset key field values`

格式：`hmset [key:[seq_num]] field values field values`

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540146964.png)

hmset 命令设置一个多域的 hash 表，hget 命令获取指定的单域，hgetall 命令获取指定 key 的所有信息。hmget 类似于 hget，只是返回一个 value 数组。

```
> hmget user:1000 username birthyear
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540248325.png)

同样可以根据需要对 hash 表的表项进行单独的操作，例如 hincrby： （原本 birthyear 为 1977，见上一图）

```
> hincrby user:1000 birthyear 10
> hincrby user:1000 birthyear 10
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540391566.png)

- **hsetnx** 如果散列已经存在,则不设置
- **hkeys/hvals** 查看散列的keys/valus
- **hlen** 返回散列指定的域(field)
- **hexists** 判断是否存在

参看:

<http://redis.io/topics/data-types-intro>

# 4. 操作

前面讲述了 Redis 的基本数据类型，接下来继续讲解 Redis 相关命令及管理操作。

在 Redis 中，命令大小写不敏感。

- 适合全体类型的常用命令
- Redis 时间相关命令
- Redis 设置相关命令
- 查询信息

### 适合全体类型的常用命令

启动 redis 服务和 redis-cli 命令界面继续后续实验：

```
sudo service redis-server start
redis-cli
```

####  EXISTS and DEL

exists key：判断一个 key 是否存在，存在返回 1，否则返回 0。

`del` key：删除某个 key，或是一系列 key，比如：del key1 key2 key3 key4。成功返回 1，失败返回 0（key 值不存在）。

```
> set mykey hello
> exists mykey
> del mykey
> exists mykey
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540770677.png)

####  TYPE and KEYS

`type` key：返回某个 key 元素的数据类型 (none:不存在，string:字符，list:列表，set:元组，zset:有序集合，hash:哈希)，key 不存在返回空。

`keys` key—pattern：返回匹配的 key 列表，比如：keys foo* 表示查找 foo 开头的 keys。

```
> set mykey x
> type mykey
> keys my*
> del mykey
> keys my*
> type mykey
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540853713.png)

####  RANDOMKEY and CLEAR

`randomkey`：随机获得一个已经存在的 key，如果当前数据库为空，则返回空字符串。

```
> randomkey
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558540896368.png)

`clear`：清除界面。

```
> clear
```

####  RENAME and RENAMENX

`rename oldname newname`：更改 key 的名字，新键如果存在将被覆盖。

`renamenx oldname newname`：更改 key 的名字，新键如果存在则更新失败。

比如这里 randomkey 结果为 mylist，将此 key 值更名为 newlist。

```
> randomkey
> rename mylist newlist
> exists mylist
> exists newlist
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558542077323.png)

####  DBSIZE

`dbsize`：返回当前数据库的 key 的总数。

```
> dbsize
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558542094849.png)

### Redis 时间相关命令

下面我们将会学习 Redis 时间相关命令。

####  限定 key 生存时间

这同样是一个无视数据类型的命令，对于临时存储很有用处。避免进行大量的 DEL 操作。

`expire`：设置某个 key 的过期时间（秒)，比如：expire bruce 1000 表示设置 bruce 这个 key 1000 秒后系统自动删除，注意：如果在还没有过期的时候，对值进行了改变，那么那个值会被清除。

```
> set key some-value
> expire key 10
> get key       (马上执行此命令)
> get key       (10s后执行此命令)
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558542163817.png)

结果显示：执行 expire 命令后，马上 get 会显示 key 存在；10 秒后再 get 时，key 已经被自动删除。

####  查询 key 剩余生存时间

限时操作可以在 set 命令中实现，并且可用 ttl 命令查询 key 剩余生存时间。

`ttl`：查找某个 key 还有多长时间过期，返回时间单位为秒。

```
> set key 100 ex 30
> ttl key
> ttl key
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558542626030.png)

####  清除 key

`flushdb`：清空当前数据库中的所有键。

`flushall`：清空所有数据库中的所有键。

```
> flushdb
> flushall
```

### Redis 设置相关命令

Redis 有其配置文件，可以通过 client-command 窗口查看或者更改相关配置。下面介绍相关命令。

####  CONFIG GET and CONFIG SET

`config get`：用来读取运行 Redis 服务器的配置参数。

`config set`：用于更改运行 Redis 服务器的配置参数。

`auth`：认证密码。

下面针对 Redis 密码的示例：

```
> config get requirepass  # 查看密码
> config set requirepass 123456  # 设置密码为 123456
> config get requirepass  # 报错，没有认证
> auth 123456  # 认证密码
> config get requirepass
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558542785780.png)

由结果可知，刚开始时 Reids 并未设置密码，密码查询结果为空。然后设置密码为 123456，再次查询报错。经过 auth 命令认证后，可正常查询。

可以通过修改 Redis 的配置文件 redis.conf 修改密码。

# 5. 查询信息

`info [section]`：查询 Redis 相关信息。

info 命令可以查询 Redis 几乎所有的信息，其命令选项有如下：

- server: Redis server 的常规信息
- clients: Client 的连接选项
- memory: 存储占用相关信息
- persistence: RDB and AOF 相关信息
- stats: 常规统计
- replication: Master/Slave 请求信息
- cpu: CPU 占用信息统计
- cluster: Redis 集群信息
- keyspace: 数据库信息统计
- all: 返回所有信息
- default: 返回常规设置信息

若命令参数为空，info 命令返回所有信息。

```
> info server
```

操作截图：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558542924652.png)

参考:

<http://redis.io/commands/config-resetstat>

# 6. 高级操作

前面学习了 Redis 的基础知识和基本命令，接下来继续讲解 Redis 的高级应用，包括：安全性设置，主从复制，事务处理，持久化机制，虚拟内存的使用。

**知识点** 

- 安全性
- 主从复制
- 事务处理
- 持久化机制
- 虚拟内存的使用

### 安全性

涉及到客户端连接是需要指定密码的（由于 redis 速度相当的快，一秒钟可以 150K 次的密码尝试，所以需要设置一个强度很大的密码）。

设置密码的方式有两种：

- 使用 `config set` 命令的 requirepass 参数，具体格式为 `config set requirepass “password”`。
- 在 redis.conf 文件中设置 requirepass 属性，后面为密码。

输入认证的方式也有两种：

- 登录时可以使用 `redis-cli -a password`。
- 登录后可以使用 `auth password`。

####  设置密码

第一种密码设置方式在上一个实验中已经提到（在 CONFIG SET 命令讲解的实例），此处我们来看看第二种方式设置密码。

首先需要进入 Redis 的安装目录，然后修改配置文件 redis.conf。根据 grep 命令的结果，使用 vim 编辑器修改 “# requirepass foobared” 为 “requirepass test123”，然后保存退出。

```
sudo vim /etc/redis/redis.conf
```

编辑 redis.conf 的结果：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/1558543168293.png)

重启之后就需要验证密码了

### 主从复制

为了分担服务器压力，会在特定情况下部署多台服务器分别用于缓存的读和写操作，用于写操作的服务器称为主服务器，用于读操作的服务器称为从服务器。

从服务器通过 psync 操作同步主服务器的写操作，并按照一定的时间间隔更新主服务器上新写入的内容。

Redis 主从复制的过程：

1. Slave 与 Master 建立连接，发送 psync 同步命令。
2. Master 会启动一个后台进程，将数据库快照保存到文件中，同时 Master 主进程会开始收集新的写命令并缓存。
3. 后台完成保存后，就将此文件发送给 Slave。
4. Slave 将此文件保存到磁盘上。

Redis 主从复制特点：

1. 可以拥有多个 Slave。
2. 多个 Slave 可以连接同一个 Master 外，还可以连接到其它的 Slave。（当 Master 宕机后，相连的 Slave 转变为 Master）
3. 主从复制不会阻塞 Master，在同步数据时， Master 可以继续处理 Client 请求。
4. 提高了系统的可伸缩性。

从服务器的主要作用是响应客户端的数据请求，比如返回一篇博客信息。

上面说到了主从复制是不会阻塞 Master 的，就是说 Slave 在从 Master 复制数据时，Master 的删改插入等操作继续进行不受影响。

如果在同步过程中，主服务器修改了一篇博客，而同步到从服务器上的博客是修改前的。这时候就会出现时间差，即修改了博客过后，在访问网站的时候还是原来的数据，这是因为从服务器还未同步最新的更改，这也就意味着非阻塞式的同步只能应用于对读数据延迟接受度较高的场景。

**【主从服务器的配置】**

（1）首先确保云服务器开启了6379（默认的redis端口）端口，本地电脑6379端口一般默认是开的。

开启云服务器端口的方法：

① 新建安全组

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201020211017731.png" style="zoom: 80%;" />

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201020211134490.png" style="zoom:80%;" />

② 将新建的安全组加入到云服务器实例中

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201020211429320.png" style="zoom:80%;" />

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201020211632016.png" style="zoom:80%;" />

（2）主服务器配置

装好redis之后，`redis-cli`可启动redis。

命令`ps -aux|grep redis`出现：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201020221441021.png)

会发现redis只能本机访问（127.0.0.1代表本机ip）

需要做如下修改

编辑/etc/redis/redis.conf文件：

```
sudo vim /etc/redis/redis.conf
```

将 bind 127.0.0.1 注释掉：# bind 127.0.0.1

增加：bind 0.0.0.0    代表任何ip都可以访问

然后确认：daemonize yes  若为no，则改为yes

然后确认：protected-mode yes  若为no，则改为yes (若这一行本身没有，则不用管)

设置密码：requirepass 123456

最后重启redis服务：

```
 /etc/init.d/redis-server restart
```

然后就可以看到：

![](https://image--1.oss-cn-shenzhen.aliyuncs.com/image-20201020222540484.png)

这样就允许任何ip地址访问了。

（2）从服务器配置

Linux中编辑/etc/redis/redis.conf文件：

```
sudo vim /etc/redis/redis.conf
```

将 bind 127.0.0.1 注释掉：# bind 127.0.0.1

增加：bind 0.0.0.0    代表任何ip都可以访问

然后确认：daemonize yes  若为no，则改为yes

设置密码：requirepass 123456

增加：slaveof 81.68.121.123 6379  (绑定主服务器ip和redis端口)

增加：masterauth 123456   （加上主服务器的密码）

最后重启redis服务。

（若是windows系统，在redis.windows.conf和redis.windows-service.conf文件中设置上述参数，重启命令为`net stop redis` 和`net start redis`）

（3）验证是否连接成功

在从服务器中，输入命令：

```
redis-cli
auth 123456
info replication
```

若出现`master_link_status:up`则说明连接成功。

若出现`master_link_status:down`则说明连接失败。

可以尝试关闭主服务器的防火墙：

```
sudo ufw status #查看防火墙状态
sudo ufw disable #关闭防火墙
sudo ufw enable  #启用防火墙
```

-----------------------------------------------------------------------------------------------------------------------------------------

要建立这样一个主从关系的缓存服务器，只需要在 Slave 端执行命令:

```
> SLAVEOF 81.68.121.172 6379
> config set masterauth <password>
```

以上命令可以实现设置从服务器和设置密码的目的，但是不持久，当关闭redis服务时，设置将会失效。

这样，**当前服务器就作为 81.68.121.142:6379 下的一个从服务器**，它将定期从该服务器复制数据到自身。

在以前的版本中（2.8 以前），你应该慎用 redis 的主从复制功能，因为它的同步机制效率低下，可以想象每一次短线重连都要复制主服务器上的全部数据，算上网络通讯所耗费的时间，反而可能达不到通过 redis 缓存来提升应用响应速度的效果。但是幸运的是，官方在 2.8 以后推出了解决方案，通过部分同步来解决大量的重复操作。

这需要主服务器和从服务器都至少达到 2.8 的版本要求。

### 事务处理

Redis 的事务处理比较简单。只能保证 client 发起的事务中的命令可以连续的执行，而且不会插入其它的 client 命令，当一个 client 在连接中发出 `multi` 命令时，这个连接就进入一个事务的上下文，该连接后续的命令不会执行，而是存放到一个队列中，当执行 `exec` 命令时，redis 会顺序的执行队列中的所有命令。

需要注意的是，redis 对于事务的处理方式比较特殊，它不会在事务过程中出错时恢复到之前的状态，这在实际应用中导致我们不能依赖 redis 的事务来保证数据一致性。

### 持久化机制

内存和磁盘的区别除了速度差别以外，还有就是内存中的数据会在重启之后消失，持久化的作用就是要将这些数据长久存到磁盘中以支持长久使用。

Redis 是一个支持持久化的内存数据库，Redis 需要经常将内存中的数据同步到磁盘来保证持久化。

Redis 支持两种持久化方式：

1、`snapshotting`（快照）：将数据存放到文件里，默认方式。

是将内存中的数据以快照的方式写入到二进制文件中，默认文件 dump.rdb，可以通过配置设置自动做快照持久化的方式。可配置 Redis 在 n 秒内如果超过 m 个 key 被修改就自动保存快照。比如：

`save 900 1`：900 秒内如果超过 1 个 key 被修改，则发起快照保存。

`save 300 10`：300 秒内如果超过 10 个 key 被修改，则快照保存。

2、`Append-only file`（缩写为 aof）：将读写操作存放到文件中。

由于快照方式在一定间隔时间做一次，所以如果 Redis 意外 down 掉的话，就会丢失最后一次快照后的所有修改。

aof 比快照方式有更好的持久化性，是由于使用 aof 时，redis 会将每一个收到的写命令都通过 write 函数写入到文件中，当 redis 启动时会通过重新执行文件中保存的写命令来在内存中重新建立整个数据库的内容。

由于 os 会在内核中缓存 write 做的修改，所以可能不是立即写到磁盘上，这样 aof 方式的持久化也还是有可能会丢失一部分数据。可以通过配置文件告诉 redis 我们想要通过 fsync 函数强制 os 写入到磁盘的时机。

配置文件中的可配置参数：

```
appendonly yes //启用 aof 持久化方式

# appendfsync always //收到写命令就立即写入磁盘，最慢，但是保证了数据的完整持久化

appendfsync everysec //每秒钟写入磁盘一次，在性能和持久化方面做了很好的折中

# appendfsync no //完全依赖 os，性能最好，持久化没有保证
```

在 redis-cli 的命令中，`save` 命令是将数据写入磁盘中。

```
> help save
> save
```

操作截图：

<img src="https://image--1.oss-cn-shenzhen.aliyuncs.com/1558542924652.png" alt="1558542924652" style="zoom:67%;" />

# 7.一些报错

（1）由于一些不正常原因，redis安装损坏，重新`sudo apt install redis-server`发现显示redis已经安装，没法再安装了。

解决办法：

```
sudo apt-get install --reinstall redis-server
sudo apt-get upgrade
```

此时会发现redis已经安装好，`ps -aux|grep redis`会发现redis已经在后台运行。

但是，`redis-cli`会显示此命令不存在。

原因是客户端没有安装，依次输入下面的命令，即可解决。

```
wget http://download.redis.io/redis-stable.tar.gz（下载redis-cli的压缩包）
tar xvzf redis-stable.tar.gz（解压）
cd redis-stable（进入redis-stable目录）
make（安装）
sudo cp src/redis-cli /usr/local/bin/（将redis-cli拷贝到bin下，让redis-cli指令可以在任意目录下直接使用）
```

