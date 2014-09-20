---
layout: post
title:  "vxixi.com seo"
date:   2014-06-30
categories: seo
---

第一阶段：     
1.熟悉.net mvc的网站建设，刚接触ruby与ror     
2.对seo完全没有了解     
3.使用ror、bootstrap与ruby爬虫爬取的数据简单建立了一个站点并部署     
4.百度显示有索引量，但是site:vxixi.com搜索无结果，特别是product相关页     

网站搁置大概4~5个月，期间使用zhangzhan.baidu.com提交了站点信息，首页被收录，期间陆续增加了一些product内容，并有一段时间由于vps问题站点无法访问。     


第二阶段：     
1.对ruby、ror与seo有了进一步的了解     
2.偶然间发现关键词**羽毛球比赛规则**带来了自然流量,chinaz显示网站权重从0变为了1     

第三阶段：     
开始对该站点进行seo的尝试，下面是我的一些思考与相应的执行方法。     

Q-1 首先是**羽毛球比赛规则**带来的第一批自然流量，查看了seo排名靠前的站点，发现大多是N久以前的内容，在内容的准确性，页面体验等方面都无法满足用户的需求     
A-1 将羽毛球规则提至网站首页，丰富内容，优化页面前台（当然这个与seo无关）     

一些采用的优化方法：     
1.增加相关文字和图片内容，文字可以的地方用目标关键字代替，图片alt使用目标关键字相关短语     
2.title使用目标关键字（短title）     
3.meta标签 keyword description 围绕目标关键字展开 （description貌似可以更相关一点，即将测试效果）     
4.站内链接a标签的text使用目标关键字链接至首页     
5.增加站内页面，增加收录量，youku视频的爬取     
6.使用百度相关搜索在video详细页罗列关键词来使搜索引擎收录页面     

目标关键词seo结果至百度首页，搜狗首页，平均排名为5，uv 100左右     
增加了嵌入式的广告，无点击     

下一阶段：     
1.按照现有逻辑继续增加可以被收录的页面     
2.优化视频列表页     
3.观察目标关键词的seo排名     

计划：     
1.5分钟每次的相关搜索持续跑     
2.优化视频列表页     
3.每天增加一项video_list 与 video     

ps：暂时未遇到性能问题     

每天搜索引擎到访的抓取是有限的...     


计划完成情况：     
1.页面收录得到了增加，目前是200+     
2.视频列表页得到了优化，但是仍未被收录，目测是由于p标签内容太少，链接列表太多...     
3.未做到每天在增加...这项争取吧     

发现的新状况：
1.据说申请百度联盟备案信息填海外空间能够审批通过，目前正在等待中     
2.百度移动端会自动转码，体验并不好     
3.embed标签在手机端无法播放视频 除首页外其他内容移动端体验都不好...虽然这些只是用来充数的，但是...还是需要优化下     
4.考虑使用siteapp.baidu.com制作一个简易的手机端页面     

百度联盟申请再一次失败，重试中     
重做了siteapp，无备案无法使用流量变现，意义不是特别大     
未防止百度移动端自动转码截取流量，需要研究开放适配功能，直接在移动端展示移动端手机页面     
采用下面的方式防止百度对移动端网站进行自动转码：     
A. no-transform协议     
TC支持的no-transform协议为如下两种形式：     
a. HTTP Response中显式声明Cache-control为no-transform。     
b. meta标签中显式声明Cache-control为no-tranform，格式为：     
<head>     
<meta http-equiv="Cache-Control" content="no-transform " />     
</head>     
如第三方站点不希望页面被转码，可添加此协议，当用户进入第三方网站时，先进入中间提示页，页面将引导用户自主选择跳转至原网页。     
B. handheld协议     
页面通过<link>标签显式指定WAP网页，声明格式如下：     
<link rel="alternate" type="application/vnd.wap.xhtml+xml" media="handheld" href="target"/>     
如第三方站点不希望页面被转码，可添加此协议，告知我们原网页对应有一个WAP版页面，当用户进入第三方网站时，先进入中间提示页，让用户自主选择跳转至原网页或第三方网站自有的wap页面。     
C. User-Agent相关     
TC抓取页面时，使用的User-Agent为：     
Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0; baidu Transcoder;)     
如第三方站点不希望转码HTML页面，且又可以提供对应的自有WAP页面，则可以根据这个User-Agent，返回自有WAP页，同时在HTTP Response显示声明：Content-Type为：vnd.wap.xhtml+xml，那么TC将不对这个页面转码，而是直接跳转至相应的自有WAP页面。     

现采用开放适配做一下首页     
同时在首页中添加了 <meta http-equiv="Cache-Control" content="no-transform " />     
各种版式的手机页，目前发现了，html5版式、wml版式、xhtml版式     
     
今晚开始优化手机站     
开放式配在2014/4/21的18点左右生效了，现在百度移动端搜索展示结果为m.vxixi.com，百度不会对其转码，而是直接跳转     
10天左右的时间开放式配生效     

继续优化手机站点，可以关注一下手机站点的流量          
- - -
##当前站点情况 2014-06-30 更新     
百度收录1400+页面，期间又一次收录大量减少的情况        
“羽毛球比赛规则” 百度PC端SEO到首页第二名，有个别几天到了头名，在移动端已到了第一位。        
开放适配的移动端页面与PC页面的移动端索引会发生冲突，冲突若干天后还是开放适配的移动端页面为最终展示。         
每日PV PC端+移动端稳定在1000+，且因为移动端排名更高，移动端的PV更高         
最高PV2100+，当PC端SEO头名时发生的        
优化了部分页面展示        
增加了大量的VIDEO页面，百度尚未展示是否已收录        
增加了[羽毛球拍](http://vxixi.com/guize/qiupai)，[羽毛球场地标准尺寸](http://vxixi.com/guize/biaozhunchangdi)，两个高搜索量的页面，等待百度收录        
现有大量页面但是百度爬取与收录相对极慢...不容易搞定啊，[vxixi.com video_list sitemap](http://vxixi.com/video/sitemap)     
    


















