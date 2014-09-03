---
layout: post
title:  "linux 常用指令"
categories: linux
date: 2014-09-03
---

## cd 目录跳转 change dictionary
```bash
cd .. #上一层目录
cd ~ #家目录
cd #家目录
cd / #根目录
cd - #返回上一个目录
cd . #当前目录
pwd #显示当前目录
pwd -P #显示完整路径
mkdir #创建目录
rmdir #删除一个空目录
```
使用zsh时，目录跳转通常无需输入cd

## ls 显示目录信息
```bash
ls -al #显示当前目录下的文件信息
```

## cp 拷贝文件
```bash
cp file1 file2
```

## scp 远程拷贝
```bash
scp file1 mc@sem2:file2
```

## rm 删除文件
```bash
rm file1 #删除文件
rm -rf /file #删除文件夹及其里面的文件
```

## 查看文件
```bash
cat file1 #查看文件，输出文件内容到标准输出
tail file1 #查看文件最后几行
less file1 #分页浏览文件 可以向上 或向下查看
more file1 #分页浏览文件
```

## 解压文件
```
gzip -d filename
```

## curl 下载文件
```
curl -o save_as_filename uri
```

## iconv 文件转码
gb18030 to utf8       
-c 忽略无法转换的字符    
```
iconv -t utf8 -f gb18030 -c source_file > target_file
```
