---
author : "HeYSH"
type : "post"
tags :
    - font
    - 思源
    - Noto
    - linux
categories :
    - linux
title: "2017年的fontconfig设置"
description: "使用思源宋体和思源黑体"
date: 2017-10-29T13:40:50+08:00
draft: false
---

都7012年了linux还要自己设置字体……

不过思源（Noto）系列字体真的是开源界的福音，字重多，字符全，自带hinting，长得还漂亮，换上以后整个系统都显得高级起来了～

为了获得“高级感”，首先你需要安装`noto-fonts`和`noto-fonts-cjk`。因为是7012年，中文字体里自带的英文字符也不算太丑，所以不需要像[ArchWiki](https://wiki.archlinux.org/index.php/Font_Configuration/Chinese_Font_Configurations_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#Windows.C2.AE.E6.98.BE.E7.A4.BA.E6.95.88.E6.9E.9C.E7.9A.84.E5.AD.97.E4.BD.93.E5.8F.82.E8.80.83.E9.85.8D.E7.BD.AE)和[这篇博文](https://ohmyarch.github.io/2017/01/15/Linux%E4%B8%8B%E7%BB%88%E6%9E%81%E5%AD%97%E4%BD%93%E9%85%8D%E7%BD%AE%E6%96%B9%E6%A1%88/)一样，分别定义中文和英文。全用Noto就好！（而且，分别定义的方法在chrome上有些小问题……）

`~/.config/fontconfig/fonts.conf`的内容附在[这里](https://gist.github.com/heyeshuang/03d69c1823b8cf769819d36b34f34f71)，保存之后用`fc-cache --force --verbose`更新字体缓存就行了。

## 接下来是截图时间：

{{% figure src="/font/5.png" title="这才是真正的宋体" width="50%"%}}
{{% figure src="/font/3.png" title="在正文也毫无压力" width="50%"%}}
{{% figure src="/font/4.png" title="还有这个light字重" width="50%"%}}

题外话，中文可用的字体实在是太少了。因为`SimSun`太渣，windows下的中文连个可用的`serif`字体都没有，每天只能对着呆板的雅黑干瞪眼……
