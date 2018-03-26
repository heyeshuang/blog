---
author : "HeYSH"
type : "post"
tags :
    - Firefox
    - Waterfox
categories :
    - 折腾
title: "Yesterday Once More: Firefox to Waterfox"
date: 2017-09-30T23:44:33+08:00
draft: false
---

[Firefox Quantum](https://www.mozilla.org/en-US/firefox/quantum/)（就是Firefox 57） 发布了 beta 版，主动放弃了`XUL`和老版本扩展。我不知道这会不会又是一个“燃烧的平台”，只想体验一下Quantum传说中的新技术——顺便看看和Chrome有什么区别。

当然，在升级57之前，需要给 Firefox 的状态存个盘。Waterfox是一个很好的选择：

- 不久前刚刚引入了自己的配置文件，可以和Firefox同时开启；
- 对比Pale Moon（另一个Firefox的Fork），分叉时间更晚，对“现代”扩展支持得更好。

## 迁移
迁移的过程还是很简单的：只要还没升级到57，就可以用`FEBE`扩展进行备份和恢复。注意在恢复到Waterfox的时候（选择“恢复配置”），需要新建一个Profile。

## 中文
为了显示中文界面，需要安装[Chinese Simplified (zh-CN) Language Pack](https://addons.mozilla.org/en-US/firefox/addon/chinese-simplified-zh-cn-la/versions/)，并访问`about:config`，将`intl.locale.matchOS`改为`true`。这样，~~Firefox~~ Waterfox就可以按照系统的语言来显示了。

## Pentadactyl
Waterfox默认开启了多进程模式，会导致`Pentadactyl`不能正常工作：
```
Error: Permission denied to access property "length"
```

仍然是在`about:config`页面，查找`browser.tabs.remote.autostart`，把所有找到的项都改成`false`。
```
browser.tabs.remote.autostart=false
browser.tabs.remote.autostart.2=false
```

OK，保存完成，是时候升级[Firefox Quantum](https://www.mozilla.org/zh-CN/firefox/channel/desktop/#beta)了。

啊对了，本文同时发在[我的博客](http://heyeshuang.tk)和[Steemit](https://steemit.com/cn/@heyeshuang/yesterday-once-more-firefox-to-waterfox)上。基于时髦的区块链技术，应该会让我的博客活得时间稍微长一点。

——当然，一些更加私人的东西是只会在我的博客上发的。为了躲避猎人，有时候我需要把自己的脚印扫掉。
