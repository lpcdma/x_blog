title: "linux配置设备权限"
date: 2014-03-04 16:42:52
tags:
id: 551
comment: false
categories:
  - 学习笔记
---

新建
sudo vim /etc/udev/rules.d/51-android.rules
查看USB设备
$ lsusb
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 002 Device 002: ID 0e0f:0003 VMware, Inc. Virtual Mouse
Bus 002 Device 003: ID 0e0f:0002 VMware, Inc. Virtual USB Hub
Bus 001 Device 010: ID 12d1:1038 Huawei Technologies Co., Ltd. Ideos (debug mode)
得到ID写入51-android.rules格式如下
SUBSYSTEM=="usb", ATTR{idVendor}=="12d1", ATTR{idProduct}=="1038", MODE="0600", OWNER="santoku"