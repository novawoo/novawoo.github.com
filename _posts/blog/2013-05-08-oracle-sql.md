---
layout:     post
title:      SQL in Oracle
category:   Oracle & SQL
description:   SQL基础
tags: 
 - oracle
 - sql
---
## 1. 简介
### 1.1 Oracle公司介绍

<li>全球第一大数据库厂商</li>
<li>全球第二大独立软件供应商</li>
<li>旗下产品有：Oracle数据库、Mysql数据库、Java等</li>
<br>

### 1.2 DB、DBMS、RDBMS

- DB -- DataBase 数据库  
- DBMS -- DataBase Management System 数据库管理系统  
- RDMBMS -- Relationship Database Management System 关系型数据库管理系统  
- 常用的RDBMS有：  
    - Oracle 10g/11g(Oracle)  
    - DB2(IBM)  
    - Sysbase(Sysbase)  
    - SQL Server(MS)  
    - MySql(Oracle)  

### 1.3 Oracle DBA认证
>DBA -- 数据库管理员

- Oracle DBA认证包括：  
    - oca 初级认证  
    - ocp 专家级认证  
    - ocm 大师认证  

### 1.4 SQL语言介绍
  SQL(Structured Query Language)结构化查询语言，包括：

- 数据定义语言(DDL Data Definition Language)  
        eg:create、drop、alter、rename等  
- 数据操作语言(DML Data Manipulation Language)  
        eg:insert、delete、update等  
- 事务控制语言(Transaction Control Language)  
        eg:commit、savepoint、rollback等  
- 数据查询语言(DQL Data Query Language)  
        eg:select  
- 数据控制语言(DCL Data Control Language)
        用于设置或更改数据库用户或角色的语句  
        eg:grant、revoke、password

## 2. Oracle安装
  安装后会自动生成 sys 和 system用户:

1. sys用户是 超级用户，具有最高权限，具有sysdba角色，有create database的权限，该用户默认密码是change_on_install  
2. system用户是 管理操作员，权限也很大，具有sysoper角色，没有create database的权限，默认的密码是manager  
3. 一般讲，对数据库维护，使用system用户登录就可以了

### 2.1 Oracle启动(windows)
> "我的电脑" --> 右击 --> 管理 --> 服务,找到Oracle打头的两个服务并启动  
>  1. Oracle*TNSListener -- TNS监听器服务  
>  2. OracleService[SID] -- 对应[SID]的数据库实例服务  

## 3. Oracle管理工具介绍
1. 自带软件--sql plus  
     启动方法：win+r --> sqlplusw
2. 自带软件--Enterprise Manager Console
3. 经典工具--PL/SQL Developer
4. 官方工具--SQL Developer

## 4. sql plus常用命令
### 4.1 常用
0. `show user`
> 显示当前用户
1. `conn[ect]`
> 用法：conn 用户名/密码@网络服务名 [as sysdba/sysoper]
        当特权用户连接时，必须带上 as sysdba 或是 as sysoper
2. `disc[onnect]`
3. `passw[ord]`
> 说明：该命令用于修改密码。如果要修改其他用户的密码，需要用sysdba 或 sysoper登录
4. `exit`
> 退出当前连接
5. `colum coloumName format a20`
> 设置列格式（a20 20个字符）

### 4.2 文件操作命令
1. `start`和`@`
> 说明：运行sql脚本
  ######案例：
<pre class="brush:sql">
  sql>@ d:\a.sql
  --或者
  sql>START d:\a.sql
</pre>
2. `edit`
> 说明：该命令可以编辑指定的sql脚本
  案例：
<pre class="brush:sql">
sql>edit d:\a.sql
</pre>
3. `spool`
> 说明：该命令可以将sql plus屏幕上的内容输出到指定文件中去
案例：
<pre class="brush:sql">
sql>spool d:\b.sql 
sql>spool off
</pre>
4. `1`
> 说明：显示缓冲区中最近的一条语句
5. `/`
> 说明：执行语句
6. `clear scr`
> 说明：清屏

### 4.3 交换式命令
1. `&`
> 说明：可以替代变量，而该变量在执行时，需要用户输入。
案例：
<pre class="brush:sql">
sql>select * from emp where job='&job'
</pre>
2. `edit`
> 说明：该命令可以编辑指定的sql脚本
案例：
<pre class="brush:sql">
sql>edit d:\a.sql
</pre>
3. `spool`
  ...
 
### 4.4 显示和设置环境变量
概述：可以用来控制输出的各种格式，set show 如果希望永久的保存相关的设置，可以去修改 glogin.sql 脚本

1. linesize  
说明：设置显示行的宽度，默认是80个字符  
案例：
<pre class="brush:sql">
sql>show linesize
sql>set linesize 90
</pre>
2. pagesize  
  说明：设置每页显示的行数目，默认是14，用法和linesize一样
3. timing  
说明：显示sql执行操作所消耗的时间  
用法：set timing on；  
session中常用的其他参数：  
<pre class="brush:sql">
sql>alter session set nls_date_format='yyyy-mm-dd hh24:mi:ss';
sql>alter session set nls_language='american';
sql>alter session set nls_territory=america;
</pre>
4. 显示环境变量的值  
用法：echo $环境变量的名称  
案例：
<pre class="brush:sql">
sql>echo $NLS_DATA_FORMART
</pre>

## 5. 需要了解的oracle概念
### 5.1 Oracle权限
####权限树
<pre>
  权限
   |-系统权限(大概140多种)
   |     用户对数据库的相关权限（如：create session）
   |-对象权限（25种）
   |     用户对其他用户的数据对象操作的权限
   |   |-select
   |   |-insert
   |   |-update
   |   |-delete
   |   |-all
   |   |-create index
   |   |- ... ...
</pre>

### 5.2 数据对象
  比如：函数、过程、包、触发器、库、序列、角色、表空间、表、视图
### 5.3 角色
  概述：角色是一系列权限的集合
####分类
<pre>
  角色
   |-预定义角色
   |     如：connect
   |-自定义角色
</pre>
####常用角色：
    connect：7种基本权限
    dba：数据库管理员权限
    resource：在任何一个表空间建表
    
### 5.4 权限传递
<pre class="brush:sql">
sql>grant [privilege] on [tableName] to [userName] [with grant option]
sql>grant [roleNmae] to [username] [with admin option]
</pre>
__用户通过权限传递获得的权限，在授权者的权限被回收后，对应的权限也被回收__

## 6. Oracle用户管理
### 6.1 创建用户
概述：在Oracle中要创建一个新的用户使用 `create user` 语句，一般是具有dba的权限才能使用。  
用法：
<pre class="brush:sql">
sql>create user [username] identified by [passwd]
</pre>
案例：
<pre class="brush:sql">
sql>create user xiaoming identified by pw123
</pre>
### 6.2 修改密码
  用法：
