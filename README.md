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
        修改表名：rename table_name to new_table_name;
    删除表：
        截断数据：truncate table table_name;
        删除表：drop table table_name;

管理表中的数据：
    
    添加数据：insert into table_name (column1,column2,……) values (value1,value2,……);
    复制数据：
        在创建表时复制：create table table_new as select column,...|* from table_old;
        在添加时复制：insert into table_new [(column1,...)] select column1,...|* from table_old;
    修改数据：
        update table_name set column1 = value1,... [where conditions]
    删除数据：
        delete from table_name [where conditions]
### 约束
约束的作用：定义规则、确保完整性（精确性跟可靠性）

    非空约束
        创建表：create table table_name (column_name datatype not null,...);
        修改表：alter table table_name modify column_name datatype not null;
        修改表时去掉非空约束：alter table table_name modify column_name datatype  null;
    主键约束（非空、唯一，可以由多个字段构成，称为联合主键或复合主键）
        创建表：
            列级：create table table_name(column_name datatype primary key,...)
            表级：constraint constraint_name primary key (column_name1,...)
        修改表：
            add constraint constraint_name primary key(column_name1,...);
        更改约束名称：
            rename constraint old_name to new_name;
        删除约束：
            禁用：disable|enable constraint constraint_name;
            删除：drop constraint constraint_name;
                 drop primary key[cascade];
    外键约束：
        创建表：
            列级：create table table1(column_name datatype references table2(column_name),...);
            表级：constraint constraint_name foreign key(column_name) references table_name(column_name) [on delete cascade];
        修改表：
            add constraint constraint_name foreign key(column_name) references table_name(column_name) [on delete cascade];
        删除约束：
            禁用：disable|enable constraint constraint_name;
            删除：drop constraint constraint_name;
    唯一约束：
        创建表：
            列级：create table table_name (column_name datatype unique,...);
            表级：constraint constraint_name unique(column_name);
        修改表：
            add constraint constraint_name unique(column_name);
        删除约束：
            禁用：disable|enable constraint constraint_name;
            删除：drop constraint constraint_name;
    检查约束：
        创建表：
            列级：create table table_name (column_name datatype check(expressions),...);
            表级：constraint constraint_name check(expressions);
        修改表：
            add constraint constraint_name check(expressions);
        删除约束：
            禁用：disable|enable constraint constraint_name;
            删除：drop constraint constraint_name;

总结：
* 主键约束每张表中只能有一个，但可以由多个字段构成
* 外键约束是唯一一个涉及两张表的关系
* 创建表时，只有非空约束只能在列级设置，不能在表级设置，并且，非空约束是没有名字的
* 修改表时，非空约束的语法跟其他约束不一样，是修改表字段的语句。
* 更改约束的名称，非空约束不能改名。如果不知道约束名称，可以通过user_constraints数据字典查看
* 删除约束：非空约束是用修改表字段的语法，其他约束都有禁用跟删除两种方式。删除主键约束可以使用drop primary key 因为主键约束是特殊的，一个表只有一个。

## 查询语句
    基本查询语句：select [distinct] column_name1,...|* from table_name [where conditions]  (distinct 不显示重复行)
    给字段设置别名：select column_name as new_name,... from table_name;
    逻辑运算符：not and or
    模糊查询：
        like
        _ ：一个_只能代表一个字符
        % ：可以代表0到多个任意字符
    范围查询：
        between...and
        in/not in
    对查询结果排序：select ... from ... [where ...] order by column1 desc/asc,...
    case ... when：
        case column_name when value1 then result1,... [else result] end
        case when column_name = value1 then result1,...[else result] end