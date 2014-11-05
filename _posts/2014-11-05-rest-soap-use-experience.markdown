---
layout: post
title:  "restful rpc soap 概念及应用"
date:   2014-11-4
categories: db
---
最近越来越多的接触到名词RESTful，一直未求甚解，这货是什么呢？是时候深入得了解下了。
##Web Service
WebService（Network Api)，当下后端程序员都会用到，或是调用别人API，或是自己就是开发API的。  

移动端应用的爆发，云端的日趋成熟，WebService被广泛使用，更好用的服务需求也更强烈。

自己遇到的一些应用场景：    
* 公司内部各业务组间数据库隔离、代码库隔离，各种业务系统无耦合，简单来讲，前台用户下单不可以影响到库房进货，退换货不能把财务系统给弄死掉。
是不是这些系统看上去不相关？当最初这些系统（c/s架构）由一拨人（甚至一个人）搞定，代码、数据库都是一套的时候，数据库出了故障所有一线员工就可以休息
等故障恢复了。于是各种业务系统需要各自独立，但是在业务上都是有上下游关系的，顾客下单，库房要处理订单，供应链要上货，而这一切都要过财务。这个时候
各个子系统间的交互就需要WebService了。
* 然后是不同公司间的系统交互，这个时候不能相互访问数据库呀，从系统的异构性与安全性考虑，需要WebService了。
* 移动端App，多是将数据存储于云端服务器，通过WebService来请求相关数据的。

简单来讲，当需求数据或某种服务，自己没有或不方便提供，那么就到网络上去找吧，通过WebService。

硬性需求已存在，服务的提供者就在考虑如何提供方便、易用的WebService了。

##RPC
RPC：remote procedure call，远程服务调用，我接触到关于WebService最古老的名词了，据说在2000年初的时候比较火。

调用大致流程：客户端构造xml或json文本，通过http发往服务器端，服务器端接到请求后再以xml或json格式响应，具体格式双方约定好就OK了。

下面问题来了，约定request response格式是个费力的活，每家模板不一样，这个上面会损失很多人力。计算机科学人士在提升生产力与生产关系上可
是好手，分分钟搞出约定标准，于是有了SOAP。

##SOAP的使用
SOAP：simple object access protocol       
WSDL：web service description language     
RPC + xml + http + wsdl = soap      
有一个远程可调用的服务，服务方法 参数 返回值 数值类型都是用WSDL定义好了，构造请求与返回信息都是xml，调用方式使用http。

nodejs + coffee_script 调用soap的方式在另一篇blog里有比较详细的说明，使用ruby调用soap api的文档google一下就知道了，很多很方便。

##WCF
.net c#下面用WCF够方便，自动生成代理类，强类型...c#语言特性也是及优雅的，但是...      
只怕以后是不会再用了

##简单的web server，http请求，simple http interface shapi
现在绝大多数的WebService都是简单的Http Request & Response，控制好授权，controller action params设计的合理一点，都是挺好用的。

当下频繁使用的Api：baidu sem，qihu sem，http header里面放身份认证信息，url填对，params传正确，性能与易用性都不错。

##RESTful API
REST：representational state trasfer        
看了些中文资料，觉得没有哪个中文翻译的够霸气。      
个人觉得，REST与上面介绍的shi不一样的地方，它会将url中的动词提取出来，用http协议自身的动作（get post put delete 等）去代替，
举一些例子：
获取video列表    
shapi：http://www.vxixi.com/video/list 
restful：http://www.vxixi.com/video（最好是复数）    
获取id=1的video    
shapi：http://www.vxixi.com/video/show/1
restful：http://www.vxixicom/videos/1 (get: 对应action show, post: 对应action create, put: 对应action edit ...)    

uri是资源，http method 表示动作，params自定义就好了     

用rails以rest原则来开发web service，太方便了    

###RESTful API 选择？
个人认为为当前基于网络的软件设计架构提供了新的思路与新的选择。     
优雅的代码时大家的共同追求，严格按照REST原则来设计API，diaobao了。     
具体架构选择看实际应用了，比如baidu sem api，很多公司系统是java .net搞的，还是需要一套soap的接口方便大家使用。    