<pre class="brush:sql">
--给自己修改密码
sql>passwrod [yourUserName]
--给别人修改密码（需要dba权限或alter user的系统权限）
sql>alter user [userName] identified by [newPWD]
</pre>
### 6.3 删除用户
  概述：一般以dba的身份去删除某个用户，如果用其他用户去删除用户则需要具有`drop user`的权限  
  用法：`drop user [username] [cascade]`  
  用法说明：在删除用户时，注意如果要删除的用户已经创建了表，那么就需要在删除时带一个参数 cascade，表示将该用户创建的表一起删除，称为 __级联删除__
### 6.4 用户授权
概述：创建的新用户是没有任何权限的，甚至连登录数据库的权 限都没有，需要为其指定相应的权限。给一个用户赋权限使用命令`grant`，回收权限使用命令`revoke`  
####授权：
<pre class="brush:sql">
--1
sql>grant [roleNmae] to [username] [with admin option]
--2
sql>grant [privilege] on [tableName] to [userName] [with grant option]
</pre>
用法说明：  
1. `roleName` 角色名称或系统权限  
   `username` 被授权的用户名  
   `with admin option` 是被授权的用户可以将此次被赋予的权限再授给其他用户，即权限传递的概念  
2. `privilege` 权限名称，对象权限  
   `tableName` 表名  
   `userName` 被授权的用户  
   `with grant option` 是被授权的用户可以将此次被赋予的权限再授给其他用户，即权限传递的概念  
####回收权限：
`revoke [privilege] on [tableName] from [userName]`
### 6.5 使用profile管理用户口令
概述：profile是口令限制，资源限制的命令集合，当建立数据库时，oracle会自动建立名称为default的profile。当建立用户没有指定profile选项，那oracle就会将default分配给用户。
####锁定用户
概述：指定该帐户(用户)登录时最多可以输入密码的次数，也可以指定用户锁定的时间(天)一般用dba的身份去执行该命令  
例子：指定tea这个用户最多只能尝试3次登录，锁定时间为2天  
--创建profile文件
<pre class="brush:sql">
sql>[create profile] lock_account [limit failed_login_attempts] 3 [password_lock_time] 2;
--profile应用于用户tea
sql>alter user tea profile lock_account;
</pre>
####解锁用户
<pre class="brush:sql">
sql>[alter user] tea [account unlock];
</pre>
####终止口令
为了让用户定期修改密码可以使用终止口令的指令来完成，同样这个命令需要dba身份操作。  
例子：给前面创建的用户tea创建一个profile文件，要求该用户每个10天要修改自家的登录密码，宽限期为2天。
<pre class="brush:sql">
sql>[create profile] myprofle [limit password_life_time] 10 [password_grace_time] 2;
sql>[alter user] tea [profile] myprofile;
</pre>
####口令历史
概述：如果希望用户在修改密码时，不能使用以前使用过的密码，可以使用口令历史，这样oracle就会将口令修改的信息存放到数据字典中，这样当用户修改密码时，oracle就会对新旧密码进行比较，当发现新旧密码一样时，就提示用户重新输入密码。  
例子：  
--建立profile
<pre class="brush:sql">
sql>create profile password_history limit password_life_time 10 password_grace_time 2 password_reuser_time 10;
   --password_reuse_time //指定口令可重用时间即10天后就可以重用
   --分配给某用户
</pre>
####删除profile
概述：当不需要某个profile文件时，可以删除文件。
<pre class="brush:sql">
sql>drop profile password_history cascade
</pre>
## 7. Oracle表的管理
### 7.1 表名和列的命名规则
必须以字母开头  
长度不能超过30个字符  
不能使用oracle的保留字  
只能使用如下字符 A-Z,a-z,0-9,$,#等

### 7.2 oracle支持的数据类型
####字符型
  `char` 定长 最大2000字符 可能浪费空间，但查询速度快  
  `varchar2` oracle特有类型 变长 最大4000字符 节省空间，但查询速度慢  
  `clob` 字符型大对象 最大4G
####数字类型
  `number` 范围 -10的38次方 至 10的38次方，可以表示整数，也可以表示小数  
  `number(5,2)` 表示一个小数有5位有效数，2位小数，范围 -999.99 至 999.99  
  `number(5)` 表示一个五位整数，范围 -99999至99999  
####日期类型
  `date`  包括年月日和时分秒  
  `timestamp`
####二进制
  `blob` 二进制数据 可以存放图片/声音 最大4G
### 7.3 建表
<pre class="brush:sql">
  sql>create table student(--表名
             xh number(4),--学号
             xm varchar2(20),--姓名
             sex char(2),--性别
             birthday date,--出生日期
             sal number(7,2)--奖学金
      );
</pre>
### 7.4 修改表
#### 7.4.1 添加一个字段
<pre class="brush:sql">
sql>alter table student add (classid number(2));
</pre>
#### 7.4.2 修改字段的长度
<pre class="brush:sql">
sql>alter table student modify (xm varchar2(30));
</pre>
#### 7.4.3 修改字段的类型(不能有数据)
<pre class="brush:sql">
sql>alter table student modify (xm char(30));
</pre>
#### 7.4.4 重命名一列
<pre class="brush:sql">
sql>alter table student rename column name to n;
</pre>
#### 7.4.5 删除一个字段
<pre class="brush:sql">
sql>alter table student drop column sal;
</pre>
#### 7.4.6 修改表的名字
<pre class="brush:sql">
sql>rename student to stu;
</pre>
#### 7.4.7 给表添加约束
<pre class="brush:sql">
sql>alter table student 
      add constraint student_pk primary key(id);
</pre>
#### 7.4.8 删除一个约束
<pre class="brush:sql">
sql>alter table student 
    drop constraint student_pk;
</pre>
#### 7.4.9 修改一个约束
先删除约束，再添加约束  
__非空约束__ 的修改特殊，使用 `alter table student modify (classid null)`

### 7.5 修改数据
#### 7.5.1 添加数据
<pre class="brush:sql">
sql>insert into student values('A001','张三','男','01-5月-05',10);
</pre>
说明：oracle中默认的日期格式'DD-MON-YY' dd日子 mon月份 yy两位的年  
修改日期的默认格式
<pre class="brush:sql">
 alter session set nls_date_format='yyyy-mm-dd';
