---
layout: post
title:  "php nginx config"
date:   2014-12-16
categories: php
---

学习一下php，这么火一定是有理由的，先搭个环境，部署个Hello world.

系统ubuntu 13.10

##使用apt 安装必要软件
```
sudo apt-get install php5 php5-fpm

sudo apt-get install nginx
```

php5安装后会自动运行apache2，需要把apache2相关进程kill掉

php5-fpm安装后会自动运行，/var/run/php5-fpm.sock

##ngnix fastcgi_params config

文件路径 /etc/nginx/fastcgi_params

```
#改动的地方 SCRIPT_NAME PATH_INFO

fastcgi_param QUERY_STRING $query_string;
fastcgi_param REQUEST_METHOD $request_method;
fastcgi_param CONTENT_TYPE $content_type;
fastcgi_param CONTENT_LENGTH $content_length;

#fastcgi_param SCRIPT_FILENAME $request_filename;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
# add PATH_INFO
fastcgi_param PATH_INFO $fastcgi_path_info;
fastcgi_param SCRIPT_NAME $fastcgi_script_name;
fastcgi_param REQUEST_URI $request_uri;
fastcgi_param DOCUMENT_URI $document_uri;
fastcgi_param DOCUMENT_ROOT $document_root;
fastcgi_param SERVER_PROTOCOL $server_protocol;

fastcgi_param GATEWAY_INTERFACE CGI/1.1;
fastcgi_param SERVER_SOFTWARE nginx/$nginx_version;

fastcgi_param REMOTE_ADDR $remote_addr;
fastcgi_param REMOTE_PORT $remote_port;
fastcgi_param SERVER_ADDR $server_addr;
fastcgi_param SERVER_PORT $server_port;
fastcgi_param SERVER_NAME $server_name;

fastcgi_param HTTPS $https;

# PHP only, required if PHP was built with --enable-force-cgi-redirect
fastcgi_param REDIRECT_STATUS 200;
```

##nginx配置

```
user www-data;
worker_processes 1;
pid /run/nginx.pid;
events {
  worker_connections 768;
# multi_accept on;
}
http {
  sendfile on;
  tcp_nopush on;
  tcp_nodelay on;
  keepalive_timeout 65;
  types_hash_max_size 2048;
  include /etc/nginx/mime.types;
  default_type application/octet-stream;
  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;
  gzip on; 
  gzip_disable "msie6";
  server{
    listen 8080;
    server_name localhost;
    root /home/php_test; #这是你网站的根目录
    index index.php;
    location ~ \.php$ {
      fastcgi_pass unix:/var/run/php5-fpm.sock; #这里指定了fastcgi进程侦听的端口,nginx就是通过这里与php交互的
      include fastcgi_params;
    }
  }
}
```
