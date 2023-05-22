---
author : "HeYSH"
type : "post"
tags :
    - IPv6
    - NAT
    - 内网
categories :
    - web
    - 折腾
title: "你可能并不需要内网穿透"
description: "如果你的IPv6网络畅通的话"
date: 2021-07-20T17:03:31+08:00
draft: false
---

> 最近搬家了，互联网从联通变成了便宜一些的电信。于是，我失去了之前的公网IP`114.*.*.*`，换来了`100.64.*.*`[^1]。不过，IPv6地址仍然是`2404::****`，这个应该也算是所谓的 *公网IP* 吧？

## 准备

1. 和安装网络的大叔说，我需要把光猫改成桥接模式，并保证“咱是专业的，不会弄坏网线，弄坏了也不会去投诉”；
2. 找一个稍微新一点的路由器，我用的是刷了`OpenWRT`的，**非常贵的**跑路K3；
4. [test-ipv6.com](https://test-ipv6.com)显示了IPv6地址而且没有给你打零分。
{{%figure src="ipv6-test.png" title="比如这样"%}}

## 打开端口

虽然没有了NAT，但是防火墙还是必不可少的，毕竟在互联网上随意敞开自己的端口和裸奔没有什么区别。当然，稍微露出一点点的话没有什么问题，所以我开了几个五位数的端口，用来SSH。

{{%figure src="ipv6-firewall.png" title="就像这样（图像经过处理，实际上有更多规则，而且端口也不是这几个）"%}}

如果对安全要求更高的话，可以参考[这里](https://blog.ptsang.net/match-ipv6-dynamic-addresses-in-iptables?utm_source=pocket_mylist)匹配IPv6地址的末几位，就像`::abcd:1234:5678:90ef/::ffff:ffff:ffff:ffff`这样。

在这个时候，你应该已经可以通过手机移动网络用SSH连接回自己主机的IPv6地址了。就和公网一模一样。

## 绑定域名

任何一个支持IPv6的AAAA地址绑定的DDNS服务都可以。我家里恰好有一个长期开机的矿渣`贝壳云`，是某次水逆之后想要买树莓派四，忍住了却又没有完全忍住的结果；之前写的[cloudflare脚本]({{%relref "cloudflare-ddns"%}})刚好能用。


## 跳板机（自称）

如果想对内网完全控制，而不是仅仅几个端口的话，可能需要所谓跳板机的配合。仔细想想，这是一个跨越`防火墙.little`的活动，对付`防火墙 the Great`的软件也完全适用。所以我在内网设备（aka矿渣）上部署了某个V开头的软件，通过Android客户端连回家里完全没有问题[^2]。

如果外面的Windows想要进来的话，我现在用的是已经跑路的`SocksCap64`[^3]。把`MSTSC.exe`加到列表里，就可以愉快地远程桌面了。

---

来自210805的更新：后来我按照[这篇教程](https://chanix.github.io/TincCookbook/introduction/)在矿渣上部署了tinc，并且增加了[ARP代理](https://tinc-vpn.org/examples/proxy-arp/)用来访问内网。我又不能直接把家里的老光猫换掉……

[^1]:用于在电信级NAT环境中服务提供商与其用户通信，[维基百科](https://zh.wikipedia.org/wiki/%E4%BF%9D%E7%95%99IP%E5%9C%B0%E5%9D%80)上说的。
[^2]:记住客户端不要`绕过局域网地址`，我们用的就是局域网。
[^3]:TODO：急求一款没有跑路的、免费的、Windows下的全局代理软件。