---
layout: post
title:  "linux mono with apache"
date:   2014-04-28
categories: linux
---

因为apt-get软件源不包含mono源，所以直接apt-get获取不到mono-xsp4包，首先需要添加源：
参考：http://badgerports.org/lucid.html
先执行：apt-key adv --keyserver keyserver.ubuntu.com --recv 0E1FAD0C，添加源的key
然后编辑文件/etc/apt/sources.list，在其中添加一行“deb http://badgerports.org lucid main“，就是网址中提到的软件源地址，最后需要执行apt-get update刷新缓存中的数据，

安装apache：
apt-get install apache2
启动：
service apache2 start/stop/restart
 
安装mono-xsp4
因为apt-get软件源不包含mono源，所以直接apt-get获取不到mono-xsp4包，首先需要添加源：
参考：http://badgerports.org/lucid.html
先执行：apt-key adv --keyserver keyserver.ubuntu.com --recv 0E1FAD0C，添加源的key
然后编辑文件/etc/apt/sources.list，在其中添加一行“deb http://badgerports.org lucid main“，就是网址中提到的软件源地址，最后需要执行apt-get update刷新缓存中的数据，之后就可以安装apt-get install mono-xsp4了。
安装mono-apache-server4
apt-get install mono-apache-server4，同时会安装很多个包
然后安装libapache2-mod-mono了，将/etc/apache2/mods-available/mod_mono.conf和.load复制到/etc/apache2/mods-enabled下并重启apache2，apache会自动加载mods-enableds下的mod。
 
配置mod_mono.conf
有两种方式部署asp.net站点：
1）自动部署方式
每个应用以虚拟目录的方式访问，只需要将站点程序放到/var/www/（apache中可以配置）文件夹下即可。你可以把刚才提到的Asp.net 2.0 Demo复制到/var/www目录下，直接使用“http://localhost/{/var/www/下某个文件夹的名称}”就可以访问你的asp.net网站了
使用这种方式需要在这个文件中增加配置：
MonoAutoApplication enabled，启用自动部署功能；
ForceType application/x-asp-net，所有请求都是asp.net请求（mvc没有扩展名，所以需要asp.net处理所有请求）
默认情况下已经使用了Framework 4，见其中的“Include /etc/mono-server4/mono-server4-hosts.conf”文件：
# Default configuration, don't edit it!
<IfModule mod_mono.c>
  MonoUnixSocket default /tmp/.mod_mono_server4
  MonoServerPath default /usr/bin/mod-mono-server4
  AddType application/x-asp-net .aspx .ashx .asmx .ascx .asax .config .ascx
  MonoApplicationsConfigDir default /etc/mono-server4
  MonoPath default /usr/lib/mono/4.0:/usr/lib
</IfModule>
修改后重启apache方可生效。
2）自定义web站点方式
每个应用一个端口，使用http://go-mono.com/config-mod-mono/Default.aspx工具生成配置文件会简单很多。在apache的sites-enabled下面，将工具生成的文件放进来即可。要注意调整端口号，asp.net mvc程序使用增加ForceType application/x-asp-net配置。
mods-enabled下面的mod_mono.conf中配置了asp.net的版本。
 
apache日志：/var/log/apache2
 
一个超好用的上传文件工具：http://www.bitvise.com/ssh-client-download