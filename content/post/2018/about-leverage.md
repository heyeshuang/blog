---
author : "HeYSH"
type : "post"
tags :
    - bitcoin
    - bitmex
categories :
    - 折腾
title: "关于BitMEX的杠杆"
date: 2018-08-16T20:35:30+08:00
draft: false
---

在比特币降到6000$以下的时候，加密货币最忠实的信徒是怎样的心情？作为一个骑墙派，我不敢妄加揣测。不过，有一点是可以肯定的：把加密货币留在账户上然后去睡觉，神经衰弱的一系列症状大约会加重。

所以[BitMEX](https://www.bitmex.com/register/O3rmQq)（这里是个返利链接）用XBT计算盈亏就显得很有趣了：当比特币上涨的时候，大家都很开心；可是万一下跌的话，实际盈亏还要乘上汇率的缩水，一不小心就会得到双倍的悲伤。这种时候，理解账户数字的 *实际价值*[^pizza]，可能对控制风险有更为重要的意义。

根据[官方指南](https://www.bitmex.com/app/pnlGuide)，盈利的XBT数目可以这样计算：

```python
win_BTC_long=(1/price_before-1/price_after)*count #做多
win_BTC_short=(-1/price_before+1/price_after)*count #做空
```
那么，经过这部分操作之后，账户的最终价值是：

```python
win_BTC_long*price_after+cash/price_before*price_after #做多，或
win_BTC_short*price_after+cash/price_before*price_after #做空
```
其中，`cash`是操作前账户的实际价值，$cash*leverage=count$。

例如，当价格从1000$升至1250$，或从1250$降至1000$时，选用不同杠杆的盈亏如下：

| leverage | 作多，跌 | 作多，涨 | 作空，跌 | 作空，涨 |
|----------|----------|----------|----------|----------|
| 0.0      | 800.0    | 1250.0   | 800.0    | 1250.0   |
| 0.1      | 780.0    | 1275.0   | 820.0    | 1225.0   |
| 0.2      | 760.0    | 1300.0   | 840.0    | 1200.0   |
| 0.3      | 740.0    | 1325.0   | 860.0    | 1175.0   |
| 0.4      | 720.0    | 1350.0   | 880.0    | 1150.0   |
| 0.5      | 700.0    | 1375.0   | 900.0    | 1125.0   |
| 0.6      | 680.0    | 1400.0   | 920.0    | 1100.0   |
| 0.7      | 660.0    | 1425.0   | 940.0    | 1075.0   |
| 0.8      | 640.0    | 1450.0   | 960.0    | 1050.0   |
| 0.9      | 620.0    | 1475.0   | 980.0    | 1025.0   |
| 1.0      | 600.0    | 1500.0   | 1000.0   | 1000.0   |
| 1.1      | 580.0    | 1525.0   | 1020.0   | 975.0    |
| 1.2      | 560.0    | 1550.0   | 1040.0   | 950.0    |
| 1.3      | 540.0    | 1575.0   | 1060.0   | 925.0    |
| 1.4      | 520.0    | 1600.0   | 1080.0   | 900.0    |
| 1.5      | 500.0    | 1625.0   | 1100.0   | 875.0    |
| 1.6      | 480.0    | 1650.0   | 1120.0   | 850.0    |
| 1.7      | 460.0    | 1675.0   | 1140.0   | 825.0    |
| 1.8      | 440.0    | 1700.0   | 1160.0   | 800.0    |
| 1.9      | 420.0    | 1725.0   | 1180.0   | 775.0    |
| 2.0      | 400.0    | 1750.0   | 1200.0   | 750.0    |
| 2.1      | 380.0    | 1775.0   | 1220.0   | 725.0    |
| 2.2      | 360.0    | 1800.0   | 1240.0   | 700.0    |
| 2.3      | 340.0    | 1825.0   | 1260.0   | 675.0    |
| 2.4      | 320.0    | 1850.0   | 1280.0   | 650.0    |
| 2.5      | 300.0    | 1875.0   | 1300.0   | 625.0    |
| 2.6      | 280.0    | 1900.0   | 1320.0   | 600.0    |
| 2.7      | 260.0    | 1925.0   | 1340.0   | 575.0    |
| 2.8      | 240.0    | 1950.0   | 1360.0   | 550.0    |
| 2.9      | 220.0    | 1975.0   | 1380.0   | 525.0    |
| 3.0      | 200.0    | 2000.0   | 1400.0   | 500.0    |
| 3.1      | 180.0    | 2025.0   | 1420.0   | 475.0    |
| 3.2      | 160.0    | 2050.0   | 1440.0   | 450.0    |
| 3.3      | 140.0    | 2075.0   | 1460.0   | 425.0    |
| 3.4      | 120.0    | 2100.0   | 1480.0   | 400.0    |
| 3.5      | 100.0    | 2125.0   | 1500.0   | 375.0    |
| 3.6      | 80.0     | 2150.0   | 1520.0   | 350.0    |
| 3.7      | 60.0     | 2175.0   | 1540.0   | 325.0    |
| 3.8      | 40.0     | 2200.0   | 1560.0   | 300.0    |
| 3.9      | 20.0     | 2225.0   | 1580.0   | 275.0    |

这样，通过恰当地选择做多和做空杠杆，可以使做空和做多的回撤接近。比如说，同样是在1000到1250之间运动，做空2.5倍的最大亏损与做多0.9倍类似。如果在两个方向都使用2.5倍杠杆，可能会造成不必要的损失。

——当然，以上分析对比特币狂信者来说并没有什么意义。


[^pizza]: 无意冒犯，这里的实际价值指的是能够换多少个比萨饼。