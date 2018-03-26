---
author : "HeYSH"
type : "post"
tags :
    - steem
    - steemit
categories :
    - 折腾
title: "更琐碎的编辑器细节 / Some more trival details about Steemit"
date: 2017-10-14T23:04:00+08:00
draft: false
---

Steemit的前端代码在<https://github.com/steemit/condenser/>，读一读它的源代码，可以了解一些关于编辑器的技术细节。

## 排版问题

### Markdown

Steemit里所有的markdown指南都指向<https://guides.github.com/features/mastering-markdown/>，但其实并没有实现其中所有的功能。大体上，使用其中标准的 *Basic syntax* （或者叫做`CommonMark`）是比较安全的，但github favor的部分就说不定了。

[编辑器](https://github.com/steemit/condenser/blob/8a4e470fea6a34a9e19f8598c5261fd414d96fa8/src/app/components/cards/MarkdownViewer.jsx)使用的markdown渲染库是[remarkable](https://github.com/jonschlinkert/remarkable)。除了基本语法以外，只支持[脚注、删除线和表格](https://github.com/jonschlinkert/remarkable/#syntax-extensions)等一些基本的语法，而且现在脚注的实现还有些问题。没有 **代码高亮** ，更没有 **数学公式渲染** ，可能这东西并不是面对我们这样的用户。

[这里](https://steemit.com/wpcommunity/@sean0010/markdown-test)是关于高级markdown特性的测试。从[这里](https://steemd.com/wpcommunity/@sean0010/markdown-test)可以看到其源码。

顺便一提，我用的`hugo`静态博客生成器使用`blackfriday`，配置项很多，我十分满意。

### 那么用HTML实现高级功能呢？

很遗憾，这也不大可能。经过[sanitize](https://github.com/steemit/condenser/blob/4d9c1d8f76366097d9a7159b1760f51d45201eb0/src/app/utils/SanitizeConfig.js)过滤，编辑器只支持以下几个标签：

```
div, iframe, del,
a, p, b, i, q, br, ul, li, ol, img,
h1, h2, h3, h4, h5, h6, hr,
blockquote, pre, code, em, strong, center,
table, thead, tbody, tr, th, td,
strike, sup, sub
```

并没有什么超过Markdown语法的新功能。

当然，编辑器好心地提供了几个class来控制文字和图像的位置：

```
'pull-right', 'pull-left', 'text-justify', 'text-rtl', 'text-center', 'text-right', 'videoWrapper'
```

用法大概是 `< div class="pull-right"> < img src="imglink" />< /div >`，或者看[这里](https://steemit.com/steemit/@primus/how-to-use-new-hidden-steemit-features-for-custom-formatting-within-post-content-floating-right-left-and-justifying)的介绍。

## 文章的URL生成原理

如果我在发布之前就知道文章的URL的话，就可以在文中引用自己了。这大概是一种自举 ;-)

如果你的文章标题是纯中文的话就很简单了：地址是随机的。所以最好在标题里加上些英文（虽然看起来有点蠢）。

URL通过[speakingurl](https://github.com/pid/speakingurl)生成。比如，本文标题经过处理（过程见[此处](https://jsfiddle.net/heyeshuang/6p7n7dks/1/)），得到`some-more-trival-details-about-steemit`，如果我之前没有发过同名文章的话，本文的URL就是<https://steemit.com/cn/@heyeshuang/some-more-trival-details-about-steemit>，大概。

[这里](https://github.com/steemit/condenser/blob/164144aa2c57c12b136eb9c3d7fe46d930dfda04/src/app/redux/TransactionSaga.js#L412-L442)是源代码中具体的实现函数，适合像我这样刻薄的人。
