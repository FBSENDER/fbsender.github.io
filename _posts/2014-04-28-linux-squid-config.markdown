---
layout: post
title:  "linux squid config"
date:   2014-04-28
categories: linux
---

1、什么是Squid
Squid是一个高性能的代理缓存服务器，Squid支持FTP、gopher和HTTP协议。和一般的代理缓存软件不同，Squid用一个单独的、非模块化的、I/O驱动的进程来处理所有的客户端请求。

Squid将数据元缓存在内存中，同时也缓存DNS查询的结果，除此之外，它还支持非模块化的DNS查询，对失败的请求进行消极缓存。Squid支持SSL，支持访问控制。由于使用了ICP（轻量Internet缓存协议），Squid能够实现层叠的代理阵列，从而最大限度地节约带宽。

Squid由一个主要的服务程序squid,一个DNS查询程序dnsserver，几个重写请求和执行认证的程序，以及几个管理工具组成。当Squid启动以后，它可以派生出预先指定数目的dnsserver进程，而每一个dnsserver进程都可以执行单独的DNS查询，这样一来就大大减少了服务器等待DNS查询的时间。

2、Squid的下载安装

从Squid的官方站点http://www.squid-cache.org下载该软件；

1) 将该文件拷贝到/usr/local目录。

2) 解开该文件 tar xvzf squid-2.5.STABLE12-20051114.tar.gz

3) 解开后，在/usr/local生成一个新的目录squid-2.5.STABLE12-20051114

4) 进入squid
cd squid-2.5.STABLE12-20051114

5) 执行./configure 可以用./confgure --prefix=/directory/you/want指定安装目录。系统缺省安装目录为/usr/local/squid。

配置命令configure有很多选项，如果不清楚可先用“-help”查看。通常情况下，用到的选项有以下几个： 

--prefix=/web/squid #指定Squid的安装位置，如果只指定这一选项，那么该目录下会有bin、sbin、man、conf等目录，而主要的配置文件此时在conf子目录中。为便于管理，最好用参数--sysconfdir=/etc把这个文件位置配置为/etc。
--enable-storeio=ufs,null #使用的文件系统通常是默认的ufs，不过如果想要做一个不缓存任何文件的代理服务器，就需要加上null文件系统。
--enable-arp-acl #这样可以在规则设置中直接通过客户端的MAC地址进行管理，防止客户使用IP欺骗。
--enable-err-languages="Simplify_Chinese" --enable-default-err-languages="Simplify_Chinese" #上面两个选项告诉Squid编入并使用简体中文错误信息。
--enable-linux-netfilter #允许使用Linux的透明代理功能。
--enable-underscore #允许解析的URL中出现下划线，因为默认情况下Squid会认为带下划线的URL是非法的，并拒绝访问该地址。 

　　整个配置编译过程如下： 

./configure --prefix=/var/squid --sysconfdir=/etc --enable-arp-acl --enable-linux-netfilter --enable-pthreads --enable-err-language="Simplify_Chinese" --enable-storeio=ufs,null --enable-default-err-language="Simplify_Chinese" --enable-auth="basic" --enable-baisc-auth-helpers="NCSA" --enable-underscore 

　　其中一些选项有特殊作用，将在下面介绍它们。 

　　最后执行make和make install两条命令，将源代码编译为可执行文件，并拷贝到指定位置。 

6) 执行 make all

7) 执行 make install

8) 安装结束后，squid的可执行文件在安装目录的bin子目录下，配置文件在etc子目录下。

3、Squid的配置

Squid配置文件为：/usr/local/squid/etc/squid.conf。安装成功以后，系统已经有了一个缺省的配置文件，用户仅仅需要修改该配置文件即可。首先我将Squid用在透明代理时的配置文件中必须打开的选项的内容列举如下：

