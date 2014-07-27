---
layout: post
title: "rails javascript runtime"
category: ruby
date: 2014-7-27 21:25
---
## 方法1：更改Gemfile
在Gemfile中加入下面两行，然后bundle install：     
```
gem 'therubyracer'   
gem 'execjs'
```
## 方法2：安装nodejs
```
sudo apt-get install nodejs
```
