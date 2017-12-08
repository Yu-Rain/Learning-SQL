# SQL 语句

[TOC]

[MySQL官方手册](https://dev.mysql.com/doc/refman/5.7/en/database-use.html)

## 1. 数据库

### 1.1 查看有哪些数据库


```c
mysql> show databases; // 查看数据库, 注意复数形式和分号
+--------------------+
| Database           |
+--------------------+
| information_schema |
| aliendatabase      |
| bank               |       |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql> 
```


### 1.2 切换使用的数据库


> USE，就像QUIT，不需要分号。（如果你愿意的话，你可以用分号来终止这样的语句;这没有什么坏处。）这个 USE语句也是另一种特殊的：它必须在一行中给出。

```c
mysql> use bank // 可以不写分号(;)
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql>
```


### 1.3 查看当前使用的数据库 


```c
mysql> select database(); 
+------------+
| database() |
+------------+
| bank       |
+------------+
1 row in set (0.00 sec)

mysql> 


```

### 1.4 查看当前使用数据库的状态

```
mysql> status;
--------------
/usr/local/mysql/bin/mysql  Ver 14.14 Distrib 5.7.14, for osx10.11 (x86_64) using  EditLine wrapper

Connection id:		134
Current database:	bank
Current user:		root@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		5.7.14 MySQL Community Server (GPL)
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	latin1
Db     characterset:	latin1
Client characterset:	utf8
Conn.  characterset:	utf8
UNIX socket:		/tmp/mysql.sock
Uptime:			28 days 6 hours 52 min 51 sec

Threads: 1  Questions: 1121  Slow queries: 0  Opens: 1082  Flush tables: 1  Open tables: 671  Queries per second avg: 0.000
--------------

mysql> 

```

### 1.5 查看当前数据库的所有表

```c
mysql> show tables; // 或者 show tables from databaseName;
+----------------+
| Tables_in_bank |
+----------------+
| account        |
| branch         |
| transaction    |
+----------------+
3 rows in set (0.00 sec)

mysql> 
```

### 1.6 查看表结构

```c
mysql> desc branch; // desc 表名
// 或者  show columns from branch
+-----------+----------------------+------+-----+---------+----------------+
| Field     | Type                 | Null | Key | Default | Extra          |
+-----------+----------------------+------+-----+---------+----------------+
| branch_id | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
| name      | varchar(20)          | NO   |     | NULL    |                |
| address   | varchar(30)          | YES  |     | NULL    |                |
| city      | varchar(20)          | YES  |     | NULL    |                |
| state     | varchar(2)           | YES  |     | NULL    |                |
| zip       | varchar(12)          | YES  |     | NULL    |                |
+-----------+----------------------+------+-----+---------+----------------+
6 rows in set (0.00 sec)

mysql> 
```




## 2. DDL
> 数据定义语言（Data Definition Language，DDL）是SQL语言集中负责数据结构定义与数据库对象定义的语言，由CREATE、ALTER与DROP三个语法所组成

[13.1数据定义语句
](https://dev.mysql.com/doc/refman/5.7/en/sql-syntax-data-definition.html)

### 2.1 创建数据库
[13.1.11 CREATE DATABASE语法
](https://dev.mysql.com/doc/refman/5.7/en/create-database.html)

>  `create databas 数据库名`
>  `create if not exists databas 数据库名`
>  `create database 数据库名 character set utf8;`  指定数据库默认的字符集


```c
mysql> create database test; // create databas 数据库名
// 或者create if not exists databas 数据库名
Query OK, 1 row affected (0.00 sec)

mysql> select database(); 
+--------------------+
| database()         |
+--------------------+
| information_schema | // 注意创建完数据库后, 当前使用的数据库不是新创建的数据库
+--------------------+
1 row in set (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| aliendatabase      |
| bank               |              |
| test               |
+--------------------+
4 rows in set (0.00 sec)

mysql>  
```

### 2.2 删除数据库
[13.1.22 DROP DATABASE语法
](https://dev.mysql.com/doc/refman/5.7/en/drop-database.html)

```c
// drop database 数据库名
mysql> drop database test; // 删除test数据库
Query OK, 0 rows affected (0.01 sec)

mysql> drop database test; // 数据库不存在会报错
ERROR 1008 (HY000): Can't drop database 'test'; database doesn't exist

mysql>
mysql> drop schema dfs; // 删除dfs数据库
Query OK, 0 rows affected (0.00 sec)

mysql>

```

```c
mysql> create database rain;
Query OK, 1 row affected (0.00 sec)

mysql> drop database if exists rain; // 添加判断条件 if exists
Query OK, 0 rows affected (0.00 sec)

mysql> drop database if exists rain; // 删除不存在的数据库不会报错, 只有一个警告
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> 
```

### 2.3 创建表
[13.1.18 CREATE TABLE语法
](https://dev.mysql.com/doc/refman/5.7/en/create-table.html)



## 3. DML 

### 3.1 插入数据
> insert语句的3个主要组成部分:
 
>  * 表名称
>  * 列名称
>  * 列的值

> `insert into 表名 (列名...) values (值 ...); `

### 3.2 更新数据

> 修改表中每一行: 
> `update 表名 set 列名=值[, 列名=值, ...]`
> 
> --------	
> 修改符合指定的数据行.	
> `update 表名 set 列名=值[, 列名=值, ...] where 列名=值;`

### 3.3 删除数据

> 删除表中所有行:  `delete from 表名;`
> 删除表中指定的行: `delete from 表名 where 列名=值;`

### 3.4 查询数据 select 子句
#### 1. 查询表中所有数据
`select * from 表名;`
#### 2. 查询表中指定的列的数据
`select 列名1, 列名2, ... from 表名;`

#### 3. select子句后可以使用哪些
> 列名, 字符, 表达式, 內建函数调用

```c
mysql> select 
    -> emp_id, // 列名
    -> 'ACTIVE', // 根据需求写的字符
    -> emp_id * 3.14159, // 表达式
    -> UPPER(lname) // 调用转化字符串为大写的函数
    -> from employee;
```

#### 4. 为查询返回的列起别名

```c
mysql> select
    -> emp_id employee_id, 
    -> 'ACTIVE' status,
    -> emp_id * 3.14159,
    -> UPPER(lname) last_name_upper
    -> from employee;
```

##### 使用 as 关键字
> 在别名前添加 `as` 关键字, 可以提高查询语句的可读性.
> 结果集和不使用 `as` 没有区别. 

```c
mysql> select 
    -> emp_id,
    -> 'ACIVE' as status,
    -> emp_id * 3.14159 as epm_x_pi,
    -> UPPER(lname) as last_name_upper
    -> from employee;
```

#### 5. 去除重复的行
> `distinct` 关键字

```
mysql> select distinct cust_id from account;
```


