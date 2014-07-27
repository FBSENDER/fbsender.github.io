---
layout: post
title: "ruby 开发环境配置 ubuntu"
categories: ruby
date: 2014-07-27 17:00
---
## 使用rvm安装ruby
1. 安装rvm     
```
curl -L https://get.rvm.io | bash -s stable      
```
2. 在家目录.bashrc下面加入这样一段代码，打开新的terminal就不用再次键入相同的指令了    
```
source ~/.rvm/scripts/rvm
```     
3. 检查是否安装正确，先重新打开一个terminal，输入irb，irb可用说明rvm安装成功了
4. 展示可以安装的ruby版本    
```
rvm list known
```
5. 安装一个指定的ruby版本    
```
rvm install 2.1.1
```
6. 验证ruby是否安装成功    
```
ruby -v
```     
```
ruby 2.1.1p76 (2014-02-24 revision 45161) [i686-linux]
```
