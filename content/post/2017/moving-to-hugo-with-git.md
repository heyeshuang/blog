+++
date = "2017-06-07T22:07:44+08:00"
tags = ["hugo", "git"]
title = "迁移到hugo，第二部分：部署到github"
author = "HeYSH"
categories = ["web"]
type = "post"
+++

书接上文 ~~（虽然我是先写的这一篇）~~，当我们有了一个可用的hugo目录后，就可以把它部署到github等静态网页托管服务了。如果你的强迫症不是很严重的话，可以把`/public`目录直接push到新的repo里；不过如果为了得到一个整洁的主页，可以用同一个repo的`master`分支保存源文件，`gh-page`分支来展示，像[这里](http://www.gohugo.org/doc/tutorials/github-pages-blog/)说的那样。

这里假设你已经`init`过了你的`hugo`目录：

```
# Create a new orphand branch (no commit history) named gh-pages
git checkout --orphan gh-pages

# Unstage all files
git rm --cached $(git ls-files)

# Grab one file from the master branch so we can make a commit
git checkout master README.md

# Add and commit that file
git add .
git commit -m "INIT: initial commit on gh-pages branch"

# Push to remote gh-pages branch
git push origin gh-pages

# Return to master branch
git checkout master

# Remove the public folder to make room for the gh-pages subtree
rm -rf public

# Add the gh-pages branch of the repository. It will look like a folder named public
git subtree add --prefix=public <你的.git地址> gh-pages --squash

# Pull down the file we just committed. This helps avoid merge conflicts
# 这里我先push了一次，否则会出现fatal: refusing to merge unrelated histories
# 好像是git的bug
git subtree push --prefix=public <你的.git地址> gh-pages
git subtree pull --prefix=public <你的.git地址> gh-pages

hugo

# Add everything
git add -A

# Commit and push to master
git commit -m "Updating site" && git push origin master

# Push the public subtree to the gh-pages branch
git subtree push --prefix=public <你的.git地址> gh-pages
```

之后每次更新需要

~~~
hugo
git add -A
git commit -m "some messages"
git push origin master
git subtree push --prefix=public <你的.git地址> gh-pages
~~~

像我这样的强迫症可以把主题也做一个`subtree`出来。