---
layout: post
title:  "linux mysql config"
date:   2014-04-28
categories: linux
---

记录配置过程
保留表创建脚本
1.安装 ubuntu下
apt-get install mysql-server
apt-get install mysql-client
验证安装是否成功：
服务器端验证：netstat -nat
端口3306为MySql
客户端验证：
登陆Mysql mysql -p 然后输入密码

登录语法：
mysql -u [username] -h[host] -p[password] dbname

MySql几个重要的目录：
数据库目录：/var/lib/mysql
配置文件目录：/usr/share/mysql
相关名利目录：/usr/bin
启动脚本目录：/etc/rc.d/init.d

修改登录密码
usr/bin/mysqladmin -u root -p password 
格式： mysqladmin -u 用户名 -p password 

启动与停止
启动：/etc/init.d/mysqld start （不成功。。。）
service mysql start 成功
停止：/usr/bin/mysqladmin -u root -p shutdown（不成功）
service mysql stop 成功
自动启动：待研究

查看Mysql运行状态：
1.mysqladmin -p status
2.在mysql命令行下 show status

自增列
1.关键字 ：auto_increment
2.自增用法 
例: 
CREATE TABLE animals ( id mediumint not null auto_increment, 
name char(30) not null, 
primary key (id));














