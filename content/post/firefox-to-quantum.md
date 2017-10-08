---
author : "HeYSH"
type : "post"
tags :
    - Firefox
    - Firefox Quantum
    - Web Extension
categories :
    - 折腾
title: "Brave New World: Firefox to Quantum"
description: "升级、降级和废弃"
date: 2017-10-01T10:29:28+08:00
draft: false
---

在把现在的配置导入Waterfox之后，就可以升级Firefox Quantum了。强烈建议用离线模式启动，并取消登陆Firefox账号，防止污染自己的同步内容。

在`不支持的扩展`页面，自带“查找替代品”功能，但是基本没有用，不如[这个列表](https://docs.google.com/spreadsheets/d/1TFcEXMcKrwoIAECIVyBU0GPoSmRqZ7A0VBvqeKYVSww/edit#gid=0)和[卡饭的帖子](http://bbs.kafan.cn/thread-2100445-1-1.html)。

另外，基于WE的扩展页面都有一种廉价感，不像XUL画的窗口那样专业——当然这只是我的偏见。

新Firefox一个进程的内存用量都快赶上整个Waterfox了，我怀疑这是三星的阴谋。

等等，servo是不是跟三星有关系来着……

下面是我安装扩展的完整列表，很长，~~没有什么用~~ 可能会有点用

## 不兼容的扩展

- [ ] Art Project 0.1.5 -> [Free Art Tab](https://addons.mozilla.org/zh-CN/firefox/addon/free-art-tab/?src=search)
  - 唔……有一些微妙的差别，我不认为这个算是被替换了
- [x] AutoPagerize 0.9.17.1 -> PageZipper
  - 除了需要手动开启之外，效果意外的还可以
- [ ] Chrome Store Foxified 2.4
  - 很急
- [ ] DownThemAll! 3.0.8
- [ ] DownThemAll! AntiContainer 1.5
  - 想都不要想，Aria2你好
- [ ] ExHentai Easy 2 0.2.9
  - 呜呜呜（sad panda表情）
- [ ] Extension List Dumper 2 1.0.2
  - 这个列表就是用这个扩展生成的，生成之后就可以删了
  - 删了以后就再也没有啦~
- [x] FireGestures 1.11.1 -> ~~Foxy Gestures~~ Gesturefy
  - ~~我就随便挑了个安装多的，当然有些方言不一样了，没有什么办法~~
  - 这个更漂亮一些
  - 而且这个扩展自带存储PDF功能，又少装一个扩展
- [x] FoxyProxy Standard 4.6.5 -> [Proxy SwitchyOmega](https://addons.mozilla.org/en-US/firefox/addon/switchyomega/)
  - 说实话，自从AutoProxy不更新以后，我看这个扩展不爽好久了
  - 对我来说Proxy SwitchyOmega还更好用一些
  - 然而PSO在隐私窗口还有些问题
- [x] gouwudang-price-comparison-tool 3.2.5 -> [bookmarklet](https://gwdang.com/app/extension?page=question&guide=bookmark)
  - 这是一个比价扩展，用Bookmarklet可以稍微凑合一下
- [x] Greasemonkey 3.16 -> Violentmonkey
  - Tempermonkey “Proprietary, poor privacy, has a 'End-User License Agreement' at installation”，他们都这么说了……
  - 而且我装的那些脚本好像也没有什么能用的了……
  - 现在的脚本安装网站是 https://greasyfork.org 了啊
  - 等等，https://sleazyfork.org/zh-CN 是什么鬼（捂脸）
- [ ] Hide Caption Titlebar Plus 3.2.9
  - 无理无理
- [ ] Hide Unwanted Results of Google Search 1.6
  - 就算是被替代了吧
- [ ] https switch 0.0.3
  - 很久没用过了
- [x] NoSquint Plus 54.0 -> [Zoom Page WE](https://addons.mozilla.org/addon/zoom-page-we/)
    - 要缩放整个页面（而不仅仅是文字），按一下`alt`键能呼出菜单，然后去掉`查看-缩放-仅缩放文字`前面的勾。
- [x] Offline QR generator 1.1.3.1-signed -> [QR Code Image Generator](https://addons.mozilla.org/zh-CN/firefox/addon/qr-code-image-generator/?src=api)
  - 当然，Offline是没有了
- [ ] PentadactylSigned 7307
  - …………………………Saka Key的感觉完全不一样
- [x] Pocket 1.0.5 -> 自带，而且我不知道我之前还装了这个
- [x] printpdf 0.76.1-signed.1-signed
  - 居然用一个鼠标手势解决了
- [x] QuickDrag 2.1.3.23.1-signed.1-signed -> Fire Drag
  - 当然还是挑了个安装多的
- [ ] ReDisposition 0.3.0
  - 这是一个可以解决下载乱码的扩展，原作者muzuiget的博客已经无法访问了，只留下了[disquz的残渣](https://disqus.com/home/discussion/qixinglu/firefox_redisposition/)和一大堆吸血的转载
  - 这不禁使我担心起本博客的未来
  - 似乎用[HeaderEditor](https://github.com/FirefoxBar/HeaderEditor/releases)可以实现，不过我还没有试过
- [ ] RefControl 0.8.17.1-signed.1-signed -> [referer control] (https://addons.mozilla.org/zh-CN/firefox/addon/referercontrol/)
  - 看不到希望小学图片……我一点都不遗憾
- [ ] RSS Handler for Feedly 1.0
  - 我当时为什么要装这个？
- [x] ScreenShot Link 2.2.1.1-signed.1-signed
  - 这功能居然自带了
- [ ] Tab Mix Plus 0.5.0.4
- [x] WizNote Web Clipper 1.0.37.1-signed.1-signed -> bookmarklet
  - 反正之前也不怎么好用
- [x] 隐私标签页 0.2.2 -> [
Incognito This Tab](https://addons.mozilla.org/zh-CN/firefox/addon/incognito-this-tab/?src=find-replacement)
  - 可是还是不能在同一个窗口打开……哇的一声哭了出来

## 原生Web Extension

- [x] Duplicate Tabs Closer 3.0.0
- [x] Enhanced Steam 9.5
- [x] Pushbullet 335
- [x] Redirector 3.1.0
  - 是S1大姨妈的时候用的，现在应该没什么用
- [x] uBlock Origin 1.14.10
- [x] 二维码扫描器 1.2.3
- [x] 印象笔记·剪藏 6.12.4
- [x] 终结内容农场 2.0.2
