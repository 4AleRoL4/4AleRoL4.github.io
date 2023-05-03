---
# type: docs 
title: 正式使用MySQL前的一些东西
date: 2023-05-02T15:18:09+08:00
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
    笔记
]
tags: [
    MySQL
]
images: []
authors: yfdkldmm
---

主要是在学习MySQL命令以前，还需要多了解的一些东西

<!--more-->

## my.ini配置文件

首先，my.ini文件就在你安装的根目录下，用txt或者notepad什么的打开就行，然后找到mysqld这一行（大致在67行），下面的就全是配置详情了。

### port

这个就是端口号，可以直接在这里改

### basedir

这个是MySQL的安装路径

### datadir

是MySQL数据存放的路径

### character-set-server

修改编码

### default-storage-engine

数据库的存储引擎

### sql-mode

设置语法模式

### max_connections

数据库的最大连接数

### 小总结

内容非常少，也没有特别需要注意的，除了改了这些东西以后，要重新启动服务

## MySQL的登入登出

### 直接使用提供的命令行

安装好MySQL以后，在开始页面里面就会多出来一个类似cmd的东西，可以直接点开，会让你直接输入MySQL密码，输入以后就跟下图差不多

![MySQLcmd](/images/正式使用MySQL前的一些东西/MySQLcmd.jpg)

再输入exit或者Ctrl+C，会直接关掉这个界面

### 使用cmd等登入MySQL

上面的方法虽然可以登入，但是只能登入root用户，而cmd可以登入其他用户。
先以登入root用户为例

```cmd
mysql -h localhost -P 3306 -u root -p
```

输入以上内容以后，就会提示输入密码，跟上面的图类似。
对于这些参数

-h，是代表主机

-P（注意一定是大写），是代表端口号

以上两个如果是直接连接本机，可省略

-u，代表用户名

-p（一定要加上，后面为空代表不默认传递），代表密码，可以直接在后面加入密码，但是屏幕上会显示密码

## 总结

没多少东西，单纯作为一个小笔记
