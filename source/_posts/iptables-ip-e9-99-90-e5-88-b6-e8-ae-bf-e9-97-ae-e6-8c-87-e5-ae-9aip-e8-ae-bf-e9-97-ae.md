title: "iptables IP限制访问 指定IP访问"
date: 2014-10-17 15:20:51
tags:
id: 742
comment: false
categories:
  - 学习笔记
---

只允许指定的一个IP访问服务器
vi /etc/sysconfig/iptables

*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]

-A INPUT -s 165.232.121.17 -j ACCEPT
-A INPUT -j DROP
COMMIT

如果你之前的防火墙设置了永久关闭，则需要解除

chkconfig --list 查看启动服务，找到要关闭服务名
chkconfig --level 235 服务名 off 【在等级3和5为开机运行服务】
系统运行级别有0—6，就在/etc/inittab中的0-6
等级0表示：表示关机
等级1表示：单用户模式
等级2表示：无网络连接的多用户命令行模式
等级3表示：有网络连接的多用户命令行模式
等级4表示：不可用
等级5表示：带图形界面的多用户模式
等级6表示：重新启动2011/10/26
================ 以下为摘录 ====================
又有人攻击服务器了，没有办法又的去防，这里简单介绍一种限制指定IP访问的办法。
单个IP的命令是
iptables -I INPUT -s 59.151.119.180 -j DROP
封IP段的命令是
iptables -I INPUT -s 211.1.0.0/16 -j DROP
iptables -I INPUT -s 211.2.0.0/16 -j DROP
iptables -I INPUT -s 211.3.0.0/16 -j DROP
封整个段的命令是
iptables -I INPUT -s 211.0.0.0/8 -j DROP
封几个段的命令是
iptables -I INPUT -s 61.37.80.0/24 -j DROP
iptables -I INPUT -s 61.37.81.0/24 -j DROP

服务器启动自运行
有三个方法：
1、把它加到/etc/rc.local中
2、vi /etc/sysconfig/iptables可以把你当前的iptables规则放到/etc/sysconfig/iptables中，系统启动iptables时自动执行。
3、service iptables save 也可以把你当前的iptables规则放/etc/sysconfig/iptables中，系统启动iptables时自动执行。
后两种更好此，一般iptables服务会在network服务之前启来，更安全
解封：
iptables -L INPUT
iptables -L --line-numbers 然后iptables -D INPUT 序号

iptables 限制ip访问
通过iptables限制9889端口的访问（只允许192.168.1.201、192.168.1.202、192.168.1.203）,其他ip都禁止访问
iptables -I INPUT -p tcp --dport 9889 -j DROP
iptables -I INPUT -s 192.168.1.201 -p tcp --dport 9889 -j ACCEPT
iptables -I INPUT -s 192.168.1.202 -p tcp --dport 9889 -j ACCEPT
iptables -I INPUT -s 192.168.1.203 -p tcp --dport 9889 -j ACCEPT

注意命令的顺序不能反了。