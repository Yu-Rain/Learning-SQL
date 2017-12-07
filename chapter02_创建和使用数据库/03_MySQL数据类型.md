# MySQL数据类型

[TOC]




## 一.  字符型 

### 字符型数据
> 定义一个字符列时, 必须指定该列所能存放字符串的最大长度.

```c
最大长度不超过20个字符

char(20)  

varchar(20)
```


字符类型 | char | varchar
---- | ---- | ----
定义 | 定长字符串 | 变长字符串
最大长度(字节) | 255 | 65535 (64KB)
使用场景 | 存储同样长度的字符串, 比如州名的简写 | 存储变长字符串.  存储自由格式的数据条目, 比如用于存储客户与公司客服部门交互的数据的notes列
总结 | 主流数据库中二者使用方式类似



```
提示:
	1. Oracle数据库, char列能容纳2000字节, varchar列能容纳4000字节
	2. SQL Server, char和varchar列都能容纳8000字节
```


### 文本数据

> 为什么使用文本数据?

> 需要存储的数据超过 **64KB** (varchar列所能容许的上限), 就需要使用文本类型


文本类型 | 最大长度 | 使用场景
---- | ---- | ----
tinytext | 255 
text | 65535
mediumtext | 16777215 | 存储文档
longtext | 4294967295 | 存储文档

> 

```
注意: 
	1. 数据超过最大长度就会被截断
	2. 不会消除数据尾部的空格
	3. 使用文本列排序或分组, 只会使用 前1024 个字节, 需要时可以放开这个限制
	4. 以上类型只针对MySQL. (SQL Server 只提供text类型, DB2和Oracle使用clob类型(Character Large Object))
	5. MySQL的varchar最大容纳65535个字节, 所以一般不需要tinytext或text类型

```


### 字符集

> 单字符集

	每个字符只需要一个字节存储, 比如: 英语

> 多字符集

	每个字符需要多个字节存储, 比如: 汉语, 韩语等.	 
	
	
#### 查看MySQL支持的字符集

> 第4列 Maxlen 大于1 代表是多字符集

```
mysql> show character set;

+----------+---------------------------------+---------------------+--------+
| Charset  | Description                     | Default collation   | Maxlen |
+----------+---------------------------------+---------------------+--------+
| big5     | Big5 Traditional Chinese        | big5_chinese_ci     |      2 |
| dec8     | DEC West European               | dec8_swedish_ci     |      1 |
| cp850    | DOS West European               | cp850_general_ci    |      1 |
| hp8      | HP West European                | hp8_english_ci      |      1 |
| koi8r    | KOI8-R Relcom Russian           | koi8r_general_ci    |      1 |
| latin1   | cp1252 West European            | latin1_swedish_ci   |      1 |
| latin2   | ISO 8859-2 Central European     | latin2_general_ci   |      1 |
| swe7     | 7bit Swedish                    | swe7_swedish_ci     |      1 |
| ascii    | US ASCII                        | ascii_general_ci    |      1 |
| ujis     | EUC-JP Japanese                 | ujis_japanese_ci    |      3 |
| sjis     | Shift-JIS Japanese              | sjis_japanese_ci    |      2 |
| hebrew   | ISO 8859-8 Hebrew               | hebrew_general_ci   |      1 |
| tis620   | TIS620 Thai                     | tis620_thai_ci      |      1 |
| euckr    | EUC-KR Korean                   | euckr_korean_ci     |      2 |
| koi8u    | KOI8-U Ukrainian                | koi8u_general_ci    |      1 |
| gb2312   | GB2312 Simplified Chinese       | gb2312_chinese_ci   |      2 |
| greek    | ISO 8859-7 Greek                | greek_general_ci    |      1 |
| cp1250   | Windows Central European        | cp1250_general_ci   |      1 |
| gbk      | GBK Simplified Chinese          | gbk_chinese_ci      |      2 |
| latin5   | ISO 8859-9 Turkish              | latin5_turkish_ci   |      1 |
| armscii8 | ARMSCII-8 Armenian              | armscii8_general_ci |      1 |
| utf8     | UTF-8 Unicode                   | utf8_general_ci     |      3 |
| ucs2     | UCS-2 Unicode                   | ucs2_general_ci     |      2 |
| cp866    | DOS Russian                     | cp866_general_ci    |      1 |
| keybcs2  | DOS Kamenicky Czech-Slovak      | keybcs2_general_ci  |      1 |
| macce    | Mac Central European            | macce_general_ci    |      1 |
| macroman | Mac West European               | macroman_general_ci |      1 |
| cp852    | DOS Central European            | cp852_general_ci    |      1 |
| latin7   | ISO 8859-13 Baltic              | latin7_general_ci   |      1 |
| utf8mb4  | UTF-8 Unicode                   | utf8mb4_general_ci  |      4 |
| cp1251   | Windows Cyrillic                | cp1251_general_ci   |      1 |
| utf16    | UTF-16 Unicode                  | utf16_general_ci    |      4 |
| utf16le  | UTF-16LE Unicode                | utf16le_general_ci  |      4 |
| cp1256   | Windows Arabic                  | cp1256_general_ci   |      1 |
| cp1257   | Windows Baltic                  | cp1257_general_ci   |      1 |
| utf32    | UTF-32 Unicode                  | utf32_general_ci    |      4 |
| binary   | Binary pseudo charset           | binary              |      1 |
| geostd8  | GEOSTD8 Georgian                | geostd8_general_ci  |      1 |
| cp932    | SJIS for Windows Japanese       | cp932_japanese_ci   |      2 |
| eucjpms  | UJIS for Windows Japanese       | eucjpms_japanese_ci |      3 |
| gb18030  | China National Standard GB18030 | gb18030_chinese_ci  |      4 |
+----------+---------------------------------+---------------------+--------+
41 rows in set (0.00 sec)

mysql> 

```


