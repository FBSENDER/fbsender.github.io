---
layout: post
title: "ActiveRecord 使用技巧"
categories: ruby
date: 2014-07-02 18:00
---
## 脱离rails单独使用ActiveRecord(mysql)
```ruby
require 'mysql2'
require 'active_record'
$dbconfig = {
   'development' => {
      :adapter  => 'mysql2',
      :username => 'xxx',
      :password => 'xxx',
      :database => 'development',
      :host     => '127.0.0.1',
      :pool     => 10
   },
   'production' => {
      :adapter  => 'mysql2',
      :username => 'xxx',
      :password => 'xxx',
      :database => 'production',
      :host     => '127.0.0.1',
      :pool     => 10

   }
}
ActiveRecord::Base.establish_connection($dbconfig['development'])
class Post < ActiveRecord::Base
end
puts Post.table_name    #posts
```

## 定义一个类 继承于ActiveRecord::Base
数据库内按照业务逻辑拆表，表内各个字段名一致，表名不同:
beijing_keywords   
shanghai_keyowrds   
guangzhou_keywords   
```ruby
class Keyword < ActiveRecord::Base
    def self.config(city_name)
        self.table_name = "#{city_name}_keywords"
        self
    end
end
keyword_class = Keyword.config("beijing")
puts keyword_class.table_name   #beijing_keywords
p keyword_class.take            #beijing_keywords first record 
```

