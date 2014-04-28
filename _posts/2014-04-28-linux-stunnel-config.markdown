---
layout: post
title:  "linux stunnel config"
date:   2014-04-28
categories: linux
---

stunnel需要在服务器端以及客户端同时安装：
 
服务器端安装
yum install stunnel
apt-get stunnel
配置stunnel其实很简单，多看看官方的文档：http://www.stunnel.org/static/stunnel.html
服务端的配置只需要两个文件（这两个文件都放在/etc/stunnel文件夹下）：
1）stunnel.pem
问网页http://www.stunnel.org/pem填写相关信息后，点击生成，在服务器上创建文件/etc/stunnel/stunnel.pem，并将生成的内容复制到pem文件中，pem文件内容如下
-----BEGIN RSA PRIVATE KEY-----
MIICXgIBAAKBgQCxG1ULJuAXegar1Lin1ydNU+heLptmF2nlxF1j2pKFs7mYHVWN
61xh10N9oxYJibzP7iud5biMB6mrdtMU+D3xhaxXeBdCWkHDbpBOiOJSUL77kb9o
0rt7hus+vPiCkYGSMXWilq9pZBSeG6v+vNbLS1staJlcN54lbYpLna+RDwIDAQAB
AoGBAItpPHRe0Z8pSv8Pn5te3W0dU5hvj5u5an6XJ/xmHVhptPpsfOAOGNZboKDR
M5OmfJ4gmOzd23s+vOxfyKCFGBwqKtfNj0/UK/TY7QK7PThqnaauRsPJHCu5VucQ
Vi0JeWb233MQq/+H8exniM1eIjPp9RdhZBrX4EBbEP1z6K6BAkEA5eh9SqBnWAQ3
qRTpTPnt1O1plttlWo2YXMIn1QzfTW/O6pB05OzO2alU3BLeblbSTe/O50Zeqctx
MJaoGcSmMQJBAMU0znHKeZHaaYG00mzgfnzH788x0ExAmfOzOH7FZpFP1ItxlbSf
h0lxtzU1+Rvny6wri883axBwEgLJKHabmz8CQQCfWprlS/L1hc7SqkTe7ujTSk+C
mcVRk41E1epn+IkakmHYIZJ0TlM9eOnxtD5qOlGAZbSChzr786Ab7oDLg4sxAkBy
fuBFjMrcdbTAC94IPKbzh5mh8EgBnZhEt49bevy77V93vnCut9hyOcWm7Tk+jGvi
AD5iBsjweEDcwTHu+xU9AkEAjCDhTFOS65Tc+oi5HJfphbxmPr8PhBZeHIipG/HQ
gb7cCQ6bYcoFU2TsDPuawrwBZKIux/4EeiZZdCobeLjD+w==
-----END RSA PRIVATE KEY-----
-----BEGIN CERTIFICATE-----
MIIClzCCAgCgAwIBAgIJAO+WhBuwPQgGMA0GCSqGSIb3DQEBBAUAMIGBMQswCQYD
VQQGEwJDTjEQMA4GA1UECBMHQmVpamluZzEQMA4GA1UEBxMHQmVpamluZzEWMBQG
A1UEChMNZGV2dGVtcHRhdGlvbjEWMBQGA1UECxMNZGV2dGVtcHRhdGlvbjEeMBwG
A1UEAxMVd3d3LmRldnRlbXB0YXRpb24uY29tMB4XDTEwMTIxNTA3MjY0N1oXDTEx
MTIxNTA3MjY0N1owgYExCzAJBgNVBAYTAkNOMRAwDgYDVQQIEwdCZWlqaW5nMRAw
DgYDVQQHEwdCZWlqaW5nMRYwFAYDVQQKEw1kZXZ0ZW1wdGF0aW9uMRYwFAYDVQQL
Ew1kZXZ0ZW1wdGF0aW9uMR4wHAYDVQQDExV3d3cuZGV2dGVtcHRhdGlvbi5jb20w
gZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBALEbVQsm4Bd6BqvUuKfXJ01T6F4u
m2YXaeXEXWPakoWzuZgdVY3rXGHXQ32jFgmJvM/uK53luIwHqat20xT4PfGFrFd4
F0JaQcNukE6I4lJQvvuRv2jSu3uG6z68+IKRgZIxdaKWr2lkFJ4bq/681stLWy1o
mVw3niVtikudr5EPAgMBAAGjFTATMBEGCWCGSAGG+EIBAQQEAwIGQDANBgkqhkiG
9w0BAQQFAAOBgQBxXFTXjpdhhUoUczIf+klqP0m7W0PS2sUgdg2tCvgSzeaJr5zh
+dSZchGgV0jU/k/dg7fKhePFWtnL/89JxfqlScmfVSVAO4kX9NNj78mpBlIPaYZu
OG7CmtT1PDwBFnbXjGht/zsrwrZNQoCwzgHovAjZhn+VwaslSVvvo2USzQ==
-----END CERTIFICATE-----
2）服务器端配置文件stunnel.conf
最少的配置如下：
; 一般配置为no，配置为yes可以看到stunnel运行情况以便调试
foreground = yes
; 这就是上面生成的文件路径了，服务器端必须要指定此配置
cert = /etc/stunnel/stunnel.pem
; 压缩方式，默认是不压缩，我觉得用zlib压缩一下会更好，数据量更少
compression = zlib
; 下面开始一个服务配置节，[]中的名称为服务名，可以自己随意命名
[squid]
;监听3129端口的请求
accept = 3129
;将3129的请求经过SSL加密后传给3128端口，也就是转发给squid服务
connect = 3128
 
修改/etc/default/stunnel4和/etc/init.d/stunnel4
将其中的ENABLED=0改为ENABLED=1
运行服务器端stunnel
服务方式
service stunnel4 restart
命令行方式
执行stunnel
服务器启动时自动运行：编辑/etc/rc.local，增加
# stunnel
/usr/bin/stunnel
 
修改服务器防火墙配置，stunnel监听端口号为3129，这时可从防火墙中去掉允许访问3128端口
iptables -I INPUT -p tcp --dport 3129 -j ACCEPT
保存防火墙配置
iptables-save > /etc/sysconfig/iptables
客户端安装
到http://www.stunnel.org/download/下载合适的安装包
客户端配置
编辑C:\Program Files\stunnel\stunnel.conf
client = yes // 去掉这行的注释
// 监听本地3129的请求，加密后转发给服务器上的stunnel服务监听端口
[proxy]
accept = 127.0.0.1:3129
connect = devtemptation.com:3129
 
启动客户端stunnel，运行C:\Program Files\stunnel\stunnel.exe
设置浏览器代理服务器
将代理服务器设置为localhost:3129端口，即本地stunnel服务监听的3129端口。
chrome 代理软件插件 Switchy!该插件可设置代理应用规则，比如
 
*.facebook.com/*
*.twitter.com/*
 
使用代理，其他网站不使用代理
然后启用Switchy!的自动切换代理模式
 
至此，大功告成～