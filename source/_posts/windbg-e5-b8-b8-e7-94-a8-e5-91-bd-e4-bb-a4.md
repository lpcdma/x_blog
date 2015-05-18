title: "windbg 常用命令"
date: 2013-12-28 20:15:06
tags:
id: 460
comment: false
categories:
  - 学习笔记
---

# 一、基础命令

程序重在编程思维，难在程序调试，写出了程序还不行，还必须进行调试，证明结果。

WinDbg是微软开发的一款强大无比的调试器。利用它我们可以进行内核双机调试。

在调试程序之前我们要掌握如何来使用Windbg调试器，也就是掌握Windbg命令。

### .sympath命令

设置符号表路径。

&nbsp;

### .reload 命令

此命令主要用于加载符号表。

.reload /f   //重新装载模块

.reload /i   //强制加载mismatched symbol

&nbsp;

### U命令

这个命令主要用于反汇编某个地址，其后面可以跟函数名和地址。

### db/dw/dd/dq/dD/df命令

这四个命令主要用于查看某地址所储存的数据。他们的不同在于所显示的数据长度。

db显示一字节的长度。

dw显示两字节的长度。

dd显示四字节的长度。

dq显示八字节的长度。

dD 显示double实数(8字节的长度)。

df 显示float实数(4字节的长度)。

### da/du/ds/dS命令

da 显示asscii值

du 显示unicode值

ds 显示ANI_STRING值

dS显示UNICODE_STRING的值

### eb/ew/ed/eq命令

eb address value 在address 这个地址写入一个字节value

ew address value 在address 这个地址写入两字节value

ed address value 在address 这个地址写入四字节字节value

eq address value 在address 这个地址写入八字节字节value

&nbsp;

## 二、对象相关命令

### dt命令

dt命令主要用于查看结构体。

### lm 命令

Lm  列出模块。

lm vm 模块名 查看模块详细信息。

&nbsp;

### !process  命令

!process 0 0//列出系统进程信息

!process 0 0 进程名  //列出该进程的信息

!process 0 1 进程名  //列出该进程更加的信息

!process 0 7  进程名 //列出该进程的详细信息，包括线程的

&nbsp;

### .process 命令

.process EPROCESS  //切入该进程中

### !object  命令

!object 地址  //显示该地址的对象信息。

&nbsp;

&nbsp;

&nbsp;

## 三、断点命令

### bp/ba命令

bp命令是通过向指定地址插入int 3 指令来完成的，这种方式容易被发现。

bp address  在地址address插入断点。

ba命令是是硬件断点命令，通过设置cpu的dx寄存器来拦截线程。

ba access size 地址//access 是访问的方式，比如 e (执行)，r (读/写)，w (写) ，size是监控访问的位置的大小，以字节为单位。值为 1、2或4，在64位机器上还可以是8。

&nbsp;

### bd/be/bc命令

bd 断点号 //此命令是关闭断点号所对应的断点 。

be 断点号 //此命令是开启断点号所对应的断点 。

bc * 去除所有断点。

bu TestDriver!DriverEntry //给驱动入口下断点

&nbsp;

## 四、其他命令

### x命令

X命令用来模糊查询。例如可以这样查看SSDT表的地址:

x nt!kes*des*table*

0: kd&gt;  x nt!kes*des*table*

8055d6c0 nt!KeServiceDescriptorTableShadow = &lt;no type information&gt;

8055d700 nt!KeServiceDescriptorTable = &lt;no type information&gt;

### dds命令

dds 地址 //此命令用来解析某连续地址的函数名。

0: kd&gt;  dd nt!KeServiceDescriptorTable  L1

8055d700  80505450 00000000 0000011c 805058c4

0: kd&gt; dds 80505450 l3

80505450  805a5614 nt!NtAcceptConnectPort

80505454  805f1adc nt!NtAccessCheck

80505458  805f5312 nt!NtAccessCheckAndAuditAlarm

&nbsp;

lm n 显示驱动模块信息

kd&gt; lm n
start end module name
00400000 005f9200 nt_400000 ntkrnlpa.exe
7c920000 7c9b6000 ntdll ntdll.dll
804d8000 806d1200 nt ntkrnlpa.exe
806d2000 806f2300 hal halaacpi.dll

kd&gt; !dh -a ede65000  //显示PE信息

File Type: EXECUTABLE IMAGE
FILE HEADER VALUES
14C machine (i386)
8 number of sections
4ED9EB19 time date stamp Sat Dec 03 17:25:45 2011

0 file pointer to symbol table