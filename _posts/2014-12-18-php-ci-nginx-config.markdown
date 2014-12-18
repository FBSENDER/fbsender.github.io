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

##这样配置的原理

首先要了解nginx try_files是什么作用，详情可以看这个页面 http://stackoverflow.com/questions/17798457/how-can-i-make-this-try-files-directive-work。不禁又要感叹一下，资料还是要看英文的，万能的stackoverflow。

简单来讲，try_files的作用域是server下的location，按照后面给定参数去依次访问，可达就返回结果，否则就报404，当然还可以指定其他的http代码。

我们指定了参数 index.php，那么每一个request就会去尝试访问index.php了。.php文件符合下面一条location规则。

那么有了新的问题，路径参数怎么办呢？下面的配置解决了该问题。

```
$config['uri_protocol'] = 'REQUEST_URI';
```

于是，预期的路由生效了，当然访问host:port/index.php/something 也是可以的，seo在想别的办法解决
