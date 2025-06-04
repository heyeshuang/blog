---
author : "HeYSH"
type : "post"
tags :
    - IPv6
categories :
    - web
title: "与IPv6地址战斗"
date: 2025-06-04T21:13:09+08:00
draft: false
---

> 捣腾家里服务器的另一些副产品。

## 配置ULA前缀

当你需要设置IPv6网关时，常常需要一个不随着运营商前缀变化的内网地址，这时候设置ULA很好用。

{{< figure src="LuCI.png" caption="OpenWrt设置ULA前缀" >}}  

这样，在需要临时修改网关时，可以直接

```bash
sudo ip -6 route del default && sudo ip -6 route add default via fd22:9:30::88:88 metric 100
```
其中，`fd22:9:30::88:88`是网关地址。

## 自定义后缀

相比于`60d1:24e0:fe4a:5c66`这样的后缀，还是`::88`更帅一些。~~回字~~自定义后缀有4种写法，我们Linux真是太厉害辣。

如果是原生Debian，用ifupdown的话：
```bash
# /etc/network/interfaces
iface <你的ensXX> inet6 auto
        post-up /sbin/ip token set ::88 dev $IFACE
```

如果是Ubuntu（或者Ubuntu味道的Armbian），一般使用networkmanager:
```bash
sudo nmcli connection modify "Wired connection 1"     ipv6.addr-gen-mode eui64
sudo nmcli connection modify "Wired connection 1" ipv6.token "::88"
```

如果是（直接安装的）OpenMediaVault，它使用netplan:
```YAML
# touch /etc/netplan/30-fix-token.yaml
network:
  ethernets:
    ens18:
      ipv6-address-token: "::123:123"
# sudo netplan apply
```

应该还会有人用[systemd-networkd](https://www.freedesktop.org/software/systemd/man/latest/systemd.network.html#Token=1)配置网络，自己读manpage去吧，祝你成功。

## 更快地推送IPv6地址

[之前]({{< relref "cloudflare-ddns.md" >}})的DDNS更新脚本，或者[ddns-go](https://github.com/jeessy2/ddns-go)，都是使用cron来轮询的。而`ip monitor`命令不需要轮询，在IP地址变更的第一时间就会发送消息。配合靠谱的推送服务，可以比别人提前……呃……大概五分钟。

~~所以没什么用处，建议直接ddns-go。~~

```
#cat /etc/systemd/system/dns-updater.service
[Unit]
Description=Dynamic DNS updater
DefaultDependencies=no
Wants=local-fs.target
After=local-fs.target
Wants=network-pre.target
Before=network-pre.target

[Service]
Type=exec
ExecStart=/opt/ddns-updater

[Install]
WantedBy=sysinit.target
```

```bash
#cat /opt/ddns-updater
#!/bin/sh -e

ip -o -6 monitor address |
    while read index iface proto addr dummy scope temporary dummy; do
        case "$iface:$proto:$scope:$temporary" in
            ens18:inet6:global:dynamic)
                addr="${addr%/*}"
                case "$addr" in
                        f*) continue ;;   # 跳过以 f 开头的地址
                esac
                if [ "$addr" != "$current" ]; then
                    NEW_IP="$addr"
                    # 我这里发送到了企业微信机器人，也可以用bark、nfty.sh之类
                    curl 'https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=xxxxxx' \
                        -H 'Content-Type: application/json' \
                        -d "{
                            \"msgtype\": \"text\",
                            \"text\": {
                                \"content\": \"IPv6地址已更新：$NEW_IP\"
                            }
                        }"
                    echo "已通知企业微信，新地址：$NEW_IP"
                    current="$addr"
                fi;;
        esac
    done
```