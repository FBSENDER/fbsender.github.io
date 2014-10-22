---
layout: post
title:  "ruby使用c/cpp扩展_extending ruby with c"
date:   2019-10-22
categories: ruby
---
最新学习到了ruby的c扩展库，试着自己写了一下，把遇到的坑记下来。    
使用c扩展ruby还是挺方便的。    

首先要有一个c语言文件：    

```c
#include <ruby.h> //ruby标准库 必要的结构、函数
#include <stdio.h>

VALUE TestC = Qnil;  //VALUE 使用c语言定义的ruby结构
VALUE test_c_1();
VALUE test_c_2();

VALUE test_c_1(){
  printf("Hello C\n");
  return INT2NUM(0);  //c int 到 ruby num 的转换
}

VALUE test_c_2(VALUE self,VALUE num){
  printf("This is %d \n",NUM2INT(num)); //ruby num 到 c int 的转换
  return num;
}


void Init_TestC(){
  TestC = rb_define_module("TestC");  //模块名 TestC
  rb_define_method(TestC,"test_c_01",test_c_1,0); //模块名 映射到的ruby方法名 c函数名，参数个数
  rb_define_method(TestC,"test_c_02",test_c_2,1); //第一个参数默认为self，所以这里的参数个数应写1，写2就报错了...
}

```

然后要有一个extconf.rb文件：    

```ruby

require 'mkmf'  #需要gem make makefile

extension_name = "TestC"  #指定扩展的模块名，对应.c文件中的函数 void Init_TestC();

create_makefile(extension_name) #建立 Makefile

```

代码写好了，接下来安装这个扩展：    

```bash
ruby extconf.rb #生成 Makefile

make

sudo make install
```

到这里TestC模块就安装好了，下面测试一下写好的扩展：    

```ruby
require 'TestC'
include TestC

test_c_01 # 输出 "Hello C"
result = test_c_02(2132) # 输出 "This is 2132"
puts result # 输出 2132

```

测试通过，一个ruby的c扩展就完成了~
