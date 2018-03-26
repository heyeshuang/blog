---
author : "HeYSH"
type : "post"
tags :
    - kcp
    - kcptun
    - kcpraw
    - 网络
categories :
    - 折腾
title: "利用kcpraw流量加速"
date: 2017-11-18T16:36:55+08:00
draft: false
---

[2015年]({{% ref "2015-01-07-shadowsocks，ipv6和便宜的vps.md"%}})和[2016年]({{% ref "2016-10-25-shadowsocks-2016-一次性认证，kcp及其他.md"%}})，我写过两篇关于 *科学上网* 的博客，之后，又在VPS上安装了[v2ray](https://v2ray.com/)，这个应用直接支持mKCP，在丢包严重的网络环境下效果不错。

然后是最近，为了贯彻**大精神，实现什么什么自信的目标，KCP依赖的udp包被自信的家伙们无情地屏蔽了。在标准的shadowsocks和使用websocket的v2ray下，网速在50k左右，我感觉自己的网瘾又要发作了——

以上是背景。

虽然UDP被屏蔽了，但是TCP暂时还幸免于难，所以，可以把UDP包伪装成TCP的样子来绕过检查。有两个程序可以实现这种功能：

* [udp2raw](https://github.com/wangyu-/udp2raw-tunnel)，可以将所有UDP包整容成TCP的样子，不过这个程序在Windows下有些问题，而且最近Firefox的一堆破事让我不太想用Linux了；
* [kcpraw](https://github.com/ccsexyz/kcpraw)，是原始kcptun的fork，和kcptun的用法类似，在Windows下只需要安装[winpcap](https://www.winpcap.org/install/default.htm)就可以用。

在这里使用kcpraw包装原来shadowsocks的流量（纯粹是因为virtualbox没配好用不了udp2raw），假设读者有基本的linux知识并已经部署了shadowsocks。

## 服务器端

### 下载kcpraw

```
mkdir ~/kcpraw
cd ~/kcpraw
wget https://github.com/ccsexyz/kcpraw/releases/download/v20171019/kcpraw-linux-amd64-20171021.tar.gz
tar zxvf kcpraw-linux-amd64-20171021.tar.gz
```

### 在后台开启kcpraw

```
 sudo -b nohup ~/kcpraw/kcpraw_server_linux_amd64  -t "127.0.0.1:443" -l ":8388" -mode fast2 &
```
其中，`443`是要包装的shadowsocks端口，同时，`8388`是kcpraw使用的端口，请保证服务器防火墙允许`8388`端口通过。

## 客户端

### 安装winpcap（如果客户端是Windows的话）

对于最新版本的Windows10，原版的`winpcap`和`win10pcap`似乎都不能用，可以使用兼容的[npcap](https://github.com/nmap/npcap)。

### 运行kcpraw

```
kcpraw_client_windows_amd64 -r "<服务器IP>:8388" -l ":1082" -mode fast2
```

其中`1082`是设定的本地kcpraw端口。

### 修改shadowsocks客户端配置

在你使用的客户端中把服务器地址设为`localhost:1082`，其余不变，即可使shadowsocks享受KCP加速。

天气越来越冷，似乎离互联网白名单也越来越近了。到那时，我可能会换一个爱好，比如做一个伟大的白日梦什么的。
