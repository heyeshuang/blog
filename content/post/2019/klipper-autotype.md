---
author : "HeYSH"
type : "post"
tags :
    - linux
    - VPS
    - KDE
    - noVNC
categories :
    - 折腾
title: "自动输入剪贴板中的内容"
description: "针对KDE（Klipper）"
date: 2019-09-22T20:54:15+08:00
draft: false
---

> 值此佳节之际，为了给伟大祖国献礼，让几个VPS炸成烟花是必不可少的庆祝活动。于是，我搭在Google Cloud Platform上的V2RAY理所当然地连不上了。[^1]正好上次薅的羊毛也快要过期了，又到了交保护费的时间。

> 其实之前我有两页关于VPS（价格）的笔记，不过计划赶不上变化，今天看到了一个叫Veesp的公司，很便宜，而且速度居然也还不错。

> 惟一的缺点是，~~我买不到红墨水~~那里的终端noVNC不支持粘贴。

> 以上是（无关紧要的）背景。

{{%spoiler "附：Veesp的测速情况" %}}
![北京联通表示情绪稳定](/img/veesp-speedtest.png)
{{%/spoiler%}}

针对noVNC的问题，根据[这个帖子](https://forum.proxmox.com/threads/novnc-copy-paste-not-works.19773/#post-101013)，可以用`xdotool`来解决。

现在我用的KDE自带剪贴板管理程序`Klipper`，能够配置自定义动作处理剪贴板中的内容。这里的动作是`xdotool type '%s'`，如图：

![setting](/img/klipper.png)

另外，在我这里`在当前剪贴板上手动执行动作`的快捷键<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>R</kbd>似乎有冲突，我改成了<kbd>Ctrl+Shift+Alt+R</kbd>。[^2]

之后在noVNC界面上按刚才的快捷键就行。

--- 

来自19-10-4的更新：俄罗斯IP被某个熊猫网站（e-h）屏蔽了……我就随口一说。


[^1]: 具体地说，是在北京联通的4G网络，协议是WS+TLS，而且并不是永封，截至发稿前又可以连接了。在同个服务器上的$$被封得更彻底一些。

[^2]: 其实我还有点期待`<kbd>`标签能出个什么特殊格式呢……