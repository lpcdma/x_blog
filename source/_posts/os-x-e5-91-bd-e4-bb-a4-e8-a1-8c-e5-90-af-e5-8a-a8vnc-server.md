title: "os x命令行启动vnc server"
date: 2015-01-16 17:35:09
tags:
id: 763
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -activate -configure -access -on -clientopts -setvnclegacy -vnclegacy yes -clientopts -setvncpw -vncpw mypasswd -restart -agent -privs -all</pre>
&nbsp;