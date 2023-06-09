---
# type: docs 
title: MySQL命令（三）
date: 2023-05-12T18:39:12+08:00
featured: false
draft: true
comment: true
toc: true
reward: true
pinned: false
carousel: false
series:
 - blog
categories: [
    笔记
]
tags: [
    MySQL
]
images: []
authors: yfdkldmm
---

涵盖了其他的一些东西

<!--more-->

部分内容比较基础，而有些内容比较难说清，所以给图解释。

## 分组查询

在使用了分组函数以后，想要依次为每个部分进行统计，就需要用到的查询方式

```mysql
select count(*), b
from a
group by b;
```

代表，从a表中查询，b列的每一项的个数。

### 分组前筛选

```mysql
select count(*), b
from a
where b like '%a%'
order by b;
```

跟之前的条件筛选没有太大区别，在分组查询的时候加入where语句即可。

### 分组后筛选

```mysql
select count(*), b
from a
group by b
having count(*)>2;
```

分组后的筛选使用having关键词，并写在分组后，同时可以与分组前筛选共同使用。

```mysql
select count(*) b
from a
where b like '%a%'
group by b
having count(*)>2;
```

语句意思为，从a表中查询，b列每一项，含有a的，个数总和大于2的数据。

### 分组排序

跟之前的排序没有什么区别。

```mysql
select count(*) b
from a
group by b
having count(*)>2
order by count(*);
```

还是从a表查询，b列每一组个数大于2的数据，按个数升序排序。

## 连接查询

用于在两个不同的表中查询数据。

```mysql
select 列1, 列2
from a, b
```

代表从a表中查询列1，从b表中查询列2。

但是如果当两个列的数量不同时，就会出现笛卡尔乘积现象。
要解决这个问题，就需要添加有效链接。

```mysql
select 列1, 列2
from a, b
where a.列3=b.列4
```

添加的链接就为，当a表中的列3的值=b表中的列4的值时，查询列1和列2的内容。

### 连接查询分类

按功能分可分为：

1. 内连接
   1. 等值连接
   2. 非等值连接
   3. 自连接
2. 外连接
   1. 左外连接
   2. 右外连接
   3. 全外连接
3. 交叉连接

同时，按照语法来分，又能分成92标准和99标准。下面的先是92标准的语法。

#### 内连接

内连接是查询多个表，或同一个表内，都拥有的数据。

##### 等值连接

```mysql
select 列1, 列2
from a, b
where a.列3=b.列4
```

这个即为等值连接。

同时，也可以为表起别名。

```mysql
select a表.列1, b表.列2
from a a表, b b表
where a表.列3=b表,列4
```

虽然看起来更抽象了，就只是展示一下怎么使用。
对于不同表的同一个名字的列，需要在前面加上表名.来指代。
另外就是，如果为表名起了别名，则不可以使用原来的表名。

![等值连接别名](/images/MySQL（命令三）/等值连接别名.jpg)

```mysql
select a表.列1, b表.列2
from b b表, a a表 
where a表.列3=b表,列4
```

其中，表的位置是可以调换的。

如果需要加入筛选条件，那么就需要使用and连接。

```mysql
select a.l1, b.l2
from a, b
where a.l3 = b.l4
and a.l1 is not null;
```

加入分组函数是可以直接加入的。

```mysql
select count(*), a.l1
from a, b
where a.l3 = b.l4
group by a.l1;
```

加排序也是一样直接order by加在后面即可。

如果想连接查询三个表，也同样可以。

```mysql
select a.l1, b.l2, c.l3
from a, b, c
where a.l4 = b.l5
and b.l5 = c.l6;
```

and的使用次数不限，还可以继续加筛选条件等。

##### 不等值连接

用法与等值连接一致，将=条件变为其他即可。

##### 自连接

也就是从同一个表中建立连接查询。

```mysql
select a1.列1, a2.列2
from a a1, a a2
where a1.列3 = a1.列2;
```

需要注意的是，自连接必须给两个不同的表分别起别名，也就相当于把同一个表，分离出两个，相同内容但是表名不同的表，此时按照等值或不等值连接那样查询即可。

![自连接](/images/MySQL（命令三）/自连接.jpg)

#### 99语法标准的写法

```mysql
select a.列1, b.列2, c.列3
from a
inner join b on a.列4 = b.列5
inner join c on a.列4 = c.列5;
```

也就是将多个表，分成inner join，并且用on来一一对应连接条件，其他语句则照常添加即可。

#### 外连接

外连接就是查询在主表中有，而从表中没有的数据。

##### 左（右）外连接

左右的区分只是主表和从表的判定不同。

