---
layout: post
title:  "linux 用户 用户组 管理"
date:   2019-09-14
categories: linux
---

##ubuntu 设置root密码
系统环境ubuntu13.04，root用户默认是没有密码的，一般情况下使用sudo执行一些需要root权限的指令，如果要使用root账户，可以按下面的方式操作：     

```bash
sudo passwd root
#输入root的新密码及确认
#设置好了root的密码
su
#输入刚设置的密码，进入root用户
```
