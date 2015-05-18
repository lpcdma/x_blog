title: "CentOS配置dhcp,无法获取ipv4"
date: 2013-06-22 21:12:03
tags:
id: 292
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp"># cat /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE="eth0"
BOOTPROTO="dhcp"
HWADDR="XX:XX:XX:XX:XX:XX"
#NM_CONTROLLED="no"
ONBOOT="yes"
TYPE="Ethernet"
UUID="f8e1217d-11be-48b1-8c45-437cddfe9b0f"

# /etc/init.d/network restart</pre>