```mysql
select b.*, a.列1
from a
left outer join b
on a.列2 = b.列3
where b.列3 is null;
```

```mysql
select b.*, a.列1
from b
right outer join a
on a.列2 = b.列3
where b.列3 is null;
```

![外连接](/images/MySQL（命令三）/外连接.jpg)

均为一个功能：查询b表所有数据，条件为a列2中的值没有对应的b列3的值。
其中，主表都为a表，从表是b表。
所以，left左边是主表，right右边是主表。
上图的结果，就是因为employees表中，一个员工的对应department_id为null，在对应的departments表没有对应数据，所以输出为null。
说法比较绕，可以看底下的总结图来理解。

##### 全外连接

```mysql
select a.列1, b.列2
from a
full outer join b
on a.列3 = b.列4;
```

这就是查询，a表中有，而b表没有，和b表中有，而a表中没有，以及两个表都有的数据。

#### 交叉连接

也就是笛卡尔乘积。

```mysql
select a.*, b.*
from a
cross join b;
```

如果两个表行数一致，则一一对应查询。
如果不一致，最终行数就是a行*b行。

#### 小总结

92语法因为占用了where语句，同时可读性比较差，还有功能并不算多，所以并不推荐。

99语法有几个图可以看作用范围。

![连接总结1](/images/MySQL命令三/连接总结1.jpg)

上图就是左连接，内连接，右连接。
outer关键词其实是不必需。

![连接总结2](/images/MySQL命令三/连接总结2.jpg)

## 子查询

按结果集的行列数分类，可分为

1. 标量子查询（结果集只有一行一列）
2. 列子查询（结果集只有一列多行）
3. 行子查询（结果集只有多列一行）
4. 表子查询（结果集一般为多列多行）

按子查询出现的位置，可分为

1. select后面：仅支持标量子查询
2. from后面：支持表子查询
3. where或having后面：标量子查询（单行），列子查询（多行），行子查询
4. exists后面：表子查询

### where后面子查询

#### where后面标量子查询

应用场景很简单，说白了也就是将where后面的条件语句，替换为查询。

```mysql
select a.列1
from a
where a.列2 > (
    select b.列3
    from b
    where b.列4 = 1
);
```

从a表中查询列1，满足的条件是，从b表中查询列3，条件是b表列4为1。
之间的关系就是属于嵌套关系，前一个的条件表达式替换为另一个查询出来的具体值。
having后面的子查询和其他类似案例也是一致，都是属于一种嵌套查询。

#### where后面列子查询（多行子查询）

一般是搭配in/not in, any/some, all等字段来使用。

in和not in，就是字面意思，判断是否在子查询出来的结果中。

any和all，这俩其实都可以被其他语句平替。
比如any，any就是判断其判断条件是否满足这其中的任意一个值。
举个栗子：

```mysql
a > any(2, 3, 4)
```

只要a的值大于括号里任意一个值，即，a可以大于4，也可以只大于2。
所以这个语句，可以直接用min来代替（反正只要匹配一个，那我都大于最小的，不就直接满足了）

而all，就是完全相反。all是判断条件必须满足所有值。

```mysql
a > all(2, 3, 4)
```

这里面就是，a必须大于2，还要大于3，还要大于4。所以......我直接大于4不就完了。
直接换成max，然后，就一样的功能了。

所以any和all，必需使用的情况，其实挺少的。
最后就是，some是any的别名，同意义。

```mysql
select a.列1
from a
where a.列2 > all(
    select b.列3
    from b
    where b.列4 = 1
);
```

格式也就如上，没有太大变化。

#### where后面行子查询

其实也就是将多个where列子查询合在一起。

```mysql
select a.列1
from a
where (a.列2, a.列3) =
(
    select min(a.列2), max(a.列3)
    from a
);
```

不过这个用得比较少。

### select后面子查询

有点像连接查询。这边直接就用数据库里的数据展示。

```mysql
# 查询每个部门的信息和员工个数
SELECT d.*, 
(
SELECT COUNT(*)
FROM employees e
WHERE e.department_id = d.department_id
) 个数
FROM departments d;

SELECT d.*, COUNT(last_name) 个数
FROM employees e
RIGHT JOIN departments d ON d.department_id = e.`department_id`
GROUP BY d.`department_id`;
```

上面分别给了两种方式，第一种就是select子查询，在查询部门信息的时候，顺便嵌套一个查询员工个数。
第二种就是分组查询，这里count不能直接查询*，因为会统计到null，从而个数有误。

### from后面子查询

用在from后面，其实就是通过子查询的结果，来形成另一张表。通常都是再配合多表查询使用。
还是用一个案例来演示。

