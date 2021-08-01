---
author : "HeYSH"
type : "post"
tags :
    - python
    - 光学
    - 工作量不饱和的证据
categories :
    - 正经
title: "Python中光学计算相关的库/Awesome Python for Optics"
date: 2019-08-04T20:03:49+08:00
draft: false
---
> 来自2021-07-24的更新：

本文的2021年版本在[这里]({{<relref "python-for-optics-in-2021">}})。

以下是2019年的原文。

---

> 这大概是本博客第一次涉及博主在现实世界中的本职工作，大概算是一个好的开始。

在光学设计及模拟的领域，商业软件，比如Zemax/Code V、LASCAD、GLAD等，提供了较为完整的解决方案，对于较为前沿的领域，国内研究的事实标准是，通过MATLAB自行编写可靠性和可维护性都存在较大问题的脚本。但是，对于一些~~研究经费不足同时工作量不饱和的~~课题组，了解这个古老的学科与当今流行语言的交集，可能也具有一定的意义。

TL; DR：光学计算是一个很宽泛的话题，针对我的要求，之后我大概会试试[LightPipes](https://github.com/opticspy/lightpipes).

以下是我找到的一些库的对比。

## [spacetelescope/poppy](https://github.com/spacetelescope/poppy)

{{< figure src="poppy.png">}}

这个库本身是为詹姆斯·韦伯空间望远镜的模拟而设计的，从其[tutorial](https://nbviewer.jupyter.org/github/spacetelescope/poppy/blob/master/notebooks/POPPY_tutorial.ipynb)也可以看出，这个库的主要目的大概是，在衍射明显的条件下模拟成像过程、计算点扩散函数并分析成像质量，特别是针对天文望远镜领域。

因为我并不真正 *理解* 光学成像，我并没有办法判断该库的潜在用途，不过在激光器设计方面该库可能并不适用。

## [Sterncat/opticspy](https://github.com/Sterncat/opticspy)


![opticspy](opticspy.png)


看起来，这个库主要用于镜片设计，类似Zemax/Code V等软件所做的那样。具体上，能够完成光线追迹（但是没说能够优化），利用泽尼克多项式拟合（透镜表面/波前？），并计算镜片表面的干涉条纹。下次如果要计算纯粹干涉方面的内容我可能会尝试一下这个。

另外，这个库散发着一种爱好者的气息，`施工中`标志散落在文档各处。~~对于这种类型的项目可能还是让企业来做比较合适；可是开源之后又赚不到钱。~~

## [NanoComp/meep](https://github.com/NanoComp/meep)

![meep](meep.png)

[FDTD](https://en.wikipedia.org/wiki/Finite-difference_time-domain_method)法计算电磁场。这玩意让我想起了我短暂的研究生岁月，那时我学到一件事……人的能力是有极限的。

下一个。

## [SymPy](https://docs.sympy.org/latest/modules/physics/optics/index.html)

SciPy的一个组成部分，拥有一个光学计算模块，但仅仅在代入公式的水平。嗯，如果只是要算算高斯光的传输矩阵什么的，问题应该不大。

## [cihologramas/pyoptools](https://github.com/cihologramas/pyoptools)

大概也是Ray Trace，大概也是个人作品，而且例子都是用一种我看不懂的语言写的。下一个。

## [rezonator](www.rezonator.orion-project.org)、[simcav/simcav](https://github.com/simcav/simcav)等

Rezonator其实不能算是Python库，不过倒也是免费的，而且做的比另一个程序更完整一些。这两个软件的功能比较类似，仅通过谐振腔传输矩阵计算激光谐振腔特性，对于简单的腔形，可能这个就足够了。

## [opticspy/lightpipes](https://github.com/opticspy/lightpipes)

![lightpipes](lightpipes.png)

最后我找到的是这个，它的[官网](http://www.okotech.com/lightpipes)上说，这本来是一个*nix下的C++库，1999年开源，并增加了免费Python接口——听起来很靠谱。
具体上来说，这玩意也包含几何光学和衍射光学的相关内容，而且在它的说明文档里直接体现了[高斯光谐振腔矩阵计算](https://github.com/opticspy/Optics/blob/master/GeometricOptics/resonator_geometric_optics.ipynb)和[强衍射条件下谐振腔的计算](https://opticspy.github.io/lightpipes/examples_of_lightpipes_for_python.html#laser-examples)（虽然我还没有看懂）。

之后，可能会在这个的基础上对激光器进行一些分析——如果计划没有变更的话。

---

> 来自2019-10-23的更新：

## [Finesse and PyKat](http://www.gwoptics.org/finesse/)

为了LIGO设计的语言，用于引力波探测器的光路设计。Finesse有一种十分简单而复古的语法，而[PyKat](http://www.gwoptics.org/pykat)与其说是它的 *封装* ，倒不如说是拿报纸包了一下。

当然，该程序的功能还是挺强大的，甚至还有一些量子光学的内容[^scs]。如果有人想要尝试的话，可以从[这里](http://www.gwoptics.org/learn/)开始。

---

> 来自2020-6-1的更新：

## [RayOpt](https://github.com/quartiq/rayopt)

![rayopt](rayopt.png)
![rayopt](rayopt2.png)

和[opticspy]({{< relref "#sterncatopticspyhttpsgithubcomsterncatopticspy" >}})一样，RayOpt也是一组用于代替Zemax的程序，**看起来**更不像一个玩具。当然，如果要对这样的库作出一个中肯的评价，我觉得至少还要学习十年左右。所以我只是把它列在这里，并且祝*路过的旅行者*好运。

另外，我下次更新这篇文章的时候，差不多要给它们分分类了。毕竟光学计算是一个很宽泛的话题。~~我先立一个flag在这里。~~


[^scs]: 比如[语法说明](http://www.gwoptics.org/finesse/reference/)里提到了squeezed vacuum input source。不要问我[那是什么](https://en.wikipedia.org/wiki/Squeezed_coherent_state)。