+++
author = "HeYSH"
categories = ["web"]
date = "2017-06-12T22:25:07+08:00"
tags = ["hugo","feed"]
title = "迁移到hugo，第三部分：可是我想要更多的feed地址…"
type = "post"
+++

之前在 <http://heyeshuang.tk> 这个域名下的时候，wordpress提供的RSS feed地址格式类似 <http://heyeshuang.tk/feed/> ，但是hugo提供的RSS地址是`index.xml`样子的，而且我翻遍了manual也没看到给RSS加alias的方法。

所以只能改模版了，思路是把`/feed`目录的列表页渲染成RSS的样子，我是直接在主题里面改的：

`\themes\<主题名字>\layouts\feed\list.html`：

~~~html
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ if eq  .Title  .Site.Title }}{{ .Site.Title }}{{ else }}{{ with .Title }}{{.}} on {{ end }}{{ .Site.Title }}{{ end }}</title>
    <link>{{ .Permalink }}</link>
    <description>Recent content {{ if ne  .Title  .Site.Title }}{{ with .Title }}in {{.}} {{ end }}{{ end }}on {{ .Site.Title }}</description>
    <generator>Hugo -- gohugo.io</generator>{{ with .Site.LanguageCode }}
    <language>{{.}}</language>{{end}}{{ with .Site.Author.email }}
    <managingEditor>{{.}}{{ with $.Site.Author.name }} ({{.}}){{end}}</managingEditor>{{end}}{{ with .Site.Author.email }}
    <webMaster>{{.}}{{ with $.Site.Author.name }} ({{.}}){{end}}</webMaster>{{end}}{{ with .Site.Copyright }}
    <copyright>{{.}}</copyright>{{end}}{{ if not .Date.IsZero }}
    <lastBuildDate>{{ .Date.Format "Mon, 02 Jan 2006 15:04:05 -0700" | safeHTML }}</lastBuildDate>{{ end }}
    <atom:link href="{{.Permalink}}" rel="self" type="application/rss+xml" />
    {{ range .Site.RegularPages }}
    <item>
      <title>{{ .Title }}</title>
      <link>{{ .Permalink }}</link>
      <pubDate>{{ .Date.Format "Mon, 02 Jan 2006 15:04:05 -0700" | safeHTML }}</pubDate>
      {{ with .Site.Author.email }}<author>{{.}}{{ with $.Site.Author.name }} ({{.}}){{end}}</author>{{end}}
      <guid>{{ .Permalink }}</guid>
      <description>{{ .Content | html }}</description>
    </item>
    {{ end }}
  </channel>
</rss>
~~~
基本上就是hugo自带的RSS模板，除了把`{{ range .Data.Pages }}`改成了`{{ range .Site.RegularPages }}`，因为我需要生成整个网站的RSS。另外，修改过的主题在<https://github.com/yoshiharuyamashita/blackburn>，稍微改了一些小bug，还换了个颜色。~~哪天我要把名字改成greenfreeze。~~

然后别忘了新建一个`\content\feed\chicken.md`：
~~~
---
title: 'chicken'
author: HeYSH
date: 2012-06-22T06:32:32+00:00
aliases:
  - /%3ffeed%3drss2
---
Nothing here.
~~~

当然，类似`/?feed=rss2`这样的地址就用不了了，我试过的。
