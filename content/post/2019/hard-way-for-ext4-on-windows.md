---
author : "HeYSH"
type : "post"
tags :
    - linux
    - windows
    - ext4
categories :
    - 折腾
title: "震惊！为了在Windows上访问EXT4分区，他竟然做出了这样的事！"
date: 2019-03-02T23:19:02+08:00
draft: false
---

呃……原谅这次的标题党，但我觉得这次自己的操作实在是太蠢了。

事情是这样的，作为一个半吊子，我的电脑上同时有Windows 10和Manjaro Linux两个操作系统，也就有了从Windows上访问Ext4分区的要求。大约在一年之前，这个要求可以用[Ext2fsd](https://www.ext2fsd.com/)来满足，可是一年过去，它已经无法识别新的Ext4版本了。

于是，我……我做了一个linux虚拟机来帮我读这个分区。

具体来说，VirtualBox支持将单独的分区视为虚拟磁盘[^1][^2]。你需要在 **具有管理员权限** 的命令提示符中运行：

```
VBoxManage internalcommands createrawvmdk -filename "C:\Users\<user_name>\VirtualBox VMs\<VM_folder_name>\<file_name>.vmdk" -rawdisk \\.\PhysicalDrive0 -partitions 1,5 -relative
```
这个程序大概在VirtualBox的安装目录下，磁盘号和windows的“磁盘管理”程序一致，分区号可以用下面的命令获得。
```
VBoxManage internalcommands listpartitions -rawdisk \\.\PhysicalDrive0
```

之后，

- 同样是用 **管理员权限** 运行VirtualBox
- 把网卡模式改为桥接
- 挂载刚才生成的VMDK文件
- **在另一个虚拟磁盘上** 安装Linux（这里我用的是Alpine Linux Extended）
- 设置Samba[^3]

就可以在宿主机上用网络硬盘访问Ext4分区了。

而且能够收获一次反思：我为什么要浪费宝贵的时间干这个？


[^1]:https://www.virtualbox.org/manual/ch09.html#rawdisk
[^2]:https://www.serverwatch.com/server-tutorials/using-a-physical-hard-drive-with-a-virtualbox-vm.html
[^3]:https://wiki.alpinelinux.org/wiki/Setting_up_a_samba-server
