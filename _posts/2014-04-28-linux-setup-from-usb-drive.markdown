---
layout: post
title:  "linux setup from usb drive"
date:   2014-04-28
categories: linux
---

制作U盘为启动盘的软件ultralso

bios设置从USB启动，引导方式FDD HDD ZIP等，制作启动盘时可以尝试不同的写入方式

Y510对于HDD引导方式不支持，设置为ZIP+后引导成功，但是安装fedora还是出错，于是安装了ubuntu

不同型号，容量不同的U盘对于各种bios的兼容性也不同

不同种厂家主板的bios支持的usb启动引导方式也不尽相同

大体来讲新机器怎么着都OK，相对旧的机器就需要通过尝试来制作能够使用的启动盘