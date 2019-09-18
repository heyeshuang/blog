---
author : "HeYSH"
type : "post"
tags :
    - 笔记
    - 软件
categories :
    - 折腾
title: "笔记软件踩雷"
date: 2018-11-17T18:26:30+08:00
draft: false
---

大概是2017年的时候，我一直在使用的为知笔记开始收费了。在交了一年的订阅费（并写了小于10篇笔记）之后，我决定换一款新的笔记软件。

如果对自己的要求有清楚的认识，当查找 *poor man's somewares* 时，[AlternativeTo](https://alternativeto.net/software/evernote/)和[Slant](https://www.slant.co/topics/2463/~best-evernote-alternatives)这两个网站经常能够带来一些灵感。对于我来说，笔记软件需要：

- （扩展的）Markdown格式，支持代码块和公式
- 支持多种格式的附件，而不是只支持图片
    - 附件的链接可以插入笔记中
- 导入导出方便，不要像臭名昭著的有道笔记一样
- 网页剪辑
- 跨平台
- 支持同步

这样的话，类似google keep、evernote的软件和收费软件（……）就首先被排除了；在markdown出现之前的老牌笔记软件也自动忽略。然后我试用了当红的boostnote和……什么来着，但这两个软件对附件的支持都不太好。最后，有两个软件满足我的大部分要求：

## [VNote](https://tamlok.github.io/vnote/)

美丽的Qt程序，纯Markdown保存，直接粘贴HTML到笔记，甚至还有Vim按键绑定，一切都很好，除了没有同步和Android客户端；当然，纯文本文件可以很容易地用git同步，所以问题变成了另外两个：没有合适的git Android，以及没有Android上的Markdown阅读器[^1]。

## [Joplin](https://joplin.cozic.net)

用javascript写应用虽然被很多人鄙视，但确实真正解决了跨平台运行的问题。这个程序似乎连Android客户端都在同一个代码仓库里，如果不在意肉眼可见的卡顿的话，react native能够让人少写好多代码，反观qt-Android……

咳咳，扯远了。总之Joplin有Android客户端，而且自带onedrive同步功能。Nothing to complain[^2].

另外还有两个小问题。

## 关于文章剪辑

试图将HTML转回markdown，就像是试图将pdf转回TEX、减少宇宙的熵、或者是把石头推上山一样徒劳，不过，仍然有[勇士](https://github.com/domchristie/turndown)在尝试这个任务，效果嘛……差强人意，从两个方面理解都可以。

## 关于为知笔记导出

> 感觉基本上没什么戏……

为知笔记的markdown格式笔记不是纯文本，而是用HTML储存的，这就是说，打开笔记之后，需要看一整段
`<!DOCTYPE html><html lang="en"（下略`
才能看到自己的笔记。导出txt格式可能会好一些，代价是损失所有图片；另外，导出时是没有附件的……

理论上，这些都可以用一个脚本来解决，不过我看着自己一百多篇各种格式的笔记，决定还是把它们扔在硬盘里吧。“反正以后光纤也跟我没关系了……”……大概。

[^1]:在VNote的某个Issue里提到过[Markor](https://github.com/gsantner/markor)，不过问题没有这么简单：git从来都没有自动同步功能，所以每次写完笔记之后还要push一次。

[^2]:除了导出文件时的一个[issue](https://github.com/laurent22/joplin/issues/853)。It looks like Chinese to me. *18-11-20更新：* 开发者回复我了！开心～

---

来自2019-09-18的更新：

最近在V2EX上看到了一个叫[notable](https://github.com/notable/notable)的软件，思路上和VNote类似，也是使用文件夹代替数据库，笔记用纯文本格式保存在磁盘中，而且对于图片和附件的位置处理得也很聪明。

另外，在这个项目的主页上还列出了很多常见的笔记软件，而且画了个漂亮的表格（在[这里](https://raw.githubusercontent.com/notable/notable/master/resources/comparison/table.png)），来比较他们的区别，对某些功能有执念的朋友可以去看看。

当然我还是继续用joplin。*feels good man.*

---

来自2019-9-18更晚些时候的更新：

我又翻了一下那个项目的README，发现那个项目不再开源了。好吧，当我今天没有更新。