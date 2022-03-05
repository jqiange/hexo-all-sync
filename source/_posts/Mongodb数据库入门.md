---
title: Mongodb数据库入门
categories: 数据库
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-03-17 22:50:33
top:
tags:
typora-root-url: ..
---



MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 **WEB 应用**提供可扩展的高性能数据存储解决方案。

MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

Mongo最大的特点是它支持的查询语言非常强大，其语法有点类似于面向对象的查询语言。

**MongoDB数据存储方式**：**虚拟内存+持久化**，使用的内存映射技术，写入数据时候只要在内存里完成就可以返回给应用程序，这样并发量自然就很高。然后再将数据从内存里写到硬盘保存。（当然，mongdb也支持把常用的数据放在内存里供读取，以提高效率）。



# 1. Mongodb准备

## 1.1 Windows

### 1.1.1 下载与安装：

下载地址：https://www.mongodb.com/download-center/community

下载完成之后可以选择自定义安装

### 1.1.2 配置

（1）认识执行命令

`mongo`  用于连接mongodb数据库服务器

`mongod` 用于开启服务器

### 1.1.3 设置环境变量

将执行文件所在的目录设置为环境变量 D:\Database\Mongodb\bin\

### 1.1.4 创建文件夹

D:\Database\Mongodb\data 用于存储数据库

D:\Database\Mongodb\log 用于存储日志

### 1.1.5 开启服务器（挂起）

CMD命令行下（以管理员身份）

`mongod -dppath D:\Database\Mongodb\data --logpath D:\Database\Mongodb\log `

开启服务，并指定数据存储位置和日志存储位置

服务开启后会自动挂在后台，可在：任务管理器\服务 中看到

将mongodb挂载成windows服务，开机自启动：

`mongod -dppath D:\Database\Mongodb\data --logpath D:\Database\Mongodb\log  --install --serviceName "Mongodb"`

操作服务的命令：

net start mongodb 开启mongodb服务

net stop mongodb 关闭mongodb服务

sc delete mongodb 卸载mongodb服务

### 1.1.6 连接服务器

`mongo`

`show dbs`显示所有数据库列表

## 1.2 Linux

下载安装：`sudo apt install mongodb-server`

添加到Path路径中：`export PATH=/usr/local/mongodb4/bin:$PATH`

设置数据存储目录及日志文件目录：

```
sudo mkdir -p /var/lib/mongo
sudo mkdir -p /var/log/mongodb
sudo chown `whoami` /var/lib/mongo     # 设置权限
sudo chown `whoami` /var/log/mongodb   # 设置权限

mongod --dbpath /var/lib/mongo --logpath /var/log/mongodb/mongod.log --fork

mongo   #启动
```



# 2. 数据库操作

## 2.1 基础命令

`show dbs;`   显示数据库列表

`db;` 显示当前正在使用的数据库

`use dbname;` 创建或切换数据库  

如果数据库不存在，则创建数据库dbname，否则切换到指定数据库dbname。创建的数据库并不在数据库的列表中，要显示它，我们需要向数据库dbname插入一些数据。

`db.dropDatabase();` 切到要删除的数据库，就可以删除数据库

**MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中。**

## 1.2 集合操作

注意：**正常来讲在 MongoDB 中不需要创建集合。当插入一些文档时，MongoDB 会自动创建集合。**

### 1.2.1 创建集合

`db.createCollection(name, options);`

name是要创建的集合的名称

options是一个文档，用于指定集合的配置

选项参数是可选的，所以只需要到指定的集合名称。

例1：不限制集合大小

`db.createCollection("stu")`

例2：限制集合大小，后面学会插入语句后可以查看效果

参数capped：默认值为false表示不设置上限，值为true表示设置上限

参数size：当capped值为true时，需要指定此参数，表示上限大小，当文档达到上限时，会将之前的数据覆盖，单位为字节。如：

`db.createCollection("sub", { capped : true, size : 10 } )`

### 1.2.2  显示集合

`show collections;` 显示数据库中的集合列表

### 1.2.3  删除集合

`db.集合名.drop();` 删除指定的集合

## 2.3 数据操作（重点）

### 2.3.1 增加数据

`db.web.insert({"name":"ghost", "age":10})  `

在web集合中插入一条新数据，如果没有web这个集合，mongodb会自动创建

_id是主键，主键是每条数据的唯一标识，不能重复，就像身份证是每个人唯一的编号一样。

### 2.3.2 查看数据

