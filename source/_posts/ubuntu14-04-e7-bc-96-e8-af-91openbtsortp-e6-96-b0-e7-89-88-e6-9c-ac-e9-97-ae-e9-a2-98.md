title: "ubuntu14.04 编译openbts,ortp新版本问题"
date: 2014-05-23 15:59:07
tags:
id: 657
comment: false
categories:
  - 学习笔记
---

ortp新版本rtp_session_set_local_addr函数问题。

问题如下：
<pre class="brush:cpp">SIPBase.cpp:952:59: error: too few arguments to function 'int rtp_session_set_local_addr(RtpSession*, const char*, int, int)'
  rtp_session_set_local_addr(mSession, "0.0.0.0", mRTPPort );
                                                           ^
In file included from /usr/include/ortp/telephonyevents.h:30:0,
                 from SIPBase.cpp:30:
/usr/include/ortp/rtpsession.h:291:17: note: declared here
 ORTP_PUBLIC int rtp_session_set_local_addr(RtpSession *session,const char *addr, int rtp_port, int rtcp_port);</pre>
问题很明显，仔细阅读了过去版本16，和最新版本22的变化后发现。

添加参数的作用是为了解决端口冲突的问题。所以只需要将
<pre class="brush:cpp">SIPBase.cpp 952行的函数参数里增加一个参数之为-1
为什么是-1，看源代码一目了然。</pre>
源代码地址[http://download.savannah.gnu.org/releases/linphone/ortp/sources/](http://download.savannah.gnu.org/releases/linphone/ortp/sources/)

&nbsp;