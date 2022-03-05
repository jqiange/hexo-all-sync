---
title: Python操作Redis
categories: 数据库
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-03-07 16:15:44
top:
tags: Redis
typora-root-url: ..
---



## 1. redis连接

> redis提供两个类Redis和StrictRedis用于实现Redis的命令，StrictRedis用于实现大部分官方的命令，并使用官方的语法和命令，Redis是StrictRedis的子类，用于向后兼容旧版本的redis-py。

> redis连接实例是线程安全的，可以直接将redis连接实例设置为一个全局变量，直接使用。如果需要另一个Redis实例（or Redis数据库）时，就需要重新创建redis连接实例来获取一个新的连接。同理，python的redis没有实现select命令。

安装redis

```
pip install redis
```

连接redis，加上decode_responses=True，写入的键值对中的value为str类型，不加这个参数写入的则为字节类型。

```python
# 导入redis模块，通过python操作redis 也可以直接在redis主机的服务端操作缓存数据库
import redis

# host是redis主机，需要redis服务端和客户端都启动 redis默认端口是6379
r = redis.Redis(host='134.175.188.27', port=6379, decode_responses=True, password=123456)
# key是"foo" value是"bar" 将键值对存入redis缓存
r.set('name', 'junxi')
print(r['name'])
# 取出键name对应的值
print(r.get('name'))
print(type(r.get('name')))

```

## 2. redis基本命令 

### 2.1 String

set(name, value, ex=None, px=None, nx=False, xx=False)

在Redis中设置值，默认，不存在则创建，存在则修改
 参数：
 ex，过期时间（秒）
 px，过期时间（毫秒）
 nx，如果设置为True，则只有name不存在时，当前set操作才执行
 xx，如果设置为True，则只有name存在时，当前set操作才执行

ex，过期时间（秒） 这里过期时间是3秒，3秒后p，键food的值就变成None

```
import redis

pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
r = redis.Redis(connection_pool=pool)
# key是"food" value是"mutton" 将键值对存入redis缓存
r.set('food', 'mutton', ex=3)
# mutton 取出键food对应的值
print(r.get('food')) 
```

px，过期时间（豪秒） 这里过期时间是3豪秒，3毫秒后，键foo的值就变成None

```
import redis

pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
r = redis.Redis(connection_pool=pool)
r.set('food', 'beef', px=3)
print(r.get('food'))
```

nx，如果设置为True，则只有name不存在时，当前set操作才执行 （新建）

```
import redis

pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
r = redis.Redis(connection_pool=pool)
print(r.set('fruit', 'watermelon', nx=True))    # True--不存在
# 如果键fruit不存在，那么输出是True；如果键fruit已经存在，输出是None
```

xx，如果设置为True，则只有name存在时，当前set操作才执行 （修改）

```
# True-- 已经存在 
# 如果键fruit已经存在，那么输出是True；如果键fruit不存在，输出是None
print((r.set('fruit', 'watermelon', xx=True)))   
```

setnx(name, value)
 设置值，只有name不存在时，执行设置操作（添加）

```
print(r.setnx('fruit1', 'banana'))  # fruit1不存在，输出为True
```

setex(name, value, time)
 设置值
 参数： time，过期时间（数字秒 或 timedelta对象）

```
import redis
import time

pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
r = redis.Redis(connection_pool=pool)
r.setex("fruit2", "orange", 5)
time.sleep(5)
# 5秒后，取值就从orange变成None
print(r.get('fruit2')) 
```

psetex(name, time_ms, value)
 设置值
 参数： time_ms，过期时间（数字毫秒 或 timedelta对象）

```
r.psetex("fruit3", 5000, "apple")
time.sleep(5)
print(r.get('fruit3'))  # 5000毫秒后，取值就从apple变成None
```

mset(*args, **kwargs)
 批量设置值

```
r.mset({'k1': 'v1', 'k2': 'v2'})
print(r.mget("k1", "k2"))   # 一次取出多个键对应的值
print(r.mget("k1"))
```

mget(keys, *args)
 批量获取 如：

```
print(r.mget('k1', 'k2'))
print(r.mget(['k1', 'k2']))
# 将目前redis缓存中的键对应的值批量取出来
print(r.mget("fruit", "fruit1", "fruit2", "k1", "k2"))  
```

