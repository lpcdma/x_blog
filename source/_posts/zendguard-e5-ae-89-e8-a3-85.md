title: "ZendGuard安装"
date: 2013-06-21 02:06:24
tags:
id: 278
comment: false
categories:
  - 编程学习
---

xmapp安装失败，linux下安装成功。

这个貌似是php非开源程序的加密的东东。

[下载地址](http://www.zend.com/en/products/guard/downloads)

php.ini添加
<pre>zend_extension=/etc/php5/conf.d/ZendGuardLoader.so
zend_loader.enable=1
zend_loader.disable_licensing=0
zend_loader.obfuscation_level_support=0</pre>
重启服务

service apache2 restart

&nbsp;