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

----

## 4. 过滤 (`where`子句)

* `and` 操作符, 与(`&&`)
* `or` 操作符, 或(`||`)
* `not` 操作符, 非(`!`)
* `()` 圆括号, 优先级最高
* `=, !=, <>, <=, >=`
* `between` 操作符, 表达式的值在设定的范围内
* `in` 操作符, 表达式的值在一个有限集合中
* `not in` 操作符, 表达式的值不在一个有限集合中
* `like` 操作符, 用于字符串匹配
	* `left()` 函数
	* 通配符
	* 正则表达式
* `null` 含义和用法
	*  `is` 操作符
* `not null` 

* 详见: 
	* [12.3运营商
](https://dev.mysql.com/doc/refman/5.7/en/non-typed-operators.html)
	* [12.5字符串函数
](https://dev.mysql.com/doc/refman/5.7/en/string-functions.html)
 	

--

### 4.1 `and`

```
// 获取只有在2007年之前入职的出纳员(title为Teller)
mysql> select * from employee where title = 'Teller' and start_date < '2007-01-01';
```
--

### 4.2 `or`
```
// 获取出纳员(title为Teller) 或者 在2007年之前入职的职员信息. 
mysql> select * from employee where title = 'Teller' or start_date < '2007-01-01';

```

--

### 4.3 `not`

> `not` 操作符对应 编程语言中的`!`运算符.
> 对于DBMS来说, 处理包含`not`操作符的`where`子句毫不费力.
> `not` 操作符的可读性不好, 不推荐. 

```
// 查找既不是出纳员(title为Teller) 也不是 在2007年之前入职的并且仍然在职的职员信息. 
mysql> select * from employee
    -> where end_date is null
    -> and
    -> not
    -> (title='Teller' or start_date < '2007-01-01');
```

--

### 4.4 `()`

```
// 获取仍然在职的 出纳员(title为Teller) 或者 在2007年之前入职的职员信息. 
mysql> select * from employee 
    -> where
    -> end_date is null // 必须为真
    -> and // 代表前后两个条件必须为真的数据才能在结果集中.
    -> (title='Teller' or start_date<'2007-01-01'); // 圆括号里面的两个小的表达式其中一个为真, 圆括号表达式就为真了
```
--

### 4.5 `=, !=, <>, <=, >=`

**相等**

 ```
 // 查询所有类型为customer account的产品
 
 mysql> select pt.name product_type, p.name product   
 	 -> from product p inner join product_type pt
   ->  on p.product_type_cd = pt.product_type_cd
   -> where pt.name = 'Customer Accounts';
 ```
 
 
```
 // 查询所有不是customer account 类型的账户
 
 mysql> select pt.name product_type, p.name product
    	-> from product p inner join product_type pt
    	->  on p.product_type_cd = pt.product_type_cd
    	-> where pt.name <> 'Customer Accounts'; // 可以使用 != 替换 <>
```

 
 

其他的以此类推
 

--

### 4.6 `between`
* between操作符指定的范围是闭合的(>= 和 <=), 也就是说上下限值本身也被包含在范围内.
* 注意: 必须首先指定范围的下限(在between后面), 然后指定范围的上限(在and后面), 不能相反.


```
// 查询 2005和2006年入职的员工信息

mysql> select emp_id, fname, lname, start_date
    -> from employee
    -> where start_date between '2005-01-01' and '2007-01-01'; // 如果2007年1月1日有入职的员工, 那么结果集中将包含该名员工信息. 
    // 所以更准确的写法应该是 where start_date between '2005-01-01' and '2006-12-31';

```

#### **注意:字符串比较**
	
字符串进行比较时, 根据所使用的字符集中字符次序进行比较.

```
// 查询客户的社会安全号码
mysql> select cust_id, fed_id
    -> from customer
    -> where cust_type_cd = 'I'
    ->  and fed_id between '500-00-0000' and '999-99-9999'; // 比如我们这里使用的utf8字符集, 那么字符'5'比字符'9'的次序小, 所以 '5' < '9'

```
	
--

### 4.7 `in`

```c
// 在account表中找出所有产品代码为'CHK', 'SAV', 'CD' 或 'MM' 的账户

mysql> select account_id, product_cd, cust_id, avail_balance
    -> from account
    -> where product_cd in ('CHK', 'SAV', 'CD', 'MM');
   // product_cd的值必须是 in 后面圆括号中的值
```


#### 有限集合是子查询的结果集

```
mysql> select account_id, product_cd, cust_id, avail_balance
    -> from account
    -> where product_cd in (select product_cd from product where product_type_cd = 'ACCOUNT');  // 子查询的结果集是有限集合. 
```

--

### 4.8 `not in`
```
mysql> select account_id, product_cd, cust_id, avail_balance
    -> from account
    -> where product_cd not in ('CHK', 'SAV', 'CD', 'MM'); // product_cd列的值不在圆括号的有限集合中.
```
    
--

### 4.9 `like`

#### `left()` 函数
* 从字符串的左边开始截取.

```
mysql> select emp_id, fname, lname
    -> from employee
    -> where left(lname, 1) = 'T'; //lname列中第一个字母等于 'T'
```

#### 通配符


通配符 | 匹配
----- | ---
-     | 正好一个字符
%     | 任意数目的字符(包括0)


```
mysql> select lname
    -> from employee
    -> where lname like '_a%e%'; // lname列的值的第二个位置必须为字符a, e必须在其后 的任何位置(包括最后一个位置)中至少出现一次.

```
    
##### 搜索表达式示例

搜索表达式 | 解释
-----    | ----
F%			| 以F打头的字符串
%t			| 以t结尾的字符串
%bas% 	| 包含`bas`子字符串的字符串
_ _ t _	| 包含4个字符, 且第3个字符为t的字符串 

```
// 查询所有人的社会安全号码
mysql> select cust_id, fed_id
    -> from customer
    -> where fed_id like '___-__-____';
```  

```
// 查询所有姓氏以F或G打头的雇员
mysql> select emp_id, fname, lname
    -> from employee
    -> where lname like 'F%' or lname like 'G%'; // 不区分大小写

```

#### 使用正则表达式(`regexp`)匹配字符串


```
// 查询所有姓氏以F或G打头的雇员

mysql> select emp_id, fname, lname
    -> from employee
    -> where lname regexp '^[FG]';
```


--

### 4.10 `null`

* null 表示值的缺失
* 适用场合: 
	* 没有合适的值
		* 比如ATM机上的自助交易并不需要employee ID列
	* 值未确定
		* 比如在客户行被创建时还不知道他的federal ID
	* 值为定义
		* 比如为某个还未添加到数据库的产品创建账户  	 
* 表达式可以为null, 但不能等于null
* 两个null值彼此**不能**判断为相等


#### `is` 

* 判断一个列的值是否是null 使用 `is` 操作运算符

```
mysql> select emp_id, fname, lname, superior_emp_id
    -> from employee
    -> where superior_emp_id is null; // 注意此处使用了 is 操作符进行判断
+--------+---------+-------+-----------------+
| emp_id | fname   | lname | superior_emp_id |
+--------+---------+-------+-----------------+
|      1 | Michael | Smith |            NULL |
+--------+---------+-------+-----------------+
1 row in set (0.00 sec)
    
```

**错误写法: **

 此查询语句实际被解析和执行, 但是根据 `null = null` 此相等条件表达式的结果为假, 所以, 不会返回任何结果. 只能使用 `is` 操作符进行判断. 

```
mysql> select emp_id, fname, lname, superior_emp_id
    -> from employee
    -> where superior_emp_id = null; // 注意此处使用 = 进行判断
Empty set (0.00 sec) // 结果为空. 正确的结果应该有一条数据

//
mysql> 
```


--


### 4.11 `not null`

```
// 查询有主管的职员信息. 

mysql> select emp_id, fname, lname, superior_emp_id
    -> from employee
    -> where superior_emp_id is not null; // superior_emp_id列不为null, 注意还有是使用is判断
```

### != 表达式条件的结果不包括null

```
// 查询所有不是Helen Fleming(employee ID为6)所管理的雇员

mysql> select emp_id, fname, lname, superior_emp_id
    -> from employee
    -> where superior_emp_id != 6; 
    // 根据需求, 判断 superior_emp_id 不等于 6.
    // 但是有一个雇员的superior_emp_id是null, 没有包含在此查询语句的结果集中.
    // superior_emp_id != 6 不包含 superior_emp_id为null的情况
```
**改进**

```
mysql> select emp_id, fname, lname, superior_emp_id
    -> from employee
    -> where superior_emp_id != 6 or superior_emp_id is null;
```



