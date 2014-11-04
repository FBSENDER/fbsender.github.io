---
layout: post
title:  "postgresql 安装 配置 常用命令"
date:   2014-09-07
categories: linux
---

## postgresql install
```bash
sudo apt-get install postgresql
```
安装完成...    
因为在哥们的ubuntu上这个指令直接就安装成功了，故没有在尝试其他的安装方式...    
如果是非ubuntu环境或安装失败，再搜一下别的帖子吧~    
## 命令行第一登陆postgresql
postgresql 默认的一个role为 ‘postgres’，第一次从命令行登陆可以用下面的指令：    
```bash
sudo -u postgres psql
```
然后就可以执行一些简单的查询指令了：      
```sql
select 1 as row1
```
查询server中的数据库信息：    
```sql
select * from pg_database;
```
从命令行中退出：    
```bash
\q
```

## 简单配置，可以在非本地，使用用户名与密码连接数据库
1.可以先新建一个测试用的数据库 test1，这个命令需要登录postgres后执行：    
```sql
create database test1;
```
2.创建成功后，查询数据库信息就能看到新增的数据库test1了，然后输入\q退出postgres的命令行，再输入下面的命令：    
```bash
sudo -u postgres psql test1
```
这样我们就登录到了数据库test1    
3.在test1下建立一个新的user：
```sql
create user test_user password 'test_user';
```
这样test1下面就多了一个用户，可以这样查询到用户信息：    
```sql
select * from pg_user;
```
4.我们有了新的数据库与用户，下面配置使用用户名与密码登录test1：
* /etc/postgresql/#{版本号}/main/pg_hba.conf 文件，增加这样一行"local  all all password"
* 保存
* 在命令行下输入：psql -d test1 -U test_user
* 再输入密码后就可以登入数据库test1了
5.修改配置文件/etc/postgresql/#{版本号}/main/postgresql.conf，有一行：     
```bash
listen 'localhost'
```
注释掉，然后pg_hba.conf 文件，增加这样一行"host  all all password",这样就可以从外网访问了。

## 其他配置及使用说明
暂时还没实践到...    
稍后再补充~    
 
