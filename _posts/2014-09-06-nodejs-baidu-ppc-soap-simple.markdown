---
layout: post
title:  "nodejs baidu ppc soap api 调用示例"
date:   2014-09-06
categories: ruby
---
最近在练习使用coffee script + nodejs写后端代码，一些原由ruby实现的功能尝试使用js实现，下面是调用百度PPC api的例子。
## coffee script 代码部分
```coffeescript
soap = require 'soap'
url = 'https://api.baidu.com/sem/sms/v3/AccountService?wsdl'

#nodejs soap包默认把‘tgn’也写在了ignored_namespace中，而不巧，baidu api中有一部分message使用的命名空间为'tgn'
#需要将tgn从ignored_namespace中剔除，否则发起的soap request body将无法自动适配命名空间
options = 
  ignoredNamespaces :
    namespaces : ['targetNamespace', 'typedNamespace'],
    override: true

#身份认证部分 注意下面需要添加命名空间，soap的命名空间又是什么呢？需要大家google一下
operation_header = () ->
  'AuthHeader' : 
    'username' : 'username',
    'password' : 'password',
    'token'    : 'token'

client = soap.createClient(url, options, (err,client) ->
  client.addSoapHeader(operation_header(),null,'common','http://api.baidu.com/sem/common/v2')
  client.getAccountInfo({},(error,result,xml) ->
    console.log result
    response = client.wsdl.xmlToObject(xml)
    console.log(response.Header.ResHeader.rquota)
  )
)

```
## ruby 代码部分

gem install baidu    
相关gem已由组内同事封装好了，同样还用sogou，qihu360
然后一切的一切多就都可以搞定了...      
