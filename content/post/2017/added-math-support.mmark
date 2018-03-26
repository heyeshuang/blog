---
title: "为hugo加入数学公式支持"
description: "利用Mmark和KaTeX"
date: 2017-10-22T13:37:58+08:00
draft: false
tags:
  - hugo
  - mmark
  - KaTeX
  - math
categories:
  - meta

---
数学学渣也要公式？其实我也不清楚我要这个有什么用……反正先弄了再说。

官方文档中其实有[这一部分内容](https://gohugo.io/content-management/formats/#mathjax-with-hugo)，但是那里是用MathJax的，而且在标准markdown文件里需要进行额外的设置。如果像[这里](http://nosubstance.me/post/a-great-toolset-for-static-blogging/)一样使用KaTeX+Mmark，写起来会方便许多。

Hugo自带Mmark的支持，只要在新建文章的时候使用`.mmark`后缀就行：

```
hugo new post/some-name.mmark
```

这种格式是markdown的超集，本来是用来写IETF文档的。具体支持的语法可以看[这里](https://github.com/miekg/mmark/wiki/Syntax)。

用这种渲染器，用`$$`包裹的文字会自动转成用`\[`或者`\(`包裹，比如，`
inline $$\sqrt{b^2-4ac}$$`会转成`\(\sqrt{b^2-4ac}\)`

之后，只要修改主题，把$$\KaTeX$$引用进去就好了。对我自己使用的主题，改动见[这里](https://github.com/heyeshuang/blackburn/commit/d265e4556238628203eb6de7d3a3f88e4f392a42)。

效果如下：

行内公式：$$\sqrt{b^2-4ac}$$

单行公式：

$$\hat{y}= \sigma(\omega^T X+b)=\frac{1}{1+e^{-(\omega^T X+b)}}$$

另外，还有一种更加复杂的`<shortcode>`[方法](http://www.latkin.org/2016/08/07/better-tex-math-typesetting-in-hugo/)，如果你是完美主义者的话。
