---
# type: docs 
title: MySQL命令（一）
date: 2023-05-02T19:47:16+08:00
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

包含基础命令，select一部分命令

<!--more-->

首先是数据库内容，之后的命令结果示意图，都会用这个数据库来作为示例。所给的命令只是一个公式，加上文字描述可能会有混淆，建议配套图片以及本地手动操作自行理解。

[点我下载绝密数据库~](/documents/MySQL命令（一）/myemployees.sql)

把上面的数据库下载，然后放到自己可视化什么什么的里面以后，打开看看表结构，如下

![数据库结构](/images/MySQL命令（一）/数据库结构.jpg)

至于内容数据什么的，就自己打开来看看。

## MySQL最基础的命令

### show databases

查询所有数据库

### use 数据库名

打开指定数据库

### show tables (from 数据库名)

不加from就是查询当前数据库，加from就是查询指定数据库名

### 创建表

create table 表名（
        列名 类型，
        列名 类型，
        ……
        ）;

### desc 表名

查看表结构

### select version()

查看MySQL版本，或者在cmd界面输入mysql --version或mysql --V

### 语法规范

每一行完整的语句后面都必须用;或者\g结尾。语句都不区分大小写，但建议关键字大写，表名，列名小写。可以缩进，也可以注释，用#，--或者/**/来注释文字

## 查询，select关键词

### 基本格式

```mysql
select 列名 from 表名;
```

列名等可用*来替代，直接查询所有（但是最好不要用，因为速度很慢）。查询多个列名时，中间可用逗号隔开。可使用\`列名\`格式，将关键词标记为字段名。
列名，常量，函数都可以作为查询对象，查询多个时可用逗号隔开。查询常量时，列名就是常量或者表达式本身，显示的是结果。

![基本格式](/images/MySQL命令（一）/基本格式.jpg)

### 起别名

```mysql
select 列名 (as) 别名 from 表名;
```

as为可选，直接空一格输入别名也同样起效。当别名有关键词或空格，#等特殊字符，可以使用单双引号，如'别名'或"别名"这样的格式来标记，推荐使用双引号。

![起别名](/images/MySQL命令（一）/起别名.jpg)

### 去重

```mysql
select distinct 列名 from 表名;
```

![去重](/images/MySQL命令（一）/去重.jpg)

### 拼接

查询的同时，将两个列表的值拼接为一个值，则用concat()函数。

```mysql
select concat("列名", "+", "列名") from 表名;
```

concat函数内接收字符，使用逗号隔开。注意，如果其中有值为null，则所有结果全为null，处理方式为判空（如下）

![拼接](/images/MySQL命令（一）/拼接.jpg)

### 判空

使用ifnull()函数来判空

```mysql
select ifnull("含有null的列名","任意字符") from 表名;
```

效果为，如果值为null，则替换为第二个字符，不是null则不替换。

![判空](/images/MySQL命令（一）/判空.jpg)

### 条件查询

#### 条件运算符

\>，<，=，!=，<>，>=，<=。

```mysql
select a from tables where b>100;
```

代表，在tables表中，查询a列中所有b>100的值。

![条件运算符](/images/MySQL命令（一）/条件运算符.jpg)

#### 逻辑运算符

&&（and），||（or），!（not）

起连接条件表达式的作用。

#### 模糊查询

关键词有：like，between and，in，is null | is not null。

##### like

一般和通配符使用，用法如下

```mysql
select * from a where b like '%c%';
```

代表从a中查询所有，b列中任意一个字符含有字符c的结果。

![like1](/images/MySQL命令（一）/like1.jpg)

```mysql
select * from a where b like '__c_d%';
```

代表从a中查询所有，b列中，第三个字符为c同时第五个字符为d的结果。

![like2](/images/MySQL命令（一）/like2.jpg)

当需要限定的字符中有_等需要转义的字符时，可用下面两种方法。

```mysql
select * from a where b like '_\_%';
select * from a where b like '_$_%' escape '$';
```

\依旧可以作为转义使用。或者使用任意一个字符，示例为$，然后后面加上excape '\$'，即可作为转义符使用。

##### between and

作用等同于同一个条件的区间限定，如"a between 1 and 20"等同于"a >= 1 and a <= 20"
不能随便调换数值，"between"完全替代>=的含义，而"and"也就完全替代<=的含义

##### in

```mysql
select * from a where b in ('c','d','e');
```

代表从a中查询所有，b列中值为c，d，e中的其中一个的结果。
不能与like的通配符使用，因为in的逻辑是取用=的条件。

![in](/images/MySQL命令（一）/in.jpg)

##### is null | is not null

=或<>不能用于判断是否为null值，此时就需要用is null或者is not null判断。
但是is不能用于普通的数值判断。

## 额外补充

### +号的作用

只有一个作用，就是运算符。

```mysql
select 10+90; #作为运算符加算
select '10'+90; #如果有一方为字符，则将转换为数字进行运算
select 'a'+90; #如果字符无法转换为数字，则值为0，并再加算
select null+90; #如果有一方为null，则结果为null
```

### 安全等于<=>

作用与=相同，但是可以判断null，也可以用于普通的数值判断。就相当于普通的=与is null的结合。
