# where 子句

[TOC]

----


> `select`, `update` 和 `delete` 语句中包含 `where`子句, `insert`语句不包含.


----
## 1. 条件评估

### 1.1 and 操作符

> and 操作符表示多个过滤条件都必须为真, 就是 `&&` 的关系.

```
// 获取只有在2007年之前入职的出纳员(title为Teller)
mysql> select * from employee where title = 'Teller' and start_date < '2007-01-01';
```


### 1.2 or 操作符
> or 操作符表示多个过滤条件中有一个为真即可, 就是 `||` 的关系.

```c
// 获取出纳员(title为Teller) 或者 在2007年之前入职的职员信息. 
mysql> select * from employee where title = 'Teller' or start_date < '2007-01-01';
```


### 1.3 () 圆括号

> 如果使用`where`子句包含了3个或更多条件, 且同时使用了 `and`和`or` 操作符, 那么需要使用 **圆括号来明确意图**.

```
// 获取仍然在职的 出纳员(title为Teller) 或者 在2007年之前入职的职员信息. 
mysql> select * from employee 
    -> where
    -> end_date is null // 必须为真
    -> and // 代表前后两个条件必须为真的数据才能在结果集中.
    -> (title='Teller' or start_date<'2007-01-01'); // 圆括号里面的两个小的表达式其中一个为真, 圆括号表达式就为真了
```



### 1.4 not 操作符

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

----
## 2. 构建条件 (过滤条件中都可以包含哪些结构)


 * 数字
 * 表或视图中的列
 * 字符串, 比如 'Teller'
 * 內建函数, 比如函数 `concat('Learning', '', 'SQL')`
 * 子查询
 * 表达式列表(比如 'Teller', 'Head Teller', 'Operations Manager');
 * 比较操作符, 如 =, !=, <, >, <>, like, in 和 between
 * 算术操作符, 比如 +, -, * 和 /






----

## 3. 条件类型
### 3.1 相等条件
 
 * 相等 =
 * 不等 !=  或  <>

 
```
 // 查询所有类型为customer account的产品
 
 mysql> select pt.name product_type, p.name product    -> from product p inner join product_type pt
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
 
```
// 修改数据
// 删除2002年关闭的账户
mysql> delete from account
    -> where status = 'CLOSED' and year(close_date) = 2002;
```

### 3.2 范围条件
* 检验表达式的值是否处于某个区间
* 关系运算符: <, >, <=, >=
* between 操作符
 
----

#### between操作符

* 当需要同时限制范围的上限和下限时, 使用被between操作符构建一个查询条件
* between操作符指定的范围是闭合的(>= 和 <=), 也就是说上下限值本身也被包含在范围内.
* 注意: 必须首先指定范围的下限(在between后面), 然后指定范围的上限(在and后面), 不能相反.

```
// 查询 2005和2006年入职的员工信息

mysql> select emp_id, fname, lname, start_date
    -> from employee
    -> where start_date between '2005-01-01' and '2007-01-01'; // 如果2007年1月1日有入职的员工, 那么结果集中将包含该名员工信息. 
    // 所以更准确的写法应该是 where start_date between '2005-01-01' and '2006-12-31';

```


#### 字符串范围

> 根据所使用的字符集中字符次序进行比较.

```
// 查询客户的社会安全号码
mysql> select cust_id, fed_id
    -> from customer
    -> where cust_type_cd = 'I'
    ->  and fed_id between '500-00-0000' and '999-99-9999'; // 比如我们这里使用的utf8字符集, 那么字符'5'比字符'9'的次序小, 所以 '5' < '9'

```

### 3.3 成员条件 (`in`操作符)

* 表达式的值在一个有限集合中.



```c
// 在account表中找出所有产品代码为'CHK', 'SAV', 'CD' 或 'MM' 的账户

mysql> select account_id, product_cd, cust_id, avail_balance
    -> from account
    -> where product_cd in ('CHK', 'SAV', 'CD', 'MM');
   // product_cd的值必须是 in 后面圆括号中的值
```

#### 使用子查询

```
mysql> select account_id, product_cd, cust_id, avail_balance
    -> from account
    -> where product_cd in (select product_cd from product where product_type_cd = 'ACCOUNT');  // 子查询的结果集是有限集合. 
```

#### 使用 `not in`

* 检查一个表达式的值是否不存在一个有限集合中. 

```
mysql> select account_id, product_cd, cust_id, avail_balance
    -> from account
    -> where product_cd not in ('CHK', 'SAV', 'CD', 'MM'); // product_cd列的值不在圆括号的有限集合中.
```


### 3.4 匹配条件 (字符串匹配)

#### `left()` 函数
* 从字符串的左边开始截取.

```
mysql> select emp_id, fname, lname
    -> from employee
    -> where left(lname, 1) = 'T'; //lname列中第一个字母等于 'T'
```
#### 使用通配符 和 `like` 操作符

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


### null: 4个字母的关键字

* null 表示值的缺失
* 适用场合: 
	* 没有合适的值
		* 比如ATM机上的自助交易并不需要employee ID列
	* 值未确定
		* 比如在客户行被创建时还不知道他的federal ID
	* 值为定义
		* 比如为某个还未添加到数据库的产品创建账户  	 
* 表达式可以为null, 但不能等于null
* 两个null值彼此不能判断为相等

#### `null` 操作符
> 为了测试表达式是否为null, 需要使用 `null`操作符

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

#### `not null` 操作符

```
// 查询有主管的职员信息. 

mysql> select emp_id, fname, lname, superior_emp_id
    -> from employee
    -> where superior_emp_id is not null; // superior_emp_id列不为null, 注意还有是使用is判断
```

#### != 表达式条件的结果不包括null

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




