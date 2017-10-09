---
author : "HeYSH"
type : "post"
tags :
    - hugo
    - spoiler
    - collapsible contents
    - hidden contents
categories :
    - html
title: "在Hugo中折叠部分内容"
date: 2017-10-07T22:25:35+08:00
draft: false
---
之前疯狂粘贴自己的[扩展]({{%ref "firefox-to-quantum.md"%}}) ~~凑字数~~ 的时候，我发现这种又臭又长的列表需要稍微隐藏一下，但是这里用的静态博客生成器 Hugo 并不原生支持折叠。所以，我利用 `shortcode` 功能自己写了一个。

`Shortcode` 的介绍在[这里](https://gohugo.io/templates/shortcode-templates/)，主要难度在于go的html模板引擎。

折叠部分用了 HTML5 的新特性：[\<details\>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details)，不支持 edge，这个浏览器应该没有人用吧……

下面是shortcode的代码，放在`/themes/<THEME>/layouts/shortcodes/spoiler.html`下：
{{% spoiler "shortcode，点击展开"%}}
```html
<details>
<summary>
  <h4>
    {{ with .Get 0}}{{.}}{{else}}click to expand{{ end }}
  </h4>
</summary>
{{.Inner}}
</details>
```
{{% /spoiler %}}

用法是这样的：
```html
{{%/* spoiler "You killed my father!" */%}}
I am your father.
NOOOOOOO!
{{%/* /spoiler */%}}
```

当然，我还稍微改了一下CSS，之后会在模板里面更新。

spoiler在steemit社区的讨论：https://github.com/steemit/condenser/issues/8