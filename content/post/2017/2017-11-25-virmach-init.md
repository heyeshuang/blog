---
author : "HeYSH"
type : "post"
tags :
    - VPS
    - virmach
categories :
    - 折腾
title: "VPS初始化checklist"
description: "环境：Virmach的KVM架构"
date: 2017-11-25T12:35:54+08:00
draft: false
---

仅供存档。另外，virmach黑五大减价了，KVM主机5.6$一年美滋滋。

这是我的邀请链接：<https://billing.virmach.com/aff.php?aff=3032>，优惠码是`2017BFSAVE20`。

## 新建用户
感谢<https://blog.roadofgrowth.com/2015/04/28/e6-96-b0-e8-b4-advps-e5-88-9d-e5-a7-8b-e5-8c-96-e5-ae-89-e5-85-a8-e9-85-8d-e7-bd-ae/>
```
adduser hh
gpasswd -a hh sudo
```
## 添加SSH-KEY验证并禁用root用户SSH远程登录
```
su - hh
mkdir .ssh
chmod 700 .ssh
vi .ssh/authorized_keys
```
在这里粘贴你的公钥。什么？noVNC无法粘贴？我写了篇[博文]({{<relref "klipper-autotype.md">}})来解决这个。
```
#vi /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication no
Port 22222
```

```bash
sudo systemctl restart ssh
```
## 安装配置防火墙 (ufw)

```
sudo apt-get update&&sudo apt-get upgrade
sudo apt-get install ufw
sudo ufw allow 22222 #允许SSH
sudo ufw enable
```
## 安装vim
## 安装shadowsocks-libev
```
sudo sh -c 'printf "deb http://httpredir.debian.org/debian jessie-backports main" > /etc/apt/sources.list.d/jessie-backports.list'
sudo apt update
sudo apt -t jessie-backports install shadowsocks-libev
sudo vim /etc/shadowsocks-libev/config.json
sudo ufw allow <PORT>
sudo systemctl start shadowsocks-libev
sudo systemctl enable shadowsocks-libev
```
## 安装4.x内核
```
sudo apt-get install -t jessie-backports linux-image-amd64
sudo update-grub
```
## 启用BBR
<https://github.com/iMeiji/shadowsocks_install/wiki/%E5%BC%80%E5%90%AFTCP-BBR%E6%8B%A5%E5%A1%9E%E6%8E%A7%E5%88%B6%E7%AE%97%E6%B3%95#%E5%BC%80%E5%90%AFbbr>
