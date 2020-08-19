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
create table if not exists 表名(
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
alter table 表名 drop column 列名;

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

1. 概述
   功能：类似于java中的方法；
   好处：提高重用性和隐藏实现细节；
   调用：select 函数名(实参列表)；

2. 单行函数
   2.1 字符函数

   | 函数    | 作用         |
   | ------- | ------------ |
   | concat  | 连接         |
   | substr  | 截取字符串   |
   | upper   | 变大写       |
   | lower   | 变小写       |
   | replace | 替换         |
   | length  | 获取字节长度 |
   | trim    | 去除前后空格 |
   | instr    | 获取子串第一次出现的索引       |
   
   2.2 数学函数

   | 函数    | 作用         |
   | ------- | ------------ |
   | ceil  | 向上取整         |
   | round  | 四舍五入   |
   | mod   | 取模       |
   | floor   | 向下取整       |
   | truncate | 截断         |
   | rand  | 获取随机数 |
   
   2.3 日期函数

   | 函数    | 作用         |
   | ------- | ------------ |
   | now  | 返回当前日期 + 时间         |
   | year  | 返回年   |
   | month   | 返回月       |
   | day   | 返回日       |
   | date_formate | 将日期转换成字符         |
   | curdate  | 返回当前日期 |
   | hour    | 小时 |
   | minute    | 分钟      |
   | second    | 秒      |
   | datediff    | 返回两个日期相差天数      |
   | monthname    | 以英文形式返回月      |
   
   2.4 其他函数

   | 函数    | 作用         |
   | ------- | ------------ |
   | version  | 当前数据库服务器的版本        |
   | database  | 当前打开的数据库   |
   | user   | 当前用户       |
   | password('字符')  | 返回该字符的密码形式       |
   | md5('字符') | 返回该字符的MD5加密形式         |

   2.5 流程控制函数
   
   ①	if (条件表达式，表达式1，表达式2)：如果条件表达式成立，返回表达式1，否则返回表达式2
   ②	case情况1
   		case 变量或表达式或字段
   		when 常量1 then 值1
   		when 常量2 then 值2
   		...
   		else 值n
   		end
   
   例子：
   
   ```sql
   SELECT
   	NAME '英雄',
   	CASE NAME
   		WHEN '德莱文' THEN
   			'斧子'
   		WHEN '德玛西亚-盖伦' THEN
   			'大宝剑'
   		WHEN '暗夜猎手-VN' THEN
   			'弩'
   		ELSE
   			'无'
   	END [AS] '装备'
   FROM
   	user_info;
   ```
   
   ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWctYmxvZy5jc2RuLm5ldC8yMDE4MDMxOTE0MjgyMDM2NT93YXRlcm1hcmsvMi90ZXh0L0x5OWliRzluTG1OelpHNHVibVYwTDNGeFh6TXdNRE00TVRFeC9mb250LzVhNkw1TDJUL2ZvbnRzaXplLzQwMC9maWxsL0kwSkJRa0ZDTUE9PS9kaXNzb2x2ZS83MA?x-oss-process=image/format,png)
   
   ③	case情况2
   		case 
   		when 条件1 then 值1
   		when 条件2 then 值2
   		...
   		else 值n
   		end
   
   例子
   
   ```sql
   SELECT
   	NAME '英雄',
   	age '年龄',
   	CASE
   		WHEN age < 18 THEN
   			'少年'
   		WHEN age < 30 THEN
   			'青年'
   		WHEN age >= 30 AND age < 50 THEN
   			'中年'
   		ELSE
   			'老年'
   	END [AS] '状态'
   FROM
   	user_info;
   ```
   
   ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWctYmxvZy5jc2RuLm5ldC8yMDE4MDMxOTE0MjkxMDkzP3dhdGVybWFyay8yL3RleHQvTHk5aWJHOW5MbU56Wkc0dWJtVjBMM0Z4WHpNd01ETTRNVEV4L2ZvbnQvNWE2TDVMMlQvZm9udHNpemUvNDAwL2ZpbGwvSTBKQlFrRkNNQT09L2Rpc3NvbHZlLzcw?x-oss-process=image/format,png)

3. 分组函数
   | 函数    | 作用         |
   | ------- | ------------ |
   | max  | 最大值        |
   | min  | 最小值   |
   | sum   | 和       |
   | avg  | 平均值       |
   | count | 计算个数        |
   
   特点
   
   ①	语法
   	select max(字段) from 表名;
   
   ②	支持的类型
   	sum 和 avg 一般用于处理数值型
   	max、min、count可以处理任何数据类型
   
   ③	以上分组函数都忽略null
   ④	都可以搭配distinct使用，实现去重的统计
   	select sum(distinct 字段) from 表;
   ⑤	count函数
   	count(字段)：统计该字段非空值的个数
   	count(*)：统计结果集的行数
   案例：查询每个部门的员工个数
   1 xx    10
   2 dd    20
   3 mm    20
   4 aa    40
   5 hh    40

count(1):统计结果集的行数

