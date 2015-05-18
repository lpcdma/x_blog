title: "linux文件属性"
date: 2013-06-07 00:04:35
tags:
id: 180
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">root@kali:/tmp# ls -al
drwxrwxrwx  8 root       root       4096  6月  6 23:30 .
drwxr-xr-x 23 root       root       4096  5月 14 21:08 ..
drwxrwxrwt  2 root       root       4096  6月  5 02:14 .ICE-unix
drwx------  2 Debian-gdm Debian-gdm 4096  6月  5 02:14 pulse-25Mkiwt3U1DM</pre>
linux文件属性由此可以看出由10位构成。

文件类型/UID/GID/OTHER

1位/3位/3位/3位

d/rwx/rwx/rwx

1/111/111/111   //1代表启用，0代表-

第一位意义：

- 普通文件

d 目录

s 套接字

p 命名管道

c 设备文件

l 链接文件

第2-4，5-7，8-10意义：

每三位分别由8进制数表示

rwx == 111(八进制) = 7 (十进制)

- == 0 == 0

rw- == 110 == 6

r-- == 100 == 4

-rx == 011 == 3

r-x == 101 == 5

特殊的s,S,T,t属性表示SUID，SGID，SBIT
<pre class="brush:cpp">root@kali:/tmp# chmod 7666 test/
drwSrwSrwT  2 root       root       4096  6月  6 23:49 test

root@kali:/tmp# chmod 7777 test/
drwsrwsrwt  2 root       root       4096  6月  6 23:49 test

root@kali:/tmp# chmod 7555 test/
dr-sr-sr-t  2 root       root       4096  6月  6 23:49 test

root@kali:/tmp# chmod 6444 test/
dr-Sr-Sr--  2 root       root       4096  6月  6 23:49 test

root@kali:/tmp# chmod 5333 test/
d-ws-ws-wt  2 root       root       4096  6月  6 23:49 test

root@kali:/tmp# chmod 4444 test/
dr-Sr-Sr--  2 root       root       4096  6月  6 23:49 test

root@kali:/tmp# chmod 0000 test/
d--S--S---  2 root       root       4096  6月  6 23:49 test

postgres@kali:/tmp$ cd test/
bash: cd: test/: 权限不够
postgres@kali:/tmp$</pre>
去年看了小范同学用s属性提权。

&nbsp;

&nbsp;