http_port 8080
cache_mem 32 MB
cache_swap_low 90
cache_swap_high 95
maximum_object_size 4096 KB
cache_dir ufs /usr/local/squid/cache 1200 16 256
cache_access_log /usr/local/squid/logs/access.log
cache_log /usr/local/squid/logs/cache.log
dns_nameservers 210.12.114.130
unlinkd_program /usr/local/squid/bin/unlinkd
acl all src 0.0.0.0/0.0.0.0
http_access allow all
cache_effective_user nobody
cache_effective_group nobody
httpd_accel_host virtual
httpd_accel_port 80
httpd_accel_with_proxy on
httpd_accel_uses_host_header on 

* http_port 

说明：定义squid监听HTTP客户连接请求的端口。缺省是3128，如果使用HTTPD加速模式 则为80。你可以指定多个端口，但是所有指定的端口都必须在一条命令行上。

*cache_mem (bytes)

说明：该选项用于指定squid可以使用的内存的理想值。这部分内存被用来存储以下对象：In-Transit objects （传入的对象）
Hot Objects （热对象，即用户常访问的对象）
Negative-Cached objects （消极存储的对象）
需要注意的是，这并没有指明squid所使用的内存一定不能超过该值，其实，该选项只定义了squid所使用的内存的一个方面，squid还在其他方面使用内存。所以squid实际使用的内存可能超过该值。缺省值为8MB。

*cache_dir Directory-Name Mbytes Level-1 Level2

说明：指定squid用来存储对象的交换空间的大小及其目录结构。可以用多个cache_dir命令来定义多个这样的交换空间，并且这些交换空间可以分布不同的磁盘分区。"directory "指明了该交换空间的顶级目录。如果你想用整个磁盘来作为交换空间，那么你可以将该目录作为装载点将整个磁盘mount上去。缺省值为/var/spool/squid。“Mbytes”定义了可用的空间总量。需要注意的是，squid进程必须拥有对该目录的读写权力。“Level-1”是可以在该顶级目录下建立的第一级子目录的数目，缺省值为16。同理，“Level-2”是可以建立的第二级子目录的数目，缺省值为256。为什么要定义这么多子目录呢？这是因为如果子目录太少，则存储在一个子目录下的文件数目将大大增加，这也会导致系统寻找某一个文件的时间大大增加，从而使系统的整体性能急剧降低。所以，为了减少每个目录下的文件数量，我们必须增加所使用的目录的数量。如果仅仅使用一级子目录则顶级目录下的子目录数目太大了，所以我们使用两级子目录结构。

那么，怎么来确定你的系统所需要的子目录数目呢？我们可以用下面的公式来估算。

已知量：

DS = 可用交换空间总量（单位KB）/ 交换空间数目

OS = 平均每个对象的大小= 20k

NO = 平均每个二级子目录所存储的对象数目 = 256

未知量：

L1 = 一级子目录的数量

L2 = 二级子目录的数量

计算公式：

L1 x L2 = DS / OS / NO

注意这是个不定方程，可以有多个解。



* cache_swap_low (percent, 0-100)
  cache_swap_high (percent, 0-100)

说明：squid使用大量的交换空间来存储对象。那么，过了一定的时间以后，该交换空间就会用完，所以还必须定期的按照某种指标来将低于某个水平线的对象清除。squid使用所谓的“最近最少使用算法”（LRU）来做这一工作。当已使用的交换空间达到cache_swap_high时，squid就根据LRU所计算的得到每个对象的值将低于某个水平线的对象清除。这种清除工作一直进行直到已用空间达到cache_swap_low。这两个值用百分比表示，如果你所使用的交换空间很大的话，建议你减少这两个值得差距，因为这时一个百分点就可能是几百兆空间，这势必影响squid的性能。缺省

cache_swap_low 90
cache_swap_high 95

* maximum_object_size 

说明：大于该值得对象将不被存储。如果你想要提高访问速度，就请降低该值；如果你想最大限度地节约带宽，降低成本，请增加该值。单位为K，缺省值为：

maximum_object_size 4096 KB

* cache_dir Directory-Name Mbytes Level-1 Level2