</pre>
#### 7.5.2 修改一个字段
<pre class="brush:sql">
sql>update student set sex='女' where xh='1';
</pre>
#### 7.5.3 修改多个字段
<pre class="brush:sql">
sql>update student set sex='男',xm='李四' where xh='1'
</pre>
#### 7.5.4 修改含有null值的数据
<pre class="brush:sql">
sql>update student set sex='女' where sal is null;
</pre>
#### 7.5.5 删除表数据
#####delete
说明：删除所有记录，表结构还在，写日志，可以恢复的，速度慢
1. 表占用的空间不释放
2. 保存旧数据到回滚段，时间长
3. 数据可恢复
<pre class="brush:sql">
sql>delete from student
</pre>
#####truncate
说明：删除表中的所有记录，表结构还在，不写日志，无法找回删除的记录，比delete的速度快
1. 释放表占用的空间
2. 不保存旧数据，时间短
3. 数据不可恢复
<pre class="brush:sql">
sql>truncate table student;
</pre>

### 7.6 重命名表
<pre class="brush:sql">
sql>rename test to test2
</pre>
### 7.7 删除表
<pre class="brush:sql">
sql>drop table tablename [cascade constraints] [purge];
</pre>
说明：`cascade constraints` -- 级联删除（先删constraint关系，再删表）  
`purge` -- Oracle 10g的新特性，用于彻底删除表
### 7.8 约束(constraint)
#### 7.8.1 约束的作用
限制无效数据进入到表中

#### 7.8.2 约束的类型
- primary key(PK) 主键约束
- unique key(UK) 唯一键约束
- not null 非空约束
- references foreign key(FK) 外键约束
- check 检查约束

#### 7.8.3 主键约束（PK）
作用：

- 不允许主键列有null记录
- 不允许主键列有重复记录

#####定义：
- 列级约束
<pre class="brush:sql">
sql>create table parent(
       c1 number（2） constraint parent_c1_pk primary key,
       c2 number
       );
</pre>
- 表级约束
<pre class="brush:sql">
sql>create table parent(
    c1 number(2),
    c2 number,
    constraint parent_c1_pk primary key(c1)
    );
</pre>
- 给存在的表增加约束（实质是表级约束）
<pre class="brush:sql">
sql>alter table parent
    add constraint parent_c1_pk primary key(c1);
</pre>
- 列级和表级约束的比较： __表级约束可以用来定义联合主键__
<pre class="brush:sql">
sql>create table parent(
       c1 number(2),
       c2 number,
       constraint parent_c1_pk primary key(c1,c2)
       );
</pre>

#### 7.8.4 非空约束
作用：不允许非空约束的列为空。
#####定义：只有列级约束的形式
<pre class="brush:sql">
   sql>create table parent(
       c1 number（2）not null,
       c2 number
       )；
</pre>
#### 7.8.5 唯一键约束(UK)
作用：
 
- 不允许唯一约束的列中的值重复
- 允许唯一约束的列中的值为空

#####定义：
- 列级约束
<pre class="brush:sql">
   sql>create table test(
       c1 number(3),
       c2 number(2) constraint test_c2_uk unique,
       c3 number(4) constraint test_c3_uk unique
       );
</pre>
- 表级约束
<pre class="brush:sql">
   sql>create table test(
       c1 number(3),
       c2 number(2),
       c3 number(4),
       constraint test_c3_uk unique(c2,c3)
       );
</pre>

#### 7.8.6 外键约束(FK)
作用：

- 保证一对多关系的实现
- 通过外键(FK)可以与同一张表的主键(PK)或唯一键(UK)建立引用关系，也可以与不同表的主键(PK)或唯一键(UK)建立引用关系
- 外键的取值必须匹配父表中已有的值或空值

#####定义：
- 列级约束
<pre class="brush:sql">
   sql>create table child(
       c1 number(3),
       c2 number(2) constraint child_c2_fk references parent(c1)
       );
   sql>create table child2(
       c1 number(3),
       c2 number(2) constraint child_c2_fk 
          references parent(c1) on delete set null
       );
</pre>
- 表级约束
<pre class="brush:sql">
   sql>create table child(
       c1 number(3),
       c2 number(2),
       constraint child_c2_fk foreign key(c2) references parent(c1)
       );
   sql>create table child1(
       c1 number(3),
       c2 number(2),
       constraint child1_c2_fk foreign key(c2) references parent(c1) on delete cascade
       );
 </pre>
 
说明：
>- foreign key 用表级约束定义外键时使用该关键字
>- references 表示引用父表中的某列
>- on delete cascade 级联删除，删除父表的记录前，先删除子表与父表的级联关系
>- on delete set null 删除父表的记录前，先将子表中外键列的相关值置空

*主外键关联中的删除*
<pre class="brush:sql">
sql>drop table parents cascade constraints;
</pre>
等价于：
<pre class="brush:sql">
sql>alter table child drop constraints child_c2_fk;
sql>drop table parent;
</pre>

#### 7.8.7 检查约束(CK)
作用：定义条件表达式，每个列值必须满足该条件
>注意：以下表达式不允许出现在CK的表达式中  
>
>- 伪列：currval、nextval、level、rownum
>- 函数：sysdate、uid、user、userenv
>- 引用其他记录的其他值

#####定义：

- 列级约束
<pre class="brush:sql">
   sql>create table test(
       c1 number(3)
       c2 number(3) constraint test_c2_ck check (c2>100)
       );
</pre>

- 表级约束
<pre class="brush:sql">
   sql>create table test(
       c1 number(3),
       c2 number(2),
       c3 number(2),
       constraint test_c2_ck check((c2+c3)>100)
       );
</pre>

#### 7.8.8 约束部分小结

- 定义约束时用户给他命名，若用户未命名，系统自动命名(形式为SYS_Cn格式)
- 何时创建约束  
    - create table时定义约束
    - 表已经存在，用alter table追加约束
- 一个列上可以有多个约束
- 一个约束定义在多个列上--联合主键
- 定义约束有表级和列级两种形式
 
#### 7.9 给表起别名
<pre class="brush:sql">
sql>create synonym account from tarena.account;
</pre>

#### 7.10 default
说明：
- 缺省值的数据类型必须匹配列的数据类型
- 有效的缺省值为文字值、表达式、sql函数：sysdate、user等
- 无效的缺省值为另一个列的列名或伪列
- default可以用于inser语句、update语句
例子：
<pre class="brush:sql">
sql>create table test(
    c1 number default 1,
    c2 number
    )
sql>insert into test (c2) values (2);
sql>insert into test values (default,3);
sql>update test set c1 = default
    where c1 = 4;
</pre>

## 8. acle表基本查询
### 8.1 sql语句的处理过程
![sql语句的处理过](/images/sql-in-oracle/01-oracle-process.jpg)

### 8.2 sql是非大小写敏感的
sql语句是非大小写敏感的，但是sql中的字符串是大写写敏感的。

