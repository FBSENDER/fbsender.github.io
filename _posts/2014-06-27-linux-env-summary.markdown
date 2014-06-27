---
layout: post
title: "linux 环境变量的设置与查看"
categories: linux
date: 2014-06-27
---
##显示所有的环境变量
```bash
env

>balabala....
```
```bash
set

>balabala....
```

##显示一个环境变量
bash下需显示一个环境变量，比如说"HOME"   
```bash
echo $HOME

>/home/mengchen
```
```bash
env | grep ^HOME

>/home/mengchen
```

##设置一个环境变量 在当前shell下有效
```bash
export LMCENV=123
```
查看
```bash
echo $LMCENV

>123
```

##修改一个环境变量 在当前shell下有效
```bash
LMCENV=321
echo $LMCENV

>321
```

##删除一个环境变量 当前shell下删除
```bash
unset LMCENV
```

再次查看   

```bash
echo $LMCENV

>（神马都没有）
```
删除成功

##设置一个环境变量为readonly
```bash
export LMCENV=123
readonly LMCENV
echo $LMCENV

>123
```
尝试修改该环境变量LMCENV      
```bash
LMCENV=321

>read-only variable: LMCENV
```
尝试删除该环境变量LMCENV   
```bash
unset LMCENV

>read-only variable: LMCENV
```

##设置一个永久生效的环境变量
使环境变量永久生效，需要修改配置文件    
1. 在/etc/profile文件中添加变量【对所有用户生效(永久的)】   
2. 在用户目录下的.bash_profile文件中增加变量【对单一用户生效(永久的)】
