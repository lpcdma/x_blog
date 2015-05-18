title: "centos ftp"
date: 2012-09-26 20:04:27
tags:
id: 115
comment: false
---

安装

yum install vsftpd

自启动

chkconfig --levels 235 vsftpd on

编辑配置文件

vi /etc/vsftpd/vsftpd.conf

修改常规安全项

anonymous_enable=NO

chroot_local_user=YES

启动服务

service vsftpd start

&nbsp;