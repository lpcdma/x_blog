title: "centos install mosh"
date: 2012-10-13 00:06:46
tags:
id: 121
comment: false
---

官方网站：[http://mosh.mit.edu/](http://mosh.mit.edu/)

$ git clone [https://github.com/keithw/mosh](https://github.com/keithw/mosh)

$ cd mosh $ ./autogen.sh

$ ./configure

$ make # make install

$ wget [ftp://ftp.muug.mb.ca/mirror/centos/6.3/os/i386/Packages/perl-IO-Tty-1.08-4.el6.i686.rpm](ftp://ftp.muug.mb.ca/mirror/centos/6.3/os/i386/Packages/perl-IO-Tty-1.08-4.el6.i686.rpm)

# rpm -ivh perl-IO-Tty-1.08-4.el6.i686.rpm

# rpm -Uvh [http://dl.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm](http://dl.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm)

# yum install log4cpp-1.0-4.el5

#yum install protobuf

#yum install protobuf-compiler

# yum list installed | grep protobuf

# yum install protobuf-devel

&nbsp;

&nbsp;

&nbsp;

&nbsp;