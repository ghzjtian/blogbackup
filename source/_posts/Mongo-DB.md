---
title: MongoDB
tags: [DB]
categories: [DB]
date: 2020-06-08 17:46:19
---


> MongoDB 基本操作

<!-- more -->

## 1.MongoDB 

* 1.[The MongoDB 4.2 Manual](https://docs.mongodb.com/manual/#the-mongodb-version-manual)
* 2.[mongodb用户创建以及权限控制](https://www.jianshu.com/p/62736bff7e2e)

#### 1.常用命令

* 1.开启/关闭 MongoDB 服务

```
brew services start mongodb-community@4.2
brew services stop mongodb-community@4.2

brew services restart mongodb-community@4.2
```

* 2.登录命令

```
mongo --username userName --password --host 127.0.0.1 --port 27017
```

* 3.数据库操作

```
// 取得数据库的名字
db.adminCommand('listDatabases')
show databases
db.getMongo().getDBNames()

// 显示表
show collections
show tables


// 创建 DB , 表 及 数据
use myNewDB
db.myNewCollection1.insertOne( { x: 1 } )

// 显示表的数据
db.myNewCollection1.find( {} )

// 删除数据库
use myNewDB
db.dropDatabase()
```

* 4.备份/恢复 数据库

```
mongodump --db=myNewDB --out=/Users/glb_gz/Downloads
mongorestore -d myNewDB /Users/glb_gz/Downloads/myNewDB
```

* 5.新增用户

```
// 新增管理员账号.

> use admin
switched to db admin
> db.createUser(
 {
  user: "admin",
   pwd: passwordPrompt(),
   roles: [ { role: "userAdminAnyDatabase", db: "admin"} ]
 }
)
```

```
// 新增普通用户

> use dashboard_admin

> db.createUser(
 {
  user : "user1",
  pwd : "password",
  roles: [ { role : "readWrite", db : "db1" } ,
           { role : "readWrite", db : "db2" } ]
 }
)

```

```
// 更新用户的 roles, 更新完后用户只有访问 db1 和 db3 的权限

db.updateUser(
   "user1",
   {
     roles: [ 	{ role : "readWrite", db : "db1" },
					{ role : "readWrite", db : "db3" },
	     ]
   }
)
```


#### 2.开启权限认证登录

> 默认 匿名用户可以登录.

```
// vim /usr/local/etc/mongod.conf

security:
    authorization: enabled

```


