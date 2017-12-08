# select 子句
[TOC]

## 1. 查询表中所有数据
`select * from 表名;`
## 2. 查询表中指定的列的数据
`select 列名1, 列名2, ... from 表名;`

## 3. select子句后可以使用哪些
> 列名, 字符, 表达式, 內建函数调用

```c
mysql> select 
    -> emp_id, // 列名
    -> 'ACTIVE', // 根据需求写的字符
    -> emp_id * 3.14159, // 表达式
    -> UPPER(lname) // 调用转化字符串为大写的函数
    -> from employee;
    
// 结果集中显示的是列的值, 设定的字符, 表达式的结果, 调用函数的返回值
+--------+--------+------------------+--------------+
| emp_id | ACTIVE | emp_id * 3.14159 | UPPER(lname) |
+--------+--------+------------------+--------------+
|      1 | ACTIVE |          3.14159 | SMITH        |
|      2 | ACTIVE |          6.28318 | BARKER       |
|      3 | ACTIVE |          9.42477 | TYLER        |
|      4 | ACTIVE |         12.56636 | HAWTHORNE    |
|      5 | ACTIVE |         15.70795 | GOODING      |
+--------+--------+------------------+--------------+
5 rows in set (0.00 sec)

mysql> 
```


## 4. 为查询返回的列起别名

```c
mysql> select
    -> emp_id employee_id, 
    -> 'ACTIVE' status,
    -> emp_id * 3.14159,
    -> UPPER(lname) last_name_upper
    -> from employee;
    
// 返回的结果集中的列名都是查询语句中的别名
+-------------+--------+------------------+-----------------+
| employee_id | status | emp_id * 3.14159 | last_name_upper |
+-------------+--------+------------------+-----------------+
|           1 | ACTIVE |          3.14159 | SMITH           |
|           2 | ACTIVE |          6.28318 | BARKER          |
|           3 | ACTIVE |          9.42477 | TYLER           |
|           4 | ACTIVE |         12.56636 | HAWTHORNE       |
+-------------+--------+------------------+-----------------+
5 rows in set (0.00 sec)

mysql> 

```



### 使用 as 关键字
> 在别名前添加 `as` 关键字, 可以提高查询语句的可读性.
> 结果集和不使用 `as` 没有区别. 

```c
mysql> select 
    -> emp_id,
    -> 'ACIVE' as status,
    -> emp_id * 3.14159 as epm_x_pi,
    -> UPPER(lname) as last_name_upper
    -> from employee;
+--------+--------+----------+-----------------+
| emp_id | status | epm_x_pi | last_name_upper |
+--------+--------+----------+-----------------+
|      1 | ACIVE  |  3.14159 | SMITH           |
|      2 | ACIVE  |  6.28318 | BARKER          |
|      3 | ACIVE  |  9.42477 | TYLER           |
|      4 | ACIVE  | 12.56636 | HAWTHORNE       |
|      5 | ACIVE  | 15.70795 | GOODING         |
+--------+--------+----------+-----------------+
5 rows in set (0.00 sec)

mysql> 

```


## 5. 去除重复的行
> `distinct` 关键字

> 查询可能会返回重复的数据, 比如一个客户会拥有多个账户, 那么在查询账户表中的客户id时就会返回重复的数据. 如下所示: 

```
mysql> select cust_id from account;
+---------+
| cust_id |
+---------+
|       1 |
|       1 |
|       1 |
|       2 |
|       2 |
|       3 |
|       3 |
|       4 |
|       4 |
|       4 |
+---------+
10 rows in set (0.01 sec)

mysql> 
```

添加`distinct`关键字, 去除重复的行: 
```
mysql> select distinct cust_id from account;
+---------+
| cust_id |
+---------+
|       1 |
|       2 |
|       3 |
|       4 |
+---------+
4 rows in set (0.01 sec)

mysql> 
```

> 不需要删除重复数据使用 `all`关键字, `all` 是系统默认, 一般省略不写.

```
提示: 
	1. 使用distinct比不使用要更耗时.
	2. 要删除重复的行, 需要先对数据排序, 所以更耗时.
	3. 先确定数据是否可能包含重复行, 以及是否需要去除重复行, 再决定是否使用distinct关键字, 减少不必要的使用.
	
```







