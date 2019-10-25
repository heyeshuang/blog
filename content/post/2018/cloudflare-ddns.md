---
author : "HeYSH"
type : "post"
tags :
    - DNS
    - Cloudflare
    - domain
categories :
    - web
title: "用Cloudflare的v4 API实现动态DNS（DDNS）"
date: 2018-07-05T21:41:42+08:00
draft: false
---
如果你有一个经常变化的公网IP，你可能会需要用DDNS将这个IP绑定在同一个域名上，这样就可以不必每次输入一串IP地址了。我用的DNS服务商Cloudflare并没有提供名为DDNS的服务，但是其API可以实现类似的效果。

之前我用的是[这里](https://lifely.today/2015/05/%E7%82%BA-cloudflare-%E8%A8%AD%E5%AE%9A%E5%8B%95%E6%85%8B-dns-ddns/)的方法，不过最近cloudflare更新了[第四代API](https://api.cloudflare.com/)，那个方法已经失效。于是水博文的机会来了。

原料：

1. 一个域名，并用Cloudflare管理
2. 一台经常开着的，有计划任务或者cron，可以运行curl的电脑
3. （可选）如果API地址被屏蔽的话，可能需要可用的代理

步骤：

- 去<https://dash.cloudflare.com/>找到你域名的`zone ID`并新建一个`API_KEY`。
- 新建一条子域名的A记录，IP写什么都行。
- 查询这条记录的`RECORD_ID`，在终端运行：

```bash
curl -X GET "https://api.cloudflare.com/client/v4/zones/<ZONE_ID>/dns_records" \
     -H "X-Auth-Email: <邮箱地址>" \
     -H "X-Auth-Key: <API_KEY>" \
     -H "Content-Type: application/json" \
     -x http://localhost:8087
```
其中 `-x`是本地代理地址，可选。大概能从返回的一坨json中找到id那一项。

- 保存下面的脚本，修改`<ZONE_ID>`，`<RECORD_ID>`，`邮箱地址`，`API_KEY`，`子域名`什么的。如果不需要代理的话删除`-x`那一行。

```bash
#!/bin/sh
NEW_IP=`curl http://members.3322.org/dyndns/getip`
CURRENT_IP=`cat /var/tmp/current_ip.txt`
if [ "$NEW_IP" = "$CURRENT_IP" ]
then
     echo "No Change in IP Adddress"
else
     curl -X PUT "https://api.cloudflare.com/client/v4/zones/<ZONE_ID>/dns_records/<RECORD_ID>" \
     -x http://localhost:8087 \
     -H "X-Auth-Email: <邮箱地址>" \
     -H "X-Auth-Key: <API_KEY>" \
     -H "Content-Type: application/json" \
     --data '{"type":"A","name":"<子域名地址>","content":"'$NEW_IP'","ttl":1}'
     echo $NEW_IP > /var/tmp/current_ip.txt
fi
```

- 接下来让它每两个小时运行一次就好了。
```bash
# crontab -e
*/120 * * * * /path/of/your/script.sh
```

~~写字好累（躺~~

---
来自19-10-25的更新：DNSpod的公网IP查询服务响应有些奇怪，所以换成了3322。