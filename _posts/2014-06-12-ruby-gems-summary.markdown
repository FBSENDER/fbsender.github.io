---
layout: post
title: "ruby gem 使用总结"
date: 2014-06-12 18:12
categories: ruby
---

gem为ruby便捷开发包
gem install ..  好多好多可选的命令，安装gem至本地
gem environment 安装rvm后 gem执行环境发生改变，rvm改变ruby版本，同时会变更gem执行环境

gem path 有两个
执行 gem install xxx --user-install 会安装在用户家目录的gem path
执行 gem uninstall xxx --user-install 从用户家目录gem path 卸载
无文档安装 gem install xxx --no-doc --no-ri

xxx.gemspec 这个文件记录了gem相关信息 

rspec require 执行路径，会将所在文件夹的lib目录加入$LOAD_PATH，以方便测试

当前编写gem的一般规范

gem 管理工具

gem无法安装的解决办法

gem source 的变更

自己搭建gem镜像

push gem 至 rubygems.org供所有人下载

git clone from github 本地build&install

Tip: Passing -r to irb will automatically require a library when irb is loaded.
Once you’ve required ap, RubyGems automatically places itslib directory on the $LOAD_PATH.
这样就可以验证load是哪一个gem path下的gem

gem path 为两条记录，require时的逻辑是怎样的呢？
require 'xxx'
  
从两个路径中选一个版本号最高的gem加入$LOAD_PATH
若版本号相同，优先使用家目录下的gem加入$LOAD_PATH
查看$LOAD_PATH方法：
  
puts $LOAD_PATH
puts $: 


