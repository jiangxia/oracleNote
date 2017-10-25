# Oracle

## 用户与表空间
### 用户
系统用户：
* sys、system：权限比较高的用户，其中sys的权限高于system。sys登录的时候必须用系统管理员的身份登录，而system可以直接登录。
* sysman：用于操作企业管理器
* scott

前三个用户的密码是自己定义的，scoot的密码默认是tiger。

    查看用户：show user
    启用用户的语句：alter user username account unlock
    如果启用scott用户：alter user scott account unlock


### 表空间
表空间概述：表空间是数据库的逻辑存储空间，在数据库中开辟的空间，用于存放数据库的对象，一个数据库可以多个表空间。oracle的表空间是跟MySQL的重要区别，oracle的优化也是通过表空间实现。我们存储的表是存储到表空间里。

表空间的分类：
* 永久表空间：数据库永久存储的对象
* 临时表空间：数据库中间执行过程存放的内容
* UNDO表空间：保存事务所修改数据的旧值。

    创建表空间：
    create [temporary] tablespace tablespace_name tempfile|datafile 'xx.dbf' size xx
    create tablespace test1_tablespace datafile 'test1file.dbf' size 10m;
    create temporary tablespace temptest1_tablespace tempfile 'tempfile1.dbf' size 10m;

    修改表空间：

    删除表空间：


自定义表空间

## 表与约束
### 创建和管理表

### 约束


## 查询语句
