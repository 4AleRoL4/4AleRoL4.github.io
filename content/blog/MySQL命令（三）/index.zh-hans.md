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

## 分组查询

在使用了分组函数以后，想要依次为每个部分进行统计，就需要用到的查询方式

```mysql
select count(*), b
from a
group by b;
```

代表，从a表中查询，b列的每一项的个数。
