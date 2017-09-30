---
author : "HeYSH"
type : "post"
tags :
    - IME
    - ctypes
    - Atom
    - python
categories :
    - 奇怪的小玩意
title: "关于那个IME helper"
date: 2017-09-06T19:46:55+08:00
draft: false
---

~~昨天~~前天我用鸟语写了一篇贼 *正经* 的[说明]({{<ref "atom-vim-mode-auto-switch-IME.md">}})，今天，我准备稍微介绍一下那里面的`IME helper`。

正如之前所说，那东西是为了配合atom插件自动开关输入法用的。问题明确、思路清晰，google一下肯定有人做过……才怪呢

嗯，这种需求，我听过一个叫AHK的语言……
```
;来自https://github.com/lspcieee/lspcieee_ahk/blob/master/IME.ahk
;没有任何要黑别人的意思，将来报道出了偏差，你们（下略）
setChineseLayout(){
	;发送中文输入法切换快捷键，请根据实际情况设置。
	send {Ctrl Down}{Shift}
	send {Ctrl Down},
	send {Ctrl Down}{Shift}
	send {Ctrl Down},
	send {Ctrl Up}
}
```
哇这语言真的是发送快捷键的哦！就没有什么更高级的方法吗？快捷键在win10里可是改了啊( >﹏<。)

然后就愉快地决定用 Python+ctypes 干这种脏活了，然后发现：windows API真是太难写了！一个更改输入法有4种写法！而且都不能用！

最后居然还是抄的[AHK社区的东西](http://www.ahk8.com/thread-3751.html?highlight=%E8%BE%93%E5%85%A5%E6%B3%95)……亏我还像白痴一样疯狂翻MSDN……

另外，pyinstaller生成的单文件exe~~跑得不够快~~用起来有延迟，大概是因为解压吧……