`db.集合名.find();`查找users集合中所有数据

`db.集合名.findOne();` 查找users集合中的第一条数据

`db.集合名.find().pretty();  `  格式化查询到的数据

示例：

```
db.web.find();
{ "_id" : ObjectId("5e70f05a7de50c32cd5380c9"), "name" : "ghost", "age" : 10 }
```

### 2.3.3 修改数据

`db.集合名.update({查询条件},{修改的目标});`

`db.集合名.update({查询条件},{$set:{'键名':'新的值'}});` 修改指定键名的值

示例：

```
db.web.update({"name":"a1"}, {"age":10});
db.web.update({"name":"a1"}, {$set:{"age":10}});
```

### 2.3.4 删除数据

`db.users.remove({});`删除集合中全部数据

`db.users.remove({查询条件});`删除集合中全部数据

示例：

```
db.users.remove({"name": "lecaf"})   
删除users集合下name=”lecaf”的数据
```

# 3. 高级命令

## 3.1 按条件查询

### 3.1.1 根据条件查询

`db.集合名.find({查询条件});`

示例：

```
db.singer.find({"sex":"男"})；
```

### 3.1.2 $gt 大于

`db.集合名.find({"键名":{$gt:值}});`

示例：

```
`db.singer.find({"age":{$gt:50}});`
查询所有年龄大于50的歌手
```

### 3.1.3 $lt 小于

`db.集合名.find({"键名":{$lt:值}});`

### 3.1.4 $gte 大于等于

`db.集合名.find({"键名":{$gte:值}});`

### 3.1.5 $lte 大于等于

`db.集合名.find({"键名":{$lte:值}});`

### 3.1.6 选择区间

`db.集合名.find({"键名":{$gt:值1,$lt:值2}});`

### 3.1.7 $ne 不等于

`db.集合名.find({"键名":{$ne:值}});`

### 3.1.8 $in 在集合中

`db.集合名.find({"键名":{$in:{值1,值2,值n}}});`

### 3.1.9 $nin 不在集合中

`db.集合名.find({"键名":{$nin:{值1,值2,值n}}});`

### 3.1.10 $size 值的个数

`db.集合名.find({"键名":{$size:num}});`

示例：

```
`db.singers.find({"works":{$size:2}});`
```

### 3.1.11 $exists 是否存在某个键名

`db.集合名.find({"键名":{$exists:true|false}});`

### 3.1.12 $or 存在任意一个

`db.集合名.find({$or:[条件1,条件2,条件n]});`

### 3.1.13 模糊查询

`db.集合名.find({"键名",值});`

示例：

```
db.singers.find({"name",/xx/});   #查出含XX的歌手

db.singers.find({'name':/^xx/})； #查出所有以xx开头的歌手

db.singers.find({'name':/xx^/})； #查出所有以xx结尾的歌手

db.singers.find({'name':/xx/i})； #查出所有含xx（忽略大小写）的歌手
```

## 3.2 排序

`db.集合名.find({}).sort({"键名1":1,"键名2":-1})`

1：升序

-1：降序

## 3.3 限制输出

### 3.3.1 limit()

`db.集合名.find({}).limit(n)`

示例：

```
db.集合名.find({}).sort({"age":1}).limit(3)
找出年龄最小的三个歌手
```

### 3.3.2 skip()

`db.集合名.find({}).skip(n)`

示例：

```
db.集合名.find({}).sort({"age":1}).skip(3).limit(3)
找出年龄最小的三个以外的2个歌手
```

这个方法通常的用法是实现分页。

## 3.4 数据类型

Mongdb中常见的数据类型：

● Object ID： ⽂档ID
● String： 字符串， 最常⽤， 必须是有效的UTF-8
● Boolean： 存储⼀个布尔值， true或false
● Integer： 整数可以是32位或64位， 这取决于服务器
● Double： 存储浮点值
● Arrays： 数组或列表， 多个值存储到⼀个键
● Object： ⽤于嵌⼊式的⽂档， 即⼀个值为⼀个⽂档
● Null： 存储Null值
● Timestamp： 时间戳， 表示从1970-1-1到现在的总秒数
● Date： 存储当前⽇期或时间的UNIX时间格式

### 3.4.1 Object ID

每个文档都有一个属性，为_id，以保证每个文档的唯一性。mongodb默认使用id作为主键。

可以自己指定设置，也可以自动生成。

默认生成的Object ID是十六进制的12位数。

# 4. 聚合与分组

