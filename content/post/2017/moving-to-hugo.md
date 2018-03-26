+++
author = "HeYSH"
categories = ["web"]
date = "2017-06-07T22:14:47+08:00"
tags = ["hugo","wordpress"]
title = "由wordpress到hugo：I"
type = "post"
#draft = true

+++

我之前的wordpress博客是建在redhat公司的OpenShift服务上的，距[上次]({{<relref "2015-08-23-为什么我的手机用不了ipv6？.md">}})我更换博客后台过了快两年时间，wordpress（或是PHP）再次把空间占满了，而不懂PHP的我只能对着SFTP干瞪眼。于是，我又种了一棵新的树苗。

wordpress的文章是通过[wordpress-to-hugo-exporter](https://github.com/SchumacherFM/wordpress-to-hugo-exporter)导出的，但是导出的文件有一些小问题：

* 标题里都是`<span>`
* 正文还是`HTML`格式
* `img`和`code`需要修改

对于正文，可以用`pandoc`和`pypandoc`转换：

```python
import pypandoc,os
root = "./content"

for rt, dirs, files in os.walk(root):
    for filename in files:
        with open(os.path.join(rt, filename), 'r', encoding='utf-8') as fin:
            data = fin.read()
        ds = data.split('---')
        ds[2] = pypandoc.convert_text(ds[2], 'md', format='html')
        do = '---\n'.join(ds)
        with open(filename, 'w', encoding='utf-8') as file:
            file.write(do)
```

其他的……手改保平安……

……

……………

漫长的修改过后，用`hugo server -w`预览。

这时候还有一个小问题：`hugo`生成URL不一定遵循文件头`url`定义，比如，对
`/2016/06/13/qart-coder：更友好的qr码/`，`hugo`会解析为`/2016/06/13/qart-coder更友好的qr码/`。看出区别了吗？

为了保证（不知道和谁的）兼容性，可以在文件头中增加`aliases`行，可以将原来的地址转发到新地址。

同样的，我也写了一个辣鸡脚本：

```python
import pypandoc
import os
root = "./content"
for rt, dirs, files in os.walk(root):
    for filename in files:
        with open(os.path.join(rt, filename), 'r', encoding='utf-8') as fin:
            data = fin.readlines()
            try:
                urlIndex = [n for n, l in enumerate(data) if l.find("url:") != -1][0]
                url = data[urlIndex].split("url:")[1]
                aliasLine = "  -" + url
                data.insert(urlIndex + 1, aliasLine)
                data.insert(urlIndex + 1, "aliases:\n")
            except:
                pass
        with open(filename, 'w', encoding='utf-8') as file:
            for d in data:
                file.write(d)
```