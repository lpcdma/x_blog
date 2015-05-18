title: "Makefile简单制作"
date: 2013-06-21 01:57:52
tags:
id: 276
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">root@kali:/tmp# mkdir fuck
root@kali:/tmp# ls
fuck  pulse-p89HKxxw6lu8  VMwareDnD  vmware-root  vmware-root-3614818879
root@kali:/tmp# cd fuck/
root@kali:/tmp/fuck# ls
root@kali:/tmp/fuck# vim fuck.c
root@kali:/tmp/fuck# autoscan 
root@kali:/tmp/fuck# ls
autoscan.log  configure.scan  fuck.c
root@kali:/tmp/fuck# mv configure.scan configure.in
root@kali:/tmp/fuck# vim configure.in 
root@kali:/tmp/fuck# ls
autoscan.log  configure.in  fuck.c
root@kali:/tmp/fuck# aclocal
root@kali:/tmp/fuck# ls
aclocal.m4  autom4te.cache  autoscan.log  configure.in  fuck.c
root@kali:/tmp/fuck# autoconf
root@kali:/tmp/fuck# ls
aclocal.m4  autom4te.cache  autoscan.log  configure  configure.in  fuck.c
root@kali:/tmp/fuck# autoheader 
root@kali:/tmp/fuck# ls
aclocal.m4      autoscan.log  configure     fuck.c
autom4te.cache  config.h.in   configure.in
root@kali:/tmp/fuck# vim Makefile.am
root@kali:/tmp/fuck# automake --add-missing
configure.in:8: installing `./install-sh'
configure.in:8: installing `./missing'
Makefile.am: installing `./depcomp'
root@kali:/tmp/fuck# ls
aclocal.m4      autoscan.log  configure     depcomp  install-sh   Makefile.in
autom4te.cache  config.h.in   configure.in  fuck.c   Makefile.am  missing
root@kali:/tmp/fuck# ./configure 
checking for a BSD-compatible install... /usr/bin/install -c
checking whether build environment is sane... yes
checking for a thread-safe mkdir -p... /bin/mkdir -p
checking for gawk... no
checking for mawk... mawk
checking whether make sets $(MAKE)... yes
checking for gcc... gcc
checking whether the C compiler works... yes
checking for C compiler default output file name... a.out
checking for suffix of executables... 
checking whether we are cross compiling... no
checking for suffix of object files... o
checking whether we are using the GNU C compiler... yes
checking whether gcc accepts -g... yes
checking for gcc option to accept ISO C89... none needed
checking for style of include used by make... GNU
checking dependency style of gcc... gcc3
configure: creating ./config.status
config.status: creating Makefile
config.status: creating config.h
config.status: executing depfiles commands
root@kali:/tmp/fuck# make
make  all-am
make[1]: Entering directory `/tmp/fuck'
gcc -DHAVE_CONFIG_H -I.     -g -O2 -MT fuck.o -MD -MP -MF .deps/fuck.Tpo -c -o fuck.o fuck.c
mv -f .deps/fuck.Tpo .deps/fuck.Po
gcc  -g -O2   -o fuck fuck.o  
make[1]: Leaving directory `/tmp/fuck'
root@kali:/tmp/fuck# ./fuck 
fuck!
root@kali:/tmp/fuck#</pre>
configure.in
<pre class="brush:cpp">#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT(fuck, 1.0, lpcdma@163.com)#程序信息设置
AC_CONFIG_SRCDIR([fuck.c])
AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE(fuck,1.0)#程序版本设置

# Checks for programs.
AC_PROG_CC

# Checks for libraries.

# Checks for header files.

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.

AC_OUTPUT([Makefile])#输出Makefile AC_OUTPUT(Makefile)</pre>
Makefile.am
<pre class="brush:cpp">AUTOMAKE_OPTIONS=foreign #automake选项提供了三种软件等级：foreign、gnu和gnits，让用户选择采用，默认等级为gnu。
bin_PROGRAMS=fuck
fuck_SOURCES=fuck.c   #fuck_SOURCES定义“fuck”这个执行程序所需要的原始文件。如果”fuck”这个程序是由多个原始文件所产生的，则必须把它所用到的所有原始文件都列出来，并用空格隔开。例如：若目标体“fuck”需要“fuck.c”、“test.c”、“fuck.h”三个依赖文件，则定义fuck_SOURCES=fuck.c test.c test.h。要注意的是，如果要定义多个执行文件，则对每个执行程序都要定义相应的file_SOURCES。</pre>
&nbsp;

&nbsp;

&nbsp;