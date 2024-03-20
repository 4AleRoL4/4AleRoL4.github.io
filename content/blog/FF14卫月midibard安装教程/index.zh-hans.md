---
# type: docs 
title: FF14卫月midibard安装教程
date: 2023-06-09T14:54:22+08:00
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
    ff14,
    Dalamud,
    演奏
]
images: [/images/FF14卫月midibard安装教程/1.jpg]
authors: Shazixnar
---

ff14国服的卫月，以及midibard安装教程。

<!--more-->

## 卫月

首先是安装卫月。因为midibard是属于卫月的一个插件。

有不少的办法，比如直接安装卫月启动器，或者单独安装卫月注入。
先放个[QQ频道链接](https://qun.qq.com/qqweb/qunpro/share?_wv=3&_wwv=128&appChannel=share&inviteCode=CZtWN&appChannel=share&businessType=9&from=181074&biz=ka&shareSource=5)

### 卫月启动器

[XIVLauncher](https://github.com/goatcorp/FFXIVQuickLauncher/wiki/Info-(Chinese-Simplified))，卫月启动器，链接里面应该说得很清楚（而且我也没有用这个），就自行查看吧。
登录好游戏，应该就会自动注入。

### 卫月框架

不想使用卫月启动器，则可以使用卫月原本的安装方式。而安装说明，在[卫月本家](https://bbs.tggfl.com/topic/32/dalamud-%E5%8D%AB%E6%9C%88%E6%A1%86%E6%9E%B6)这边也说得很清楚。
（如果下载文件并运行都不会的话......那这我也只能推荐叫亲友帮忙）

下载并解压或安装以后，找到对应目录下。
比如，我是安装在F盘。

![卫月位置](/images/FF14卫月midibard安装教程/卫月位置.jpg)

然后图中有Dalamud.Updater.exe，打开即可。（一般是默认最小化，记得自行查看，打开界面需要右键小图标）
界面如下。

![卫月更新器](/images/FF14卫月midibard安装教程/卫月更新器.jpg)

按照我这样直接设置好也可以。一般情况，如果开启了自动注入，就不需要手动注入了。
现在，打开ff14即可。

在其标题界面的左上角，出现以下图标，即为注入成功。

![卫月图标](/images/FF14卫月midibard安装教程/卫月图标.jpg)

若是没有注入成功，则打开卫月更新器。此时界面上，检查更新下面还会出现一串数字，代表进程号，若是没有，说明没有检测到游戏，请重启。最后点击注入灵魂即可。
卫月注入时，游戏会卡顿为正常现象，也可通过游戏画面是否停顿来判断。

到这里，卫月框架就安装成功。

## 如何安装插件

### 自带插件

自带插件可直接在标题界面，鼠标移到图标上，出现

![卫月安装器](/images/FF14卫月midibard安装教程/卫月安装器.jpg)

点击卫月安装器，就可以在里面查看（推荐开梯子，因为全是github的仓库下载）。

另外，进入了游戏也不需要回到标题界面，直接在游戏内部按ECS键（键盘最左上角），打开面板也可以看到。

### 第三方插件

第三方插件通常需要用到对应的github仓库地址，当你找到对应插件安装教程地址时，通常都会给出（其具体使用方式也会在里面，除非你不会看英文以及不会翻译英文）

这里还是简单说一下。

#### 仓库地址安装

最常见的方式。

依旧是上方的那个界面，选择Dalamud设置，出现界面以后点测试版。如下图。

![Dalamud设置](/images/FF14卫月midibard安装教程/Dalamud设置.jpg)

往下翻可以找到自定义插件仓库，将地址复制进去，点上√，最后记得一定要保存，就完成了。
此时返回到插件安装，搜索对应的名字，即可下载安装（不过国内大概......经常不行吧。）

如果有梯子，在同样的界面中，有个代理设置，可以打开手动配置代理。
协议不清楚的可以一个一个试，地址一般都是127.0.0.1
而端口号，需要自行查看使用的梯子使用什么端口。

有时候，安装失败可能并不是网络问题（至少我的梯子真的没问题，文件也下载到本地了），也许是因为插件是国际服，而国服版本跟不上不兼容。
若是还想用，并且强制更新最新版本的话，可以尝试下方的开发插件安装。

#### 开发插件安装

需求拥有本地文件，可以托亲友或者任何方式搞来。

同样是卫月设置里面，测试版，会有一个开发插件的版块，在里面填入本地dll插件路径即可。
如下图，我用midibard2来举例。

![开发版插件位置](/images/FF14卫月midibard安装教程/开发版插件位置.jpg)

一般插件安装路径，也就是在```\Dalamud\XIVLauncher\installedPlugins```下，而pluginConfigs则是对应插件的配置信息。

另外推荐，有强制更新的，最好是把对应的仓库地址给删除，一般不影响使用（至少我midibard2可以）

## midibard

midibard1应该就是适配国服版本的国人所作，功能与midibard2无任何差异。性能方面......（我感觉1用起来比较怪）

github有详细的介绍和安装，还是中文

地址：[点我](https://github.com/akira0245/MidiBard)

## midibard2

midibard2在上方举例时，就已经提到过如何安装。现阶段，可能国际服6.4的版本无法在国服6.3版本中使用，所以需要开发插件安装旧版本。以往是可以直接仓库地址安装。

[midibard2的github地址](https://github.com/reckhou/MidiBard2)。

至于使用的话，可以参考midibard1，而更多的使用细节，请自行摸索，或可以加入我的合奏群与我探讨。

## 小小宣传一下

既然使用了midibard，那肯定需要midi对吧，要midi，那不如直接上midishow下载我的midi！[点击这里，即可跳转！](https://www.midishow.com/u/%E8%A6%81%E9%A5%AD%5C%E5%8F%AF%E6%80%9C%E5%A4%A7%E7%8C%AB%E7%8C%AB)（虽然可能，不适合大部分人吧，毕竟人数是永远的痛）

还有我的合奏群（神秘企鹅代号：806991247），技术性问题，比如midibard使用，midi制作等，包括卫月压缩包获取，都可以在群里找到，我能解决的都会尽量解决。
还会在合奏群宣布合奏时间（以及抓大猫来合奏），不想错过的建议加入~
