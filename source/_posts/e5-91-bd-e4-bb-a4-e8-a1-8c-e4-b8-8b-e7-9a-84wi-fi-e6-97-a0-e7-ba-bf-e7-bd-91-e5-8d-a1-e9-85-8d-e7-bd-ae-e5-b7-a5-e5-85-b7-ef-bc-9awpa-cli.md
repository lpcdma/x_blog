title: "命令行下的Wi-Fi无线网卡配置工具：wpa_cli"
date: 2013-12-23 22:24:05
tags:
id: 451
comment: false
categories:
  - 学习笔记
---

wpa_cli是命令行界面下的无线网连接工具。
通过wpa_cli管理备选的网络列表。在备选网络中启用的网络，树莓派会自动试图连接。

输入sudo wpa_cli启动wpa_cli的命令行界面（必须sudo）。常用的指令如下：

status：列出目前的联网状态。
list：列出所有备选网络。目前正连接到的网络会标[CURRENT]，禁用的网络会标[DISABLE]。
add_network：增加一个备选网络，输出新网络的号码（这个号码替代下文的[network_id]）。注意新网络此时是禁用状态。
set_network [network_id] ssid "Your SSID"：设置无线网的名称（SSID）
set_network [network_id] key_mgmt WPA-PSK：设置无线网的加密方式为WPA-PSK/WPA2-PSK
set_network [network_id] psk "Your Password"：设置无线网的PSK密码
enable_network [network_id]：启用网络。启用后如果系统搜索到了这个网络，就会尝试连接。
disable_network [network_id]：禁用网络。
save_config：保存配置。

一个连网的例子：
> add_network
4                                                                <--- 记住这个号码！
> set_network 4 ssid "Your SSID"
OK
> set_network 4 key_mgmt WPA-PSK
OK
> set_network 4 psk "Your Password"
OK
> enable_network 4
OK
> save_config                   <--------别忘了这个，否则重启之后网络配置可能丢失
OK