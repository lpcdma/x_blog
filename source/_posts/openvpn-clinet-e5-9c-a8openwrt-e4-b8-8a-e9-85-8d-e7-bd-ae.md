title: "openvpn clinet 在openwrt上配置"
date: 2014-08-18 01:33:44
tags:
id: 710
comment: false
categories:
  - 工具收集
---

<span style="color: #333333;">后台以进程方式启动OpenVPN：</span>
<pre class="brush:cpp">openvpn --cd /etc/openvpn --daemon --config client.conf</pre>
<span style="color: #333333;">设置指定IP或IP段走OpenVPN：</span>
<pre class="brush:cpp">route add -host 66.102.255.41 dev tun0 
route add -net 1.1.1.0/24 dev tun0
route add -host 66.102.255.41 gw 192.168.1.1</pre>
Openwrt下客户端走Openvpn还要设置iptables地址转换并指定出口网卡：
<pre class="brush:cpp">iptables -t nat -I POSTROUTING -s 192.168.1.0/24 -d x.x.x.x -o tun0 -j MASQUERADE</pre>
指定中国地址不走VPN
<pre class="brush:cpp">wget http://www.ipdeny.com/ipblocks/data/countries/cn.zone
for ip in `cat us.zone`
do
route add -net $ip dev 192.168.1.1
done</pre>