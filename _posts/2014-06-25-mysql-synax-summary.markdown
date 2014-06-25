---
layout: post
title: "Mysql 常用函数"
categories: mysql
date: 2014-06-25
---
##mysql 字符串左侧补零
city_id 定长4位数字   
region_id 定长8位数字  
可以使用 lpad() 函数进行左侧补零操作   

```sql
update table_a
set city_id = lpad(city_id,4,'0')
where city_id = '101'

>city_id => '0101'
```
```sql
update table_b
set region_id = lpad(region_id,8,'0')
where region_id = '1234'

>region_id => '00001234'
```

##mysql 字符串连接
**mysql字符串不可以使用操作符‘+’来连接字符串**   

```sql
select '1' + '2';   > 3

select 'abc' + '1'; > 1

select 'a' + 'b';   > 0

select 'a' + 'b' + '1' + '2';   > 3

select '123a'+ 'a1' + '2b3';    > 125
```

使用 concat() 函数来进行字符串连接   

```sql
select concat('1','2'); > '12'

select concat('abc','1');   > 'abc1'

select concat('a','b');     > 'ab'

select concat('a','b','1','2'); > 'ab12'

select concat('123a','a1','2b3');   > '123aa12b3'
```

测试环境 MariaDB


