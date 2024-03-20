---
# type: docs 
title: MySQL命令（四）
date: 2023-06-10T13:14:51+08:00
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
authors: Shazixnar
---

前面学完了查询的命令，下面就该是增删改了。

<!--more-->

## 增（插入语句）

插入语句有两种方式，不过核心的都是insert关键词。

### values方式插入

语法为

```mysql
insert into 表名 (列名1, 列名2...)
values (值1, 值2...);
```

其中，有几点需要注意。

1. 列名数量和值的数量必须一致。
2. 且设置为，不为空的列必须插入有效值。
3. 可为空的列名可直接省略不写，对应的值也省略不写。或写上列名，对应的值填写为null。
4. 列名顺序可随意调换，但是值与列的对应关系依旧是从左往右一一对应。
5. 列名可直接全部省略，但这样做即默认给表的所有列插入值。

### set方式插入

语法为

```mysql
insert into 表名
set 列名1 = 值1, 列名2 = 值2...;
```

这个没什么可讲的，主要注意事项和values一致。

### 两种方式比较

values可插入多行值，中间使用逗号隔开。

```mysql
insert into 表名 (列1)
values (值1)
, (值2);
```

这样就可以同时在列1插入两个值。
而set方式不能。

其次，values还支持子查询插入。

```mysql
insert into 表名 (列1)
select 1;
```

将values替换为select关键词，然后对应写查询语句即可，支持各种查询语句，只要列可与查询结果一一对应。
而set方式不能。

## 改（修改语句）

修改语句也可分为单表修改和多表修改。

### 单表修改

语法为

```mysql
update 表名
set 列1 = 新值1, 列2 = 新值2...
where 筛选条件;
```

其实也就是插入语句中set方式，注意事项一致。
另外，一定要添加where筛选，要不然更改数据时，会把所有的列全改了。

### 多表修改

语法为

```mysql
update 表名1
inner | left | right join 表名2 on 连接条件
set 列1 = 新值1
where 筛选条件;
```

也就是加入了联表语句。

## 删（删除语句）

和改一样，分为单表和多表删除，还有一个直接删除整个表。

### 单表或多表删除

语法为

```mysql
delete from 表名
where 筛选条件;
```

如果是多表，则一样添加联表语句。
不可单独删除某一个值，只能删除一整行。

### 删除整个表的数据

语法为

```mysql
truncate table 表名;
```

没什么可说的，因为是直接清空。

但需要注意的地方是，若列含有自增属性，且插入时没有指定值。
则当使用delete删除（例如删除id = 12）的数据，并再次插入数据，且不指定id数值，让其自增。则新插入的数据id = 13。（即从断点开始自增）
而使用truncate直接清空数据以后，再插入数据，则无论之前有多少数据，都是从1开始。