### 8.3 查看表结构
<pre class="brush:sql">
sql>desc dept;
</pre>
### 8.4 查询所有列
<pre class="brush:sql">
sql>select * from dept;
</pre>
### 8.5 查询指定列
<pre class="brush:sql">
sql>select ename,sal,job from emp;
</pre>
说明：查指定列的效率比查所有列的效率高很多

### 8.6 使用数学表达式
<pre class="brush:sql">
sql>select (sal+comm)*13 年工资,ename,comm from emp;
</pre>

### 8.7 给列起别名
说明：给列起别名能够改变一个列、表达式的标识
用法：`select columu [as] alias from table`
例如：
<pre class="brush:sql">
sql>select ename,sal*12 年薪 from emp order by 年薪;
</pre>

### 8.8 空值(null)
空值(null)在输入数据时，该字段没有指定值，并且也没有缺省值

- 空值不等于0
- 空值不等于空格
- 算术表达式中包含空值将导致结果为空
- 在算术表达式中包含空值需要用空值转换函数(nvl)处理

### 8.9 处理空值
- 使用`is null`操作符判断null值
- 使用`nvl(ori,des)`函数对null值进行转

>nvl(arg1,arg2)中，arg1与arg2的类型要相同

例如：
<pre class="brush:sql">
sql>select (sal+nvl(comm,0))*13 年工资,ename,comm from emp;
</pre>

### 8.10 拼接运算符(||)
使用`||`可以将某几列或某几列与字符串拼接在一起  
用法：
<pre class="brush:sql">
sql>select colname1||colname2 from tabname;
</pre>

### 8.11 去除重复行(distinct)
说明：`select`后面跟`distinct`关键字去掉重复行，若后面为多列，所有列联合起来唯一  
例如：
<pre class="brush:sql">
sql>select distinct deptno,job from emp;
</pre>

### 8.12 使用where子句
说明：where子句可以对表里的记录进行过滤，where子句后可以跟一个或多个条件表达式(多个表达式间用`and`、`or`连接，也可以通过小括号改变顺序)，不能跟列别名  
用法：
<pre class="brush:sql">
sql>select colname1, ... from tabname
    where colname1's expression = value
    and colname2 > value
    or colname3 < value;
</pre>
### 8.13 比较和逻辑运算符
- 比较运算符  
  `>` `>=` `<` `<=` `=`
- sql比较运算符  
  `between and`  
  `in`  
  `like`  
  `is null`  
- 逻辑运算符  
  `and`  
  `or`  
  `not`  
- 比较运算符否定形式  
  `<>`  `!=`  `^=`
- SQL比较运算符否定形式  
  `not between and`  
  `not in`  
  `not like`  
  `is not null`  

### 8.14 between and运算符
说明：表示大于等于并且小于等于  
用法：
<pre class="brush:sql">
sql>select colname, ... from tabname
    where colname between val1 and val2;
</pre>

### 8.15 in运算符
说明：in表示一个集合，是离散值。表示等于其中任意一个值。  
用法：
<pre class="brush:sql">
sql>select colname, ... from tabname
    where colname in (val1,val2,val3,...);
</pre>

### 8.16 like运算符
说明：在字符串比较中，可以用like和通配符进行查找  
>通配符种类：  
  % 表示0到多个任意字符  
>_ 表示单个字符  
>
>若要查找通配符本身，需要使用`escape`进行转义

用法：
<pre class="brush:sql">
sql>select colname, ... from tabname
    where colname like 'W\_%' escape '\';
</pre>

### 8.17 使用order by子句
说明：

- 用order by子句对查询出来的结果集进行排序。  
- order by后面可以跟列名、表达式、别名、位置  
- null值中排序中的显示：升序显示在最后，降序显示在最前  

用法：
<pre class="brush:sql">
  sql>select colname1,... from tabname
      order by colname2 [asc|desc]
      --asc 默认，升序
      --desc 降序
</pre>
例如：
<pre class="brush:sql">
  sql>select ename,deptno,sal from emp order by deptno,sal desc
</pre>

### 8.18 分支(case when语句和decode函数)
####CASE WHEN 语句
说明：

- 寻找when的优先级:从上到下
- 再多的when,也只有一个出口，即其中有一个满足条件就马上退出case
- else_expr和return_expr的数据类型必须相同

用法：  
<pre class="brush:sql">
CASE WHEN condition THEN return_expr
    [WHEN condition THEN return_expr]
     ... ...
     ELSE else_expr
END
</pre>
例如：
<pre class="brush:sql">
select case when base_duration = 20 then unit_cost + 0.05
            when base_duration = 40 then unit_cost + 0.03
       else unit_cost
       end new_unit_cost,
       base_duration,
       unit_cost
from cost;
</pre>

###DECODE -- 简版的CASE WHEN
用法：
<pre class="brush:sql">
select DECODE(colname1,condition1,value1,
                      [condition2,value2,]
                       ... ...
                       valueN)
from tabname;
</pre>
说明：表示如果colname1等于condition1时,DECODE函数的结果为value1,... ...,如果不等于任何一个condition，则DECODE函数的结果为valueN。  
例子：
<pre class="brush:sql">
select decode(base_duration,20,unit_cost+0.05,
                            40,unit_cost+0.03,
                            unit_cost) new_unit_cost,
       base_duration,
       unit_cost
from cost;
</pre>

## 9. Oracle表复杂查询
### 9.1 数据分组
#### 9.1.1 分组函数
组函数操作在一组行记录上，每组返回一个结果，常用的组函数有:  
  `max` `min` `avg` `sum` `count`
> - 缺省情况组函数处理所有的非空值
> - 当组函数要处理的所有值都为null，count函数返回0,其他函数返回null

用法：
<pre class="brush:sql">
sql>select COUNT(DISTINCT colname)
	from tabname;
</pre>

#### 9.1.2 group by 和 having子句
用法：
<pre class="brush:sql">
sql>select colname,group_function
    from tabname
    where condition
    group by group_by_expression
    having group_condition
    order by column;
</pre>

#### 9.1.3 where和having的区别
- where子句过滤的是行
- having子句过滤的是分组
- where子句后面可以跟任意列名、单行函数、不能跟组函数
- having子句只能包含group by后面的表达式和组函数
- where子句执行在前，having子句执行在后
- where子句和having子句都不允许用别名

#### 9.1.4 数据分组小结
- 分组函数只能出现在选择列表、having、order by子句中
- 如果在select语句中同时包含有group by，having，order by那么他们的顺序是group by，having，order by
- 在选择列中如果有列、表达式、分组函数，那么这些列和表达式必须有一个出现在group by子句中，否则就会出错

### 9.2 子查询
#### 9.2.1 什么是子查询
子查询是指嵌入在其它sql语句中的select语句，也叫嵌套查询

