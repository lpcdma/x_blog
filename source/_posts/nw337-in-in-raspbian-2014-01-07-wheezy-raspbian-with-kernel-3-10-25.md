title: "NW337 in in Raspbian 2014-01-07-wheezy-raspbian with kernel 3.10.25+"
date: 2014-02-16 16:35:54
tags:
id: 501
comment: false
categories:
  - 工具收集
---

Thanks to [http://www.mendrugox.net](http://www.mendrugox.net/) I’ve been able to use the NW337 in the latest Raspbian 2014-01-07-wheezy-raspbian with kernel 3.10.25+. This was the procedure:
<pre>wget -O 8188eu_31024_614.zip http://www.mendrugox.net/downloads/14
unzip 8188eu_31024_614.zip
sudo mv 8188eu.ko /lib/modules/`uname -r`/kernel/drivers/net/wireless
sudo chown root:root /lib/modules/`uname -r`/kernel/drivers/net/wireless/8188eu.ko
sudo mv rtl8188eufw.bin /lib/firmware/rtlwifi/
sudo chown root:root /lib/firmware/rtlwifi/rtl8188eufw.bin
sudo depmod -a
sudo modprobe 8188eu</pre>
Edit `/etc/netwok/interfaces` like this and add your networks SSID and password:
<pre>auto lo

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0
auto wlan0

iface wlan0 inet dhcp
        wpa-ssid "**YOUR-NETWORK-SSID**"
        wpa-psk "**YOUR-PASSWORD**"</pre>
Then reboot your Raspberry Pi and it should connect to your wifi network automatically. You should see the`wlan0` device when running `ifconfig`:
<pre>$ ifconfig
wlan0     Link encap:Ethernet  HWaddr 08:10:78:26:70:4f
          inet addr:192.168.2.119  Bcast:192.168.2.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:96 errors:0 dropped:4 overruns:0 frame:0
          TX packets:21 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:18513 (18.0 KiB)  TX bytes:2746 (2.6 KiB)</pre>