# MySQL相关

## MySQL服务启动和停止

方式一：计算机——右击管理——服务
方式二：通过管理员身份运行
	net start 服务名（启动服务）
	net stop 服务名（停止服务）

## MySQL服务登陆和退出

方式一：通过mysql自带的客户端，只限于root用户；

方式二：通过windows自带的客户端
登录：
	mysql 【-h主机名 -P端口号 】-u用户名 -p密码

退出：
	exit或ctrl+C

## 查看服务器版本

方式一：登录到mysql服务端
	select version();
方式二：没有登录到mysql服务端
	mysql --version
或
	mysql --V

# SQL

​	SQL (Structured Query Language) 是具有数据操纵和数据定义等多种功能的数据库语言。

## SQL语法规范

1. SQL对大小写不敏感；
2. 每条命令用分号结尾；
3. 每条命令根据需要，可以进行缩进或换行
4. 注释：
   1. 单行注释：#注释文字
   2. 单行注释：-- 注释文字
   3. 多行注释：/* 注释文字  */

## SQL语言分类

​	DQL （Data Query Language）：数据查询语言

​			select

​	DML（Data Manipulate Language）：数据操作语言

​			insert	update	delete

​	DDL（Data Define Language）：数据定义语言

​			creat	drop	alter

​	TCL（Transaction Control Language）：事务控制语言

​			commit	rollback

## DDL语句

### 	库和表的管理

```sql
-- 创建库
create database 库名;

-- 删除库
drop database 库名；

-- 创建表
create table if not exists 库名(
	列名 数据类型 约束，
	列名 数据类型 约束，
    ...
	列名 数据类型 约束
);

-- 删除表
drop table [if exists] 表名；

-- 获取数据表结构
desc 表名;

/*
修改表 - alter
ADD|MODIFY|DROP|CHANGE COLUMN 字段名 [字段类型];
first 把该列放置在第一列
*/
-- 修改表名   
alter table 表名 rename [to] 新表名；

-- 修改表中的列
alter table 表名 [column] change 列名 新列名 数据类型;

-- 修改列的类型
alter table 表名 [column] modify 列名 数据类型；

-- 增加列
alter table 表名 add [column] 列名 数据类型；

-- 删除列
alter table 表明 drop column 列名;

-- 删除有主键的列
alter table 表名 drop column 列名;

-- 创建一个和该表结构相同的新表
create table 新表名 like 表名;

-- 创建一个和该表内容相同的新表(不赋值约束)
create table 新表名 as (select * from 表名)；

```

### 常见约束

```sql
not null 
default
unique
check
primary key
foreing key
```

## DML 语言

### 插入

```sql
/*
插入
*/
insert into 表名
	(字段名，...)
values
	(值1，...);

-- 插入
insert into 表名 (字段名1, 字段名2, ..., 字段名n) values (值1, 值2, ..., 值n);

-- 一次插入多条数据
insert into 表名 (字段名1, 字段名2, ..., 字段名n) values (值1, 值2, ..., 值n), (值1, 值2, ..., 值n), (值1, 值2, ..., 值n);

-- 字段名可以省略不写，但是写值的时候要按照表中的字段顺序
insert into 表名 values (值1, 值2, ..., 值n), (值1, 值2, ..., 值n), (值1, 值2, ..., 值n);

-- 将查询出来的数据插入表中
insert into 表名1 (字段名1, 字段名2, ..., 字段名n) select 字段名1, 字段名2, ..., 字段名n from 表名2;

-- 完全复制
insert into 表名1 select * from 表名2;

```

### 修改

```sql
/*
修改
*/
update 表名 set 字段=新值,字段=新值 [where 条件];

-- 修改多表
update 表1 别名1, 表2 别名2 set 字段=新值，字段=新值 where 连接条件 and 筛选条件;
```

### 删除

```sql
/*
删除
方式一：delete
方式二：truncate
*/

-- 单表删除
delete from 表名 [where 删选条件]

-- 多表删除
delete 别名1，别名2 from 表1 别名1，表2 别名2 where 连接条件 and 筛选条件;

-- truncate语句
truncate table 表名

/*
两种方式区别
#1. truncate不能加where条件，而delete可以加where条件；
#2. truncate的效率高一丢丢；
#3. truncate 删除带自增长的列的表后，如果再插入数据，数据从1开始；
#4. delete 删除带自增长列的表后，如果再插入数据，数据从上一次的断点处开始；
#5. truncate删除不能回滚，delete删除可以回滚
*/

```

## DQL

### 基础查询

```sql
-- 语法
select 要查询的东西 [from 表名];
# 查询的东西可以是：表中的字段，常量值，表达式，函数

/*
特点：
1. 通过select查询完的结果 ，是一个虚拟的表格，不是真实存在;
2. 要查询的东西 可以是常量值、可以是表达式、可以是字段、可以是函数;
*/
```

### 条件查询

​	根据条件过滤原始表的数据，查询到想要的数据。

```sql
-- 语法
	select 要查询的字段|表达式|常量值|函数 from 表名 where 条件;

/*
条件运算符
	> < >= <= = != <>

逻辑运算符
	and（&&）: 两个条件如果同时成立，结果为true，否则为false
	or(||) ：两个条件只要有一个成立，结果为true，否则为false
	not(!) ：如果条件成立，则not后为false，否则为true
*/

-- 模糊查询
-- 查询 name中第二个字符是”亮”的人的信息
select * from user where name like '_亮%';
```

### 排序查询

```sql
-- 语法
select 要查询的东西 from 表名 where 条件 order by 排序的字段|表达式|函数|别名 [asc|desc];
```





### 常见函数