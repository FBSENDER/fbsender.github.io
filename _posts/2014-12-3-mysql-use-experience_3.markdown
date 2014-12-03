---
layout: post
title:  "mysql使用规范与建议（三）"
date:   2014-12-03
categories: mysql
---

##SQL书写建议

1.UPDATE、DELETE语句不使用LIMIT

2.避免数据类型的隐式转换，隐式转换会导致索引失效

3.拒绝大SQL，拆分成小SQL，可以充分利用QUERY CACHE，利用多核CPU

4.使用预编译语句，只传参数，比传递SQL语句更高效，一次解析，多次使用，可以避免SQL注入

5.不使用select  * 

6.避免使用大表的JOIN，MySQL最擅长的是单表的主键/索引查询，JOIN消耗较多内存，产生临时表

7.避免在数据库中进行数学运算，MySQL不擅长数学运算，无法使用索引

8.减少与数据库的交互次数    
INSERT ... ON DUPLICATE KEY UPDATE      
REPLACE  INTO、INSERT IGNORE 、INSERT INTO VALUES(),(),()     
UPDATE … WHERE ID IN(A,B,C,…)

