title: "KingCMS 1.0 SQL Injection"
date: 2013-06-09 23:03:30
tags:
id: 206
comment: false
categories:
  - 代码审计
---

[http://www.kingcms.com/download/local/10/](http://www.kingcms.com/download/local/10/)
<pre class="brush:php">/**
	分页列表信息
	@param int listid : 列表id
	@return array
*/
public function infoList($listid=null){
	global $king;

	if(!$listid)                        //逻辑错误，导致注入。
		$listid=kc_get('listid',2,1);//必须的

	if($listid==0)
		return;

	$cachepath='portal/list/'.$listid;

	$array=$king-&gt;cache-&gt;get($cachepath);//缓存中的listInfo
	if(!$array){
		$array=array();
		if($list=$king-&gt;db-&gt;getRows_one("select * from %s_list where listid={$listid}")){
			foreach($list as $key =&gt; $val){
				if(!kc_validate($key,2))
					$array[$key]=$val;
			}
		}else{
			kc_error($king-&gt;lang-&gt;get('system/error/param').kc_clew(__FILE__,__LINE__,$king-&gt;lang-&gt;get('portal/msg/listname'))."&lt;p&gt;LISTID: $listid&lt;/p&gt;");
		}

		$modelid=$list['modelid'];
		if($modelid&gt;0){//非内置模型的时候
			$model=$this-&gt;infoModel($modelid);

			$resCount=$king-&gt;db-&gt;getRows_one("select count(*) AS ncount from %s__{$model['modeltable']} where listid=$listid and nshow=1 and kid1=0;");//显示的主题内容
			$resCountAll=$king-&gt;db-&gt;getRows_one("select count(*) AS ncount from %s__{$model['modeltable']} where listid=$listid and kid1=0;");//所有的主题内容

			if($list['ncount']!=$resCount['ncount']){//当前列表数量和实际的数字不同的时候，更新结构缓存
				$array['ncount']=$resCount['ncount'];
			}
			$array['ncountall']=$resCountAll['ncount'];

			$array['pcount']=($list['ncount']==0) ? 1:ceil($array['ncount']/$list['nlistnumber']);//列表，共pcount页，前台的

		}

		$isexist= $king-&gt;db-&gt;getRows_number('%s_list',"listid1=$listid")&gt;0 ? 1 : 0;//是否存在子栏目
		//更新列表数据
		$array_list_up=array(
			'ncount'=&gt;$array['ncount'],
			'ncountall'=&gt;$array['ncountall'],
			'isexist'=&gt;$isexist,
			);
		$king-&gt;db-&gt;update('%s_list',$array_list_up,"listid=$listid");

		$array['isexist']=$isexist;

		$site=$this-&gt;infoSite($list['siteid']);
		$array['klisttitle']=$list['ktitle'];

		//直接在这个列表信息获取函数里转换的话，无需再次进行转换
		$array_htmlspecialchars=array('ktitle','klisttitle','klistname','kkeywords','klistpath','kdescription','kimage','klanguage');//需要转换为htmlspecialchars的字段
		foreach($array_htmlspecialchars as $key =&gt; $val){
			$array[$val]=htmlspecialchars($array[$val]);
		}

		$king-&gt;cache-&gt;put($cachepath,$array);
	}
	return $array;
}
........

http://127.0.0.1/kingcms_php_1.0/index.php/list-13 and 1=1-1.html</pre>
&nbsp;

&nbsp;