---
author : "HeYSH"
type : "post"
tags :
    - IPv6
    - 内网
    - cloudflare
categories :
    - web
title: "仅IPv6家庭内网服务实现v6+v4双栈访问"
date: 2023-02-07T17:29:00+08:00
draft: false
---
是的，连回家里NAS的正常手段，当然是ZeroTier/Tinc/OpenVPN之类。但是如果还有其他人想要看照片的话，不要期待手机上会有除了浏览器之外的东西。

在这种场景，如果：

1. 有一台常开的NAS、矿渣或whatever，
2. 在Cloudflare上有一个自己的域名，
3. 家里的宽带有IPv6，而且防火墙可以自己控制（aka 光猫桥接），
4. 不想或者不能打开80、443端口，没办法直接使用Cloudflare代理；

那么可以通过本文的方案，获得高位端口的CF代理。这样，在没有IPv6的地方也能够访问家里的服务了。

如果上面的条件不满足的话，可以尝试`FRP`和`Cloudflare Tunnel`之类的东西。

## 打开端口、DDNS

首先我们需要保证NAS上的IPv6端口能从公网访问，并把域名指向家里的IPv6地址。打开端口的部分可以看看[之前的博文]({{%relref "beyond-nat.md"%}})；DDNS我在使用[ddns-go](https://github.com/jeessy2/ddns-go)（这里可能需要科学上网）。注意，此时先不要开启Cloudflare代理。

完成这部分之后，我们的域名（比如说home.example.net）指向了家里的IP，并且防火墙打开了端口（比如38000）。可以在NAS上开个`python -m http.server 38000`，并用移动网络测试一下。

对了，Cloudflare的token不要扔，待会还有用。

## Caddy

安装[包含Cloudflare DNS插件的Caddy](https://caddyserver.com/download?package=github.com%2Fcaddy-dns%2Fcloudflare)。因为家里没有80和443端口，所以需要用DNS-01方式申请TLS证书：

```Caddyfile
## cat /etc/caddy/Caddyfile
(cloudflare) {
      tls {
        dns cloudflare abcdefgh #刚才说的Cloudflare的token
      }
}
home.example.net:38000 {
      reverse_proxy http://localhost:1234 #这里是内网服务的地址
      import cloudflare #申请证书的部分
```
开启Caddy。这里结束后，移动网络（就是有IPv6地址的网络）应该能够通过`https://home.example.net:38000`访问服务了。

## Cloudflare

{{%figure src="SSL_TLS.png" title="把SSL/TLS加密模式改为完全；"%}}

{{%figure src="Origin_Rules.png" title="添加Origin Rules，把请求转至高位端口；"%}}

开启Cloudflare代理。[在DDNS服务上也开启Cloudflare代理](https://github.com/jeessy2/ddns-go/issues/336)。


好了，大概就是这样。通过CF，也许自家的网络也能稍微安全一点。