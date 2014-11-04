---
layout: post
title:  "nginx virtual server config"
date:   2014-11-4
categories: linux
---

##nginx重定向

现有两个host，www.vxixi.com与vxixi.com，两个主机下站点内容一样，又都有baidu索引，考虑到seo，仅保留www.vxixi.com，为不丢流量，将
vxixi.com重定向到www.vxixi.com。

最初的配置使用了nginx rewrite，但是要注意，nginx rewrite返回的http code 为302（临时跳转），配置方法如下：

```ruby
server {
        server_name  www.vxixi.com;
          if ($host != 'www.vxixi.com' ) {
          rewrite ^/(.*)$ http://www.vxixi.com/$1 permanent;
        }
        ...
}
```

实际上应该配置为301（永久跳转），配置方法如下：

```ruby
server {
        server_name www.vxixi.com;
        ...
}
server {
        server_name vxixi.com;
        return 301 http://www.vxixi.com$request_uri;
}
```

##nginx location

目前站点css均使用bootstrap，自适应设备屏幕尺寸的，但是很多页面只在baidu pc端建立了索引，移动端没有收录，并且近期
m.vxixi.com收到了百度移动站点sitemap测试邀请，故想把站点air.vxixi.com映射到m.vxixi.com/air/下，这样我就可以提交
新站的sitemap以增加移动端的索引了。需想办法通过nginx的配置实现这一需求。

location节点下可以设置proxy_pass（http请求代理）和root or alias（静态文件路径），举例说明下：

```ruby
server_name m.vxixi.com;
location /air/{
  proxy_pass http://air.vxixi.com/;
}
#这样在访问 http://m.vxixi.com/air/时返回的数据流为 air.vxixi.com，uri也一并代理了    
#但是存在问题，air.vxixi.com页面一些默认host的url被映射到了m.vxixi.com而非m.vxixi.com/air/，都变成404了...
```

继续配置location节点可以部分解决404问题，比如air.vxixi.com/uploads/...

```ruby
location /uploads/{
  root /airvxixicom_root/
}
#或者
location /uploads/{
  alias /airvxixicom_root/uploads/
}
```

这样会产生新问题，如果m.vxixi.com下有一个controllor或者目录为uploads，就冲突了，实际情况是assets/目录，这样配置就冲突了....

更好的解决方法还没想到

##nginx location rewrite

nginx rewrite可以应用在server层，也可以应用在location层，代码示例：

```ruby
server_name m.vxixi.com
location /air/{
  rewrite ^/air/(.*)$ http://air.vxixi.com/$1
}
```

像这样是吧m.vxixi.com/air/*重定向（302）到air.vxixi.com/*

表示临时跳转，与当前需求并不符合。

