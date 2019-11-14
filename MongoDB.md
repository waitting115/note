# cmd指令

### 启动服务：net start ``"MongoDB"

### 停止服务：net stop "MongoDB"`



查看所有数据库：show dbs;

进入到某数据库：use haibin;

查看所有集合：show collections;

查看某集合的数据：db.user.find().pretty();

### 在mongo.exe中的指令

```
show dbs;                  #查看全部数据库

show collections;          #显示当前数据库中的集合（类似关系数据库中的表）

show users;                #查看当前数据库的用户信息

use <db name>;             #切换数据库跟mysql一样

db;或者db.getName();        #查看当前所在数据库

db.help();                 #显示数据库操作命令，里面有很多的命令 
db.foo.help();             #显示集合操作命令，同样有很多的命令，foo指的是当前数据库下，一个叫foo的集合，并非真正意义上的命令 
db.foo.find();             #对于当前数据库中的foo集合进行数据查找（由于没有条件，会列出所有数据） 
db.foo.find( { a : 1 } );  #对于当前数据库中的foo集合进行查找，条件是数据中有一个属性叫a，且a的值为1
```

创建一个test数据库例子：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
> use test;             #创建数据库
switched to db test
> db;               
test
> show dbs;           #检查数据库
admin 0.000GB
local 0.000GB

> db.test.insert({"_id":"520","name":"xiaoming"})         #创建表

WriteResult({ "nInserted" : 1 })

> db.createUser({user:"xiaoming",pwd:"123456",roles:[{role:"userAdmin",db:"test"}]})        #创建用户
Successfully added user: {
"user" : "xiaoming",
"roles" : [
{
"role" : "userAdmin",
"db" : "test"
}
]
}
db.removeUser("userName");         #删除用户
show users;                        #显示当前所有用户
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

db.dropDatabase();   #删除当前使用数据库

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
> show dbs;
admin 0.000GB
local 0.000GB
test 0.000GB
test_1 0.000GB

> db;
test_1

> db.dropDatabase();
{ "dropped" : "test_1", "ok" : 1 }


> show dbs;
admin 0.000GB
local 0.000GB
test 0.000GB
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

db.stats();             #显示当前db状态

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
> db.stats();
{
    "db" : "test_1",
    "collections" : 0,
    "views" : 0,
    "objects" : 0,
    "avgObjSize" : 0,
    "dataSize" : 0,
    "storageSize" : 0,
    "numExtents" : 0,
    "indexes" : 0,
    "indexSize" : 0,
    "fileSize" : 0,
    "ok" : 1
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

db.version();         #当前db版本

```
> db.version();
3.4.10
```

db.getMongo();     #查看当前db的链接机器地址

```
> db.getMongo();
connection to 172.16.40.205:27017
```

**mongodb find查询文档**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
基本语法：

db.表名.find({'key':'value'});

实例：

> show dbs;
admin 0.000GB
easy-mock 0.001GB
local 0.000GB
> use easy-mock
switched to db easy-mock
> db
easy-mock
> show collections;
groups
mock_counts
mocks
projects
user_group
user_project
users
> db.users.find({'name':'xiaoming'});
{ "_id" : ObjectId("5bc859307e81d95b15f67c5c"), "head_img" : "//img.souche.com/20161230/png/fd9f8aecab317e177655049a49b64d02.png", "nick_name" : "1539856688465", "password" : "$2a$08$7mAecPo6N8ATesAxfKrPG.wb10Ns.LfntUNkce7p2pAJ0kAvW3fPm", "name" : "xiaoming", "create_at" : ISODate("2018-10-18T09:58:08.465Z"), "__v" : 0 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**mongodb update修改文档**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
# 查找name为xiaoming的用户，将用户的密码更改

> db.users.update({'name':'xiaoming'},{$set:{'password':'$2a$08$UIEXZ7uK1opggCsTkmE2buE.EuWjchH42GYDnPcEn0PL/Y5dVT7l2'}})
```

　> db.users.find({'name':'xiaoming'});
{ "_id" : ObjectId("5bc859307e81d95b15f67c5c"), "head_img" : "//img.souche.com/20161230/png/fd9f8aecab317e177655049a49b64d02.png", "nick_name" : "1539856688465", "password" : "$2a$08$UIEXZ7uK1opggCsTkmE2buE.EuWjchH42GYDnPcEn0PL/Y5dVT7l2", "name" : "xiaoming", "create_at" : ISODate("2018-10-18T09:58:08.465Z"), "__v" : 0 }

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**开启远程访问**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
编辑配置文件：vi /etc/mongod.conf
bindIp: 172.16.40.205      #数据库所在服务器IP地址
保存重启数据库！
本地登录：mongo 172.16.40.205/admin -uadmin -p123456
远程登录：
1. 下载mongodb压缩包
mongodb-linux-x86_64-3.4.10.tgz
2. 解压
> tar zxvf mongodb-linux-x86_64-3.4.10.tgz
3. 进入bin目录
> cd mongodb-linux-x86_64-3.4.10/bin
4. 连接远程数据库
> ./mongo 172.16.40.205:27017/admin  -u user  -p  password
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
mongodDB备份与恢复
一、mongodDB备份
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
【语法】：mongodump -h <dbhost> -d <dbname> -o <dbdirectory>

-h：
MongDB所在服务器地址，例如：127.0.0.1或localhost，当然也可以指定端口号：127.0.0.1:27017

-d：
需要备份的数据库实例名，例如：users

-o：
指定备份的数据存放的目录位置，例如：/root/mongdbbak/，当然该目录需要提前建立，在备份完成后，系统自动在/root/mongdbbak/目录下建立一个users目录，这个目录里面存放该数据库实例的备份数据。数据形式是以JSON的格式文件存储。

例如： 
mongodump -h localhost -d users -o /root/mongdbbak/
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
二、mongodDB恢复
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
【语法】：mongorestore -h <hostname><:port> -d dbname <path>

--host <:port>, -h <:port>：
MongoDB所在服务器地址，默认为:localhost:27017

-d ：
需要恢复的数据库实例名，例如：users，当然这个名称也可以和备份时候的不一样，比如user2

--drop：
恢复的时候，先删除当前数据，然后恢复备份的数据。就是说，恢复后，备份后添加修改的数据都会被删除，谨慎使用！

--dir：
指定备份的目录。

例如：
mongorestore -h localhost -d users --dir /root/mongdbbak/users
```