```mysql
CREATE TABLE job_grades
(grade_level VARCHAR(3),
 lowest_sal  int,
 highest_sal int);

INSERT INTO job_grades
VALUES ('A', 1000, 2999);

INSERT INTO job_grades
VALUES ('B', 3000, 5999);

INSERT INTO job_grades
VALUES('C', 6000, 9999);

INSERT INTO job_grades
VALUES('D', 10000, 14999);

INSERT INTO job_grades
VALUES('E', 15000, 24999);

INSERT INTO job_grades
VALUES('F', 25000, 40000);
```

先运行这一段，添加一个job_grades的表。

然后是案例：查询每个部门的平均工资等级。
肯定是要先查询部门的平均工资，然后再和刚刚创建的表联表查询，条件就是工资等级的条件。

```mysql
SELECT j.`grade_level`, a.ag, a.department_id
FROM job_grades j
INNER JOIN 
(
SELECT AVG(salary) ag, department_id
FROM employees
GROUP BY department_id
) a ON a.ag BETWEEN j.`lowest_sal` AND j.`highest_sal`
```

表的位置依旧是可交换的。其次，必须给子查询的表起别名，否则是找不到子查询出来的表。

### exists后面子查询（相关子查询）

首先是讲讲exists函数。

exists函数只会返回两个值，1和0，也就是布尔值，用于判断，exists函数里面的select语句，是否有结果。
用法如下。

```mysql
SELECT EXISTS
(SELECT last_name
FROM employees
WHERE employee_id = 10);

SELECT EXISTS
(SELECT last_name
FROM employees
WHERE employee_id = 100);
```

以上两个select语句出来的结果，分别为0和1。
因为表中数据没有employee_id=10的数据，所以exists的结果为0，反之，有employee_id=100的数据，所以为1。

用案例来进一步说明。

```mysql
# 查询有员工的部门名

SELECT department_name
FROM departments d
WHERE EXISTS (
SELECT last_name
FROM employees e
WHERE e.`department_id` = d.`department_id`
);

SELECT department_name
FROM departments d
WHERE d.`department_id` IN (
SELECT e.department_id
FROM employees e
);

SELECT d.department_name
FROM departments d
INNER JOIN employees e ON e.`department_id` = d.`department_id`
GROUP BY e.`department_id`;

```

上面用了三种方式。
第一种是用exists函数。先查询出部门名，查询的同时逐一根据部门名的部门id去比对是否有员工信息，如果有，则输出。
第二种是使用in来判断。
第三种是直接使用内连接和分组。

这三种方式在查询速度，可读性，可修改性都各有优劣。
比如，将案例改为，查询没有员工的部门名。

```mysql
# 查询没有员工的部门名

SELECT department_name
FROM departments d
WHERE NOT EXISTS (
SELECT last_name
FROM employees e
WHERE e.`department_id` = d.`department_id`
);

SELECT department_name
FROM departments d
WHERE d.`department_id` NOT IN (
SELECT e.department_id
FROM employees e
WHERE e.`department_id` IS NOT NULL
);

SELECT d.department_name
FROM departments d
LEFT JOIN employees e ON e.`department_id` = d.`department_id`
WHERE e.`last_name` IS NULL
GROUP BY d.`department_id`;
```

仅仅只是从查有，到没有，有的方法就有很大的改动。
使用exists的办法，只需添加not。
使用in的判断，则还需要在子查询中过滤null值，否则是不会有结果。
而直接多表查询的办法，就必须从内连接转变为外连接，并添加外连接的判断条件。

## 分页查询

主要用于限制查询数量。
语法如下。

```mysql
select *
from employees
limit 起始位置, 显示数量;
```

需要注意的是，起始位置是从0开始。当limit后面只有一个数字时，默认起始位置为0，显示数量为数字。
limit语句应当在select语句的最后方，执行顺序也是最后。
下图为sql语句的执行顺序。

![执行顺序](/images/MySQL（命令三）/执行顺序.jpg)

然后是开发中常用的格式。

```mysql
# 常用的格式
limit (page-1)*size, size
```

## 联合查询

主要用于需要查询多个表数据，但是两个表中没有可连接的键，就可以用union来合并查询结果

```mysql
select id, name from a
union
select b_id, b_name from b
```

需要注意的是，union两个查询结果的列是一一对应的。如上方为，id与b_id对应，数据显示为同一列，name和b_name也是为同一列。
且，对应的列数不一致时，则会报错。
union查询的结果默认去重，即两个表中有相同数据时，则只显示前一个表的数据，后续的表都因为去重而不显示。可使用union all来取消去重。

## 小结

一大长串的查询语句总算结束了。