#### 指定字符集

> `latin1` 是 MySQL 默认字符集

1.为数据列指定非默认字符集
	

```
varchar(20) character set utf8;

// 注意: utf8 不可以写成 utf-8
```


2.改变整个数据库的默认字符集

```
create database foreign_sales character set utf8;
```


## 二.  数值型数据

### 整数类型

> 在类型前添加 `unsigned` 关键字, 代表无符号(>=0)

类型 | 带符号的范围 | 无符号的范围 | 字节
---- | --------- | ---------- | ---- |
tinyint | -128 ~ 127 | 0 ~ 255 | 1
smallint | -32768 ~ 32767 | 0 ~ 65535 | 2
mediumint | -8388608 ~ 8388607 | 0 ~ 16777215 | 3
int | -2147483648 ~ 2147483647 | 0 ~ 4294967295 | 4
bigint | -9223372036854775808 ~ 9223372036854775807 | 0 ~ 18446774073709551615 | 8


> 选择类型时确保能够容纳预期的**最大数字**即可, 避免浪费不必要的存储空间


### 浮点类型

> 参数p, 指定浮点类型的精度(数字总位数)
> 参数s, 指定浮点类型的有效位(小数点右边所允许的数字位数)

类型 | 负数值范围 | 正数值范围
---- | ------ | ------
float(p,s) | -3.402823466E+38 ~ -1.175494351E-38                               | 1.175494351E-38 ~ 3.402823466E+38 
double(p,s) |-1.7976931348623157E+308 ~ -2.2250738585072014E-308 | 2.2250738585072014E-308 ~      1.7976931348623157E+308

```
注意:
	1. 数字为超过定义的精度或有效位, 数据会被四舍五入

	举例: 
		float(4, 2)
	
		添加数据 
		27.44 或 8.19 正确
		17.8675 四舍五入为 12.87
		178.375 产生错误	
		
	2. 浮点列也可以被定义为unsigned. 和整数列的区别是: 浮点列只是禁止列中存放负数, 不像整数列一样改变了所存储数据的范围.		

```

## 三. 日期型数据

类型 | 默认格式 | 允许的值
---- | ------- | ------
date | YYYY-MM-DD | 1000-01-01 ~ 9999-12-31
datetime | YYYY-MM-DD HH:MI:SS | 1000-01-01 00:00:00 ~ 9999-12-31 23:59:59
timestamp | YYYY-MM-DD HH:MI:SS | 1970-01-01 00:00:00 ~ 2037-12-31 23:59:59
year | YYYY | 1901 ~2155
time | HHH:MI:SS | -838:59:59 ~ 838:59:59

> HH 代表小时, 00 ~ 23
> HHH 小时(过去的) -838 ~ 838


