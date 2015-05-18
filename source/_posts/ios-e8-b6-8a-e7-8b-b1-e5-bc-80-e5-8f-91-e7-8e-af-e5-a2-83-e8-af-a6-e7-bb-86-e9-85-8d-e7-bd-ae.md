title: "ios越狱开发环境详细配置"
date: 2014-02-27 13:30:56
tags:
id: 528
comment: false
categories:
  - 学习笔记
---

针对ios7.x版本，使用iOSOpenDev进行开发。

一、Mac端

基础要求：

Mac OS X 10.8及以上（推荐Mac OS X 10.9）；

XCode5.0及以上；

Command Line Tools已安装；

1.安装MacPorts（此处是为了安装dpkg）

这里不可以安装homebrew，若已安装homebrew，请删除。因为homebrew中的dpkg版本过高，对deb包的结构进行了调整，导致打包时iphone中的dpkg无法解析。

[MacPorts官网](http://www.macports.org/)

2.安装dpkg

在终端中执行以下命令：

sudo port -v selfupdate (若第一次使用macports，需要先update)

sudo port install dpkg

3.安装theos

[theos介绍](http://iphonedevwiki.net/index.php/Theos/Getting_Started%E2%80%8E)

在终端中执行以下命令:

cd （保证处于$HOME下）

vim .bash_profile

添加以下内容：export $THEOS=/opt/theos

必须保证theos处于/opt/theos下，因为iOSOpenDev需要它处于这个位置。

sudo git clone https://github.com/DHowett/theos  $THEOS

4.安装iOSOpenDev

[iOSOpenDev官网](http://www.iosopendev.com/)

&nbsp;

5.XCode破解

[参考链接](http://bbs.weiphone.com/read-htm-tid-7056725.html)

&nbsp;

6.iPhone破解

[ios7越狱工具](http://evasi0n.com/)

&nbsp;

二、iPhone端

1.打开cydia，添加源repo.hackyouriphone.org，安装afc2add，appsync 7.x

（此步骤可跳过，非必须步骤）。

2.安装substrate，搜索cydia substate或mobile substrate

3.安装apt6.0 traditional（应该是这么拼的）。

4.安装MobileTerminal

打开mobileterminal，修改密码

执行以下命令

初始为mobile用户

passwd

原始密码为alpine（所有ios设备都一样）

su切换至root用户，密码为alpine。

修改密码即可。

这一段若看不明白，请搜索“linux修改用户密码”了解相关知识。

5.配置theos。

Mac端新建文件coredev.nl.list，填入 //已经无法获得

deb http://coredev.nl/cydia iphone main

新建文件howett.net.list，填入

deb http://nix.howett.net/theos ./

终端执行以下命令

cd 至文件所在路径

scp coredev.nl.list howett.net.list root@“此处为设备ip，无引号”:/etc/apt/sources.list.d

此处可能需要密码，填入上面你修改的密码即可。

若连接失败，如"Connection refused lost connection"，在cydia中搜索安装openssh即可。

&nbsp;

相关资料请搜索ssh，了解使用方法。

iPhone端打开MobileTerminal，

执行以下命令

su

apt-get update

apt-get install perl net.howett.theos

6.导入ssh key

此步骤在Mac端完成

Mac端使用ssh-key创建公钥，若嫌麻烦，简单方法为

Mac端执行ssh root@“设备ip,无引号”

会自动创建一个ssh公钥

完成后执行以下命令

iosod sshkey -h “设备ip,无引号”

iosod为iOSOpenDev内工具。

&nbsp;

三、创建测试工程

打开XCode，创建工程，会发现多了iOSOpenDev模板，找到Logos Tweak，创建一个工程即可。

substrate动态连接库需要手动添加，位于/opt/iOSOpenDev/lib中。

&nbsp;

四、一些开源工程

[http://iphonedevwiki.net/index.php/Open_Source_Projects](http://iphonedevwiki.net/index.php/Open_Source_Projects)

&nbsp;

五、ios7私有库头文件

[https://github.com/MP0w/iOS-Headers](https://github.com/MP0w/iOS-Headers)

[http://gitweb.saurik.com/](http://gitweb.saurik.com/)

转自：[http://my.oschina.net/SteveKou/blog/194913](http://my.oschina.net/SteveKou/blog/194913)