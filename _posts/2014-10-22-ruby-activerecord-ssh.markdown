---
layout: post
title:  "rails ActiveRecord ssh"
date:   2019-10-27
categories: ruby
---

由于某些安全策略，有时无法在开发机直连数据库，需要通过一台跳板机来完成。      
下面的代码可以使ActiveRecord通过配置好的ssh通道连接到指定的MySql数据库。    
```ruby
require 'active_record'
require 'mysql2'
require 'net/ssh/gateway'

gateway = Net::SSH::Gateway.new(
    'seo',     #跳板机host
    'op',      #用户名  
    :password => 'op' #密码
)
port = gateway.open("sem2", 3306)  #目标机的host及端口号，返回一个本机ssh连接使用的端口号

puts gateway.active?
puts port
db_read_only = {
  :adapter  => 'mysql2',
  :username => 'public_user',
  :password => 'public_user',
  :database => 'production',
  :host     => '127.0.0.1', #这里需要填本机ip
  :pool     => 10,
  :port     => port         #本机端口号
}
ActiveRecord::Base.establish_connection(db_read_only)
class Keyword < ActiveRecord::Base
  self.table_name = 'sem_baidu_mhotel_exact_keywords'
end
p Keyword.take
gateway.shutdown!

```

这个样子就可以在开发机通过跳板机连接生产环境的数据库了，当然跳板机需要有访问生产机的权限...
