---
# type: docs 
title: Blog搭建及问题
date: 2023-04-27T09:20:00+08:00
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
    Hugo
]
authors: Shazixnar
images: [/images/blog搭建及问题/index.jpg]
---

## 首先确立blog框架

我使用的是[Hugo](https://gohugo.io/)框架来搭建blog，需要安装Go语言以及Git。

<!--more-->

## 安装步骤

1. 首先Hugo有自带的安装步骤，按照那上面一步一步来。
   我先是选择了Build from source，从源代码搭建（然后失败，留档视为耻辱，我正确安装的方式在第三条）。
   安装Go，然后添加环境变量，最后打开cmd，输入以下代码

    ```cmd
    go version
    ```

    出现下图类似的版本编号，即Go环境变量配置完成

    ![环境变量](/images/blog搭建及问题/Go环境变量.jpg)

2. 之后返回Hugo，按照步骤该执行以下代码

    ```cmd
    go install -tags extended github.com/gohugoio/hugo@latest
    ```

    我出现了如下错误

    ![Hugo通信](/images/blog搭建及问题/Hugo通信.jpg)

    之后再在下面打一行

    ```cmd
    go env -w GOPROXY=https://goproxy.cn
    ```

    之后再次输入上面的代码，之后冒出来一大串，等待安装完成就行

3. 直接使用包管理工具安装
   hugo是给了三个办法，我选择了[Scoop](https://github.com/ScoopInstaller/Install#for-admin%20for%20details)
   在安装Scoop前，需要PowerShell，正好hugo运行也需要PowerShell，所以就直接安装了吧，[直通地址](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.3)，然后在PowerShell中输入

   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

   再输入

   ```powershell
   irm get.scoop.sh | iex
   ```

   因为网络问题，可能会出现图下状况

   ![Scoop失败](/images/blog搭建及问题/Scoop失败.jpg)

   我自己是因为网络，无法连接，重复执行一两次以后，就成功了

   ![Scoop成功](/images/blog搭建及问题/Scoop成功.jpg)

   之后再根据hugo给的代码，通过scoop安装hugo

   ```powershell
   scoop install hugo-extended
   ```

   安装完成就如下图

   ![Scoop安装](/images/blog搭建及问题/Scoop安装.jpg)

   最后打一行代码看看是否真的成功

   ![hugo安装](/images/blog搭建及问题/hugo安装.jpg)

## 创建本地网站（本大标题废弃，留作耻辱，真正部署成功步骤在下一个标题）

1. 首先，在Hugo教程步骤上明确说明，不要使用cmd，也不要使用Windows PowerShell，而是要PowerShell（和Windows PowerShell是两回事），看到这肯定已经安装好了
2. 在powershell中cd到你要安装的目录下，开始按照网站给的代码一行一行输入

    ```powershell
    hugo new site quickstart
    cd quickstart
    git init
    git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke themes/ananke
    echo "theme = 'ananke'" >> config.toml
    hugo server
    ```

    一行一行地输入后，没有任何报错，就会有下图

    ![默认地址](/images/blog搭建及问题/默认地址.jpg)

    在第二行能找到localhost:端口号，将这串网站在自己浏览器打开，就会有一个默认的blog界面

    ![默认界面](/images/blog搭建及问题/默认界面.jpg)

    很普通，但至少说明网站框架有了。接下来按Ctrl+C停止服务，以便于后续的更改。

## 开始正式增加blog

尽管界面还很简略，但是已经可以作为一个blog开始发布md文档，那下面就是一些基础的操作
先别管别的，直接执行下面的代码

```powershell
hugo new posts/my-first-post.md
```

然后，下一行代码

```powershell
hugo service -D
```

当你再次去查看网页并刷新时，就会发现多出个内容

![first-post](/images/blog搭建及问题/first-post.jpg)

那么很显然，这就是我们刚刚使用默认模板直接创建的一个md文档，并且网站实时更新出来了。
既然到了这一步，就可以开始正式开始尝试将我们自己的md文档放在网站上了。

### md文档放在哪

首先根据我之前按照给出的命令行执行的内容，以及我自己更改的目录结构，最后我的blog网站项目是在D:\blog\quickstart的目录下，以后要启动项目，也得先cd到这个目录下才行。
接着，自己打开目录，会发现里面有很多的文件夹。
别的暂且不用管，专注于content文件夹，点进去，会有posts文件夹，再点进去，就会发现我们之前默认生成的一个md文档，文件名为：my-first-posts。
所以很明显，md文档放在主目录/content/posts/目录下，就会被网站识别上传（也许会有别的办法更改目录什么的，总之先这样好了）

### 编写md文档

呃，首先是想确认一下，如果你真的md文档什么都不会的话，可以[点这里](https://www.runoob.com/markdown/md-tutorial.html)，然后看着那教程来编写，需要用到的也不多，就是标题，正文，代码块，列表，链接，图片，这些就足够了，同时推荐vsc的一个小插件，名叫Markdown Preview Enhanced，这个可以在vsc中编写md文档并且实时预览，安装好以后在md文档里面右键，在菜单栏下面找到MPE:打开侧边预览，或者快捷键，先Ctrl+K，再按下V
md文档基础相关就到这，接下来是hugo相关的基础东西
首先打开生成的默认模板

![默认模板](/images/blog搭建及问题/%E9%BB%98%E8%AE%A4%E6%A8%A1%E6%9D%BF.jpg)

可以发现与普通md文档不同的是，没有一级标题，取而代之是---括起来的一块区域。
在这一块当中，就是hugo给出的一个默认的front matter，分别是三个属性（而且还有更多参数），具体内容可以自己查阅[hugo网站](https://gohugo.io/content-management/front-matter/)，我就直接以实用方式开说了。

#### title

title顾名思义，就是标题，在这里的作用，其实就是替代了一级标题，所以不需要我们自己再去创建一级标题，正文直接从二级标题开始即可。同时，这个一级标题也会体现在blog总览界面，所以功能比一级标题多一点

#### date

date就是blog的发布日期，具体写哪天，可以自己琢磨，格式也就是YYYY-MM-DDTHH:MM:SSZ（标准的RFC 3339格式），通俗点就是，直接使用UTC时间来标注

#### draft

draft（草稿），其值只有true或false（或者直接删除）。在生成静态网页的时候，如果是不带任何参数的hugo service，那么在本地预览的时候，是会忽略掉拥有draft=true的md文档的。而在后面加了-D参数，表示也要将标记为draft（草稿）的md文档加入预览中。具体如何使用这个参数，就自己去思考吧。
同时，和draft作用相似的，还有date（当设置的时间为未来时），publishDate（当设置的时间为未来时），expiryDate（当设置的时间为过去时），这三个以后再说。
总之就是，要么发布时记得把draft删掉或false，要么就是启动的时候加上--buildDrafts参数

### 发布网站

上面都做好了，那么就该最后一步了，将网站发布出去。
首先，按照步骤，需要去根目录中找到config.toml，来编辑一些东西

![网站配置](/images/blog搭建及问题/%E7%BD%91%E7%AB%99%E9%85%8D%E7%BD%AE.jpg)

baseURL里面就填写你的地址，如果还没有在github pages里面配置域名的话，那这个地方就是填写的是待会儿在github pages那个仓库的名字。languageCode就是语言+国家缩写，title的话，其实就是默认页面上面，那一排大大的字。最后的theme明显就是网站的那个主题模板，在正式开始美化前，先暂时不管吧
然后呢，就是去github创建仓库，并且命名为```username.github.io```，username就是你github的名字，比如我github名为4AleRoL4，那么就该写"4AleRoL4.github.io"，创建过程中要不要创建md文件看个人。
创建好以后，点setting→pages，然后build and deployment下，source选择github actions，那么在github上的配置就完成了。最后是记得去Code→绿色图标的code那边，将local下的https地址复制一下，后面使用git会用。
之后就是按着hugo教程来（因为我在使用这个方法的时候失败了，怎么也解决不了气得我第二天直接换了一个底子，部署和配置什么的，都不一样，所以这边就没法说的很详细）。
遇到的问题可以稍微说一下，教程中所说的创建.github/workflows/什么的.yaml文件，应该是创建在hugo server的目录下，而不是在hugo以后出现的public文件夹内，上传的时候是在quickstart目录下，进行上传。或者说......[请~](https://cuttontail.blog/blog/create-a-wesite-using-github-pages-and-hugo/#1-%E6%A6%82%E5%BF%B5%E6%90%AD%E5%BB%BA%E6%80%9D%E8%B7%AF%E5%92%8C%E8%BF%90%E8%A1%8C%E7%8E%AF%E5%A2%83)，这篇blog写得还不错，我没说到的细节也可以在这里得到补充，只可惜我跟ta的步骤几乎颠倒，也没办法再回头，索性不如，全部重头来

## 重新开始

这部分我觉得，应该没有多少参考性了，因为我使用的theme的安装方式有点奇特，是从github直接复制工程下来，再自己更改好以后上传到自己的仓库里，不需要hugo new site，也不需要自己创建yaml东西，所以跟前面的步骤，相当于是跨了很大一截（然后出现更奇怪的错误），作为记录，我还是稍微记一下

### 克隆文件

先贴一下我使用的[HBS主站](https://github.com/razonyang/hugo-theme-bootstrap)，然后在下方的介绍界面会找到，或者直接点击[HBS新手模板](https://github.com/razonyang/hugo-theme-bootstrap-skeleton)，在这个github页面下，就是我们需要复制下来的仓库。
在这个页面下的md文档也有说如何操作，我就稍微讲一讲。
第一步，自己创建以"username.github.io"命名的仓库，username替换为自己的github名字，我这里就是4AleRoL4。默认选项里的话，可以选择不添加md文档（反正最后是要改的），我是添加了md文档。
创建好以后，就可以回到本地，然后输入

```git
git clone https://github.com/razonyang/hugo-theme-bootstrap-skeleton.git blog
```

里面的参数blog其实就是接下来你的项目文件名，这里我就取了yfdkldmm-blog。
之后再按照下面3行依次每行执行

```git
cd yfdkldmm-blog
rm -rf .git
git init -b main
```

一定要小心rm -rf .git这行命令，请确保cd到对应目录下才开始输入
接下来就可以看到一大串文件

![blog文件](/images/blog搭建及问题/blog文件.jpg)

### 更改配置并第一次提交

之后是更改go.mod文件内容，第一行的

```mod
module github.com/razonyang/hugo-theme-bootstrap-skeleton
```

将其地址改为自己的仓库地址

```mod
module github.com/4AleRoL4/4AleRoL4.github.io
```

更改完以后，就可以提交到远程仓库，查看是否部署成功
但在此之前，请

```hugo
hugo
```

一下，输入这一行代码以后，会将你的网页打包成静态网页，并放入public文件夹中。每次在部署前，在本地修改了内容，就记得hugo一下之后，再开始上传，后面就不会再赘述了。
另外提一下，hugo以后，同样可以进入public文件夹中查看网页，只不过这时要用到别的东西，之后再说。
继续部署的配置吧。
首先，需要设置远程仓库连接（请将我的链接替换为自己仓库的链接，就在code→绿色的code里面）

```git
git remote add origin https://github.com/4AleRoL4/4AleRoL4.github.io.git
```

建立好连接以后，请一定记住，在提交远程仓库前，要先pull一下，与远程仓库同步一下，特别是创建仓库的时候设置了默认md文档的，如果不执行pull，就会无法继续下一步。首先，是需要本地提交一下

```git
git add .
git commit -m "First commit"
```

接着就是pull

```git
git pull --rebase origin main
```

此时，你打开你的README.md文档，可能会发现多了些>>>>>>>，<<<<<<这样的符号，如果了解git的，就知道这是需要处理的分支冲突，解决办法就是，将这些非常明显的字符全部删掉，并且留下你希望上传的内容，保存，接着继续输入以下内容

```git
git rebase --continue
```

之后，可能还需要再本地提交一次，最后是输入

```git
git push origin main
```

运行成功，刷新自己的github仓库，就会看到有很多东西上传上来了。
但是上面的部分，我不确定是否真的需要，因为我初次使用的时候，是用到了，但我没截图。当我再次更改README.md文件，并且提交上去时，却没有这部分步骤，改好文档，commit提交，pull下来直接push就完了，所以给不了什么具体截图

![git截图](/images/blog搭建及问题/git界面.jpg)

总之，要是有问题，尝试自己解决一下吧
最后，提交上去以后，回到github仓库，setting→Pages→Build and deployment→Source这里，应该使用默认的Deploy from a branch，同时在下面Branch，选择gh-pages，后面一个文件夹选/(root)，最后save一下，如下图

![pages设置](/images/blog搭建及问题/pages设置.jpg)

之后，网站应该就会自主部署了，详细信息可在Action里面查看

![Action](/images/blog搭建及问题/Action.jpg)

输入自己的域名，我的就是"4AleRoL4.github.io"，就可以访问了（只不过界面可能......不尽人意）

### 正确加载主题

如果点我的页面，和你自己的界面的话，会有非常大的差距，这是因为并没有完全配置好。在入门模板里面也还有着后续的操作需要去完成。
会出现大量空白，并且排布很乱的情况，这就是因为主题并没有成功加载到网页上，还需要在本地执行

```powershell
npm i
hugo mod get github.com/razonyang/hugo-theme-bootstrap@master
hugo mod npm pack
npm update
git add go.mod go.sum package.json package-lock.json
git commit -m 'Update the theme'
```

这一长串的代码（小提醒，go.mod里面的东西记得改哦），除此之外，你还需要去网站根目录下的/.github/workflows/gh-pages.yml的文件里面，大概第42行左右那里

```yml
- name: Build
run: hugo --minify --gc --enableGitInfo -b https://projects.razonyang.com/hugo-theme-bootstrap-skeleton/
```

在这一长串，将包含-b以及后面的东西全删了（原教程只说了-b，但我没找到，踩了一次坑，实际就是，网页还是那种缺东西的样子）。
总之，这些都搞好了，那么再一次提交到远程库上面，等运行完以后，再次点击，那么网站主题应该就彻底弄好了。
而这也说明，你的网站也就搭建成功！（不过只是有个骨架，核心的还得慢慢搞呢）

## 额外内容

### 关于gh-pages.yml文件的一些内容说明

这个其实是一位群友给我小编辑一下的图（而他也是从头指导我搭建好这个网站的人，感激之情无以表达），下面就放一放那个图

![gh-pages说明](/images/blog搭建及问题/gh-pages%E8%AF%B4%E6%98%8E.jpg)

### 域名配置

真的不会觉得"username.github.io"这个域名作为自己专属的blog域名，很俗吗？
那肯定必须得改！
首先第一步就是，得去买一个域名，自己百度一下阿里云或者腾讯云的域名购买，去那边买一个就好，或者有能力的可以选择"www.name.com"去购买。我是腾讯云买的。
其次对域名小说明一下，对于我个人的blog域名[yfd.kldmm.icu](yfd.kldmm.icu)这个域名，要买的其实是kldmm.icu，而yfd.是子域名，自己设置就好。然后价格，最通用的.com域名一直都非常贵，而别的不流通的.icu啊.top什么的，就会很便宜（另外，真的有人会用中文域名嘛？），其次跟你要买的长度有关，yfdkldmm.net和kldmm.net两个也是一个价格差，总之自己挑选吧
购买好以后，需要进入域名配置，大概就是这个界面

![dns域名](/images/blog搭建及问题/dns%E5%9F%9F%E5%90%8D.jpg)

点击你自己购买的域名，进入

![dns域名2](/images/blog搭建及问题/dns%E5%9F%9F%E5%90%8D2.jpg)

我这边已经配置过了，正常情况是空的，需要自己点添加记录开始添加。图我就不放了，只需要记住：

1. 主机记录写@代表空子域名，填写什么就是什么的子域名。
2. 我这里是使用CNAME方式，记录值就是你的"username.github.io"，都设置好了，这边就结束了

接着再回到github页面，在你网站的根目录下，创建一个名为CNAME的文件，内容为你的域名

![CNAME](/images/blog搭建及问题/CNAME.jpg)

最后，再去setting→pages设置你的自定义域名就可以了。[参考来源](https://www.zhihu.com/question/31377141)，不过这里面的方法是说记录值为IP，我本人尝试以后是报错，说请使用CNAME方式跳转

### 关于hugo以后的预览

执行hugo打包以后，所有的东西会被打包进public文件夹中，在里面你可以找到非常多的html文件，但是你无法直接打开，直接打开的话就会是各种奇奇怪怪的页面。这个时候就需要用到http-server。
首先可以通过node.js安装

```node.js
npm install --global http-server
```

安装好以后，就可以使用

```http-server
http-server [path] [option]
```

来预览打包以后的网页。path填写你的public文件夹路径，要么是绝对路径，要么是相对路径，以目前就在网站根目录下为例，path就是./public。option是可选，在[这个链接](https://www.npmjs.com/package/http-server)有详细说明，不过一般空着就行

另外就是......我本人的电脑似乎无法成功安装，显示安装成功后依旧无法使用。幸好还有免安装的办法

```node.js
npx http-server [path] [option]
```

通过这个方法，可以直接运行，不需要安装，我也是通过这个才实现了预览。

![http-server1](/images/blog%E6%90%AD%E5%BB%BA%E5%8F%8A%E9%97%AE%E9%A2%98/http-server1.jpg)

运行出来，可以看到端口号是8080，那么通过localhost:8080就可以访问了。

### 一些小问题

本来没有这个板块的，但是某一天我起来突然发现hugo server报错了，那就顺便记录一下。

![autoprefixerError](/images/blog%E6%90%AD%E5%BB%BA%E5%8F%8A%E9%97%AE%E9%A2%98/autoprefixerError.jpg)

错误提示就在上面，Error：Cannot find module autoprefixer
上网一查，或者仔细看那些给出的目录下，大致也能猜到autoprefixer是一个nodejs里面的模块。之前没有出问题，但是突然冒出来可能是因为......我改了npm的global模块位置。
解决办法也很简单

```npm
npm install autoprefixer
npm install autoprefixer -g
```

这两行都运行一下，就能解决了

![autoprefixersolve](/images/blog%E6%90%AD%E5%BB%BA%E5%8F%8A%E9%97%AE%E9%A2%98/autoprefixersolve.jpg)

## 总结

至此，作为一个单纯的网站搭建以及部署，就完成了。虽然这个网站的东西基本都不是你自己的，但至少，域名是我们自己的，已经可以给朋友炫耀一下下了。然后下一次blog，就是关于如何将这个网站，彻底变为自己的东西的相关资料