#### 9.2.2 非关联子查询和关联子查询
<pre class="brush:sql">
sql>select colname1,...
    from tabname
    where expr operator
      (select colname2
       from subtabname);
</pre>

<pre class="brush:sql">
sql>select column1,column2,...
    from table1 o
    where column1 operator
      (select column1,column2
       from table2 i
       where i.expr1 = o.expr2)
    ;
</pre>

#### 9.2.3 非关联子查询的执行过程
<li>先执行子查询，子查询的返回结果作为主查询的条件，再执行主查询</li>
<li>子查询只执行一遍</li>
- 若子查询的返回结果为多个值，oracle会去掉重复值之后，再将结果返回给主查询

#### 9.2.4 关联子查询的执行过程
关联子查询采用的是循环的方式，执行步骤如下：

1. 外部查询得到一条记录(查询先从outer表中读取数据)并将其传入到内部查询。
2. 内部查询基于传入的值执行。
3. 内部查询从其结果中把值传回到外部查询，外部查询使用这些值来完成其他处理，若符合条件，outer表中得到的那条记录就放入结果集中，否则放弃，该记录不符合条件。
4. 重复执行步骤1-3，直到把outer表中的所有记录判断一遍。

#### 9.2.3 单行子查询
单行子查询指只返回一行数据的子查询语句
  
#### 9.2.4 多行子查询
概述：多行子查询指返回多行数据的子查询
#####在多行子查询中使用`all`操作符
<pre class="brush:sql">
  sql>select ename,sal,deptno
      from emp
      where sal > all 
        (select sal 
         from emp
         where deptno=30)
      ;
</pre>
#####在多行子查询中使用`any`操作符
<pre class="brush:sql">
  sql>select ename,sal,deptno
      from emp
      where sal > any 
        (select sal 
         from emp
         where deptno=30)
      ;
</pre>

#####在多行子查询中使用`exists`操作符
<pre class="brush:sql">
  sql>select a.real_name 
      from account a 
      where exists
        (select 1 from service
         where a.id = account_id);
</pre>
#####在多行子查询中使用`not exists`操作符
<pre class="brush:sql">
  sql>select a.real_name 
      from account a 
      where not exists
        (select 1 from service
         where a.id = account_id);
</pre>

####`in`和`exists`的区别
- EXISTS是用循环的方式，由outer表的记录数决定循环的次数，对于EXISTS影响最大，所以，外表的记录数少时，EXISTS的效率高
- IN先执行子查询，子查询的返回结果去重之后，再执行主查询，所以子查询的返回结果少时，IN的效率

#### 9.2.7 多列子查询
单行子查询是指子查询只返回单列、单行数据，多行子查询是指返回单列多行数据，都是针对单行而言的，而多行子查询则是指查询返回多个列数据的子查询语句
<pre class="brush:sql">
  sql>select * from emp
      where (deptno,job) = 
        (select deptno,job
         from emp
         where ename='smith')
      ;
</pre>

#### 9.2.8 子查询与null值
若子查询的返回结果中包含空值(null)，并且运算符为`not in`，那么整个查询不会返回任何行。任何值跟null比(包含null本身)，结果都不为true。

#### 9.2.9 在from子句中使用子查询
例子：
<pre class="brush:sql">
  sql>select e.deptno,e.ename,e.sal
      from emp e,
          (select deptno,avg(sal) avgsal
           from emp
           group by deptno) s
      where e.sal > s.avgsal
      and e.deptno = s.deptno
      ;
</pre>
说明：在form子句中使用子查询时，该子查询会被作为一个视图来对待，因此叫作内嵌视图，当在form子句中使用子查询时，必须给子查询指定别名。

### 9.3 多表查询
根据结果集生成的规则不同，多表连接可以分为三种：
> __约定__  
>`驱动表`  
>`匹配表`

#### 9.3.1 交叉连接（cross join）
笛卡尔积形式  
规定：多表查询的条件是 至少不能少于表的个数减一
用法：
<pre class="brush:sql">
sql>select tabname1.colname1, tablname2.colname2
    from tabname1 cross join tabname2;
</pre>
例子：
<pre class="brush:sql">
sql>select * from account a cross join service s;
</pre>
此查询的结果集为account和service表中所有记录的组合
  
#### 9.3.2 内连接（inner join）
核心：匹配  
用法：
<pre class="brush:sql">
sql>select tabname1.colname1,
           tabname2.colname2
    from tabname1 [INNER] JOIN tabname2
    ON tablename1.colname1 = tabname2.colname2
    AND tabname1.colname2 = val1;
</pre>
例子：
<pre class="brush:sql">
  sql>select * from account a inner join service s
      on a.id = s.account_id 
      and a.real_name='huangr';
</pre>
#####原理
... ...

#####结果集
内连结果集为所有参与内连表的列的和

#####执行顺序
先根据ON和AND条件对要连接的表进行过滤，将过滤后的结果集进行内连接操作，再根据select语句的定义生成最终结果集。

#####经典案例：
<pre class="brush:sql">
sql>select t1.real_name client,
           decode(t1.id,t2.id,'no recommender',
                              t2.real_name) recommender
    from account t1 join account t2
    where nvl(t1.recommender_id,t1.id) = t2.id
</pre>

#### 9.3.3 外连接（outer join）(3种)
核心：一个都不能少
#####left join
<pre class="brush:sql">
  from t1 left outer join t2
  on t1.c1 = t2.c2
</pre>
说明：

1. t1 是驱动表，t2 是匹配表
2. 将驱动表里的数据拿到匹配表中匹配，如果匹配，则将驱动表中的数据和匹配表中的数据合并后加入到结果集中；如果没有匹配则将驱动表中的数据和匹配表中的空记录合并后加入到结果集中。

######与内连接的区别
  1. 驱动表是固定的
  2. 驱动表的所有记录都在结果集中
  
######如何确定哪张表做驱动表
  那张表的数据要全部出来的表做驱动表
  
######外连接的执行顺序
<pre class="brush:sql">
  from t1 left join t2
  on t1.c1 = t2.c2
    and t2.c3 = ''
</pre>
说明：在外连接之前对匹配表做过滤
        from t1 t2 --> 对t2过滤 --> 外连接
<pre class="brush:sql">
  from t1 left join t2
    on t1.c1 = t2.c2
  where t2.c1 is null
</pre>
说明：在外连接之后对结果集做过滤
      from t1 t2 --> 外连接 -->where 通过t2的列对外连接的结果过滤
<pre class="brush:sql">
  from t1 left join t2
  on t1.c1 = t2.c2
    and t2.c3 = ''
  where t2.c1 is null
</pre>
  说明：from t1 t2 --> 对t2过滤 --> 外连接 -->where 通过t2的列对外连接的结果过滤

  规则：对驱动表的必须用where子句
        不能用内连接的否定形式解决否定问题
