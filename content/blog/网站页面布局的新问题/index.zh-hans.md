---
# type: docs 
title: 网站页面布局的新问题
date: 2023-05-01T11:48:09+08:00
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
    额外内容
]
tags: [
    Hugo
]
images: []
authors: yfdkldmm
---

主要是因为想要自定义一下网站，然后找了一个朋友帮（zhi）忙（jiao），得到的一些经验。
先丢两个压缩图片的网址：[1](https://tinypng.com/)和[2](https://www.imagesmaller.com/compress-png/)，两个分别有不同的压缩率。2比1文件大小更小，但是同时也更模糊

<!--more-->

## 直接进行一个复制粘贴再改动

这部分是一个进阶的自定义改法，我也都不是很会，多亏朋友我才稍微有那么一点点眉目。主要原理，是hugo给出的覆盖原则，优先使用本地对应目录下的文件，其次再去找主题预设好的样式。

### 权重修改

而我因为是想......将docs里面的排布，从权重由小到大的往前排列（也就是直接反转网站默认的权重排列顺序），解决办法就是朋友去主题源码上面复制粘贴文件下来，再对代码进行修改，然后hugo逻辑复写。
对于我的这个需求，就是在[主题仓库](https://github.com/razonyang/hugo-theme-bootstrap)上，找到"\layouts\partials\docs\nav.html"这个文件，然后进行一个复制粘贴到本地对应的位置，也就是根目录\layouts\partials\docs\nav.html，对应着在20行左右的地方，将`{{- $pages := sort (where $section.Pages "Params.navWeight" ">" 0) "Params.navWeight" "desc" -}}`修改为`{{- $pages := sort (where $section.Pages "Params.navWeight" ">" 0) "Params.navWeight" "asc" -}}`就解决了
实际其实就只是改了最后的那一个参数，desc变为asc

### div生成顺序

根据此方法，我又去找了对应的一个文件，是\layouts\partials\sidebar\main.html文件，而我想做的，其实只是调节div的生成顺序，让blog详情里面的目录，在作者详情上方生成。
现在要做的，就是在浏览器上用开发者工具，去找到目录对应的类名，然后到main.html中对比，最后调节位置就可以了

## 粘性小工具sticky

那么我为什么想要调节div顺序呢？还是因为这可以跟着网页动的作者小工具很Cooooooool诶！
通过添加css的position: sticky，可以让对应的div跟着网页滑动一起移动，[指南](https://hbs.razonyang.com/v1/zh-hans/docs/topics/sticky-widgets/)其实也有给具体实现方法，
但，是！
他只给了profile的分块，如果我想让右边3个一起跟着动呢？
第一个方法就是，按照他的步骤（前俩步骤都可以不要，逻辑是单独生成一个div，然后再去复写profile这个类名，使其被生成的div给捕捉，实际可以直接进行scss的复写。），对三个部件都添加相同的参数。
然后你就会发现......粘是粘住了，但是布局就会......

![sticky1](/images/网站页面布局的新问题/sticky1.jpg)

因为sticky是根据top以及其他另外三个方向参数来决定黏着位置的，如果是简单地设置为三个div，且top为一致，就会出现挤在一块。或者就算自己去计算边距，然后给每个top都设置好，也会出现类似上图的样子。
对于这个问题，能想到一个非常简单的解决办法：直接把那一整块都给sticky不就完了？
所以，就尝试给上一层父级进行复写定义。但是很快就出现了问题，单纯地按照他给的模板，将.profile改为.container，没有任何效果，并且即使做任何别的修改，也无济于事。
在尝试N久以后，最后是朋友帮我解决的。
因为container是已经定义了flex，直接复写position: sticky是没有任何作用的，只能按下面办法写：

```css
.sidebar {
    .container {
        align-self: flex-start;
        position: sticky;
        top: 84px;
    }
}
```

加入align-self: felx-start，才有效。这是朋友确定了sticky本身不会有问题，只可能出现在flex上，然后网上搜，搜出来[这个链接](https://stackoverflow.com/questions/44446671/my-position-sticky-element-isnt-sticky-when-using-flexbox)，里面有大佬回答才解决了这个问题。
感谢他们

### 小工具附带问题

尽管通过上面方法将问题解决了，但是出现了新的问题，就是他的sidebar和container，其实几乎是所有页面右边作者小工具都使用同一个文件来定义的，也就是上面说到的\layouts\partials\sidebar\main.html文件。这本来是没有问题的，但是当我们将container整个定义sticky以后，就会发现点开blog，页面滑动的时候，右边一整块都在动，而目录就被压在底下根本看不到！
那我这目录还有什么用呢？
然后再看一眼div结构，会发现，新的目录跟profile并列，也无法再次拆分什么的。
那么解决办法，我就只想到了，要么直接把目录移到上面去（就如上面所说的做法），要么就是，没有目录时，对container进行sticky，有目录时，只对目录进行sticky，main.html也有类似if的写法检测目录是否生成，但是必须得对他的html写法进行一个了解，所以暂时......我就只用了第一个方法（这个也有弊端，目录特别长的话，是不能放全目录的）

## docs文件名

在整理docs那里面的文件，并打算上传我的小说的时候，发现怎么也不能和他默认模板那样，形成列表什么的，结果就自己研究了一下午，最后找出了问题。
最大的原因就是，他那文件名从index变成了_index，只有文件名作为_index，才能让其检测该目录下的其他文件夹

## 总结

一些小知识也不知道会不会用到，所以单独放出来，以后也会不定时更新一下，只要我对我的网站进行了新的重要改动的话
