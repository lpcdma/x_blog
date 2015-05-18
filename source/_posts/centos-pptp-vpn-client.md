title: "CentOS pptp vpn client"
date: 2013-06-22 22:23:33
tags:
id: 294
comment: false
categories:
  - 学习笔记
---

yum -y install ppp pptp pptp-setup

pptpsetup –create VPN名 –server xxx.xxx.xxx.xxx–username 用户名 –password 密码
<pre class="brush:cpp">[root@localhost ppp]# cat /etc/ppp/peers/VPN名 
# written by pptpsetup
pty "pptp 172.246.16.129 --nolaunchpppd"
lock
noauth
nobsdcomp
nodeflate
name vpn
remotename VPN名
ipparam VPN名

defaultroute   #使用本连接作为默认路由
persist   #当连接丢失时让pppd再次拨号，已验证 
require-mppe-128  
refuse-pap  
refuse-chap  
refuse-eap  
refuse-mschap</pre>
cp /usr/share/doc/ppp-2.4.5/scripts/pon /usr/sbin/
cp /usr/share/doc/ppp-2.4.5/scripts/poff /usr/sbin/

chmod 755 /usr/sbin/pon
chmod 755 /usr/sbin/poff
pon VPN名

route add -net 0.0.0.0 dev ppp0

poff VPN名

&nbsp;

&nbsp;