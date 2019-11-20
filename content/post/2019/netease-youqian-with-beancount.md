---
author : "HeYSH"
type : "post"
tags :
    - 网易有钱
    - 记账
    - Beancount
    - import
categories :
    - 折腾
    - 生活
title: "Beancount试用"
description: "批量建立账户，以及导入网易有钱账本"
date: 2019-11-07T21:58:22+08:00
draft: false
---
自从2017年开始，我一直使用 *网易有钱* 记账。不得不说，如果像某落水狗讲的那样，“中国人不在乎隐私”的话，这确实是一个非常好用的APP，下载账单、导入数据一气呵成，连余额宝的利息都能够算得分毫不差——

直到最近，大概是骂挨得太多，或是界面里终于塞不下更多广告，这个APP①用“维护中”挡住了所有同步按钮，并且②停止了维护。

用8元买回了本属于自己的数据后，我想起之前听说过的[Beancount](https://www.byvoid.com/zht/blog/beancount-bookkeeping-1)。于是，我给那些数据找到了一些新用途。

首先，我阅读了[这篇博文](https://yuchi.me/post/beancount-intro/)，搭起了Beancount。接着，我又写了一些其它的[脚本](https://github.com/heyeshuang/beancount-homemade-importers)。

## 自动建立收支账户

之前的理财软件大多会提供一些初始分类，比如说mint所用的类别列表在[这里](https://www.mint.com/mint-categories)。
对于本人[^self]之前的账本，有过记录的大概是这样几类：

`
{'交通,',
 '交通,打车',
 '交通,机票',
 '交通,火车',
 '人情,',
 '住房,',
 '住房,家具',
 '住房,家纺',
 '住房,物业水电',
 '住房,装修',
 '其他,',
 '医疗,',
 '医疗,药品',
 '娱乐,',
 '娱乐,电影',
 '投资亏损,',
 '文教,',
 '文教,书刊',
 '文教,文具',
 '旅行,',
 '旅行,景点门票',
 '旅行,酒店',
 '汽车,',
 '购物,',
 '购物,数码',
 '购物,日用',
 '购物,玩具',
 '购物,电器',
 '购物,美妆',
 '购物,运动用品',
 '购物,鞋服',
 '购物,饰品',
 '通讯,',
 '通讯,话费',
 '零食烟酒,',
 '零食烟酒,水果',
 '零食烟酒,烟酒',
 '零食烟酒,茶水',
 '零食烟酒,零食',
 '零食烟酒,饮料',
 '餐饮,',
 '餐饮,三餐',
 '餐饮,食材'}
`

对这些分类，利用人工~~智能~~，为每一项建立了对应的`account`，形成了一个[dict](https://github.com/heyeshuang/beancount-homemade-importers/blob/master/importers/youqian/youqianDict.py)。

如果你已经下载了[beancount-homemade-importers](https://github.com/heyeshuang/beancount-homemade-importers)，你可以用里面的`importers/
youqian/accountInit.py`来自动建立这些`accounts`：

```bash
python importers/youqian/accountInit.py > New_Accounts.bean
# 或：如果之前已经建立了一些账户的话
python importers/youqian/accountInit.py Existing_Accounts.bean > New_Accounts.bean
```

## 导入网易有钱的收入、支出数据

Beancount的[文档](https://docs.google.com/document/d/11EwQdujzEo2cxqaF5PgxCEZXWfKKQCYSMfdJowp_1S8/edit#)里已经对导入脚本进行了比较详细的说明。从[这个例子](https://bitbucket.org/blais/beancount/src/tip/examples/ingest/office/importers/utrade/)开始，基本上只要实现几个函数就行。

在[beancount-homemade-importers](https://github.com/heyeshuang/beancount-homemade-importers)中，我写了一个简单的网易有钱导入脚本。在使用之前，需要用excel把默认的`xlsx`格式转换为`收入.csv`和`支出.csv`。

之后，使用

```
bean-extract importers/youqian/youqian_expense.import documents.tmp/支出.csv > 支出.bean
bean-extract importers/youqian/youqian_income.import documents.tmp/收入.csv > 收入.bean
```

来导入。

另外，我还写了中国银行、招商银行和微信钱包对账单的导入脚本，当然基本上是从[这里](https://yuchi.me/post/beancount-intro/)抄袭的。

---

又及：

> 我跟你港，这次的内容超适合作Python教程的，还能顺便学英语！只要学会了这个，年薪百万不是梦！

> 现在诚征 100 个~~凯子~~幸运读者给我打钱，我的支付宝是：……

> 呃，对不起。

以上不代表博主观点。

---

来自2019-11-20的更新：

我又仔细梳理了一下自己的花销，发现只要导入微信账单、一张信用卡（招商）和一张借记卡（中行）就可以覆盖绝大多数消费情况；另外，我还发现了[smart_importer](https://github.com/beancount/smart_importer)，只要简单地增加hook，就可以给导入脚本增加更好的去重、智能猜测分类等更帅气的功能。

根据这些，在刚才我更新了自己使用的脚本，基本上可以覆盖自己的需求了[^1]。

[^self]:一个普通的、抠抠索索的中青年男性，未婚，不修边幅，被阳光照到会燃烧。
[^1]:除了还要手动把中国银行的账单改成UTF-8