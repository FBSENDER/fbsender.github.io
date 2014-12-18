---
layout: post
title:  "php CodeIgniter nginx config - php CI nginx route config"
date:   2014-12-18
categories: php
---

学习使用PHP framework CodeIgniter (CI)，web server 使用 nginx，初始一个application后，路由无法正常工作，悲剧的很...

在这里记录一下新手可能会遇到的坑。

##nginx转发配置错误

CI对uri进行美化，uri是不带.php后缀的。

上一篇blog中nginx location节点是这样配置的：

```
location ~ \.php$ {
  fastcgi_pass unix:/var/run/php5-fpm.sock; #这里指定了fastcgi进程侦听的端口,nginx就是通过这里与php交互的
  include fastcgi_params;
}
```

于是，类似于 http://localhost/welcome 的url都汇报 404 error，而且这个是nginx报出的错误，原因就是web request 并没有发送给php cgi。

##正确的配置

CI的 web request 需要通过访问root根目录下的index.php来进行处理，所以url应该像这样http://host/index.php/xxxx，要带着index.php。

但是这样不好看，要想办法把index.php给隐去。

nginx配置：

```
  server{
    listen 8080;
    server_name localhost;
    root  /home/current_user/php_test;          #网站的根目录
    index index.php;
    location / {
      #check if a file or directory index file exists, else route it to index.php.
      try_files $uri $uri/ /index.php;
    }

    location ~* \.php$ {
      fastcgi_pass unix:/var/run/php5-fpm.sock;
      include fastcgi_params;
    }
  }

```

application/config.php 修改：

```
$config['index_page'] = ''; #默认值为 index.php
$config['uri_protocol'] = 'REQUEST_URI'; #默认值为 AUTO
```

这样配置好之后，nginx需要重启一下，然后php route就生效了。