getrange(key, start, end)
 获取子序列（根据字节获取，非字符）
 参数：
 name，Redis 的 name
 start，起始位置（字节）
 end，结束位置（字节）
 如： "君惜大大" ，0-3表示 "君"

```
r.set("cn_name", "君惜大大") # 汉字
# 取索引号是0-2 前3位的字节 君 切片操作 （一个汉字3个字节 1个字母一个字节 每个字节8bit）
print(r.getrange("cn_name", 0, 2))
# 取所有的字节 君惜大大 切片操作
print(r.getrange("cn_name", 0, -1)) 
# 字母
r.set("en_name","junxi") 
# 取索引号是0-2 前3位的字节 jun 切片操作 （一个汉字3个字节 1个字母一个字节 每个字节8bit）
print(r.getrange("en_name", 0, 2))
# 取所有的字节 junxi 切片操作
print(r.getrange("en_name", 0, -1))
```

setrange(name, offset, value)
 修改字符串内容，从指定字符串索引开始向后替换（新值太长时，则向后添加）
 参数：
 offset，字符串的索引，字节（一个汉字三个字节）
 value，要设置的值

```
r.setrange("en_name", 1, "ccc")
# jccci 原始值是junxi 从索引号是1开始替换成ccc 变成 jccci
print(r.get("en_name"))   
```

strlen(name)
 返回name对应值的字节长度（一个汉字3个字节）

```
print(r.strlen("foo"))  # 4 'goo1'的长度是4
```

incr(self, name, amount=1)
 自增 name对应的值，当name不存在时，则创建name＝amount，否则，则自增。
 参数：
 name,Redis的name
 amount,自增数（必须是整数）
 注：同incrby

```
r.set("foo", 123)
print(r.mget("foo", "foo1", "foo2", "k1", "k2"))
r.incr("foo", amount=1)
print(r.mget("foo", "foo1", "foo2", "k1", "k2"))
```

应用场景 – 页面点击数
假定我们对一系列页面需要记录点击次数。例如论坛的每个帖子都要记录点击次数，而点击次数比回帖的次数的多得多。如果使用关系数据库来存储点击，可能存在大量的行级锁争用。所以，点击数的增加使用redis的INCR命令最好不过了。
当redis服务器启动时，可以从关系数据库读入点击数的初始值（12306这个页面被访问了34634次）

```
r.set("visit:12306:totals", 34634)
print(r.get("visit:12306:totals"))
```

每当有一个页面点击，则使用INCR增加点击数即可。

```
r.incr("visit:12306:totals")
r.incr("visit:12306:totals")
```

页面载入的时候则可直接获取这个值

```
print(r.get("visit:12306:totals"))
```

incrbyfloat(self, name, amount=1.0)
 自增 name对应的值，当name不存在时，则创建name＝amount，否则，则自增。
 参数：
 name,Redis的name
 amount,自增数（浮点型）

```
r.set("foo1", "123.0")
r.set("foo2", "221.0")
print(r.mget("foo1", "foo2"))
r.incrbyfloat("foo1", amount=2.0)
r.incrbyfloat("foo2", amount=3.0)
print(r.mget("foo1", "foo2"))
```

decr(self, name, amount=1)
 自减 name对应的值，当name不存在时，则创建name＝amount，否则，则自减。
 参数：
 name,Redis的name
 amount,自减数（整数)

```
r.decr("foo4", amount=3) # 递减3
r.decr("foo1", amount=1) # 递减1
print(r.mget("foo1", "foo4"))
```

append(key, value)
 在redis name对应的值后面追加内容
 参数：
 key, redis的name
 value, 要追加的字符串

```
r.append("name", "haha")    # 在name对应的值junxi后面追加字符串haha
print(r.mget("name"))
```

2.2 ###  list

增加（类似于list的append，只是这里是从左边新增加）--没有就新建
 lpush(name,values)
 在name对应的list中添加元素，每个新的元素都添加到列表的最左边
 如：

```
import redis
import time

pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
r = redis.Redis(connection_pool=pool)

r.lpush("list1", 11, 22, 33)
print(r.lrange('list1', 0, -1))
```

保存顺序为: 33,22,11

扩展：

```
r.rpush("list2", 11, 22, 33)  # 表示从右向左操作
print(r.llen("list2"))  # 列表长度
print(r.lrange("list2", 0, 3))  # 切片取出值，范围是索引号0-3
```

