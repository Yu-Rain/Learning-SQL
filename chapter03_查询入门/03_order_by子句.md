# `order by` 子句


## 升序, 降序 

> `asc` 升序, 默认, 可省略
> `desc` 降序

```c
mysql> select open_emp_id, product_cd from account 
    -> order by open_emp_id; // 升序 asc省略 
```
```c
mysql> select open_emp_id, product_cd from account 
    -> order by open_emp_id, product_cd; // 先根据open_emp_id进行升序排序, 如果open_emp_id相同, 再根据product_cd升序排序
    

注意: order by 子句中各列出现的顺序决定了各列进行排序的顺序.    
```

```c
mysql> select open_emp_id, product_cd from account  
	  -> order by open_emp_id desc; // 降序 desc 不能省略
```

## 根据表达式排序

```c
mysql> select cust_id, cust_type_cd, city, state, fed_id 
    -> from customer 
    -> order by right(fed_id, 3); // 內建函数right()提取fed_id列的最后3个字符, 使用提取的3个字符进行排序. 
+---------+--------------+------------+-------+-------------+
| cust_id | cust_type_cd | city       | state | fed_id      |
+---------+--------------+------------+-------+-------------+
|       1 | I            | Lynnfield  | MA    | 111-11-1111 |
|      10 | B            | Salem      | NH    | 04-1111111  |
|       2 | I            | Woburn     | MA    | 222-22-2222 |
|      11 | B            | Wilmington | MA    | 04-2222222  |
|       3 | I            | Quincy     | MA    | 333-33-3333 |
|      12 | B            | Salem      | NH    | 04-3333333  |
|       4 | I            | Waltham    | MA    | 444-44-4444 |
|      13 | B            | Quincy     | MA    | 04-4444444  |
|       5 | I            | Salem      | NH    | 555-55-5555 |
|       6 | I            | Waltham    | MA    | 666-66-6666 |
|       7 | I            | Wilmington | MA    | 777-77-7777 |
|       8 | I            | Salem      | NH    | 888-88-8888 |
|       9 | I            | Newton     | MA    | 999-99-9999 |
+---------+--------------+------------+-------+-------------+
13 rows in set (0.00 sec)

mysql> 
```

## 根据数字占位符排序

> 不推荐以下做法.
> 如果select 子句中增加了新列, 而order by子句中没有同步改变序号, 则会造成预料之外的结果.


```c
mysql> select emp_id, title, start_date, fname, lname 
    -> from employee 
    -> order by 2, 5; // 根据select子句后的第2列和第5列进行排序
+--------+--------------------+------------+----------+-----------+
| emp_id | title              | start_date | fname    | lname     |
+--------+--------------------+------------+----------+-----------+
|     13 | Head Teller        | 2000-05-11 | John     | Blake     |
|      6 | Head Teller        | 2004-03-17 | Helen    | Fleming   |
|     16 | Head Teller        | 2001-03-15 | Theresa  | Markham   |
|     10 | Head Teller        | 2002-07-27 | Paula    | Roberts   |
|      5 | Loan Manager       | 2003-11-14 | John     | Gooding   |
|      4 | Operations Manager | 2002-04-24 | Susan    | Hawthorne |
|      1 | President          | 2001-06-22 | Michael  | Smith     |
|     17 | Teller             | 2002-06-29 | Beth     | Fowler    |
|      9 | Teller             | 2002-05-03 | Jane     | Grossman  |
+--------+--------------------+------------+----------+-----------+
9 rows in set (0.00 sec)

mysql> 

```


