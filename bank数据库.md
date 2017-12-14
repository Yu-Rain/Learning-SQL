# bank 数据库

[TOC]

--

表名 | 定义
---- | ----
branch | 银行的支行表
employee | 银行的职员表
department | 部门表
customer | 账户表(包括个人和公司)
individual | 个人账户表(customer表的子类型)
business | 公司账户表(customer表的子类型)
officer | 负责公司客户业务的职员
product | 向顾客提供的银行服务表
product_type | 银行服务分组
account | 账户和银行服务之间的对应关系表
transaction | 改变账户余额的操作

----

## employee 表 
> 职员表


| Field | Description
------- | ---------
emp_id  | 职员id号
fname 	| 名 
lname   | 姓
start_date | 入职时间
end_date | 离职时间
superior_emp_id | 此职员的主管id号
dept_id | 此职员所在部门的id
title | 职位名称
assigned_branch_id | 所在支行的id号






| Field              | Type                 | Null | Key | Default | Extra          |
--------------------+----------------------+------+-----+---------+----------------+
| emp_id             | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
| fname              | varchar(20)          | NO   |     | NULL    |                |
| lname              | varchar(20)          | NO   |     | NULL    |                |
| start_date         | date                 | NO   |     | NULL    |                |
| end_date           | date                 | YES  |     | NULL    |                |
| superior_emp_id    | smallint(5) unsigned | YES  | MUL | NULL    |                |
| dept_id            | smallint(5) unsigned | YES  | MUL | NULL    |                |
| title              | varchar(20)          | YES  |     | NULL    |                |
| assigned_branch_id | smallint(5) unsigned | YES  | MUL | NULL    |                |




-----
	
	
	
## department
> 部门表


Field | Description
----- | ---------
dept_id | 部门的id号
name  | 部门的名称


| Field   | Type                 | Null | Key | Default | Extra          |
---------+----------------------+------+-----+---------+----------------+
| dept_id | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
| name    | varchar(20)          | NO   |     | NULL    
	

----


## account
> 账户和银行服务之间的对应关系表

> 一个账户可以购买多个银行服务, 一个银行服务也可以被多个账户购买, 所以创建`account`表, 存放账户和银行服务之间的关系和信息的表.

> 这个表中存放了账户id, 账户所购买的银行服务id, 给账户办理银行服务的职员id, 支行id,  以及开始和结束时间等其他信息, 

Field | Description
----- | -----------
account_id | 产品id号
product_cd | 银行服务的id号(product表)
cust_id		| 账户id号(customer表)
open_date	| 产品开放时间
close_date | 产品关闭时间
last_activity_date | 
status	  | 产品当前状态('ACTIVE','CLOSED','FROZEN')
open_branch_id	 | 开放产品的支行id号 (branch表)
open_emp_id		| 负责产品的职员id号(employee表)
avail_balance	| 
pending_balance | 


| Field              | Type                             | Null | Key | Default | Extra          |
--------------------+----------------------------------+------+-----+---------+----------------+
| account_id         | int(10) unsigned                 | NO   | PRI | NULL    | auto_increment |
| product_cd         | varchar(10)                      | NO   | MUL | NULL    |                |
| cust_id            | int(10) unsigned                 | NO   | MUL | NULL    |                |
| open_date          | date                             | NO   |     | NULL    |                |
| close_date         | date                             | YES  |     | NULL    |                |
| last_activity_date | date                             | YES  |     | NULL    |                |
| status             | enum('ACTIVE','CLOSED','FROZEN') | YES  |     | NULL    |                |
| open_branch_id     | smallint(5) unsigned             | YES  | MUL | NULL    |                |
| open_emp_id        | smallint(5) unsigned             | YES  | MUL | NULL    |                |
| avail_balance      | float(10,2)                      | YES  |     | NULL    |                |
| pending_balance    | float(10,2)                      | YES  |     | NULL    |                |

----

## customer
> 账户表

Field | Description
----- | -----------
cust_id | 账户id号
fed_id	| 唯一标识(身份证号或公司税务号码)
cust_type_cd | 账户类型('I','B')
address 	| 地址信息
city	| 城市
state	 | 国家
postal_code | 邮政编码


| Field        | Type             | Null | Key | Default | Extra          |
--------------+------------------+------+-----+---------+----------------+
| cust_id      | int(10) unsigned | NO   | PRI | NULL    | auto_increment |
| fed_id       | varchar(12)      | NO   |     | NULL    |                |
| cust_type_cd | enum('I','B')    | NO   |     | NULL    |                |
| address      | varchar(30)      | YES  |     | NULL    |                |
| city         | varchar(20)      | YES  |     | NULL    |                |
| state        | varchar(20)      | YES  |     | NULL    |                |
| postal_code  | varchar(10)      | YES  |     | NULL    

----

## business
> 公司账户表(customer表的子类型)账户


Field | Description
----- | -----------
cust_id	| 公司账户id号
name	| 公司名称
state_id		| 公司标识id号
incorp_date | 公司成立时间

| Field       | Type             | Null | Key | Default | Extra |
-------------+------------------+------+-----+---------+-------+
| cust_id     | int(10) unsigned | NO   | PRI | NULL    |       |
| name        | varchar(40)      | NO   |     | NULL    |       |
| state_id    | varchar(10)      | NO   |     | NULL    |       |
| incorp_date | date             | YES  |     | NULL    |       |

----



## branch
 
Field | Description
----- | -----------
branch_id | 银行支行id号
name	| 银行支行名称
address | 银行支行的地址
city	| 银行支行所在的城市
state | 银行支行所在的省份
zip	| 


| Field     | Type                 | Null | Key | Default | Extra          |
-----------+----------------------+------+-----+---------+----------------+
| branch_id | smallint(5) unsigned | NO   | PRI | NULL    | auto_increment |
| name      | varchar(20)          | NO   |     | NULL    |                |
| address   | varchar(30)          | YES  |     | NULL    |                |
| city      | varchar(20)          | YES  |     | NULL    |                |
| state     | varchar(2)           | YES  |     | NULL    |                |
| zip       | varchar(12)          | YES  |     | NULL    |                |




