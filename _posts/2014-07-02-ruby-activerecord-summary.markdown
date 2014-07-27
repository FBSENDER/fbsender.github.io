---
layout: post
title: "ActiveRecord 使用技巧"
categories: ruby
date: 2014-07-02 18:00
---
## gem install mysql2
直接gem install 发生异常，需要先安装mysql开发包，ubuntu下是这样的：
```
sudo apt-get install libmysql-ruby libmysqlclient-dev
```    

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
beijing\_keywords   
shanghai\_keyowrds   
guangzhou\_keywords   

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

## ActiveRecord Association
有下面两套表：   
beijing\_keywords   
shanghai\_keyowrds   
guangzhou\_keywords    
beijing\_adgroups    
shanghai\_adgroups     
guangzhou\_adgroups    
keyword与adgroup是1-n的关系   

```ruby
class Keyword < ActiveRecord::Base
    belongs_to :adgroup
    def self.config(city_name)
        self.table_name = "#{city_name}_keywords"
        self
    end
end
class Adgroup < ActiveRecord::Base
    has_many :keywords
    def self.config(city_name)
        self.table_name = "#{city_name}_keywords"
        self
    end
end

city_name = "beijing"
keyword_class = Keyword.config(city_name)
adgroup_class = Adgroup.config(city_name)
#根据adgroup的一个字段adgroup_name = "hello"查询keywords
keyword_class.joins(:adgroup).where("#{adgroup_class.table_name}.adgroup_name" => "hello" )
#获取id = 1的keyword对应的adgroup
keyword = keyword_class.find(1)
keyword.adgroup
#获取id = 10的 adgroup 对应的 keywords
adgroup = adgroup_class.find(10)
adgroup.keywords
```
