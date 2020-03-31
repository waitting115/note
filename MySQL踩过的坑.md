---

---

# 报错信息：重新安装mysql的时候出现的提示

E:\developsoftware\mysql-8.0.13-winx64\bin>mysqld install
The service already exists!
The current server installed: E:\MySql\bin\mysqld MySQL
报错原因：以前机器上装过mysql没有卸载干净。

解决方案：

C:\windows\system32>sc query mysql

SERVICE_NAME: mysql
        TYPE               : 10  WIN32_OWN_PROCESS
        STATE              : 1  STOPPED
        WIN32_EXIT_CODE    : 0  (0x0)
        SERVICE_EXIT_CODE  : 0  (0x0)
        CHECKPOINT         : 0x0
        WAIT_HINT          : 0x7d0

C:\windows\system32>sc delete mysql
[SC] DeleteService 成功
等确认以前装过的所有的旧版的东西都清理干净后再装新的就可以了。

————————————————
版权声明：本文为CSDN博主「于华_」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Myuhua/article/details/84817491

---

# mysql-8.0.11-winx64安装时 MySQL 服务正在启动 MySQL 服务无法启动

版权声明：本文为博主原创文章，遵循[ CC 4.0 BY-SA ](http://creativecommons.org/licenses/by-sa/4.0/)版权协议，转载请附上原文出处链接和本声明。

本文链接：<https://blog.csdn.net/mukouping82/article/details/81105831>

按照操作网上常规步骤在mysql的根目录下编写my.ini并创建data文件夹

[mysql]

设置mysql客户端默认字符集

default-character-set=utf8 
[mysqld]
#设置3306端口
port = 3306 

设置mysql的安装目录

basedir=D:\mysql-8.0.11-winx64

设置mysql数据库的数据的存放目录

datadir=D:\mysql-8.0.11-winx64\data

允许最大连接数

### 

max_connections=200

服务端使用的字符集默认为8比特编码的latin1字符集

character-set-server=utf8

创建新表时将使用的默认存储引擎

default-storage-engine=INNODB



故障现象

1.D:\mysql-8.0.11-winx64\bin>net start mysql

MySQL 服务正在启动 .
MySQL 服务无法启动。

服务没有报告任何错误。

请键入 NET HELPMSG 3534 以获得更多的帮助。

2.进一步错误

D:\mysql-8.0.11-winx64\bin>mysqld --console
2018-07-18T13:21:30.946001Z 0 [System] [MY-010116] [Server] D:\mysql-8.0.11-winx64\bin\mysqld.exe (mysqld 8.0.11) starting as process 11760
2018-07-18T13:21:30.983631Z 1 [ERROR] [MY-011011] [Server] Failed to find valid data directory.
2018-07-18T13:21:30.986677Z 0 [ERROR] [MY-010020] [Server] Data Dictionary initialization failed.
2018-07-18T13:21:30.988397Z 0 [ERROR] [MY-010119] [Server] Aborting
2018-07-18T13:21:30.990846Z 0 [System] [MY-010910] [Server] D:\mysql-8.0.11-winx64\bin\mysqld.exe: Shutdown complete (mysqld 8.0.11)  MySQL Community Server - GPL.

解决方案：

1.删除自己手动创建的data文件夹；

2.管理员权限CMD的bin目录下，移除已错误安装的mysqld服务；

D:\mysql-8.0.11-winx64\bin>mysqld -remove MySQL
The service doesn't exist!

3.在CMD的bin目录下执行mysqld --initialize-insecure

会发现程序在mysql的根目录下自动创建了data文件夹以及相关的文件

4.bin目录下执行mysqld -install

Service successfully installed.

5.bin目录下执行mysql服务启动net start mysql
MySQL 服务正在启动 ..
MySQL 服务已经启动成功。

执行完成；

MySQL8.0以上版本更改密码：

> mysql -u root -p直接回车
>
> mysql> USE mysql;
> Database changed
> mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY "123456";
> Query OK, 0 rows affected (0.01 sec)
>
> mysql> quit;
> Bye
>
> C:\WINDOWS\system32>mysql -u root -p
> Enter password: ******
>
> Welcome to the MySQL monitor.  Commands end with ; or \g.
> Your MySQL connection id is 19
> Server version: 8.0.17 MySQL Community Server - GPL
>
> Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
>
> Oracle is a registered trademark of Oracle Corporation and/or its
> affiliates. Other names may be trademarks of their respective
> owners.
>
> Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

---

