---
# type: docs 
title: MySql的安装
date: 2023-05-02T14:06:46+08:00
featured: true
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

使用的MySQL版本是5.5，之前有安装过8.0（然后密码给忘了），下面就是，如何将MySQL8.0卸载，并且再重新装上5.5的过程

<!--more-->

## 卸载MySQL

首先是停掉MySQL服务，方法挺多的，比如管理员模式打开cmd，输入net stop 数据库名。其他的网上一搜就有（而且我本身，似乎就根本启动不了服务，所以不需要这一步）

然后是从磁盘上面将MySQL8.0卸载，推荐用卸载软件或者系统自带的东西卸载。我用的是geek，国外的一个免费软件，挺好用的（也比不少国内的东西良心），卸载的同时也可以关注一下卸载目录，自己再去文件里找找有没有少删掉的东西。

下一步，找注册表，win10可以直接左下角搜索，或者win+R运行regedit.exe，找到HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services目录下的MySQL或者其他指定过的MySQL名，删除

做完以后，cmd里也可以再试试mysqld remove命令，这样应该就算删除完了

## 安装MySQL5.5

### 安装文件

打开安装包，一开始的界面我就不详细说了，一直到三选一的界面

![三选一](/images/MySql的安装/三选一.jpg)

选择中间的Custom来自定义一下。进去以后，点下面的Browse选路径。选自定义主要就是为了这个，我个人之前按照默认路径安装，然后发现......路径在powershell里根本cd不进去，所以就再删了一次然后重装，并且自己选了绝对不会冲突的路径（C:/MySQL_5.5/）

### 配置文件

当安装好以后，安装界面最后会有一个launch，这个启动的exe就是用来配置的程序。如果没有启动，可以自己去bin目录下找，我这里的路径就是C:/MySQL_5.5/bin/MySQLInstanceConfig.exe

![MySQLInstanceConfig](/images/MySql的安装/MySQLInstanceConfig.jpg)

然后在这里，推荐其实是先关掉程序，然后右键属性→兼容性，选择XP，再管理员启动。

打开以后，点第一次next，会看到出现了两个选项，一个是5.5，一个是8.0的配置文件（虽然我本地都删了，但是不知道为什么还会有）

![my](/images/MySql的安装/my.jpg)

这里5.5没有亮红，是因为我已经配置过了，正常来说是亮红的
选择好以后，如果不想仔细了解，可以全部默认直接跳过

![端口号](/images/MySql的安装/端口号.jpg)

这里，最需要注意的就是port number，因为可能存在端口号被占用的情况，可以自己在cmd中运行

```cmd
netstat -ano | findstr : 端口号
```

来检查是否被占用。被占用了就需要结束掉进程，或者换一个端口号。
再点一下next，来到下一个界面

![utf8](/images/MySql的安装/utf8.jpg)

推荐是选择第三个，并且选择utf8（不是utf-8）

再下一页，就是设置MySQL的名字以及是否加入系统变量。反正是推荐系统变量加入，然后MySQL的名字的话，就看个人要不要改

再下一页，是设置密码的地方。密码一定要记住，密码一定要记住，密码一定要记住！

要是密码忘了，改起来非常麻烦，要不然就是完全卸载再重装（本来想用8.0，但是来用5.5，就是因为，密码忘了！）

另外的选项，都可以不选，不过可以把密码底下的那个小框框点上，是选择是否允许远程机来访问的。最底下的那个是创建用户，不是必需。

最后点一次next，就是配置安装了

![execute](/images/MySql的安装/execute.jpg)

直接点execute，上面四个点是来看进度的。如果卡在第一个点没动，根本没打勾，并且兼容性什么都弄好了，可以选择重启一下（我是这样解决的），然后第三四条错误的，具体可以看[这个链接](https://blog.csdn.net/m0_67394002/article/details/124322134?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-124322134-blog-78433177.235^v32^pc_relevant_default_base3&spm=1001.2101.3001.4242.1&utm_relevant_index=3)，给了挺多的解决办法

我本人就是卡在第三条的start service，然后本地cmd能出来mysql服务，自己net start mysql的时候，无法访问，搞来搞去无非就是，再找找是不是文件没删干净，然后改了mysql安装的路径，还有就是mysqld remove命令在cmd运行一下，一直到mysql和mysqld都没有反应之后，再重新安装，就成功了

## 总结

说实话，安装完了，我也是懵的，不敢保证方法一定有用
