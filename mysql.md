# 常用命令

打开数据库： mysql -u root -p  然后输入密码，如果没有密码则直接回车

展示所有数据库：SHOW DATABASES

创建数据库：CREATE DATABASE RUNOOB

进入某数据库： USE RUNOOB

创建数据表：CREATE TEBLE allUser(

​	openID VARCHAR(100) NOT NULL,

​	userName VARCHAR(100) NOT NULL,

​	PRIMARY KEY (openID)

​	)ENGINE=InnoDB DEFAULT CHARSET=UTF8;

(用InnoDB引擎，默认编码为utf-8)

查看数据表：show tables

查看表结构：desc runoob

删除数据表：DROP TABLE runoob

插入数据：

```mysql
INSERT INTO alluser
-> (openID,userName,headPortraitUrl,recommendation,meFocus,focusMe,myCollection)
-> VALUES
-> ("five","five","../../iamges/userHead/cat.jpg","'考研资料',’球类运动','租好物'","'snwn','xxg','hx','ch','lsiy'","'snwn','hx','fzzf'","'lsjy-7','snwn-8','hx-14','hx-16'");
```
查询数据：

​	SELECT * FROM alluser WHERE openID='five';

更新数据表：

​	 UPDATE alluser SET userName='Five' WHERE openID='five';

删除表数据：

​	DELETE FROM alluser WHERE openID='five';

LIKE语句：

​	 select * from alluser where openID like '%ve';

。。。

添加主外键关系：

​	alter table **goods** add constraint FK_ID foreign key(**openID**) references **alluser(openID)**;

删除主外键关系：

​	先找到要删除外键的表，然后  show create table **goods**;

​	然后找到   CONSTRAINT  后面引号里面的约束名

​	然后   alter table **goods** drop foreign key **FK_ID**;

​	就ok了。

删除主键约束：

​	alter table **goods** drop primary key;

添加主键约束：

​	 alter table **goods** add primary key (**userName**);

删除某一字段：

​	alter table **goods** drop **openID**;

如果想添加复合主键，需要将连接该表的主外键关系先清除，然后将主键删除约束，然后：

​	alter table **mymessage** add primary key (**openID**,**userName**);

​	就可以了。









SQL注入：

​	所谓SQL注入，就是通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。 

​	我们永远不要信任用户的输入，我们必须认定用户输入的数据都是不安全的，我们都需要对用户输入的数据进行过滤处理。

防止SQL注入，我们需要注意以下几个要点：

- 1.永远不要信任用户的输入。对用户的输入进行校验，可以通过正则表达式，或限制长度；对单引号和 双"-"进行转换等。
- 2.永远不要使用动态拼装sql，可以使用参数化的sql或者直接使用存储过程进行数据查询存取。
- 3.永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接。
- 4.不要把机密信息直接存放，加密或者hash掉密码和敏感的信息。
- 5.应用的异常信息应该给出尽可能少的提示，最好使用自定义的错误信息对原始错误信息进行包装
- 6.sql注入的检测方法一般采取辅助软件或网站平台来检测，软件一般采用sql注入检测工具jsky，网站平台就有亿思网站安全平台检测工具。MDCSOFT SCAN等。采用MDCSOFT-IPS可以有效的防御SQL注入，XSS攻击等。

在脚本语言，如Perl和PHP你可以对用户输入的数据进行转义从而来防止SQL注入。 

PHP的MySQL扩展提供了mysqli_real_escape_string()函数来转义特殊的输入字符。

like查询时，如果用户输入的值有"_"和"%"，则会出现这种情况：用户本来只是想查询"abcd_"，查询结果中却有"abcd_"、"abcde"、"abcdf"等等；用户要查询"30%"（注：百分之三十）时也会出现问题。 

在PHP脚本中我们可以使用addcslashes()函数来处理以上情况，



