增加（从右边增加）--没有就新建

```
r.rpush("list2", 44, 55, 66)    # 在列表的右边，依次添加44,55,66
print(r.llen("list2"))  # 列表长度
print(r.lrange("list2", 0, -1)) # 切片取出值，范围是索引号0到-1(最后一个元素)
```

往已经有的name的列表的左边添加元素，没有的话无法创建
lpushx(name,value)
在name对应的list中添加元素，只有name已经存在时，值添加到列表的最左边
更多：

```
r.lpushx("list10", 10)   # 这里list10不存在
print(r.llen("list10"))  # 0
print(r.lrange("list10", 0, -1))  # []
r.lpushx("list2", 77)   # 这里"list2"之前已经存在，往列表最左边添加一个元素，一次只能添加一个
print(r.llen("list2"))  # 列表长度
print(r.lrange("list2", 0, -1)) # 切片取出值，范围是索引号0到-1(最后一个元素
```

往已经有的name的列表的右边添加元素，没有的话无法创建

```
r.rpushx("list2", 99)   # 这里"foo_list1"之前已经存在，往列表最右边添加一个元素，一次只能添加一个
print(r.llen("list2"))  # 列表长度
print(r.lrange("list2", 0, -1)) # 切片取出值，范围是索引号0到-1(最后一个元素)
```

新增（固定索引号位置插入元素）
linsert(name, where, refvalue, value))
在name对应的列表的某一个值前或后插入一个新值
参数：
name，redis的name
where，BEFORE或AFTER
refvalue，标杆值，即：在它前后插入数据
value，要插入的数据

```
r.linsert("list2", "before", "11", "00")   # 往列表中左边第一个出现的元素"11"前插入元素"00"
print(r.lrange("list2", 0, -1))   # 切片取出值，范围是索引号0-最后一个元素
```

修改（指定索引号进行修改）
 r.lset(name, index, value)
 对name对应的list中的某一个索引位置重新赋值
 参数：
 name，redis的name
 index，list的索引位置
 value，要设置的值

```
r.lset("list2", 0, -11)    # 把索引号是0的元素修改成-11
print(r.lrange("list2", 0, -1))
```

删除（指定值进行删除）
 r.lrem(name, value, num)
 在name对应的list中删除指定的值
 参数：
 name，redis的name
 value，要删除的值
 num， num=0，删除列表中所有的指定值；
 num=2,从前到后，删除2个； num=1,从前到后，删除左边第1个
 num=-2,从后向前，删除2个

```
r.lrem("list2", "11", 1)    # 将列表中左边第一次出现的"11"删除
print(r.lrange("list2", 0, -1))
r.lrem("list2", "99", -1)    # 将列表中右边第一次出现的"99"删除
print(r.lrange("list2", 0, -1))
r.lrem("list2", "22", 0)    # 将列表中所有的"22"删除
print(r.lrange("list2", 0, -1))
```

删除并返回
lpop(name)
在name对应的列表的左侧获取第一个元素并在列表中移除，返回值则是第一个元素
更多：
rpop(name) 表示从右向左操作

```
r.lpop("list2")    # 删除列表最左边的元素，并且返回删除的元素
print(r.lrange("list2", 0, -1))
r.rpop("list2")    # 删除列表最右边的元素，并且返回删除的元素
print(r.lrange("list2", 0, -1))
```

删除索引之外的值
ltrim(name, start, end)
在name对应的列表中移除没有在start-end索引之间的值
参数：
name，redis的name
start，索引的起始位置
end，索引结束位置

```
r.ltrim("list2", 0, 2)    # 删除索引号是0-2之外的元素，值保留索引号是0-2的元素
print(r.lrange("list2", 0, -1))
```

取值（根据索引号取值）
lindex(name, index)
在name对应的列表中根据索引获取列表元素

```
print(r.lindex("list2", 0))  # 取出索引号是0的值
```

### 2.3 set

新增
 sadd(name,values)
 name对应的集合中添加元素

```
r.sadd("set1", 33, 44, 55, 66)  # 往集合中添加元素
print(r.scard("set1"))  # 集合的长度是4
print(r.smembers("set1"))   # 获取集合中所有的成员
```

获取元素个数 类似于len
scard(name)
获取name对应的集合中元素个数

```
print(r.scard("set1"))  # 集合的长度是4
```