## 4.1 聚合

```
db.集合名.aggregate([
                    {管道1:{表达式1}},
                    {管道2:{表达式2}},
                    {管道3:{表达式3}},
                    ])
```

管道：管道一般用于将当前的命令的输出结果作为下一个命令的输入，在mongodb中，管道具有同样的作用，文档处理完毕后，通过管道进行下一次的处理。

**常用的管道：**

- $project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
- $match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
- $limit：用来限制MongoDB聚合管道返回的文档数。
- $skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
- $unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
- $group：将集合中的文档分组，可用于统计结果。
- $sort：将输入文档排序后输出。
- $geoNear：输出接近某一地理位置的有序文档。

**常用的表达式：**

- $sum：对目标键对应的值计算总和。

- $avg：对目标键对应的值计算平均值。

- $min：取目标键对应的值的最小值。

- $max：取目标键对应的值的最大值。

- $push：取出目标值组成一个数组

- $addToSet：在结果文档中插入值到一个数组中，但不创建副本。

- $first：取目标键对应的值的第一个文档数据。

- $last：取目标键对应的值的最后一个文档数据。

更多请参阅官方文档。

## 4.2 聚合示例

### 4.2.1 $group

作用：分组

```
db.集合名.aggreate(
     {$group:
         {
         _id:'$字段名'
         别名：{$聚合函数：'$字段名'}
         }
      })
```

示例：

```
统计男和女生的总人数
db.singer.aggreate(
     {$group:
         {
         _id:'$sex',
         count：{$sum：1}
         }
      })
输出结果：
<"_id":"男","count":11>
<"_id":"女","count":4>

统计男和女生的总人数及名单
db.singer.aggreate(
     {$group:
         {
         _id:null,
         总人数:{$sum：1}
         平均年龄:{'$avg':'$age'}
         名单:{$push:'$name'}
         }
      })
输出结果：
<"_id":null,"总人数":15,"平均年龄":42.666,"名单":['刘德华','许嵩','周华健'……]>
```

### 4.2.2 $match

作用：过滤

```
db.集合名.aggreate(
     {$match:{"键名":{$gt:30}}})
```

示例：

```
对年龄大于40的歌手进行性别分组统计
db.singer.aggreate([
     {$match:{"age":{$gt:40}}},
     {$group:{_id:'$sex',总人数:{$sum:1}}}
     ])
输出结果：
<"_id":"男","总人数":8>
<"_id":"女","总人数":1>
```

### 4.2.3 $project

作用：限定输出字段

```
db.集合名.aggreate(
     {$project:{"键名":1)  //1表示显示，0表示不显示
```

示例：

```
找出年龄大于50的选手，只显示姓名和年龄
db.singer.aggreate([
     {$match:{"age":{$gt:50}}},
     {$group:{_id:0,name:1,age:1}}
     ])
```

### 4.2.4 $sort

作用：排序

```
db.集合名.aggreate(
     {$project:{"键名":1|-1}}
     )  //1表示升序，-1表示降序
```

示例：

```
db.singer.aggreate([
     {$match:{"age":{$gt:50}}},
     {$group:{_id:0,name:1,age:1}},
     {$sort:{age:1}},
     {$limit:3}
     ])
```

### 4.2.5 $skip

作用：跳过

```
db.集合名.aggreate(
     {$skip:n})  
```

### 4.2.6 $unwind

作用：拆分字段

```
db.集合名.aggreate(
     {$unwind:"键名"}
     )  
```

# 5. 安全

## 5.1 进入管理平台

首先以无密码形式登陆：`mongo`

## 5.2 创建管理员

新建数据库：admin

`use admin`

创建用户名及密码

`db.createUser({user:"admin",pwd:"123",roles:["root"]})`

## 5.3 验证账户和密码

`db.auth("admin","123")`

## 5.4 挂起需要验证的服务

先停止已有的服务，并卸载：`sc delete mongodb`

重新挂载服务：

`mongod -dppath D:\Database\Mongodb\data --logpath D:\Database\Mongodb\log  --install --serviceName "Mongodb" --auth`

--auth 开启需要身份验证

服务可以临时挂起多个。

## 5.5 测试账户

`show dbs` 会显示 无法访问，需要身份验证

切换到admin数据库，再验证。

`db.auth(账户,密码)`

## 5.6 为指定数据库添加用户

`db.createUser({user:"itsource",pwd:"123",roles:[{role:"dbOwer",db:"python"}]})`

