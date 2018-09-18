---
layout: post
title: MongoDB安装使用
tags:
  - MongoDB
categories:
  - MongoDB
date: 2018-05-09 17:00:14
---

## MongoDB
MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。

MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

## 安装
vim /etc/yum.repos.d/mongodb-org-3.6.repo
```
[mongodb-org-3.6][mongodb-org-3.6]
 namename==MongoDB RepositoryMongoDB Repository
 baseurlbaseurl==https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/https://repo.mongodb.org/yum/redha 
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```
修改数据文件存储位置
vim /etc/mongod.conf 中的dbPath

```
yum install -y mongodb-org
mongod #启动 3.6.4 启动 service mongod start
mongo #进入命令行交互
```

## 开启认证
1. MongoDB是没有默认管理员账号，所以要先添加管理员账号，再开启权限认证。
2. 切换到admin数据库，添加的账号才是管理员账号。
3. 用户只能在用户所在数据库登录，包括管理员账号。
4. 管理员可以管理所有数据库，但是不能直接管理其他数据库，要先在admin数据库认证后才可以
```
use admin
// 创建用户
db.createUser(
  {
    user: "admin",
    pwd: "admin",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)

// 删除用户
db.dropUser(用户名);

// 查看用户权限
db.runCommand(
  {
    usersInfo:"fastschooladmin",
    showPrivileges:true
  }
)

// 查看当前用户
db.runCommand({connectionStatus : 1})

// 修改用户密码
db.changeUserPassword(用户名, 新密码);
```



修改文件
vim /etc/mongod.conf
```
security:
  authorization: enabled
```
重启mongod


修改提示信息
```
You can also add the user name to prompt by overriding the prompt function in .mongorc.js file, under OS user home directory. Roughly:

prompt = function() {
    user = db.runCommand({connectionStatus : 1}).authInfo.authenticatedUsers[0]
    if (user) {
        return "user: " + user.user + ">"
    }
    return ">"
}       
```

## 基本命令
### 数据库
1. show dbs显示数据库列表
2. db 显示当前数据库对象或集合
3. use local 切换数据库，如果数据库不存在，则创建数据库，否则切换到指定数据库。
如果刚创建的数据库在show dbs看不到，需要先插入数据才能看到。
4. db.dropDatabase() 删除数据库

### 集合
1. show tables 查看集合
2. db.createCollection(name, options) 创建集合
或者当你插入一些文档时，MongoDB 会自动创建集合。
3. db.集合名.drop() 删除集合

### 文档
1. db.COLLECTION_NAME.insert(document) 插入文档
2. update() 和 save() 方法来更新集合中的文档
```
query : update的查询条件，类似sql update查询内where后面的。
update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
writeConcern :可选，抛出异常的级别。

```

3. db.集合名.remove() 删除
```
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```
参数说明：
query :（可选）删除的文档的条件。
justOne : （可选）如果设为 true 或 1，则只删除一个文档。
writeConcern :（可选）抛出异常的级别。

删除所有数据：db.col.remove({})

4. db.集合名.find(query, projection)
参数说明：
query ：可选，使用查询操作符指定查询条件
projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。

以易读格式输出
db.col.find().pretty()

```
等于  {<key>:<value>}
小于  {<key>:{$lt:<value>}}   
小于或等于   {<key>:{$lte:<value>}}
大于  {<key>:{$gt:<value>}}
大于或等于   {<key>:{$gte:<value>}}
不等于 {<key>:{$ne:<value>}}
```

AND 条件
db.col.find({key1:value1, key2:value2}).pretty()

OR 条件
db.col.find({$or: [{key1: value1}, {key2:value2}).pretty()

5. limit()指定数量的数据记录
6. sort() 排序
7. ensureIndex()
db.COLLECTION_NAME.ensureIndex({KEY:1})
1为指定按升序创建索引，如果你想按降序来创建索引指定为-1即可
8. aggregate()  用于处理数据(诸如统计平均值,求和等)
db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)


