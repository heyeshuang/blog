---
author : "HeYSH"
type : "post"
tags :
    - atom
    - vim
    - python
categories :
    - 奇怪的小玩意
title: "Auto-Switch IME in Vim-Mode-Plus for Atom Editor"
date: 2017-09-05T21:03:41+08:00
draft: false
languageCode : "en"
---

Update 2018-5: Now vim-mode-plus introduced [new method](https://github.com/t9md/atom-vim-mode-plus/blob/master/CHANGELOG.md#1300) to disable IME in the setting page. It's time for this post to retire.

---

As a vim-user and Chinese-speaker, I prefer [vim-mode-plus](https://atom.io/packages/vim-mode-plus) for Atom editor, but always disrupted by the IME in normal mode, like this:

![IME-in-atom](/IME-in-atom.png)
<!--more-->

If we could [disable IMEs in normal mode](https://github.com/t9md/atom-vim-mode-plus/issues/446) then everything will be fine again. However, due to [the lack of the APIs](https://github.com/atom/atom/issues/1092), it is hardly possible to solve the problem in the editor.

Thus, we could solve it *out of the box*. There is a package named [atom-vim-mode-plus-auto-ime](https://atom.io/packages/vim-mode-plus-auto-ime), which could run a custom command when the mode changes. The goal now is to find some commands to control the IME:

* macOS users can use [input-source-switcher](https://github.com/vovkasm/input-source-switcher) to toogle the IME.
* For Linux, the IME `fcitx` has a command `fcitx-remote` to control itself.
* And for Windows, I wrote a [python script](https://github.com/heyeshuang/ime_helper) to switch the IME of the *foreground window*.

Then we could configure this package.
```bash
#windows:
python E:\\path\\to\\ime_helper.py --current #Get Current Input Source
python E:\\path\\to\\ime_helper.py --dec ${source} #insert
python E:\\path\\to\\ime_helper.py --locale en_US #normal
python E:\\path\\to\\ime_helper.py --locale en_US #visual
#linux:
/usr/bin/fcitx-remote #Get Current Input Source
if (( "${source}" <= "1" ));then /usr/bin/fcitx-remote -c; else /usr/bin/fcitx-remote -o ;fi #insert
/usr/bin/fcitx-remote -c #normal
/usr/bin/fcitx-remote -c #visual
#macOS: just use the default config
```
![configure](/atom-vim-configure.png)