#####什么时候用外连接？
内连接解决匹配问题,外连接解决：

1. 匹配问题 + 不匹配问题(一个都不能少)
2. 不匹配问题(outer join + where 匹配表.非空列 is null)，类似于 not in ，not exists

#####3表连接
<pre class="brush:sql">
  from t1 left join t2
  on t1.c1 = t2.c2
    and t2.c3 = ''
  left join t3
  on t3.c1 = t1.c1
  where t2.c1 is null
</pre>

####right join
<pre class="brush:sql">
  from t1 right outer join t2
  on t1.c1 = t2.c2
</pre>
说明：t2 是驱动表，t1 是匹配表
####full join
<pre class="brush:sql">
  from t1 full outer join t2
  on t1.c1 = t2.c2
</pre>

#### 9.3.4 join部分小结
#####各类连接的应用场合
######交叉连接（cross join）
  笛卡尔积形式，很少用
######内连接（inner join）
  解决匹配问题
######外连接（outer join）
  解决不匹配问题  
  表的所有记录出现在结果集

### 9.4 合并查询
说明：有时在实际应用中，为了合并多个select语句的结果，可以使用集合操作符号 union，union all，intersect，minus，使用这个语法进行查询的效率比在查询语句的where子句中使用and、or的效率要高，在大数据量查找时会用到。  
合并查询的前提条件：要合并的结果集必须是同构的，即要合并的结果集的结构要一致。  
注意：相当于进行了多次查询，提高了数据库的负担，查询效率也不高
#### 9.4.1 union
概述：该操作符用于取得两个结果集的并集。当使用该操作符时，会自动去掉结果集中重复行。
<pre class="brush:sql">
sql>select ename,sal,job from emp where sal>2500
    union
    select ename,sal,job from emp where job='manager';
</pre>
说明：union连接的2个select语句后，相当于2个select语句“或”的关系
#### 9.4.2 union all
概述：该操作符的用法与union一样，功能也相似，但他不会取消重复行，而且不会排序。取得多个结果集的“交集”。
#### 9.4.3 intersect
概述：该操作符的用法与union一样，功能是取得两个结果集的交集，不包含重复记录。
<pre class="brush:sql">
sql>select enmae,sal,job from emp where sal>2500
    intersect 
    select ename,sal,job from emp where job='manager';
</pre>
#### 9.4.4 minus
概述：该操作符的用法与union一样，功能是取得两个结果集的差集，他只会显示存在第一个集合中，而不存在第二个集合中的数据。
<pre class="brush:sql">
sql>select ename.sal,job from emp where sal>2500
    minus
    select ename,sal,job from emp where job='manager';
</pre>
### 9.5 分页查询
####rownum
说明：rownum是一个伪列，对查询返回的行号编号即行号，由1开始依次递增
<pre class="brush:sql">
sql>select *
    from (select a1.*,rownum rn
          from (select * from emp) a1
                where rownum<=10)
    where rn>=6
    ;
</pre>
说明：  

1. 指定查询列，只需修改最里层的子查询
2. 排序也只要修改最里层的子查询

## 10. oracle表查询中的函数
### 10.1 字符函数
字符函数是oracle中最常用的函数

- lower(char):将字符串转化为小写
- upper(char):将字符串转化为大写
- length(char):返回字符串的长度
- substr(char,m,n):取字符串的子串

>    char -- 被截取的字符串  
     m -- 从哪个位置开始
>    n -- 截取几个字符串

例子：首字母大写，后面的字母小写
<pre class="brush:sql">
sql>select 
      upper(substr(ename,1,1))||lower(substr(ename,2,length(ename)-1))
    from emp;
</pre>

- replace(char1,search_string,replace_string):替换字符串

>char1 -- 原字符串  
 search_string -- 要被替换的字符串  
>replace_string -- 替换为的字符串
- instr(char1,char2,[,n[,m]]):取子串在字符串的位置
- lpad(str,padded_length,[pad_string]):左填充  
>str -- 需要填充的字符串  
>padded_length-- 返回的字符串的数量，如果这个数量比原字符串的长度要短，lpad函数将会把字符串截取成padded_length  
>pad_string -- 可选参数，这个字符串是要粘贴到str的左边，如果这个参数未写，lpad函数将会在str的左边粘贴空格。

- rpad(str,padded_length,[pad_string]):右填充，用法与lpad相同

### 10.2 数学函数
数学函数的输入参数和返回值的数据类型都是数字类型的。数学函数常用的有：

- round(n,[m]):四舍五入，如果省掉m，则四舍五入到整数；如果m是正数，则四舍五入到小数点的m位后。如果m是负数，则四舍五入到小数点的m位前。
<pre class="brush:sql">
sql>select round(sal,1)
    from emp;
</pre>
- trunc(n,[m]):截取数字。如果省掉m，就截去小数部分，如果m是正数就截取到小数点的m位后，如果m是负数，则截取到小数点的前m位
<pre class="brush:sql">
sql>select trunc(sal,1)
    from emp;
</pre>
- mod(m,n):取模，返回 m%n 的值
<pre class="brush:sql">
sql>select mod(10,2)
    from dual;
</pre>
- floor(n):向上取整，返回小于或是等于n的最大 **整数**
- ceil(n):向下取整，返回大于或是等于n的最小 __整数__

### 10.3 日期函数
>__日期类型之间可以进行运算__
>
>- 对日期加减一个数字，返回值为一个日期
>- 两个日期相减表示两个日期相差多少天
>
>例如：
><pre class="brush:sql">
>sql>select ename,trunc(sysdate-hiredate) >ts
>   from emp
></pre>
>    

日期函数用于处理date类型的数据。
默认情况下日期格式是dd-mon-yy 即12-7月-78

- sysdate：该函数返回系统日期
<pre class="brush:sql">
sql>select sysdate
    from dual;
</pre>
- months_between(date1,date2):两个日期之间相差多少个月
- add months(d,n)：返回原日期增加n个月之后的日期  
    `d` -- 原日期  
    `n` -- 增加的月份数
<pre class="brush:sql">
sql>select * from emp
    where sysdate>add_months(hiredate,8);
</pre>
- next_day(date,arg):根据参数出现下一个的日期
<pre class="brush:sql">
sql>select next_day('01-SEP-95','FRIDAY')
    from dual;
--将返回 08-SEP-95
</pre>
- last_day(d):返回指定日期所在月份的最后一天
<pre class="brush:sql">
sql>select ename
    from emp
    where (last_day(hiredate)-hiredate)=2
</pre>

### 10.4 转换函数
转换函数用于将数据类型从一种转为另外一种。

