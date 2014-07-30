---
layout: post
title: "ruby rake task params"
categories: ruby
date: 2014-7-30 14:00
---
##rake task 从命令行传递参数     
```ruby
task :my_task, [:arg1, :arg2] do |t, args|
  puts "#{args}"
end
```      
```
rake my_task["hello","world"]

输出： {:arg1 => "hello",:arg2 => "world"}
```
