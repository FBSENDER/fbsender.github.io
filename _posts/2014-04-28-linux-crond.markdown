---
layout: post
title:  "linux crond"
date:   2014-04-28
categories: linux
---

linux crond服务简介：
定时执行系统命令


查看crond服务状态：
[root@www ~]# /sbin/service crond status

启动、停止、重启crond服务：
[root@www ~]# /sbin/service crond start
[root@www ~]# /sbin/service crond stop
[root@www ~]# /sbin/service crond restart

系统启动自动启动crond服务，在/etc/rc.d/rc.local文件中加一行：
/sbin/service crond start


crontab 格式：
进入vi编辑模式，编辑的内容一定要符合下面的格式：*/1 * * * * ls >> /tmp/ls.txt 

这个格 式的前一部分是对时间的设定，后面一部分是要执行的命令，如果要执行的命令太多，可以把这些命令写到一个脚本里面，然后在这里直接调用这个脚本就可以了， 调用的时候记得写出命令的完整路径。时间的设定我们有一定的约定，前面五个*号代表五个数字，数字的取值范围和含义如下： 

分钟　（0-59） 
小時　（0-23） 
日期　（1-31） 
月份　（1-12） 
星期　（0-6）//0代表星期天 

除了数字还有几个个特殊的符号就是"*"、"/"和"-"、","，*代表所有的取值范围内的数字，"/"代表每的意思,"*/5"表示每5个单位，"-"代表从某个数字到某个数字,","分开几个离散的数字。以下举几个例子说明问题： 

每天早上6点 

0 6 * * * echo "Good morning." >> /tmp/test.txt //注意单纯echo，从屏幕上看不到任何输出，因为cron把任何输出都email到root的信箱了。 

每两个小时 

0 */2 * * * echo "Have a break now." >> /tmp/test.txt 

晚上11点到早上8点之间每两个小时，早上八点 

0 23-7/2，8 * * * echo "Have a good dream：）" >> /tmp/test.txt 

每个月的4号和每个礼拜的礼拜一到礼拜三的早上11点 

0 11 4 * 1-3 command line 

1月1日早上4点 

0 4 1 1 * command line 

每 次编辑完某个用户的cron设置后，cron自动在/var/spool/cron下生成一个与此用户同名的文件，此用户的cron信息都记录在这个文件 中，这个文件是不可以直接编辑的，只可以用crontab -e 来编辑。cron启动后每过一份钟读一次这个文件，检查是否要执行里面的命令。因此此文 件修改后不需要重新启动cron服务。