---
author : "HeYSH"
type : "post"
tags :
    - python
    - 光学
    - 工作量不饱和的证据
categories :
    - 正经
title: "Python与光学计算，2021"
description: "Not-So-Awesome Python for Optics in 2021"
date: 2021-07-21T18:14:49+08:00
draft: true
toc: true
---

本文是[2019年同名博文]({{%relref python-for-optics%}})的更新版本。就像在那里说的，光学计算是一个非常宽泛的话题，把所有的库放在一起，倒是具有一种<ruby>全栈<rp>(</rp><rt>full-stack</rt><rp>)</rp></ruby>工程师的气质，可是好像很容易造成*Stack Overflow*。

这次，我尝试对这些代码进行简单的分类。因为我不属于*光学前端工程师*[^no]，也不属于*光学后端工程师*[^no]，而更像是*光学系统运维*[^no]，分类得大概并不算准确。

## 光线追迹（Ray Tracing）


## 有关物理光学的演示

## 真正的光学设计：为望远镜/日冕仪准备的Python

你想学习怎样设计日冕仪吗？ 

{{% figure src="poppy.png" title="詹姆斯·韦伯空间望远镜的光学设计，来自spacetelescope/poppy"%}}

就像所有的专业软件一样，如果你理解了整个物理过程，中间的示意图就并不是那么重要了。这样的库包括：

1. [brandondube/prysm](https://github.com/brandondube/prysm) ([examples](https://prysm.readthedocs.io/en/stable/examples/index.html))
2. [spacetelescope/poppy](https://github.com/spacetelescope/poppy)
3. [ajeldorado/falco-python](https://github.com/ajeldorado/falco-python)[^FALCO]
4. [ehpor/hcipy](https://github.com/ehpor/hcipy) ([examples](https://docs.hcipy.org/dev/tutorials/index.html))

等。

它们大概都能够计算光学系统的点扩散函数（PSF）、调制传递函数（MTF）、点列图之类的，而优化算法似乎欠奉。如果你对上面的一系列名词不大了解的话，建议和我一起补习[《光学系统设计》](http://www.optzmx.com/forum.php?mod=viewthread&tid=1131&highlight=%B9%E2%D1%A7%CF%B5%CD%B3)。

{{% figure src="know-everything.jpg" alt="我逐渐理解一切" width="30%"%}}

这些库的对比可以看[这里](https://arxiv.org/abs/1807.07042)。[^OOC]如果你真的想设计日冕仪的话也可以读一读这篇文献，那里对设计方法也有一些介绍。

就我而言，我比较喜欢`HCIPy`，至少这里面还包含一些传递过程的内容，不至于直接跳到结论。

## 电磁场

https://github.com/demisjohn/CAMFR

## 激光谐振腔

## 非线性光学

### [PyNLO](https://github.com/pyNLO/PyNLO)

![pyNLO](pynlo.png)

大概是`SNLO`的某种代替品，总之祝大家好运吧。

[^no]: 据我了解并不存在前两种职业。而运维在哪里都存在。
[^OOC]: Ruane, G., “Review of high-contrast imaging systems for current and future ground- and space-based telescopes I: coronagraph design methods and optical performance metrics”, in <i>Space Telescopes and Instrumentation 2018: Optical, Infrared, and Millimeter Wave</i>, 2018, vol. 10698. doi:10.1117/12.2312948. 
[^FALCO]: A J Eldorado Riggs, Garreth Ruane, Erkin Sidick, Carl Coker, Brian D. Kern, Stuart B. Shaklan, "Fast linearized coronagraph optimizer (FALCO) I: a software toolbox for rapid coronagraphic design and wavefront correction," Proc. SPIE 10698, Space Telescopes and Instrumentation 2018: Optical, Infrared, and Millimeter Wave, 106982V (9 August 2018); https://doi.org/10.1117/12.2313812，[PDF文件](https://core.ac.uk/download/pdf/211386255.pdf)
