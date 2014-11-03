---
layout: post
title: "ruby thread使用总结"
date: 2014-06-19
categories: ruby
---

## ruby thread 使用总结

### ruby thread 基本用法

```ruby
puts "start"
Thread.new{
  sleep 1
  puts "In Thread"
  }
puts "end"
```
```bash
>start
>end
```
**进程结束子线程也随之结束**

```ruby
puts "start"
Thread.new{
  sleep 1
  puts "In Thread"
  }.join
puts "end"
```
```bash
>start
>In Thread
>end
```

**使用 Thread 实例方法 join，进程会等待子线程执行完成后继续向下执行**

### 启用多个子线程

一般方式：
```ruby
threads = []
5.times do 
    threads << Thread.new{
        sleep 1
        puts "thread_id #{Thread.current.object_id}"
    }
end
threads.each{|thread| thread.join}
puts "end"
```

错误的方式：
```ruby
5.times do 
    Thread.new{
        sleep 1
        puts "thread_id #{Thread.current.object_id}"
    }.join
end
puts "end"
```
这样会在一个线程结束后再启动下一个线程

### 线程状态
```ruby
Thread.current.status
```
"sleep"
Returned if this thread is sleeping or waiting on I/O

"run"
When this thread is executing

"aborting"
If this thread is aborting

false
When this thread is terminated normally

nil
If terminated with an exception.

### 传入参数

```ruby
Thread.new(:args_1 => 1,:args_2){ |args|
    p args
}.join
```
```bash
>{:args_1 => 1,:args_2 => 2}
```

### 多线程使用ActiveRecord连接池进行查询
ActiveRecord 版本4.1.1
adaptor： mysql2
pool： 10

```ruby
class Keyword < ActiveRecord::Base
end
index = 0
11.times do 
    Thread.new{
        keywords = Keyword.where(:status => 10).to_a
        puts index
        p keywords
    }.join
    index += 1
end
```

启动第11个线程时会报错：获取mysql连接超时
原因是前面10个线程与mysql的连接并未自动释放，mysql连接池达到最大连接数

```ruby
class Keyword < ActiveRecord::Base
end
index = 0
11.times do 
    Thread.new{
        keywords = Keyword.where(:status => 10).to_a
        puts index
        p keywords
        ActiveRecord::Base.connection_pool.release_connection
    }.join
    index += 1
end
```
调用方法ActiveRecord::Base.connection_pool.release_connection释放连接池内连接，程序正常运行

