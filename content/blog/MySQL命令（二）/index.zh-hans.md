---
# type: docs 
title: MySQL命令（二）
date: 2023-05-08T12:53:36+08:00
featured: false
draft: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
series:
 - blog
categories: [
    教程
]
tags: [
    MySQL
]
images: []
authors: yfdkldmm
---

这次就是比较进阶的一部分

<!--more-->

有不少的内容其实更适合自己尝试，结果也不会有多少改变，就不给图了。

## 排序查询

### 格式

```mysql
select *
from a
(where 筛选条件)
order by b (asc|desc);
```

代表从a表中查询所有，按照b列升序或降序排列。

![排序格式](/images/MySQL命令（二）/排序格式.jpg)

### 特点

asc代表升序，desc代表降序。若不写，则默认升序。

### 用法详解

```mysql
select b*100 案例1
from a
order by b*100(案例1);
```

排序对象可为表达式，或者替换为括号中的案例1，以别名来排序。

```mysql
select length(b) 案例2, b
from a
order by 案例2;
```

排序对象可为函数。这代表意义为从a表中查询b列的字节长度和b列，并按照字节长度升序排序。

```mysql
select *
from a
order by b, c;
```

代表从a表中查询所有，并先按照b列升序排序，再按照c列升序排序。

## 常见函数

一般分为

1. 单行函数，如concat，length，ifnull等。
2. 分组函数，又称为统计函数，聚合函数等

### 单行函数

单行函数又能被分为

1. 字符函数
2. 数学函数
3. 日期函数
4. 其他函数
5. 流程控制函数

#### 字符函数

##### length

获取参数值的字节个数

```mysql
select length('abc');
select length('二abc');
```

显示结果，第一个为3，第二个则是为6，因为在utf-8下，中文字符占3个字节数。

##### concat

拼接字符串。

```mysql
select concat('a','+','b');
```

显示结果为a+b。

##### upper、lower

分别将字符变为大写或小写。

```mysql
select upper('a');
```

显示结果为A。

##### substr、substring

作用为截取指定区间的字符。
注意：sql语言的索引是从1开始。

```mysql
select substr('abcdefg', 3);
select substr('abcdefg' from 3);
select substr('abcdefg', 3, 1);
select substr('abcdefg' from 3 for 1);
```

其中，1，2行语句结果一致，皆为从第三个字符开始截取，即从字符c开始到字符末尾。
3，4行语句结果一致，皆为从第三个字符开始截取，截取1个字符，即从字符c开始，数一个字符，也就只返回c。

##### instr

返回子串第一次出现的索引位置，如果没有，则返回0.

```mysql
select substr('abcdefgb', 'b');
select substr('abcdefgb', 'h');
```

第一个结果为2。
第二个结果为0。

##### trim

去掉头尾指定的字符，未指定，则默认去掉空格。

```mysql
select trim('     案例1      ');
select trim('a' from 'aaaaaaa案aaaaaa例1aaaaaaaa');
```

第一个返回案例1。
第二个返回案aaaaaa例1，因为只是去掉头尾。

##### lpad、rpad

用指定的字符左（右）填充到指定长度。
若原字符比指定长度长，则从左（右）开始截取指定长度字符。

```mysql
select lpad('123',5,'*');
select lpad('123',2,'*');
```

第一个结果为**123。
第二个结果为12。

##### replace

替换字符

```mysql
select replace('abcdefg','c','e');
```

结果为abedefg。

#### 数学函数

##### round

四舍五入，默认保留整数部分

```mysql
select round(1.55);
select round(1.556,2);
```

第一个结果为2。
第二个结果为1.56。

##### ceil、floor

向上取整，返回>=（<=）参数的第一个整数

```mysql
select ceil(1.00);
select ceil(1.1);
```

结果分别为1和2。

##### truncate

从指定的小数位数开始舍去

```mysql
select truncate(1.99999,1);
```

结果为1.9。

##### mod

取余，与%运算符作用相似。
其返回值与被除数的正负性有关。

#### 日期函数

##### now

返回当前系统日期+时间。

```mysql
select now();
```

##### curdate

返回当前系统日期，不包含时间。

```mysql
select curdate();
```

##### curtime

返回当前系统时间，不包含日期。

```mysql
select curtime();
```

##### 获取日期的指定部分

有很多函数，对应的英文即函数名，同时，月还可以通过monthname获取对应月份的英文名。

```mysql
select year(now());
select month(now());
select monthname(now());
select date(now());
select hour(now());
select minute(now());
select second(now());
```

##### str_to_date

通过指定的格式，将字符转换为日期。

```mysql
select str_to_date('20-9 2002', '%d-%c %Y');
select str_to_date('09-20-2002', '%m-%d-%Y');
```

结果都为2002-09-20。
其参数选择，可在下图查看

![日期参数](/images/MySQL命令（二）/日期参数.jpg)

##### date_format

将日期转换为指定的格式（就是str_to_date的反转）

```mysql
select date_format(now(), '%y年%m月%d');
```

#### 其他函数

比较少，也没什么可讲的，就单纯给出来好了。

```mysql
select version();
select database();
select user();
```

#### 流程控制函数

##### if

MySQL的if函数是包含了if和else的效果。

```mysql
select if(10>5,'大','小');
select if(5<10,'小','大');
```

结果分别为大，小。

##### case

第一个用法就相当于java中的switch case。

```mysql
select b
case c
when 1 then b*1
when 2 then b*2
when 3 then b*3
else b*4
end
from a;
```

含义为，从a表中查询b列，当c列满足等于1的条件时，值为b\*1，满足等于2的条件时，值为b\*2，以此类推。都不满足的情况，值为b*4。

第二个用法也就是类似Python中的elif。

```mysql
select b
case c
when c>1 then b*1
when c>2 then b*2
else b*3
end
from a;
```

因为when的后面可以识别条件判断，所以能起到多重if的作用，也就是Python的elif。

### 分组函数

有sum（求和），avg（求平均值），max（最大值），min（最小值），count（求总个数）五个函数。
用法只需传递一整个列。

```mysql
select sum(b)
from a;
```

五个用法一致。
其次，sum和avg只能用于数字计算，而另外三个则可以对任何类型字符使用。

此外，都可以与distinct搭配使用，达到去重统计。

```mysql
select sum(distinct b)
from a;
```

#### count

count除了统计单列总行数，也可以通过

```mysql
select count(*), count(1)
from a;
```

来统计表内总行数，因为单列可能会有null，判断为null则不统计。
其中，count(*)，count(1)在不同引擎下有不同的效率，myisan下，count(\*)比count(1)快，而另一个innodb引擎，效率相差不大。

最后，分组函数不能与普通查询共用，会限制显示数据的具体数量，除非使用group by后的查询字段。