>__隐式转换__：在某些情况下，oracle server允许值的数据类型和实际的不一样，这时oracle server会隐含的转化数据类型  
>例如：
><pre class="brush:sql">
>sql>create table t1(id int);
>sql>insert into t1 values('10')
>--oralce会自动的将'10'-->10
></pre>

- to_char(date/number,format)

>date/number -- 要转换的内容  
>format -- 格式字符串，可以由以下内容组成：
>1. 适合日期  
>`yy`:两位数字的年份 2012->02  
>`yyyy`:四位数字的年份 2012  
>`mm`:两位数字的月份 8月->08  
>`dd`:2位数字的天 30号->30  
>`hh24`: 8点 -> 20  
>`hh12`: 8点 -> 08  
>`mi`、`ss` --> 显示分钟、秒
>2. 适合数字  
>`9`: 显示数字，并忽略前面的0  
>`0`: 显示数字，如位数不足，则用0补齐  
>`.`: 在指定位置显示小数点  
>`,`: 在指定位置显示逗号  
>`$`: 在数字前加美元  
>`L`: 在数字前加本地货币符号(L->local)，与语言环境有关  
>`C`: 在数字前加国际货币符号  
>`G`: 在指定位置显示组分隔符  
>`D`: 在指定位置显示小数点符号(.)  
例如：
<pre class="brush:sql">
sql>select ename,to_char(hiredate,'yyyy-mm-dd hh24:mi:ss)，to_char(sal,'L99999,99')
    from emp;
sql>select * from emp
    where to_char(hiredate,'yyyy')=1980;
</pre>

- to_date(date,format)  
用于将字符串转化成date类型的数据。format的格式说明参看`to_char`函数

### 10.5 系统函数
- sys_context('userenv',arg)

<pre>
    'userenv' - 固定值
    arg - 想要查询的环境属性参数    
    arg参数选项：  
    terminal: 当前会话客户所对应的终端的标识符  
    lanuage: 语言  
    db_name: 当前数据库名称  
    nls_date_format: 当前会话客户所对应的日期格式
    session_user: 当前会话客户所对应的数据库用户名
    current_schema: 当前会话客户所对应的默认方案名  
    host: 返回数据库所在主机的名称
</pre>
例如：
<pre class="brush:sql">
sql>select sys_context('userenv','db_name')
    from dual;
</pre>

###分支函数
- decode：分支选择

###组函数
- avg	平均数
- sum 	求和
- count 计数
- max	最大值
- min	最小值

> 注意:  
    1. 组函数默认处理的是所有的非空值  
    2. 对所有的非空值的处理，返回值不为null  
>   3. 处理的值都为null，count返回0，其他的返回null  


## 11. SQL顺序
### 11.1 SQL语法顺序
select from where group by having order by
### 11.2 SQL执行顺序
from where group by having select order by

## 12. 事务处理
### 12.1 什么是事务
概述：事务用于保证数据的一致性，它由一组相关的dml(数据操作语言，对表进行增删改)语句组成，该组的dml语句要么全部成功，要么全部失败。  

事务由一组DML语句和commit/rollback组成，是改变数据库的最小逻辑单元。如果是commit，表示数据入库；如果是rollback，表示取消所有的DML操作  

事务的开始：

- 上一个事务的结束就是下一个事务的开始

事务的结束：

- commit/rollback
- DDL语句自动提交(commit)
  数据库中有很多个session，每个session只有一个活动事务(未commit)，
  
### 12.2 事务的隔离级别
  数据库应用程序中最常用的隔离级别 -- read committed
  read committed：一个事务只可以读取在事务开始之前提交的数据和本事务正在修改的数据
  
### 12.3 事务的特性(ACID)
- 原子性(atomicity)  
    一个事务或者完全发生或者完全不发生
- 一致性(consistency)  
    事务把数据库从一个一致状态转变到另一个状态
- 隔离性(isolation)  
    在事务提交之前，其他事务觉察不到事务的影响
- 持久性(durability)  
    一旦事务提交，它是永久的

### 12.4 数据库开发的关键挑战
  在开发多用户、数据库驱动的应用程序中，关键性的挑战之一是要使并行的访问量达到最大化，同时还要保证每一个用户(会话)可以以一致的方式读取并修改数据。
  
- 锁(lock)机制
   用来管理对一个共享资源的并行访问
- 多版本一致读
- 非阻塞查询：写不阻塞读，读不阻塞写
- 一致读查询：在某一时刻查询产生一致结果

### 12.5 锁的基本概念
- 排他锁(X锁)  
    如果一个对象上加了X锁，在这个锁被采用后，直到commit或rollback释放它之前，该对象上不能施加任何其他类型的锁
- 共享锁(S锁)
    如果一个对象被加上了S锁，该对象上可以加其他类型的S锁，但是，在该锁释放之前，该对象不能被任何其他类型的X锁
- DML锁
    用于保护数据的完整性
    - TX锁，即事务锁（行级锁），类型为X锁
    - TM锁，即意向锁（表级锁），属于一种S锁
- DDL锁
    用于保护数据库对象的结构(例如表、索引的结构定义)
    - X类型的DDL锁，这些锁定防止其他会话自己获得DDL锁定或TM(DML)锁定。这意味着可以在DDL期间查询一个表，但不可以以任何方式进行修改

### 12.6 事务不提交的后果
- 其他事务看不见它的操作结果
- 表和行上加的锁不释放，会阻塞其他事务的操作
- 它所操作的数据可以恢复到之前的状态
- 占用的回滚段资源不释放

### 12.7 回滚事务
- 数据的改变就像从未发生过一样
- 插入的数据没有了，更新和删除的数据都恢复出来
- 锁被释放

### 12.8 保留点(savepoint)
- 用savepoint在当前事务里创建一个保留点
- 用rollback to savepoint命令将事务回滚到标记点
<pre class="brush:sql">
  sql>commit
  sql>insert ...
  sql>update ...
  sql>SAVEPOINT update_done
  Savepoint created.
  sql>insert ...
  sql>ROLLBACK TO update_done
  Rollback complete.
</pre>

### 12.9 事务的几个重要操作
####`savepoint a`设置保存点
####`rollback to a`取消部分事务
####`rollback`取消全部事务

## 13. 数据库对象操作
### 13.1 view
#### 13.1.1 什么是view
- 视图在数据库中不存储数据值，即不占空间
- 只在系统表中存储对视图的定义
- 视图实际就是select语句
- 类似windws中的快捷方式

#### 13.1.2 视图的应用
- 简化操作，屏蔽了复杂的sql语句，直接对视图操作
- 控制权限，只允许查询一张表中的部分数据。解决办法：对其创建视图，授予用户读视图的权限，而非读表的权限。
- 通过视图将多张表union all成一张逻辑表，作为单独一个数据库对象，实现表的超集

#### 13.1.3 视图的分类
- 简单视图  
基于单张表并且不包含函数或表达式的视图，在该视图上可以执行DML语句(即可执行增、删、改操作)
<li>复杂视图</li>  
包含函数、表达式或者分组数据的视图，在该视图上执行DML语句时必须要符合特定条件。  
在定义复杂视图时必须为函数或表达式定义别名。
- 连接视图  
基于多个表建立的视图，一般来说不会在该视图上执行insert、update、delet操作。

#### 13.1.4 视图的维护
<pre class="brush:sql">
--创建视图
sql>create or replace view view_name;

--删除视图
sql>drop view view_name;

--编译视图
sql>alter view view_name compile;

--查询 view_name 的状态，invalid/valid
sql>select object_name,object_type,status 
    from user_objects
    where object_name='view_name';
</pre>

#### 13.1.5 视图中的with check option约束
说明：  
通过视图进行的修改，必须也能通过该视图看到修改后的结果，即在对视图进行`insert`、`update`时，会对修改进行检查，已保证修改后，通过视图可看到修改后的结果。相当于增加了一个约束，对视图的where条件进行检查，该约束的constraint_type为ｖ。
用法：
<pre class="brush:sql">
sql>create or replace view view_name
    as
    select * from test
    where c1 = 1 
    with check option;
</pre>

#### 13.1.6 视图中的with read only约束
说明：  
声明本视图为只读视图，只是查看，不做更新，Oracle对只读视图不会加锁，视图的查询速度更快。只读视图的约束类型constraint_type为o。
用法：
<pre class="brush:sql">
sql>create or replace view view_name
    as
    select * from test
    where c1 = 1 
    with read only;
</pre>

### 13.2 index
#### 13.2.1 创建index
用法：
<pre class="brush:sql">
sql>create index index_name
    on table_name (colname);
    -- 要求 colname 列的取值必须唯一。
</pre>
例子：
<pre class="brush:sql">
sql>create index service_account_id_idx
    on service (account_id);
</pre>

#### 13.2.2 为什么要使用索引
- Oracle server通过roid快速定位要找的行
- 通过rowid定位数据能有效降低读取数据块的数量
- 索引的使用和维护是自动的，一般情况不需要用户干预

#### 13.2.3 哪些列适合建索引
- 经常出现在where子句的列
- 经常用于表连接的列
- 该列是高基数数据列(高基数数据列是指有很多不同的值)
- 该列包含许多`null`值
- 表很大，查询的结果集小
- 主键(pk)列、唯一键(uk)列
- 外键(fk)列
- 经常需要排序(order by)和分组(group by)的列
- 索引不是万能的

#### 13.2.3 索引的类型
- 唯一性索引(unique)  
  等价于唯一性约束
- 非唯一性索引  
  用于提高查询效率
- 单列索引  
  索引建在一列上
- 联合索引  
  索引建在多列上

#### 13.2.4 哪些写法会导致索引用不了
- 函数导致索引用不了  
  `where upper(colname) = 'carmen'`
- 表达式导致索引用不了  
  `where colname*12 = 12000`
- 部分隐式数据类型转换导致索引用不了  
  `where c1 = 2`(c1为varchar2类型)
- `like`和`substr`  
  `where colname like 'CA%'`  
  `where substr(colname,1,2) = 'CA'`
- 查询所有的null值  
  `where colname is null`
- 否定形式  
  `not in`  
  `<>`

### 13.3 序列号`sequence`
#### 13.3.1 什么是`sequence`
- Oracle提供的数据库对象
- 为了解决主键值和唯一键值的唯一性
- 按照预定义的模式自动生成整数的一种机制，保证数字的自动增长

#### 13.3.2 创建`Sequence`
<pre class="brush:sql">
sql>create sequence seq_name
    [increment by 1|integer]
    [start with integer]
    [maxvalue integer|nomaxvalue]
    [minvalue integer|nominvalue]
    [cycle|nocycle]
    [cache 20|integer|no cache]
    ;
</pre>

## 14. Java操作Oracle
###14.1 桥接 JdbcOdbc
  驱动：sun.jdbc.odbc.JdbcOdbcDriver

###14.2 JDBC连接
驱动：oracle.jdbc.driver.OracleDriver  

要使用连接字符串：jdbc:oracle:thin:@127.0.0.1:1521:orcl

####java中使用事务
<pre class="brush:java">
//加入事务处理
con.setAutoCommit(false);
//提交事务
conn.commit();
//回滚事务
conn.rollback(); 
</pre>

## 15. 奇技淫巧
### 15.1 使用子查询插入数据
概述：当使用values子句时，一次只能插入一行数据，当使用子查询插入数据时，一条insert语句可以插入大量的数据。当处理行迁移或者装载外部表的数据到数据库时，可以使用子查询来插入数据。

用法：
<pre class="brush:sql">
sql>insert into kkk(id,name,deptno)
    select empno,ename,deptno
    from emp
    where deptno = 10
    ;
</pre>

### 15.2 使用子查询更新数据
概述：使用update语句更新数据时，既可以使用表达式或者数值直接修改数据，也可以使用子查询修改数据。

用法：
<pre class="brush:sql">
sql>update emp set （job,sal,comm）=
    (select job,sal,comm from emp where ename='smith') 
     where ename='scott'
</pre>

### 15.3 用查询结果创建新表
<pre class="brush:sql">
sql>create table mytable(empno,ename,sal,job,deptno)
    as select empno,ename,sal,job,deptno from emp where 1 = 1;
</pre>
说明：
- 创建表结构并将被查询表中的数据导入到新建的表中
- `where 1 = 1` 恒等式，表示将`emp`表中的所有数据都插入到`mytable`中
- `where 1 = 2`恒不等式，表示不将`emp`表中的数据插入到`mytable`中
- 表结构由子查询的selet语句决定，create table指定的列的数量要跟select语句指定的列的数量一致
- create table定义列只能定义列名、缺省值、完整性约束。非空约束不需要定义可以直接复制过来。

### 15.4 使用dual表进行函数测
<pre class="brush:sql">
sql>select to_number('13.3') + 2
    from dual;
</pre>

### 15.5 使用数据字典查找表约束
<pre class="brush:sql">
sql>select constraint_name,constraint_type,search_condition
from user_constraints;
</pre>
>where table_name='test'；  
>constraint_name 约束名  
>constraint_type 约束类型 p u r c
<pre class="brush:sql">
sql>select constraint_name,column_name
from user_cons_columns
where table_name = 'test';
</pre>

### 15.6 使用数据字典查找表名
<pre class="brush:sql">
sql>select table_name 
from user_tables
where table_name like '%7'; 
</pre>

