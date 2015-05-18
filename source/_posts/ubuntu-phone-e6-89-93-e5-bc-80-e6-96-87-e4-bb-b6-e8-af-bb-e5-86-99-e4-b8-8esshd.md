title: "ubuntu phone打开文件读写与sshd"
date: 2015-05-16 04:31:41
tags:
id: 785
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">sudo mount -o remount,rw /</pre>
<pre class="brush:cpp">adb shell android-gadget-service enable ssh

adb shell mkdir /home/phablet/.ssh
adb push ~/.ssh/id_rsa.pub /home/phablet/.ssh/authorized_keys
adb shell chown -R phablet.phablet /home/phablet/.ssh
adb shell chmod 700 /home/phablet/.ssh
adb shell chmod 600 /home/phablet/.ssh/authorized_keys

adb shell ip addr show wlan0|grep inet
ssh phablet@lpcdma</pre>