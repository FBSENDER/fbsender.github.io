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

没有发现gem server 进程，确认服务没起。

```shell
gem server --help #查看gem server帮助信息
```

gem server 命令，默认选项为 --port 8808 --dir /usr/local/lib/ruby/gems/2.2.0 --no-daemon

端口路径都没有问题，我需要gem server在后台以守护进程的形式运行

```shell
nohup gem server &
```

需求达成，且当前shell退出后gem server 守护进程不会随之退出。

新的问题来了，如何设置gem server 指令随机器启动而自动启动？

