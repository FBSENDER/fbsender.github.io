---
layout: post
title: "git push 403 error 解决方法"
date: 2014-06-12 16:04:00
categories: git
---

git push origin master 报错如下：
error: The requested URL returned error: 403 Forbidden while accessing https://github.com/FBSENDER/fbsender.github.io.git/info/refs

fatal: HTTP request failed

解决方式：
git clone 时 url 使用的是: https://github.com/FBSENDER/fbsender.github.io.git
于是git remote origin 默认值为: https://github.com/FBSENDER/fbsender.github.io.git

将远端仓库url设置为ssh协议，即：
git remote set-url origin git@github.com:FBSENDER/fbsender.github.io.git

问题得到解决