获取集合中所有的成员
smembers(name)
获取name对应的集合的所有成员

```
print(r.smembers("set1"))   # 获取集合中所有的成员
```

获取集合中所有的成员--元组形式
sscan(name, cursor=0, match=None, count=None)

```
print(r.sscan("set1"))
```

获取集合中所有的成员--迭代器的方式
 sscan_iter(name, match=None, count=None)
 同字符串的操作，用于增量迭代分批获取元素，避免内存消耗太大

```
for i in r.sscan_iter("set1"):
    print(i)
```

差集
sdiff(keys, *args)
在第一个name对应的集合中且不在其他name对应的集合的元素集合

```
r.sadd("set2", 11, 22, 33)
print(r.smembers("set1"))   # 获取集合中所有的成员
print(r.smembers("set2"))
print(r.sdiff("set1", "set2"))   # 在集合set1但是不在集合set2中
print(r.sdiff("set2", "set1"))   # 在集合set2但是不在集合set1中
```

差集--差集存在一个新的集合中
sdiffstore(dest, keys, *args)
获取第一个name对应的集合中且不在其他name对应的集合，再将其新加入到dest对应的集合中

```
r.sdiffstore("set3", "set1", "set2")    # 在集合set1但是不在集合set2中
print(r.smembers("set3"))   # 获取集合3中所有的成员
```

交集
sinter(keys, *args)
获取多一个name对应集合的交集

```
print(r.sinter("set1", "set2")) # 取2个集合的交集
```

交集--交集存在一个新的集合中
sinterstore(dest, keys, *args)
获取多一个name对应集合的并集，再将其加入到dest对应的集合中

```
print(r.sinterstore("set3", "set1", "set2")) # 取2个集合的交集
print(r.smembers("set3"))
```

并集
 sunion(keys, *args)
 获取多个name对应的集合的并集

```
print(r.sunion("set1", "set2")) # 取2个集合的并集
```

并集--并集存在一个新的集合
 sunionstore(dest,keys, *args)
 获取多一个name对应的集合的并集，并将结果保存到dest对应的集合中

```
print(r.sunionstore("set3", "set1", "set2")) # 取2个集合的并集
print(r.smembers("set3"))
```

判断是否是集合的成员 类似in
sismember(name, value)
检查value是否是name对应的集合的成员，结果为True和False

```
print(r.sismember("set1", 33))  # 33是集合的成员
print(r.sismember("set1", 23))  # 23不是集合的成员
```

移动
smove(src, dst, value)
将某个成员从一个集合中移动到另外一个集合

```
r.smove("set1", "set2", 44)
print(r.smembers("set1"))
print(r.smembers("set2"))
```

删除--随机删除并且返回被删除值
spop(name)
从集合移除一个成员，并将其返回,说明一下，集合是无序的，所有是随机删除的

```
print(r.spop("set2"))   # 这个删除的值是随机删除的，集合是无序的
print(r.smembers("set2"))
```

删除--指定值删除
srem(name, values)
在name对应的集合中删除某些值

```
print(r.srem("set2", 11))   # 从集合中删除指定值 11
print(r.smembers("set2"))
```

### 2.4  Zset

Set操作，Set集合就是不允许重复的列表，本身是无序的
 有序集合，在集合的基础上，为每元素排序；元素的排序需要根据另外一个值来进行比较，
 所以，对于有序集合，每一个元素有两个值，即：值和分数，分数专门用来做排序。

新增
zadd(name, *args, **kwargs)
在name对应的有序集合中添加元素
如：

```
import redis
import time

pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
r = redis.Redis(connection_pool=pool)

r.zadd("zset1", n1=11, n2=22)
r.zadd("zset2", 'm1', 22, 'm2', 44)
print(r.zcard("zset1")) # 集合长度
print(r.zcard("zset2")) # 集合长度
print(r.zrange("zset1", 0, -1))   # 获取有序集合中所有元素
print(r.zrange("zset2", 0, -1, withscores=True))   # 获取有序集合中所有元素和分数
```

获取有序集合元素个数 类似于len
zcard(name)
获取name对应的有序集合元素的数量

```
print(r.zcard("zset1")) # 集合长度
```

