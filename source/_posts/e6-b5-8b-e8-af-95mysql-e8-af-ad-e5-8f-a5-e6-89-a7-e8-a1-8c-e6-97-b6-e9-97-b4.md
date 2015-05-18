title: "测试mysql语句执行时间"
date: 2015-04-22 16:49:23
tags:
id: 782
comment: false
categories:
  - 学习笔记
---

<pre class="brush:sql">show profiles;
show variables like "%pro%";
set profiling=1;
--获取时间.
set @d=now(); 
select * from user_msg; 
select timestampdiff(second,@d,now());</pre>