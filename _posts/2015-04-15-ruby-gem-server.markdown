---
layout: post
title:  "搭建自己的 ruby gem server"
date:   2015-04-15
categories: ruby
---

## gem server 端口不通

rubygem guide 原文链接：http://guides.rubygems.org/run-your-own-gem-server/

组内一台虚拟机上运行着gem server，里面有一些私有gem。

昨天物理机重启，导致虚拟机down机半天，今天发现该机器gem server端口（默认的8808）不通，gem source无法使用，于是有了这篇帖子。

## run your own gem server

之前该虚拟机不在我的管理下，问题要逐步排查。机器重启，而后gem server端口不通，很有可能gem server 服务没有在自动启动项内，服务没起来，于是相应的端口不通。

```shell
ps aux | grep gem
```

没有发现gem server 进程，服务没起。

