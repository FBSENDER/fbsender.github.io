---
layout: post
title:  "nodejs mysql orm 及 ssh tunnel 示例"
date:   2019-09-06
categories: ruby
---

## nodejs orm 调用示例
nodejs orm，使用npm 包为 node-orm2，初步使用，无法自动适配mysql中已存在的表的列名，查询得到的数据只能在回调函数中使用，别的都还好，下面是coffee scirpt示例代码：    

```coffeescript
orm = require 'orm'
#mysql 连接配置 方式1
#db = orm.connect('mysql://public_user:public_user@seo/development')
#mysql 连接配置 方式2
db = orm.connect(
  host: 'seo'
  database: 'production'
  protocol: 'mysql'
  port: '3306'
  query:
    pool: true
  user: 'public_user'
  password: 'public_user'
)

acc_obj = {
  id:
    type: "serial"
    key: true
  account_name:
    type: "text"
  password:
    type: "text"
  api_key:
    type: "text"
}

ses = ['baidu','sogou']
products = ['hotel','mhotel','jiudian','mjiudian']
mts = ['exact','phrase']
tables = []

#数据库中存在多套结构相同的表，按业务划分，表名不同
for se in ses
  for product in products
    for mt in mts
      tables.push('sem_' + se + '_' + product + '_' + mt + '_accounts')

#输出表名列表到标准输出
for table in tables
  console.log table

my_result = []

#数据库查询 回调函数
radd = (err,accounts) ->
  console.log err if err
  for account in accounts
    my_result.push({
      id: account.id
      account_name: account.account_name
      password: account.password
      api_key: account.api_key
    })
  console.log my_result

db.on('connect',(err) ->
  if err
    console.log("error",err)
    return
  for table in tables
    console.log table
    #orm bid 绑定表名，定义列名对应关系
    account_class = db.define(table,acc_obj)
    account_class.all(radd)
)

#关闭数据库连接，需要放在合适的位置
#如果放在下面一行，还未进行数据查询
#db.driver.db.end()
```

## mysql orm ssh 示例
有时连接数据库需要用到ssh，比如我们在开发过程中个人机器是无法直接连接数据库的，需要先ssh连接一台跳板机，然后通过跳板机来连接数据库。      
ruby的实现网上已有很多可用示例，前阵子自己也写代码调通了，现在使用nodejs来实现该功能，google了好久，终于也得到了解决。    
### 方法1 使用linux自身的ssh command通过跳板机建立ssh通道
```coffeescript
ssh user@sshserver -L 127.0.0.1:33333:database_server:3306    
```    
新开一个terminal，输入指令，就是登陆到ssh server，并将本地的33333端口绑定到数据库服务器的3306端口，ssh通道已建立。    
之后再修改上面代码中的mysql连接配置：    

```coffeescript
orm = require 'orm'
db = orm.connect(
  host: '127.0.0.1'
  database: 'production'
  protocol: 'mysql'
  port: '33333'
  query:
    pool: true
  user: 'user'
  password: 'password'
)
```

### 方法2 使用nodejs coffee script 代码建立ssh 通道
npm install tunnel-ssh    
这个nodejs包直接使用有一些问题，于是自己fork下来做了些修正，已发起pull request，应该很快就可以使用了。
使用这个包的代码如下：    

```coffeescript
Tunnel = require "tunnel-ssh"

config = 
  remoteHost: 'database_server'
  remotePort: 3306  #localport
  localPort: 33333  #remoteport
  verbose: true     #dump information to stdout
  disabled: false   #set this to true to disable tunnel (useful to keep architecture for local connections)
  sshConfig:        #ssh2 configuration (https://github.com/mscdex/ssh2)
    host: 'ssh_server'
    port: 22
    username: 'user'
    password: 'password'
    
tunnel = new Tunnel(config)

tunnel.connect((error) ->
  console.log error if error
  #do something
  #关闭ssh 管道
  tunnel.close();
)
```
mysql的配置：    

```coffeescript
orm = require 'orm'
db = orm.connect(
  host: '127.0.0.1'
  database: 'production'
  protocol: 'mysql'
  port: '33333'
  query:
    pool: true
  user: 'user'
  password: 'password'
)
```

具体的orm操作可以写在tunnel连接的回调函数中，也可以在另外一个进程中运行。



