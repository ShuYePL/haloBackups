---
title: MySQL基本语句
date: 2023-03-17 09:55:20.937
updated: 2023-03-17 09:55:20.937
url: /archives/mysql-ji-ben-yu-ju
categories: 
- sql
tags: 
---

# 基本概念

**SQL** (Structured Query Language:结构化查询语言)，用于管理关系数据库管理系统（RDBMS），它是一种与数据库交互的语言，很多关系型数据库系统都支持SQL语言。（下面的内容都是是MySQL为例的）

# MySQL的启动和停止（Windows系统）

启动

~~~
net start MySQL80
~~~

停止

~~~
net stop MySQL80
~~~

stop 后面跟着的名字要去查一下自己电脑上的MySQL服务的名称是什么，要和这个服务的名字对应上。然后，在命令行输入这两条命令的时候是需要管理员权限的，如果没有开启管理员权限的话会报下面的错误信息

~~~
C:\Users\jh>net stop MySQL80
发生系统错误 5。

拒绝访问。
~~~

打开管理员权限的命令行操作：[(10条消息) 使用管理员权限打开cmd（命令提示符）的方法 （Windows10）_lgxo的博客-CSDN博客](https://blog.csdn.net/m0_58547974/article/details/119978383)

正常启动与停止服务

~~~
C:\WINDOWS\system32>net start MySQL80
MySQL80 服务正在启动 .
MySQL80 服务已经启动成功。

C:\WINDOWS\system32>net stop MySQL80
MySQL80 服务正在停止.
MySQL80 服务已成功停止。
~~~

# MySQL数据库的登录

~~~
mysql -uroot -p
~~~

输入上面的命令后回车，输入密码，再回车。

# MySQL语句分类

## DDL（Data Definition Languages 数据定义语言）

主要用在数据库对象的创建、删除、修改等操作，不涉及表内的数据操作

## DML（Data Manipulation Language 数据操作语句）

主要负责对表内数据的操作，基本的CRUD都在这里。

## DCL（Data Control Languege 数据控制语句）

对于访问权限的控制。

# DDL基本语句

## 数据库操作

### 创建数据库

~~~mysql
create database dbname
~~~

### 删除数据库

~~~mysql
drop database dbname
~~~

### 切换数据库

```mysql
use dbname
```

## 设置编码格式

```mysql
set names utf8;
```

## 表操作

### 创建表

~~~mysql
create table emp(ename varchar(10), hiredate date, sal decimal(10, 2), deptno int(2));
~~~

### 查看表的定义

~~~mysql
desc 表名;
~~~

### 查看建表语句

~~~mysql
show create table 表名 \G;
~~~

### 删除表

~~~mysql
drop table 表名;
~~~

### 修改表

#### 修改字段类型

~~~mysql
alter table 表名 modify 字段名 字段类型(字段长度);
~~~

#### 增加表字段

~~~mysql
alter table 表名 add column 字段名 字段类型(字段长度);
~~~

#### 删除字段

~~~mysql
alter table 表名 drop column 字段名;
~~~

#### 字段改名

~~~mysql
alter table 表名 change 旧字段名 新字段名 字段类型(字段长度);
~~~

#### 改变字段顺序

对于上面的操作，add默认将字段增加在现有字段的后面，chage和modify默认不会改变字段的位置，如果在上面的这些alter语句后面加上[first|after column_name]的话就可以改变字段的位置，first或者after后面的**字段名**如果没有指定的话，默认是放在整张表字段的最前方，或者整张表字段的最后面，例子：

~~~mysql
alter table 表名 add 新添加的字段 字段类型 after 添加在该字段的后面;
~~~

#### 修改表名

~~~mysql
alter table 表名 rename 新表名;
~~~

# DML基本语句

## 插入表数据

### INSERT INTO 语句

```mysql
INSERT INTO table_name
VALUES (value1,value2,value3,...);
或者
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);
```

## 修改表数据

### update语句

~~~mysql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
~~~

## 删除记录

### delete语句

~~~mysql
DELETE FROM table_name
WHERE condition;

删除所有数据，但保留表
DELETE FROM table_name;
~~~

## 查询表记录

### select语句

~~~mysql
SELECT column1, column2, ...
FROM table_name;
或者
SELECT * FROM table_name;
~~~

### distinct去重查询

~~~mysql
SELECT DISTINCT column1, column2, ...
FROM table_name;
~~~

distinct后面跟着的字段都会被作为筛选的条件

### order by查询结果排序

~~~mysql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
~~~

order by默认情况下是升序排序的，ASC指定为升序，DESC指定为降序，每个字段后面都可以指定降序排序还是升序排序，生效字段按照出现的顺序从左到右生效。

### where条件查询

```mysql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

#### 运算符使用

~~~mysql
select * from websites where country='CN' and alexa > 50

select * from websites where country='USA' or country='CN';
~~~

#### like字句

```mysql
SELECT column1, column2, ...
FROM table_name
WHERE column LIKE pattern;

SELECT * FROM Websites
WHERE name NOT LIKE '%oo%';
```

##### 通配符

|   通配符    |         代表含义         |
| :---------: | :----------------------: |
|      %      |      0个或多个字符       |
|      _      |         一个字符         |
| [字符列表]  |   字符列表中的一个字符   |
| [^字符列表] | 不在字符列表中的一个字符 |
| [!字符列表] | 不在字符列表中的一个字符 |

前两个通配符配合like和not like使用，后三个字符列表相关的通配符是正则表达式的。

#### in字句

```mysql
SELECT column1, column2, ...
FROM table_name
WHERE column IN (value1, value2, ...);
```

与in对应的还有一个not in的。

#### between字句

```mysql
SELECT column1, column2, ...
FROM table_name
WHERE column BETWEEN value1 AND value2;

SELECT * FROM Websites
WHERE alexa NOT BETWEEN 1 AND 20;
```

between可以操作数值、字符、日期。

### limit限制显示的数据条数

```mysql
SELECT column_name(s)
FROM table_name
LIMIT number;
```

还可以指定从哪一条记录开始显示，显示number条数据，起始下标从0开始

~~~mysql
select * from emp limit 起始下标,显示数据条数;
~~~

### 聚合操作

这里需要用到一些MySQL的函数了，这个算是属于综合查询了

~~~mysql
SELECT [fieldl,field2,....fieldn] fun_name 
FROM tablename
[WHERE where_contition] 
[GROUP BY field1,field2,....fieldn [WITH ROLLUP]]
[HAVING where_contition]
~~~

` with rollup `的作用是统计总数量。

### union all和union语句

将查询的结果进行合并，其中union all不会去重，而union会进行去重，使用方式就是左右两边两个select语句，中间一个union或者union all。

~~~mysql
(select...) union (select...)

(select...) union all (select...)
~~~

### 连接查询

连接分为内连接和外连接，区别是内连接匹配两张表都有的，而外连接能匹配表中不匹配的

##### 内连接

~~~mysql
select ename,deptname from emp, dept where emp.deptno = dept.deptno;
~~~

##### 外连接

```mysql
SELECT column1, column2, ...
FROM table1
JOIN table2 ON condition;
```

### 别名

可以为表名或者列名指定别名。

```mysql
SELECT column_name AS alias_name
FROM table_name;
```

# DCL语句

这一部分的内容主要是DBA在操作的，用于权限控制使用

## 授予权限

~~用grant一键创建用户并且授权的方式在8.0版本的MySQL不能使用了~~

8.0版本授予权限必须先创建用户、再授予权限、刷新权限。

~~~mysql
create user 'z1'@'localhost' identified by '123';
grant insert,select on test1.* to 'z1'@'localhost';
flush privileges;
~~~

## 回收权限

~~~mysql
revoke insert on test1.* from 'z1'@'localhost';
~~~

## 删除用户

~~~mysql
drop user 'z1'@'localhost';
~~~

# 帮助命令

查看可供查询的分类

~~~mysql
? contents
~~~

查看分类细节

~~~mysql
? 类别名称（上个指令中列出来的）
~~~

依次类推的，对于上一个指令列出来的内容，还可以继续?对详细信息进行查询

# 细碎的知识点

## 文本值

文本值（这里我觉得就是字符类型）使用 '' 或者 “” 包围起来表示，如果是数字，可以直接使用，不需要使用单引号或者双引号包围起来，SQL对于大小写不敏感不只是对于关键字，而且对于文本值来说也是大小写不敏感的。对于查询语句是不敏感的，但是你插入进去数据库中的字符串是区分大小写的。

## having和where的区别

having是对聚合之后的结果再进行过滤操作，而where是在聚合前先对解过进行过滤然后再进行聚合操作。在实际操作数据库的时候尽量先使用where过滤，然后再聚合，减少操作的数据量，聚合的效率更高一些。

---

参考资料：

- 菜鸟教程-->SQL教程：[SQL 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/sql/sql-tutorial.html)
- 深入浅出MySQL  数据库开发、优化、与管理维护（第二版 唐汉明）
- [MySQL删除用户（DROP USER） (biancheng.net)](http://c.biancheng.net/view/2613.html)

