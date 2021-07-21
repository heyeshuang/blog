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

本文是[2019年博文]({{%relref python-for-optics%}})的更新版本。就像在那里说的，光学计算是一个非常宽泛的话题，把所有的库放在一起，倒是具有一种全栈工程师的气质，可是好像很容易造成*Stack Overflow*。

这次，我尝试对这些代码进行简单的分类。因为我不属于*光学前端工程师*[^no]，也不属于*光学后端工程师*[^no]，而更像是*光学系统运维*[^no]，分类得大概并不算准确。

## 光线追迹（Ray Tracing）


## 有关物理光学的演示

## dive in：为望远镜/日冕仪准备的Python

你想学习怎样设计日冕仪吗？

### [Prysm](https://github.com/brandondube/prysm)

这些库的对比可以看[这里](https://arxiv.org/abs/1807.07042)。[^OOC]如果你真的想设计日冕仪的话也可以读一读上一篇文献。
![我逐渐理解一切](know-everything.jpg)

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
