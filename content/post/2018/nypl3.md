---
author : "HeYSH"
type : "post"
tags :
    - 纽约公众图书馆
    - 素材
categories :
    - 折腾
title: "这次是纽约公共图书馆……而且居然还有第三部分？"
date: 2018-02-24T00:03:59+08:00
draft: false
---

> 在图书馆的第三天，到了废纸回收环节

之前下到了一叠厚厚的[图集](https://digitalcollections.nypl.org/collections/trait-des-arbres-et-arbustes-que-lon-cultive-en-france-en-pleine-terre#/?tab=about)，可是要真正用起来，还需要一点点简单的处理，比如说，沿着边线把图案剪下来：
{{< figure src="/nypl/before.jpg" title="加工前" >}}
{{< figure src="/nypl/after.png" title="加工后" >}}

通过一些简单的图形学处理就可以完成这项任务，当然，对于不懂画画的乡巴佬（我），要读好多`skimage`的教程才能搞定——总之，我把折腾的结果传到了[github](https://github.com/heyeshuang/Trait-des-arbres-et-arbustes-que-l-on-cultive-en-France-en-pleine-terre/)上，包括：

- 如何在`jupyter notebook`里[剪好一张图](https://github.com/heyeshuang/Trait-des-arbres-et-arbustes-que-l-on-cultive-en-France-en-pleine-terre/blob/master/sobel.ipynb)
- 然后改成[批量修改的版本](https://github.com/heyeshuang/Trait-des-arbres-et-arbustes-que-l-on-cultive-en-France-en-pleine-terre/blob/master/sobel-batch.ipynb)
- 以及[原图](https://github.com/heyeshuang/Trait-des-arbres-et-arbustes-que-l-on-cultive-en-France-en-pleine-terre/tree/master/batch/France)和修改之后的[结果](https://github.com/heyeshuang/Trait-des-arbres-et-arbustes-que-l-on-cultive-en-France-en-pleine-terre/tree/master/france)

欢迎下载、使用、或者拿去卖钱，反正是公有领域。