获取有序集合的所有元素
r.zrange( name, start, end, desc=False, withscores=False, score_cast_func=float)
按照索引范围获取name对应的有序集合的元素
参数：
name，redis的name
start，有序集合索引起始位置（非分数）
end，有序集合索引结束位置（非分数）
desc，排序规则，默认按照分数从小到大排序
withscores，是否获取元素的分数，默认只获取元素的值
score_cast_func，对分数进行数据转换的函数

从大到小排序(同zrange，集合是从大到小排序的)
 zrevrange(name, start, end, withscores=False, score_cast_func=float)

```
print(r.zrevrange("zset1", 0, -1))    # 只获取元素，不显示分数
print(r.zrevrange("zset1", 0, -1, withscores=True)) # 获取有序集合中所有元素和分数,分数倒序
```

按照分数范围获取name对应的有序集合的元素
zrangebyscore(name, min, max, start=None, num=None, withscores=False, score_cast_func=float)

```
for i in range(1, 30):
   element = 'n' + str(i)
   r.zadd("zset3", element, i)
print(r.zrangebyscore("zset3", 15, 25)) # # 在分数是15-25之间，取出符合条件的元素
print(r.zrangebyscore("zset3", 12, 22, withscores=True))    # 在分数是12-22之间，取出符合条件的元素（带分数）
```

按照分数范围获取有序集合的元素并排序（默认从大到小排序）
 zrevrangebyscore(name, max, min, start=None, num=None, withscores=False, score_cast_func=float)

```
print(r.zrevrangebyscore("zset3", 22, 11, withscores=True)) # 在分数是22-11之间，取出符合条件的元素 按照分数倒序
```

获取所有元素--默认按照分数顺序排序
zscan(name, cursor=0, match=None, count=None, score_cast_func=float)

```
print(r.zscan("zset3"))
```

获取值的索引号
zrank(name, value)
获取某个值在 name对应的有序集合中的索引（从 0 开始）
更多：
 zrevrank(name, value)，从大到小排序

```
print(r.zrank("zset3", "n1"))   # n1的索引号是0 这里按照分数顺序（从小到大）
print(r.zrank("zset3", "n6"))   # n6的索引号是1

print(r.zrevrank("zset3", "n1"))    # n1的索引号是29 这里安照分数倒序（从大到小）
```

删除--指定值删除
zrem(name, values)
删除name对应的有序集合中值是values的成员

```
r.zrem("zset3", "n3")   # 删除有序集合中的元素n3 删除单个
print(r.zrange("zset3", 0, -1))
```

删除--根据排行范围删除，按照索引号来删除
zremrangebyrank(name, min, max)
根据排行范围删除

```
r.zremrangebyrank("zset3", 0, 1)  # 删除有序集合中的索引号是0, 1的元素
print(r.zrange("zset3", 0, -1))
```

删除--根据分数范围删除
zremrangebyscore(name, min, max)
根据分数范围删除

```
r.zremrangebyscore("zset3", 11, 22)   # 删除有序集合中的分数是11-22的元素
print(r.zrange("zset3", 0, -1))
```

获取值对应的分数
zscore(name, value)
获取name对应有序集合中 value 对应的分数

```
print(r.zscore("zset3", "n27"))   # 获取元素n27对应的分数27
```

### 2.5 hash

单个增加--修改(单个取出)--没有就新增，有的话就修改
hset(name, key, value)
name对应的hash中设置一个键值对（不存在，则创建；否则，修改）
参数：
name，redis的name
key，name对应的hash中的key
value，name对应的hash中的value
注：
hsetnx(name, key, value),当name对应的hash中不存在当前key时则创建（相当于添加）

```
import redis
import time

pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
r = redis.Redis(connection_pool=pool)

r.hset("hash1", "k1", "v1")
r.hset("hash1", "k2", "v2")
print(r.hkeys("hash1")) # 取hash中所有的key
print(r.hget("hash1", "k1"))    # 单个取hash的key对应的值
print(r.hmget("hash1", "k1", "k2")) # 多个取hash的key对应的值
r.hsetnx("hash1", "k2", "v3") # 只能新建
print(r.hget("hash1", "k2"))
```

批量增加（取出）
hmset(name, mapping)
在name对应的hash中批量设置键值对
参数：
name，redis的name
mapping，字典，如：{'k1':'v1', 'k2': 'v2'}
如：

```
r.hmset("hash2", {"k2": "v2", "k3": "v3"})
```