# 6. 主从服务器

主从服务器：也叫复制，将数据同步在多个服务器的过程。主服务器用来写，从服务器用来读。

复制提供了数据的冗余备份，并在多个服务器上存储数据副本，提高了数据的可用性， 并可以保证数据的安全性。

复制还允许您从硬件故障和服务中断中恢复数据。

- 复制原理

  mongodb的复制至少需要两个节点。其中一个是主节点，负责处理客户端请求，其余的都是从节点，负责复制主节点上的数据。

  mongodb各个节点常见的搭配方式为：一主一从、一主多从。

  主节点记录在其上的所有操作oplog，从节点定期轮询主节点获取这些操作，然后对自己的数据副本执行这些操作，从而保证从节点的数据与主节点一致。

- 为什么要复制

  数据备份

  数据灾难恢复

  读写分离

  数据可用性高

  无宕机维护

  副本集对应的程序是透明

  

## 6.1 设置复制节点

### 6.1.1 创建数据库目录

创建两个数据库目录，如db1，db2，用于挂起两个mongodb服务。

### 6.1.2 启动mongodb服务

使用如下格式启动mongod，注意replSet的名称是一样的。

`mongod --bind_ip 192.168.0.104 --port 27017 --dbpath d:\Database\Mongodb\db1 --replSet rs0`

`mongod --bind_ip 192.168.0.104 --port 27018 --dbpath d:\Database\Mongodb\db2 --replSet rs0`

**注意上面的要写自己的ip，开两个命令行分别输入以上两个命令，这两个服务器同属于集群rs0。**

### 6.1.3 连接服务器

连接服务器，此处设置`192.168.0.104:27017`为主服务器。再打开一个命令行，输入：

`mongo --host 192.168.0.104 --port 27017`

先连接的为主服务器。

### 6.1.4 初始化

`rs.initiate()`

### 6.1.5 查看当前状态

`rs.status()`

### 6.1.6 添加副本集

`rs.add("192.168.0.104:27018")`

### 6.1.7 连接从服务器

`mongo --host 192.168.0.104 --port 27018`

### 6.1.8 在从服务器中查询

在从服务器中进行读操作，需要设置：

`rs.slaveOk()`

`db.users,find()`

### 6.1.9 其他

主从切换：主从服务器可以自行切换，无需人为设置

```
主服务器标志：
rs0:PRIMARY>

从服务器标志：
rs0:SECONDARY>
```

删除从节点：

`rs.remove("192.168.0.104:27018")`

# 7.备份与恢复

## 7.1 备份

语法：`mongodump --h dbhost -d dbname -o dbdirectory`

-h：指定服务器地址，也可指定端口号

-d：需要备份的数据库名称

-o：备份数据存放的位置，此目录中存放这备份出来的数据

示例：

```
mongodump --h 127.0.0.1:27017 -d itsource -o d:\bakup
```

**注意：备份前保证服务器在后台开着**

## 7.2 恢复

语法：`mongorestore --h dbhost -d dbname --dir dbdirectory`

-h：指定服务器地址，也可指定端口号

-d：需要恢复的数据库名称

-o：备份数据存放的位置。

# 8. python操作

官方文档：https://pymongo.readthedocs.io/en/stable/tutorial.html

首先安装python包：`pip install pymongo`

## 8.1 连接数据库

```python
import pymongo
client = pymongo.MongoClient('localhost', 27017)
db=client['itsource']
```

## 8.2 插入数据

```python
db.admin.insert_one({'name':'小明','age':'18'})  //插入一条数据
db.admin.insert_many([{'name':'小明','age':'18'},{'name':'张飞','age':'27'}]) 
//插入多条数据
```

## 8.3 读取数据

```python
data=db.admin.find_one()  //读取一条数据
datas=db.admin.find()   //这里得到的游标，可通过遍历游标获取数据
for data in datas:
    print(data)
print(datas.count) //打印读取到的数据数目
```

还可按条件获取数据：

```
datas=db.admin.find('条件')
```

条件语法结构可参照【3.1】

## 8.4 修改文档

```
db.admin.update_one(filter={查询条件},update={'$set':{修改目标}})
```

示例：

```
db.admin.update_one(filter={'name':'小明'},update={'$set':{'age':18}})
```

## 8.5 删除数据

语法：`db.admin.delete_one({条件})`

`db.admin.delete_many({条件})`

示例：

```
db.admin.delete_many({'name':'小明'})
```


