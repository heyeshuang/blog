---
title: "Unicode与数学公式"
description: "在一个文本框中能做到怎样的程度？"
date: 2019-09-05T20:24:27+08:00
draft: false
author : "HeYSH"
type : "post"
tags :
    - Unicode
    - 和我最讨厌的
    - 数学
categories :
    - 折腾
---


如果你混迹某小蓝鸟APP，或者像我一样常常用辣鸡微信干活，那么在你需要一点数学的时候，可能会觉得有些尴尬。如果直接写一些裸的LaTeX显得有些不近人情，可是像百度知道大神一样直接把公式“展平”，又显得有一点点……低龄。[^2]

借助现代Unicode的力量，可以输入一些比较简单的数学，大概能到这种程度：√X̅ₘ̅²̅+̅Y̅ᵦ̅²̅≥0

这玩意粘贴到微信和小蓝鸟上效果居然还不错。（假装这里有个图）

（呃，上面输入的是 $\sqrt{X_m^2+Y_\beta^2}\geq0$。）


这里用到了[Unicode数学符号](https://en.wikipedia.org/wiki/Mathematical_operators_and_symbols_in_Unicode)、[上下标](https://en.wikipedia.org/wiki/Unicode_subscripts_and_superscripts)和[组合附加符号](https://zh.wikipedia.org/wiki/%E7%B5%84%E5%90%88%E9%99%84%E5%8A%A0%E7%AC%A6%E8%99%9F)[^1]。

另外，在[中日韩兼容字符集](https://en.wikipedia.org/wiki/CJK_Compatibility)里还有类似`㎛`这样的符号，个人觉得……有点玩过头了。


[^1]:用来输入上划线，具体是U+0305，可是效果好像不大好。另外，[这个网站](https://fsymbols.com/generators/overline/)可以直接添加上划线。
[^2]:其实在这种情况下，[AsciiMath](https://en.wikipedia.org/wiki/AsciiMath)可能是一个好选择，毕竟这玩意比LaTeX长得好认多了。还有一种格式叫做[UnicodeMath](https://www.unicode.org/notes/tn28/UTN28-PlainTextMath-v3.1.pdf)，长得其实也不赖。