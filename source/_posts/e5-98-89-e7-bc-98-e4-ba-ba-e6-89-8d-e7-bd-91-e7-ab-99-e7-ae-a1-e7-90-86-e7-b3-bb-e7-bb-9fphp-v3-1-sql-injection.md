title: "嘉缘人才网站管理系统PHP V3.1 SQL Injection"
date: 2013-06-30 15:35:08
tags:
id: 304
comment: false
categories:
  - 代码审计
---

官网:[http://www.finereason.com/index.html](http://www.finereason.com/index.html)

./zph/info.php
<pre class="brush:php">...
while($row=$db-&gt;fetch_array($query)){
    $i++;
    if($i==1&amp;&amp;!$id){$id=$row['z_id'];}
    $zlist[]=$row;
}
if($id){
    $db-&gt;query("UPDATE `".$cfg['tb_pre']."zph` SET `z_views`=`z_views`+1 WHERE `z_id`=$id");    
   //注入很明显。但是没法我利用少个表,下面的zphorder表在数据库中没有。
    $rs = $db-&gt;get_one("SELECT * FROM `".$cfg['tb_pre']."zph` WHERE `z_id`=$id LIMIT 0,1");

    //gope-2012.8.15.add-start 获取参会企业
    //$sql="SELECT * FROM `".$cfg['tb_pre']."zphorder` WHERE `zo_zid`=$id and `zo_state`=1 ORDER BY `zo_adddate` ASC";
    $sql="SELECT `".$cfg['tb_pre']."zphorder`.*,`".$cfg['tb_pre']."member`.m_typeid,`".$cfg['tb_pre']."member`.m_id FROM `".$cfg['tb_pre']."zphorder` INNER JOIN `".$cfg['tb_pre']."member` on `zo_mid`=`m_id` where `zo_zid`=$id and `zo_state`=1 ORDER BY `zo_adddate` ASC";
    print $sql;
        $query=$db-&gt;query($sql);$clist=array();
    while($row=$db-&gt;fetch_array($query)){
        $i++;
        //if($i==1&amp;&amp;!$id){$id=$row['z_id'];}
        $clist[]=$row;
    }
    //gope-2012.8.15.add-end
}
....</pre>
官方出了个补丁：[http://bbs.finereason.com/read/31816](http://bbs.finereason.com/read/31816)
这下可以用了
<pre class="brush:php">........
while($row=$db-&gt;fetch_array($query)){
    $i++;
    if($i==1&amp;&amp;!$id){$id=$row['z_id'];}
    $zlist[]=$row;
}
if($id){
    $db-&gt;query("UPDATE `".$cfg['tb_pre']."zph` SET `z_views`=`z_views`+1 WHERE `z_id`=$id");

    $rs = $db-&gt;get_one("SELECT * FROM `".$cfg['tb_pre']."zph` WHERE `z_id`=$id LIMIT 0,1");
}
......</pre>
漏洞证明

[![59](http://lpcdma.com/wp-content/uploads/2013/06/59-300x179.jpg)](http://lpcdma.com/wp-content/uploads/2013/06/59.jpg)

&nbsp;