说明：指定squid用来存储对象的交换空间的大小及其目录结构。可以用多个cache_dir命令来定义多个这样的交换空间，并且这些交换空间可以分布不同的磁盘分区。"directory "指明了该交换空间的顶级目录。如果你想用整个磁盘来作为交换空间，那么你可以将该目录作为装载点将整个磁盘mount上去。缺省值为/var/spool/squid。“Mbytes”定义了可用的空间总量。需要注意的是，squid进程必须拥有对该目录的读写权力。“Level-1”是可以在该顶级目录下建立的第一级子目录的数目，缺省值为16。同理，“Level-2”是可以建立的第二级子目录的数目，缺省值为256。为什么要定义这么多子目录呢？这是因为如果子目录太少，则存储在一个子目录下的文件数目将大大增加，这也会导致系统寻找某一个文件的时间大大增加，从而使系统的整体性能急剧降低。所以，为了减少每个目录下的文件数量，我们必须增加所使用的目录的数量。如果仅仅使用一级子目录则顶级目录下的子目录数目太大了，所以我们使用两级子目录结构。

那么，怎么来确定你的系统所需要的子目录数目呢？我们可以用下面的公式来估算。

已知量：

DS = 可用交换空间总量（单位KB）/ 交换空间数目

OS = 平均每个对象的大小= 20k

NO = 平均每个二级子目录所存储的对象数目 = 256

未知量：

L1 = 一级子目录的数量

L2 = 二级子目录的数量

计算公式：

L1 x L2 = DS / OS / NO

注意这是个不定方程，可以有多个解。

* cache_access_log

说明：指定客户请求记录日志的完整路径（包括文件的名称及所在的目录），该请求可以是来自一般用户的HTTP请求或来自邻居的ICP请求。缺省值为：

cache_access_log /var/log/squid/access.log

如果你不需要该日志，可以用以下语句取消：

cache_access_log none

* cache_log

说明：指定squid一般信息日志的完整路径（包括文件的名称及所在的目录）。缺省路径为：

cache_log /var/log/squid/cache.log

* dns_nameservers 100.100.100.101

该选项用来定义Squid进行域名解析时使用的域名服务器的，因为在使用代理协议时，客户端并不进行域名查询，而是通过代理进行的，因此需要为代理服务器指定域名服务器来进行域名解析。

* unlinkd_program

说明：指定文件删除进程的完整路径。

缺省设置为：unlinkd_program /usr/lib/squid/unlinkd

* acl

说明：定义访问控制列表。

定义语法为：
acl aclname acltype string1 ...
acl aclname acltype "file" ...
当使用文件时，该文件的格式为每行包含一个条目。

acltype 可以是 src dst srcdomain dstdomain url_pattern urlpath_pattern time port proto method browser user 中的一种。

分别说明如下：

src 指明源地址。可以用以下的方法指定：

acl aclname src ip-address/netmask ... (客户ip地址)
acl aclname src addr1-addr2/netmask ... (地址范围)

dst 指明目标地址。语法为：

acl aclname dst ip-address/netmask ...(即客户请求的服务器的ip地址)

srcdomain 指明客户所属的域。语法为：

acl aclname srcdomain foo.com ... squid将根据客户ip反向查询DNS。

dstdomain 指明请求服务器所属的域。语法为：

acl aclname dstdomain foo.com ... 由客户请求的URL决定。 

注意，如果用户使用服务器ip而非完整的域名时，squid将进行反向的DNS解析来确定其完整域名，如果失败就记录为“none”。

time 指明访问时间。语法如下： 

acl aclname time [day-abbrevs] [h1:m1-h2:m2][hh:mm-hh:mm]

day-abbrevs:

S - Sunday
M - Monday
T - Tuesday
W - Wednesday
H - Thursday
F - Friday
A - Saturday

h1:m1 必须小于 h2:m2，表达示为[hh:mm-hh:mm]。

port 指定访问端口。可以指定多个端口，比如：

