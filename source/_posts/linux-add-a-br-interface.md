title: "linux add a br Interface"
date: 2014-12-23 00:40:28
tags:
id: 759
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">    sudo brctl addbr docker0 # create your bridge  
    sudo brctl addif docker0 eth0 # mask an existing interface using the bridge  
    sudo ip link set dev docker0 up # bring it up - not really sure if this is necessary or is it done automatically  
    sudo ifconfig docker0 10.0.0.4 # give it an IP</pre>