---
# type: docs 
title: MySQL命令（五）
date: 2023-06-15T19:12:01+08:00
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

这一部分是DDL语言。也就是对库和表的管理。

<!--more-->

## 库的管理

### 库的创建

```mysql
create database [if not exists] 库名;
```

没什么可说的。至于if not exists，就字面意思，让其不会在已经有同名数据库时报错。

### 库的修改

一般来说，不会去改库的名称，以前版本有语句可以修改，但是会丢失数据。
如果真要修改库的名称，就自己去MySQL安装目录下的data里面重命名。

```mysql
alter database 库名 character set gbk;
```

如上，可以修改库的字符集。

### 库的删除

```mysql
drop database [if exists] 库名;
```

没什么可说的。

## 表的管理

### 表的创建

```mysql
create table 表名(
    列名 列的类型【（长度） 约束】,
    列名 列的类型【（长度） 约束】,
    列名 列的类型【（长度） 约束】,
    ...
    列名 列的类型【（长度） 约束】
);
```

用【】括起来的地方是可选。

比如，创建一个book表。

```mysql
create table book(
    id int, # 编号
    bname varchar(20), # 图书名
    price double, # 价格
    authorid int, # 作者编号
    publishdate datetime # 出版日期
);
```

如上就创建出book表。

### 表的修改

#### 修改列名

```mysql
alter table 表名 change column 原列名 新列名 数值类型;
```

语法如上，改列名时必须和数值类型一起输入，所以也就可以在改列名时，和数值类型一起修改。

#### 修改列的类型或约束

```mysql
alter table 表名 modify column 列名 新数值类型;
```

语法如上。

#### 添加新列

```mysql
alter table 表名 add column 列名 数值类型;
```

语法如上。

#### 删除列

```mysql
alter table 表名 drop column 列名;
```

语法如上。

#### 修改表名

```mysql
alter table 表名 rename to 新表名;
```

语法如上。

### 表的删除

```mysql
drop table 表名;
```

没什么可说的。

### 小细节

如果需要重新建立数据库或者表，一般先配合if exists语句删除，再创建。

```mysql
drop database if exists 旧库名;
create database 新库名;

drop table if exists 旧表名;
create table 表名();
```

### 表的复制

#### 仅仅复制表结构

```mysql
create table 表名 like 被复制的表名;
```

这样就能从like后面的表中，复制表结构。

#### 其他复制方法

通过子查询来达到效果。

##### 复制全部数据

```mysql
create table 表名
select * from 被复制的表名;
```

##### 只复制一部分数据

```mysql
create table 表名
select 列名
from 被复制的表名
where 筛选条件;
```

就是利用子查询来将想要的数据复制过去。

##### 只复制一部分的结构（字段）

不想带有数据，且只想要其中一部分的列，可以将筛选条件变为false，并子查询对应的字段。

```mysql
create table 表名
select 列名
from 被复制的表名
where 0;
```

这样就可以只将select的字段复制过去，而不带有任何数据。
