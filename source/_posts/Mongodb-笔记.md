---
title: Mongodb 笔记
date: 2016-06-30 17:54:13
tags: MongoDB
---

MongoDB 最近几年，以其独特的性能优势，及独特的文档型数据结构，占有了大部分 Nosql 市场，因此，熟悉掌握 MongoDB 的使用，对于开发者显得十分必要。

<!--more-->

## MongoDB 的安装

* windows 下安装，[参见此教程](http://www.runoob.com/mongodb/mongodb-window-install.html)

* 将其添加到系统服务，在 cmd 系统管理员模式下运行下面命令(路径按实际情况改动)

```
mongod.exe  --logpath "C:\Program Files\MongoDB\data\log\mongo.log" --logappend --dbpath "C:\Program Files\MongoDB\data\db" --port 27017 --serviceName "mongodb" --serviceDisplayName "mongodb" --install
```

* 设置开机自启动

cmd 下运行services.msc 找到mongo 后设置自动启动

* 连接远程的mongo

```
mongo 192.168.111.200:27017/mdoctor -u mdoctor -p password
```

## MongoDB 的数据结构以及底层

存储格式：面向集合存储，一个collection 相当于mysql中的一个table，底层文件存储格式为BSON（一种JSON的扩展），Binary JSON 


## MongoDB 的业务场景

 mongodb的主要目标是在键/值存储方式（提供了高性能和高度伸缩性）以及传统的RDBMS系统（丰富的功能）架起一座桥梁，集两者的优势于一身。mongo适用于以下场景：

* 网站数据：mongo非常适合实时的插入，更新与查询，并具备网站实时数据存储所需的复制及高度伸缩性。

* 缓存：由于性能很高，mongo也适合作为信息基础设施的缓存层。在系统重启之后，由mongo搭建的持久化缓存可以避免下层的数据源过载。

* 大尺寸、低价值的数据，比如评论、回复、模板数据。使用传统的关系数据库存储一些数据时可能会比较贵，在此之前，很多程序员往往会选择传统的文件进行存储。

* 高伸缩性的场景：mongo非常适合由数十或者数百台服务器组成的数据库。

* 用于对象及JSON数据的存储：mongo的BSON数据格式非常适合文档格式化的存储及查询。

不适合的场景：
 
* 高度事物性的系统：例如银行或会计系统。传统的关系型数据库目前还是更适用于需要大量原子性复杂事务的应用程序

## MongoDB 常用命令 

* 查看数据库：show databases;

* 使用数据库：use db_name;

* 查看有哪些表，或者是集合：show collections|show table;

* 查看集合，返回的是一个数组：db.getCollections();

* 创建集合，db.createCollection("nsmr"，"一些可选参数,可以不填")

* 在集合中插入数据 db.sites.insert({name:"itbilu.com"})

* 重新命名集合类 db.COLLECTION_NAME.renameCollection("NEW_NAME")

* 删除集合类 db.COLLECTION_NAME.drop();

* 查询当前集合的数据条数：db.yourColl.count();
 
* 查看数据空间大小:db.userInfo.dataSize();
 
* 得到当前聚集集合所在的数据库:db.userInfo.getDB();
 
* 得到当前聚集的状态，可以查看到当前collection 的size、count啥的：db.userInfo.stats();
 
* 得到聚集集合总大小：db.userInfo.totalSize();
 
* collection重命名：db.userInfo.renameCollection("users");
 
* 删除当前collection：db.userInfo.drop();


### 查询

* 查询所有记录：db.userInfo.find();
相当于：select * from userInfo;

* 查询去掉后的当前聚集集合中的某列的重复数据：db.userInfo.distinct("name");
相当于：select distict name from userInfo;
 
* 查询age = 22的记录：db.userInfo.find({"age": 22});
相当于： select * from userInfo where age = 22;
 
* 查询age > 22的记录
db.userInfo.find({age: {$gt: 22}});
相当于：select * from userInfo where age > 22;
 
* 查询age < 22的记录
db.userInfo.find({age: {$lt: 22}});
相当于：select * from userInfo where age < 22;
 
* 查询age >= 25的记录
db.userInfo.find({age: {$gte: 25}});
相当于：select * from userInfo where age >= 25;
 
* 查询age <= 25的记录
db.userInfo.find({age: {$lte: 25}});
 
* 查询age >= 23 并且 age <= 26
db.userInfo.find({age: {$gte: 23, $lte: 26}});
 
* 查询name中包含 mongo的数据
db.userInfo.find({name: /mongo/});
select * from userInfo where name like ‘%mongo%’;
 
* 查询name中以mongo开头的
db.userInfo.find({name: /^mongo/});
select * from userInfo where name like ‘mongo%’;
 
* 查询之正则
db.userInfo..find({name:{$regex:/aaa/}})

* 查询指定列name、age数据
db.userInfo.find({}, {name: 1, age: 1});
相当于：select name, age from userInfo;
当然name也可以用true或false,当用ture的情况下河name:1效果一样，如果用false就是排除name，显示name以外的列信息。
 
* 查询指定列name、age数据, age > 25
db.userInfo.find({age: {$gt: 25}}, {name: 1, age: 1});
相当于：select name, age from userInfo where age > 25;
 
* 按照年龄排序
升序：db.userInfo.find().sort({age: 1});
降序：db.userInfo.find().sort({age: -1});
 
* 查询name = zhangsan, age = 22的数据
db.userInfo.find({name: 'zhangsan', age: 22});
相当于：select * from userInfo where name = ‘zhangsan’ and age = ‘22’;
 
* 查询前5条数据
db.userInfo.find().limit(5);
相当于：select top 5 * from userInfo;
 
* 查询10条以后的数据
db.userInfo.find().skip(10);
 
* 查询在5-10之间的数据
db.userInfo.find().limit(10).skip(5);
可用于分页，limit是pageSize，skip是第几页*pageSize
 
* or与 查询
db.userInfo.find({$or: [{age: 22}, {age: 25}]});
相当于：select * from userInfo where age = 22 or age = 25;
 

* in 查询
db.userInfo.find({_id:({$in:[20368,20369]})})

* 查询第一条数据
db.userInfo.findOne();
相当于：select top 1 * from userInfo;
db.userInfo.find().limit(1);
 
* 查询某个结果集的记录条数
db.userInfo.find({age: {$gte: 25}}).count();
相当于：select count(*) from userInfo where age >= 20;
 
* 按照某列进行排序
db.userInfo.find({sex: {$exists: true}}).count();
相当于：select count(sex) from userInfo;

### 索引相关

db.userInfo.ensureIndex({name: 1});
db.userInfo.ensureIndex({name: 1, ts: -1});
 
2、查询当前聚集集合所有索引
db.userInfo.getIndexes();
 
3、查看总索引记录大小
db.userInfo.totalIndexSize();
 
4、读取当前集合的所有index信息
db.users.reIndex();
 
5、删除指定索引
db.users.dropIndex("name_1");
 
6、删除所有索引索引
db.users.dropIndexes();

### 添加

db.users.save({name: ‘zhangsan’, age: 25, sex: true});
添加的数据的数据列，没有固定，根据添加的数据为准
 
### 修改

db.users.update({age: 25}, {$set: {name: 'changeName'}}, false, true);
相当于：update users set name = ‘changeName’ where age = 25;
 
db.users.update({name: 'Lisi'}, {$inc: {age: 50}}, false, true);
相当于：update users set age = age + 50 where name = ‘Lisi’;
 
db.users.update({name: 'Lisi'}, {$inc: {age: 50}, $set: {name: 'hoho'}}, false, true);
相当于：update users set age = age + 50, name = ‘hoho’ where name = ‘Lisi’;
 
### 删除

db.users.remove({age: 132});
 
### 查询修改删除

db.users.findAndModify({
    query: {age: {$gte: 25}}, 
    sort: {age: -1}, 
    update: {$set: {name: 'a2'}, $inc: {age: 2}},
    remove: true
});
 
db.runCommand({ findandmodify : "users", 
    query: {age: {$gte: 25}}, 
    sort: {age: -1}, 
    update: {$set: {name: 'a2'}, $inc: {age: 2}},
    remove: true
});
update 或 remove 其中一个是必须的参数; 其他参数可选。