hget(name,key)
 在name对应的hash中获取根据key获取value
 hmget(name, keys, *args)
 在name对应的hash中获取多个key的值
 参数：
 name，reids对应的name
 keys，要获取key集合，如：['k1', 'k2', 'k3']
 *args，要获取的key，如：k1,k2,k3
 如：

```
print(r.hget("hash2", "k2"))  # 单个取出"hash2"的key-k2对应的value
print(r.hmget("hash2", "k2", "k3"))  # 批量取出"hash2"的key-k2 k3对应的value --方式1
print(r.hmget("hash2", ["k2", "k3"]))  # 批量取出"hash2"的key-k2 k3对应的value --方式2
```

取出所有的键值对
 hgetall(name)
 获取name对应hash的所有键值

```
print(r.hgetall("hash1"))
```

得到所有键值对的格式 hash长度
 hlen(name)
 获取name对应的hash中键值对的个数

```
print(r.hlen("hash1"))
```

得到所有的keys（类似字典的取所有keys）
 hkeys(name)
 获取name对应的hash中所有的key的值

```
print(r.hkeys("hash1"))
```

得到所有的value（类似字典的取所有value）
 hvals(name)
 获取name对应的hash中所有的value的值

```
print(r.hvals("hash1"))
```

判断成员是否存在（类似字典的in）
 hexists(name, key)
 检查name对应的hash是否存在当前传入的key

```
print(r.hexists("hash1", "k4"))  # False 不存在
print(r.hexists("hash1", "k1"))  # True 存在
```

删除键值对
 hdel(name,*keys)
 将name对应的hash中指定key的键值对删除

```
print(r.hgetall("hash1"))
r.hset("hash1", "k2", "v222")   # 修改已有的key k2
r.hset("hash1", "k11", "v1")   # 新增键值对 k11
r.hdel("hash1", "k1")    # 删除一个键值对
print(r.hgetall("hash1"))
```



## 3. 其他常用操作

**删除**
 delete(*names)
 根据删除redis中的任意数据类型（string、hash、list、set、有序set）

```
r.delete("gender")  # 删除key为gender的键值对
```

**检查名字是否存在**
 exists(name)
 检测redis的name是否存在，存在就是True，False 不存在

```
print(r.exists("zset1"))
```

**模糊匹配**
 keys(pattern='*') 根据模型获取redis的name 更多： KEYS \* 匹配数据库中所有 key 。 KEYS h?llo 匹配 hello ， hallo 和 hxllo 等。 KEYS h*llo 匹配 hllo 和 heeeeello 等。
 KEYS h[ae]llo 匹配 hello 和 hallo ，但不匹配 hillo

```
print(r.keys("foo*"))
```

**设置超时时间**
 expire(name ,time)
 为某个redis的某个name设置超时时间

```
r.lpush("list5", 11, 22)
r.expire("list5", time=3)
print(r.lrange("list5", 0, -1))
time.sleep(3)
print(r.lrange("list5", 0, -1))
```

**重命名**
 rename(src, dst)
 对redis的name重命名

```
r.lpush("list5", 11, 22)
r.rename("list5", "list5-1")
```

**随机获取name**
 randomkey()
 随机获取一个redis的name（不删除）

```
print(r.randomkey())
```

**获取类型**
 type(name)
 获取name对应值的类型

```
print(r.type("set1"))
print(r.type("hash2"))
```

**查看所有元素**
 scan(cursor=0, match=None, count=None)

```
print(r.hscan("hash2"))
print(r.sscan("set3"))
print(r.zscan("zset2"))
print(r.getrange("foo1", 0, -1))
print(r.lrange("list2", 0, -1))
print(r.smembers("set3"))
print(r.zrange("zset3", 0, -1))
print(r.hgetall("hash1"))
```

**查看所有元素--迭代器**
 scan_iter(match=None, count=None)

```
for i in r.hscan_iter("hash1"):
    print(i)

for i in r.sscan_iter("set3"):
    print(i)

for i in r.zscan_iter("zset3"):
    print(i)
```

**其他方法**

```
print(r.get('name'))    # 查询key为name的值
r.delete("gender")  # 删除key为gender的键值对
print(r.keys()) # 查询所有的Key
print(r.dbsize())   # 当前redis包含多少条数据
r.save()    # 执行"检查点"操作，将数据写回磁盘。保存时阻塞
# r.flushdb()        # 清空r中的所有数据
```



