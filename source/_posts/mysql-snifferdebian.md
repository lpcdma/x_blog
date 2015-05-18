title: "mysql sniffer(debian)"
date: 2013-07-09 11:18:03
tags:
id: 317
comment: false
categories:
  - 工具收集
---

<pre class="brush:cpp">apt-get install libpcap-dev

gcc -O2 -lpcap -o mysqlsniffer mysqlsniffer.c packet_handlers.c misc.c

./mysqlsniffer lo
mysqlsniffer listening for MySQL on interface lo port 3306
127.0.0.1.35364 &gt; server: ID 0 len 1 COM_QUIT
server &gt; 127.0.0.1.35390: ID 0 len 84 Handshake &lt;proto 10 ver 5.5.31-0+wheezy1 thd 239&gt; 
127.0.0.1.35390 &gt; server: ID 1 len 60 Handshake (new auth) &lt;user root db (null) max pkt 16777216&gt; 
server &gt; 127.0.0.1.35390: ID 2 len 7 OK &lt;fields 0 affected rows 0 insert id 0 warnings 0&gt; 
127.0.0.1.35390 &gt; server: ID 0 len 33 COM_QUERY: select @@version_comment limit 1
server &gt; 127.0.0.1.35390: ID 1 len 1 1 Fields
	ID 2 len 39 Field: ..@@version_comment &lt;type var string (253) size 24&gt;
	ID 3 len 5 End &lt;warnings 0&gt; 
	ID 4 len 9 || (Debian) ||
	ID 5 len 5 End &lt;warnings 0&gt; 
127.0.0.1.35390 &gt; server: ID 0 len 18 COM_QUERY: SELECT DATABASE()
server &gt; 127.0.0.1.35390: ID 1 len 1 1 Fields
	ID 2 len 32 Field: ..DATABASE() &lt;type var string (253) size 102&gt;
	ID 3 len 5 End &lt;warnings 0&gt; 
	ID 4 len 1 || NULL ||
	ID 5 len 5 End &lt;warnings 0&gt; 
127.0.0.1.35390 &gt; server: ID 0 len 6 COM_INIT_DB: mysql
......</pre>
下载安装[mysqlsniffer](http://lpcdma.com/wp-content/uploads/2013/07/mysqlsniffer.tar)

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;