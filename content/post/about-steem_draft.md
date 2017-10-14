---
author : "HeYSH"
type : "post"
tags :
    - steem
    - steemit
categories :
    - 折腾
title: "Steem"
date: 2017-10-08T22:56:21+08:00
draft: true
---

sanitize
https://github.com/steemit/condenser/blob/4d9c1d8f76366097d9a7159b1760f51d45201eb0/src/app/utils/SanitizeConfig.js
div, iframe, del,
a, p, b, i, q, br, ul, li, ol, img, h1, h2, h3, h4, h5, h6, hr,
blockquote, pre, code, em, strong, center, table, thead, tbody, tr, th, td,
strike, sup, sub
< div class="pull-right"> < img src="imglink" />< /div >
const classWhitelist = ['pull-right', 'pull-left', 'text-justify', 'text-rtl', 'text-center', 'text-right', 'videoWrapper']
SteemData：数据库接口

markdown 库是[remarkable](https://github.com/jonschlinkert/remarkable)
steemit地址 https://github.com/steemit/condenser/

https://github.com/steemit/condenser/blob/8a4e470fea6a34a9e19f8598c5261fd414d96fa8/src/app/components/cards/MarkdownViewer.jsx

网址：通过<https://github.com/pid/speakingurl>生成
是否适合当博客？

格式受到限制
