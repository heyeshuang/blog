---
author : "HeYSH"
type : "post"
tags :
    - IPv4
    - NAT
    - 内网
    - Wireguard
    - NATMap
    - VPN
categories :
    - web
title: "内网访问第三季：在运营商的CGNAT网络下"
description: "用回Wireguard吧，再加上NATMap"
date: 2023-05-10T21:05:13+08:00
draft: false
---

> 是的，在[这次]({{< ref "beyond-nat" >}})和[这次]({{< ref "connect-every-something">}}/#远程访问tailscaletinc或wireguard )之后，在酒店里百无聊赖的现在，我又开始折腾起VPN来了。

> 是的，在酒店里仍然没有IPv6地址。

## 为什么

正如[前文]({{< ref "connect-every-something">}}/#远程访问tailscaletinc或wireguard )所述，无论是Tailscale/Headscale、Nebula或Netmaker，原理均大同小异，都是在Wireguard基础上，用类STUN协议来穿越NAT，或利用TURN（DERP）服务器进行转发。在国内家庭宽带网络环境下，一般存在路由器、光猫、运营商三重NAT防火墙，STUN需要跨越多重阻碍，自动穿越希望渺茫；另一方面，公开转发服务多在国外，延迟高居不下，而国内私有云价格亦是高不可攀，自建服务并非经济的选择。

然而，三重NAT也并非坚不可摧。光猫一级，只要改为桥接，便可迎刃而解；路由一级，可以通过端口映射来绕过；而运营商级多为NAT1，通过[Natter](https://github.com/MikeWang000000/Natter/tree/v0.9)或[natmap](https://github.com/heiher/natmap)，可以获得近似公网的效果。这样，使用纯粹的Wireguard，也能够直接回到家庭网络内部，免去国外中转的烦恼。

{{< figure src="ping1.png" caption="从广东联通到北京联通。" attr="" attrlink="" >}}
{{< figure src="ping2.png" caption="从广东联通经北京联通到甘肃移动。个人觉得，在这种延迟下追求Full Mesh也不再重要了。" attr="" attrlink="" >}}

## 怎么做

在开始之前，首先检查是否满足以下要求：

1. 一台长期开启的设备。
    > 既然有远程访问的要求，远处有一台服务器是很自然的吧。
1. 光猫处于桥接状态。
1. 主路由是OpenWRT，或者内网里有DMZ主机。
    > 或者，你是端口转发专家，可以从光猫外侧一路转发到最内部。
1. 没有公网IPv4，但在路由器处测试NAT类型为NAT1。
    > 这里可以用[Natter](https://github.com/MikeWang000000/Natter/tree/v0.9)自带的功能来测试。如果你有公网IPv4的话，直接打开端口就好，而且我会很羡慕你。
1. 一个自己的域名，最好是在Cloudflare上托管的。
    > 需要DDNS功能实时更新域名。如果没有域名的话，可能需要一些别的手段来实时得到端口。

具体配置部分已经有人写的很详细了。首先按照[WireGuard Point to Site Configuration](https://www.procustodibus.com/blog/2020/11/wireguard-point-to-site-config/)设置点到站点的连接，然后按照[natmap Wiki](https://github.com/heiher/natmap/wiki/wireguard)设置NATMap即可。注意，在路由器上操作的时候，一定要记得在防火墙中**打开对应端口**。

完成以上步骤之后，应该已经可以从移动网络访问内网的Wireguard Peer了。

## 一点问题

由于运营商网关不受我们控制，外网的IP和端口号都是随机分配的，每当地址变化时，NATMap将执行自定义脚本。在上面的Wiki中，利用DDNS，把IPv4地址和端口编码进IPv6的AAAA记录中。这并不是一种标准的技术，不过既然`2001::`就是给`teredo`使用的，在这里随便用用也无所谓。

对于Windows下的Wireguard客户端，我（和ChatGPT一起）写了一个[PowerShell脚本](https://gist.github.com/heyeshuang/0054c73e3f2762f12a16165a5cfe8213#file-wg-ps1)，能够自动修改配置文件的`Endpoint`并调用`wireguard.exe`进行连接。

使用方法：

1. 安装[wireguard-windows](https://github.com/WireGuard/wireguard-windows/blob/master/docs/enterprise.md)，用客户端连接测试成功。
2. 在文件夹`C:\example`下建立[wg.ps1](https://gist.github.com/heyeshuang/0054c73e3f2762f12a16165a5cfe8213#file-wg-ps1)和[nat.conf](https://gist.github.com/heyeshuang/0054c73e3f2762f12a16165a5cfe8213#file-nat-conf)，粘贴Gist内容。
3. 按照实际情况修改`nat.conf`，以及`wg.ps1`中`$Hostname`部分。`Endpoint`不必修改。
4. 以管理员身份运行`PowerShell`
5. 设置`ps1`脚本运行权限：`Set-ExecutionPolicy RemoteSigned`（或Unrestricted）
6. 启动、重启Wireguard：`C:\example\wg.ps1 -up`
7. 停止Wireguard：`C:\example\wg.ps1 -down`

在Windows 11, Powershell  5.1.22621.963测试通过，也可以配合Windows下的[sudo](https://bjansen.github.io/scoop-apps/main/sudo/)使用。

另外，在Android下，也可以用termux运行[nm-echo.sh](https://gist.github.com/heyeshuang/0054c73e3f2762f12a16165a5cfe8213#file-nm-echo-sh)来获得IP地址，手动修改Wireguard官方客户端中的IP。甚至，可以使用你喜欢的代理客户端（比如NB4A），配置好Wireguard Outbond和路由就可以了~

{{< figure src="连接时间.png" caption="我这里最近一次分配的端口坚持了18天，所以应该不必时常刷新。" >}}

如果需要更为稳定的访问，可以参考[reresolve-dns.ps1](https://gist.github.com/z0mb1e-kgd/54aede86adf2e30e390dba13886d18e1)，这个脚本可以在上一次握手时间过久时刷新DNS，但是因为要添加计划任务，有一点过于复杂了。

## Bonus

Natmap的另一种用法是映射BT客户端，从而使外来连接能够主动发起连接，获得所谓的`High ID`。见[wits-fe/bittorrent-NAT-hole-punching](https://github.com/wits-fe/bittorrent-NAT-hole-punching)。在PT站做种的时候应该会很有用。

## 附：性能测试

> 随便找了一个公共WiFi，用手机（一加7T）上Termux中的`iperf3`测速。

由于多层NAT的限制，Nebula类组网工具必须部署在路由器位置。可以看出，对路由器（万元级，K3）带来的压力还是比较大的。

{{< figure src="iperf-1.svg" caption="WireGuard for Android" attr="" attrlink="" >}}
{{< figure src="iperf-2.svg" caption="NB4A提供的Wireguard Outbound" attr="" attrlink="" >}}
{{< figure src="iperf-3.svg" caption="Nebula，对端部署在路由器上" attr="" attrlink="" >}}

很难想象，对于访问内网这样一个简单的需求，我居然花费了如此多的精力。不过，这次应该算是当前比较满意的方案，应该能坚持到下次水逆开始。