acl aclname port 80 70 21 ...
acl aclname port 0-1024 ... （指定一个端口范围）

proto 指定使用协议。可以指定多个协议： 

acl aclname proto HTTP FTP ...

method 指定请求方法。比如：

acl aclname method GET POST ...

这里定义了一个名为all的组，包括所有的主机。

*http_access

说明：根据访问控制列表允许或禁止某一类用户访问。

如果某个访问没有相符合的项目,则缺省为应用最后一条项目的“非”。比如最后一条为允许，则缺省就是禁止。所以，通常应该把最后的条目设为"deny all" 或 "allow all" 来避免安全性隐患。

这里我们允许所有的地址访问代理服务，但是在下面我们使用Ipchains来限制只允许拨号用户来访问该透明代理服务器。

* cache_effective_user

cache_effective_group

说明：如果用root启动squid，squid将变成这两条语句指定的用户和用户组。缺省变为squid用户和squid用户组。注意这里指定的用户和用户组必须真是存在于/etc/passwd中。如果用非root帐号启动squid，则squid将保持改用户及用户组运行，这时候，你不能指定小于1024地http_port。

cache_effective_user nobody

cache_effective_group nobody

*httpd_accel_host virtual

httpd_accel_port 80

这两个选项本来是用来定义squid加速模式的。在这里我们用virtual来指定为虚拟主机模式。80端口为要加速的请求端口。采用这种模式时，squid就取消了缓存及ICP功能，假如你需要这些功能，这必须设置httpd_accel_with_proxy选项。

* httpd_accel_with_proxy on

该选项在透明代理模式下是必须设置成on的。在该模式下，squid既是web请求的加速器，又是缓存代理服务器。

* httpd_accel_uses_host_header on

在透明代理模式下，如果你想让你代理服务器的缓存功能正确工作的话，你必须将该选项设为on。设为on时，squid会把存储的对象加上主机名而不是ip地址作为索引。这一点在你想建立代理服务器阵列时显得尤为重要。

4、Squid的启动

首先确定你的内核已经配置了以下特性：
Network firewalls

[ ] Socket Filtering
Unix domain sockets
TCP/IP networking

[ ] IP: multicasting

[ ] IP: advanced router

[ ] IP: kernel level autoconfiguration
IP: firewalling

[ ] IP: firewall packet netlink device
IP: always defragment (required for masquerading)
IP: transparent proxy support

如果没有，请你重新编译内核。一般在RedHat6.x以上，系统已经缺省配置了这些特性。

下来，需要为Squid创建 Cache目录，使用如下命令来创建：

[root@squid bin]# /usr/local/squid/sbin/squid –z

下来需要指定Log目录为nobody用户具有写权限：

chown nobody:nobody  /usr/local/squid/var/logs



最后启动squid：

  [root@iptable logs]# /usr/local/squid/bin/RunCache &


查看进程列表： 

  [root@iptable logs]# ps ax
　　应该出现如下几个进程：

  1372 pts/0 S 0:00 /bin/sh /usr/local/squid/bin/RunCache
  1375 pts/0 S 0:00 squid -NsY
  1376 ? S 0:00 (unlinkd)
　　并且系统中应该有如下几个端口被监听：

  [root@proxy logs]# netstat -ln
  tcp 0 0 0.0.0.0:3128 0.0.0.0:* LISTEN
  udp 0 0 0.0.0.0:3130 0.0.0.0:*
　　这些说明squid已经正常启动了。

然后/etc/rc.d/rc.local文件最后添加 /usr/local/squid/bin/RunCache & 以使得系统启动时自动启动squid服务器。

首先应该打开包转发功能：

echo 1 > /proc/sys/net/ipv4/ip_forward

为了让启动时能自动打开包转发功能，可以将上一行的内容添加到/etc/rc.d/rc.local文件的末尾。

/sbin/iptables -F -t nat 
iptables -t nat -A PREROUTING -i eth0 -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 3128 #将所有80端口的包转发到3128端口 
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE #对eth0端口进行欺骗
