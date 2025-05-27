---
author : "HeYSH"
type : "post"
tags :
    - NAS
    - 服务器
    - 远程控制
    - 内网
categories :
    - 折腾
title: "半年前的NAS升级"
date: 2025-04-12T11:37:49+08:00
draft: false
toc: true
---
2024年10月，我决定把之前的垃圾一号锐角云和垃圾二号蜗牛星际退役，整点性能更高的东西，从海鲜市场捡了一套HP Elitedesk 800G4 SFF。


{{% spoiler "当时的价格是这样："%}}
|          	| 型号          	| 价格 	| 渠道 	|
|----------	|---------------	|------	|------	|
| 准系统   	| 800G4         	| 330  	| 闲鱼 	|
| CPU      	| I5-8500       	| 289  	| 淘宝 	|
| 内存     	| 金龙惠宇16G*2 	| 200  	| PDD  	|
| SSD      	|  1T           	| 400  	| JD   	|
| 万兆网卡 	| MNPA19-XTR*2  	| 71   	| 淘宝 	|
| 交换机   	| SE106Pro      	| 129  	| 淘宝 	|
| 网线     	| 光纤AOC线*2   	| 19   	| 淘宝 	|
| SATA线   	| 单弯头的比较好    	| 19   	| 淘宝 	|
| 合计   	|               	| 1457 	|      	|
{{% /spoiler %}}


不算万兆互联和SSD的部分大概是800块。听说我下单之后这套准系统就开始涨价了，B站带货真可怕啊。

这台NAS（或者家用服务器）在这半年的服役中效果良好，得益于白金电源，加上万兆网卡和3.5硬盘×2、2.5×1，平均耗电大概只有40W左右。因为是品牌机，据说不支持9代CPU，但互联网上的技术支持比一般的山寨主板要好的多，甚至有完整的[维护手册](https://h10032.www1.hp.com/ctg/Manual/c06472102.pdf)。硬盘螺钉之类的小配件JS会单卖，不过如果有3D打印机的话，也可以自己打：

- [3.5寸硬盘螺钉](https://www.printables.com/model/817439-hp-prodesk-hard-drive-grommet-for-6-32-screws?lang=en)（5块钱4个）
- [2.5寸硬盘螺钉](https://www.printables.com/model/252727-hp-hard-drive-mounting-grommet?lang=en)
- [SFF支架](https://www.printables.com/model/720124-stand-for-hp-elitedesk-800-g8-sff?lang=en)
- [Mellanox ConnectX-2 半高挡板](https://www.thingiverse.com/thing:3119803/comments)（7块1个）

{{% figure src="10G_eth.png" title="PLA打印的半高挡板，暂时还没有融化" %}}

软件方面，使用大家都喜欢的Proxmox VE，一共开了4个虚拟机，没有LXC容器：

- `HAOS`给“智能”家居用，直通了ZBDongle-P。
- `gateway`用来和互联网联系。用`cloudflared`和`caddy`代理内网服务，`wireguard`和`natmap`来访问内网。
- `OpenMediaVault`直通SATA控制器。24年8月买的二手`HC530`暂时还能用。
- `docker-host`用来放一些小服务，因为docker会在iptables里搞东搞西，可能会把`gateway`里的服务弄坏。

{{% figure src="gateway.png" title="按照惯例（？），放一张服务的图" %}}

## 远程控制

另外，因为是企业级系统，800G4支持Intel vPro，配合8500以上级别的CPU可以开启AMT，实现类似IPMI、IP-KVM的“带外”远程控制功能。不过还有一个问题：在开启GPU直通之后，HDMI的输出不再来自主机，远程控制没有图像信号。

为了解决这个问题，可以使用AMT的Serial-Over-LAN功能，使用串口和主机通信。反正没有人会在PVE主机上安装GUI程序对吧。

首先，在BIOS里开启AMT和SOL，记得设置静态IP地址，不需要和PVE系统地址一致；密码要设的长一些，应该包含大小写字母和数字。

控制上建议使用[meshcmd](https://www.meshcommander.com/meshcommander/meshcmd)，在ARM、x86上都可以运行。`./meshcmd meshcommander`会开启一个服务器，算是远程访问的远程访问。

BIOS、GRUB和PVE都需要通过串口控制，因为是企业级，HP的BIOS直接支持串口访问；GRUB和Linux[开启串口访问](https://forum.proxmox.com/threads/retain-host-video-output-when-passing-through-igpu.154855/post-705409)需要

- 找到TTY端口，我这里是TTY4
```bash
root@pve-hp:~# dmesg|grep tty
...
[    0.419539] 0000:00:16.3: ttyS4 at I/O 0x3088 (irq = 19, base_baud = 115200) is a 16550A
...
```
- 在`/etc/default/grub`里添加串口设置
```bash
# cat /etc/default/grub
...
GRUB_CMDLINE_LINUX_DEFAULT="quiet console=tty4 console=ttyS4,115200n8" #在原始参数后追加
GRUB_TERMINAL="console serial" #GRUB自身也采用串口输出
GRUB_SERIAL_COMMAND="serial --unit=4 --speed=115200"
...
```
- 之后，运行`update-grub`和`systemctl enable serial-getty@ttyS4.service`，就能通过`meshcommander`访问主机了。

{{% figure src="MeshCommander-1.png" %}}


突然想到，对于普通电脑，只要搞两根USB转RS-232串口线交叉连结，就可以实现 [IP-KVM]({{< relref "ipkvm_4_poor_man" >}}) 80%的效果——就是有点过于low-tech。

## 另外的一些小问题

- I219-LM网络问题：[e1000e drvier hang](https://forum.proxmox.com/threads/e1000-driver-hang.58284/page-4#post-303366)
> NETDEV WATCHDOG: enp0s31f6 (e1000e): transmit queue 0 timed out
```bash
# /etc/network/interfaces
iface eno1 inet manual
        post-up ethtool -K eno1 tso off gso off
```
- GPU直通：[iGPU Passthrough to VM](https://3os.org/infrastructure/proxmox/gpu-passthrough/igpu-passthrough-to-vm/?utm_source=pocket_saves#proxmox-configuration-for-igpu-full-passthrough)

建议使用GRUB，不建议用sysctl方式设置，万一搞坏了系统，在GRUB界面改回去就能取消直通。
