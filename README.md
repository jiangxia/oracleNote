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

表空间操作

    创建表空间：
    create [temporary] tablespace tablespace_name tempfile|datafile 'xx.dbf' size xx
    create tablespace test1_tablespace datafile 'test1file.dbf' size 10m;
    create temporary tablespace temptest1_tablespace tempfile 'tempfile1.dbf' size 10m;
    修改表空间：
        修改表空间状态
            设置联机或者脱机状态：alter tablespace tablespace_name online|offline
            设置只读或者可读写状态：alter tablespace tablespace_name read only|read write
        修改数据文件：
            增加数据文件：alter tablespace tablespace_name add datafile 'xx.dbf' size xx;
            删除数据文件：alter tablespace tablespace_name drop datafile 'filename.dbf';
    删除表空间：
        drop tablespace tablespace_name [including contents]

## 表与约束
### 创建和管理表
* 认识表：存储在表空间内，是基本存储单位，二维结构（行和列）。
* 数据类型：
    - 字符型：
        + CHAR(n)、NCHAR(n)：固定长度类型，CHAR最长是2000个字符，NCHAR最长1000个字符
        + VARCHAR2(n)、NVARCHAR2(n)：可变长度类型，VARCHAR2最长是4000个字符，、NVARCHAR2最长2000个字符
    - 数值型：
        + NUMBER(p,s)：p为有效数字，s小数点后的位数
        + FLOAT(n)：一般使用NUMBER
    - 日期类型：
        + DATE
        + TIMESTAMP
    - 其他类型
        + BLOB：4G数据，二进制数据
        + CLOB：4G数据，字符串类型
        
管理表：
    
    创建表：create table table_name (column_name datatype, ……)
    修改表：
        添加字段：alter table table_name add column_name datatype;
        更改字段数据类型：alter table table_name modify column_name datatype;
        删除字段：alter table table_name drop column column_name;
        修改字段名：alter table table_name rename column column_name to new_column_name;
        修改表名：alter table table_name add column_name datatype;
### 约束


## 查询语句
