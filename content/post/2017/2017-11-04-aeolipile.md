---
author : "HeYSH"
type : "post"
categories:
  - 奇怪的小玩意
tags:
  - steem
  - steemit
title: "社交恐惧症患者也想用Steem，所以我做了Aeolipile"
description: "而且还能显示代码和公式"
date: 2017-11-04T11:55:46+08:00
draft: false
---
> TL;DR: 你可以在[这个链接](https://heyeshuang.github.io/aeolipile/#/@heyeshuang/steem-aeolipile)看到Aeolipile。

[我曾经说](https://steemit.com/cn/@heyeshuang/steem-a-closer-look-to-steem)不会把博客完全搬到Steem上。除了之前说过的原因之外，还有一些感性上的理由。

和那些[网红博主（开玩笑的）](https://steemit.com/cn/@tumutanzi/5bbm33)不同，我的博客没有什么人会光顾，这并不是什么坏事：只有这样，我才能感到真正的自由。

但是Steemit不同。页面下面的评论框和小数字，这些东西时刻在提醒我，你的文字是有人看的。对其他人来说，这可能是一件好事。可是，对于我来说，意识到有人在看这些东西，让我不自觉地在意起别人的感受了。我在敲键盘的时候，常常会想：“别人是怎么看这段话的？”然后，强烈的自我厌恶就会让我没办法接着写下去了。

而那些小数字更是推波助澜，它们直接给我的文字标上了一个价码，然后跟我说：“大家好像不太喜欢你的东西啊，这次只有这些，下次继续努力吧。”

可是我是因为喜欢才踢罐子的啊。[^bottle]

[^bottle]: https://www.zhihu.com/question/19575136/answer/14470032

如果我的内心能够更强大一点的话，我当然可以说：“F**k yourself，老子乐意”。但是显然现在的我还不行。所以，我做了这个`Aeolipile`，来 *暂时* 平复一下内心的情绪。

言归正传。Aeolipile是一个Steem文章的渲染工具，（明面上的）用途是为了增强Steemit的Markdown功能，比如说代码、公式什么的。比如这篇文章，可能也需要访问[这个页面](https://heyeshuang.github.io/aeolipile/#/@heyeshuang/steem-aeolipile)才能看到脚注。具体支持的功能，可以看[这篇文章](https://heyeshuang.github.io/aeolipile/#/@heyeshuang/aeolipile-your-code-and-formula-on-steem)。大体上说，你可以在这里使用几乎所有Markdown的扩展语法。（而且之后我可能会把页面做得更好看些……也许）

已有的文章都可以用Aeolipile渲染，只要访问这个页面就行：
```
https://heyeshuang.github.io/aeolipile/#/@作者名字/文章链接/
```
和Steemit本身链接的构成差不多。

如果你也想使用这个工具的话，我建议：

* 先为你的文章起个名字，名字中需要有英文单词——否则会产生随机的链接
* 在[这里](https://jsfiddle.net/6p7n7dks/2/)输入标题，得到你文章的链接地址
  * 过几天我可能会做一个单独的页面，可能
* 还是在Steemit的编辑器里写下内容
* 把Aeolipile的地址写在文章前面
* 点击发送，不需要做什么其他的工作

这个工具本身是开源的：

<https://github.com/heyeshuang/aeolipile>

欢迎fork，如果不嫌弃代码写得烂的话。

最后，我还想说一句台词：

> I could be bounded in a nutshell and count myself a king of infinite space.[^Shakespeare]

[^Shakespeare]: Hamlet: Act 2, Scene 2, Page 11

当然，没准一个Aeolipile也行。