效率上：
MyISAM存储引擎，count(*) 最高
InnoDB存储引擎，count(*) 和 count(1) 效率 > count(字段)

⑥ 和分组函数一同查询的字段，要求是group by后出现的字段

### 分组查询

1. 语法

```sql
SELECT 分组函数， 分组后字段
FROM 表名
[WHERE 筛选条件]
GROUP BY 分组字段
[HAVING 分组后的删选]
[ORDER BY 排序列表]
```

2. 特点

| 关键字                | 筛选的表     | 位置         |
| --------------------- | ------------ | ------------ |
| WHERE（分组前筛选）   | 原始表       | GROUP BY前边 |
| HAVING （分组后筛选） | 分组后的结果 | GROUP BY后边 |

### 连接查询

sql99【推荐使用】
		内连接
			等值
			非等值
			自连接
		外连接
			左外
			右外
			全外（mysql不支持）
		交叉连接

1. **内连接**

```sql
SELECT 查询列名
FROM 表1 AS 别名
	INNER JOIN 表2 AS 别名
WHERE 筛选条件（筛选条件分为：等值非、非等值、自连接）
GROUP BY 分组列表
HAVING 分组后的筛选
ORDER BY 排序列表
LIMIT 子句;
```

​	特点：
​	①	表的顺序可以调换
​	②	内连接的结果=多表的交集
​	③	n表连接至少需要n-1个连接条件

2. **外连接**

   外连接又分为左外连接和又外连接；

```sql
SELECT 查询列名
FROM 表1 AS 别名
	LEFT(RIGHT) JOIN 表2 AS 别名
ON 连接条件（连接条件分为：等值非、非等值、自连接）
WHERE 筛选条件 （筛选条件分为：等值非、非等值、自连接）
GROUP BY 分组列表
HAVING 分组后的筛选
ORDER BY 排序列表
LIMIT 子句;
```

​	特点：
​	①	查询的结果 = 主表中所有的行，如果从表和它匹配的将显示匹配行，如果从表没有匹配的则显示null
​	②	left join 左边的就是主表，right join 右边的就是主表，full join 两边都是主表
​	③	一般用于查询除了交集部分的剩余的不匹配的行

3. **交叉连接**

```sql
SELECT 查询列名
FROM 表1 AS 别名
	CROSS JOIN 表2 AS 别名
WHERE 筛选条件（筛选条件分为：等值非、非等值、自连接）
GROUP BY 分组列表
HAVING 分组后的筛选
ORDER BY 排序列表
LIMIT 子句;
```

​	特点：
​	类似于笛卡尔乘积；

### 子查询

1. 含义
   嵌套在其他语句内部的 select 语句称为子查询或内查询；
   外面的语句可以是 insert、update、delete、selec t等，一般 select 作为外面语句较多；
   外面如果为 select 语句，则此语句称为外查询或主查询；

2. 分类
   2.1、按出现位置
   select 后面：
   		仅仅支持标量子查询
   from 后面：
   		表子查询
   where 或 having 后面：
   		标量子查询
   		列子查询
   		行子查询
   exists 后面：
   		标量子查询
   		列子查询
   		行子查询
   		表子查询

   2.2、按结果集的行列
   标量子查询（单行子查询）：结果集为一行一列
   列子查询（多行子查询）：结果集为多行一列
   行子查询：结果集为多行多列
   表子查询：结果集为多行多列
   ​	示例一（标量子查询）
   ​	where或having后面
   ​	案例：查询最低工资的员工姓名和工资
   ​	①	最低工资
   ​	select min(salary) from employees

   ​	②	查询员工的姓名和工资，要求工资 = ①
   ​	select last_name,salary
   ​	from employees
   ​	where salary=(
   ​		select min(salary) from employees
   ​	);

   ​	实例二（列子查询）
   ​	案例：查询所有是领导的员工姓名
   ​	①	查询所有员工的 manager_id
   ​	select manager_id
   ​	from employees

   ​	②	查询姓名，employee_id属于①列表的一个
   ​	select last_name
   ​	from employees
   ​	where employee_id in (
   ​		select manager_id
   ​		from employees
   ​	);

### 联合查询

1. 含义
   union：合并、联合，将多次查询结果合并成一个结果
2. 语法

```sql
查询语句1
union 【all】
查询语句2
union 【all】
...
```

3. 意义
   1、将一条比较复杂的查询语句拆分成多条语句
   2、适用于查询多个表的时候，查询的列基本是一致

4. 特点
   1、要求多条查询语句的查询列数必须一致
   2、要求多条查询语句的查询的各列类型、顺序最好一致
   3、union 去重，union all包含重复项

### 查询总结

```sql
-- 语法，其后的数组表示执行顺序
SELECT 查询列表    		-- 7
FROM 表1 别名      	 --1
	连接类型 join 表2   -- 2
ON 连接条件         	-- 3
WHERE 筛选条件          -- 4
GROUP BY 分组列表   	-- 5
HAVING 筛选         	-- 6
ORDER BY 排序列表    	-- 8
LIMIT 起始条目索引，条目数;  -- 9
```



