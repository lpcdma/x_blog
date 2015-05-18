title: "linux进程目录分析"
date: 2014-04-02 15:20:37
tags:
id: 621
comment: false
categories:
  - 学习笔记
---

dr-xr-xr-x 2 root root 0 Apr 2 02:30 attr 进程属性getattr获取，参考[http://blog.csdn.net/ysu108/article/details/7733996](http://blog.csdn.net/ysu108/article/details/7733996)
-rw-r--r-- 1 root root 0 Apr 2 02:30 autogroup 未知

-r-------- 1 root root 0 Apr 2 02:30 auxv  二进制文件，auxv_t结构数组，包含进程执行时传递给动态链接器的初始值。

-r--r--r-- 1 root root 0 Apr 2 02:30 cgroup 该文件描述了控制流程/任务所属的组。

--w------- 1 root root 0 Apr 2 02:30 clear_refs 清除页面引导。

-r--r--r-- 1 root root 0 Mar 12 06:27 cmdline 进程启动的命令行参数。

-rw-r--r-- 1 root root 0 Apr 2 02:30 comm 可执行文件名。

-rw-r--r-- 1 root root 0 Apr 2 02:30 coredump_filter 核心转存过滤设置。

-r--r--r-- 1 root root 0 Apr 2 02:30 cpuset 字面意思是cpu设置类容是/

lrwxrwxrwx 1 root root 0 Apr 2 02:30 cwd -&gt; /
-r-------- 1 root root 0 Apr 2 02:30 environ 用到的环境变量

lrwxrwxrwx 1 root root 0 Mar 12 06:27 exe -&gt; /sbin/init 指向可执行文件

dr-x------ 2 root root 0 Mar 18 13:39 fd 包含当前进程所有文件描述符的目录。

dr-x------ 2 root root 0 Apr 2 02:30 fdinfo 打开文件的信息

-r-------- 1 root root 0 Apr 2 02:30 io 显示I/O的解释范围。

-r--r--r-- 1 root root 0 Apr 2 02:30 latency 内容为延迟版本。

-r--r--r-- 1 root root 0 Mar 11 06:55 limits 一些数据的范围吧。

-rw-r--r-- 1 root root 0 Apr 2 02:30 loginuid 登录id，不知道是啥。

dr-x------ 2 root root 0 Apr 2 02:30 map_files 内存映射文件

-r--r--r-- 1 root root 0 Apr 2 02:30 maps 保存内存映象信息。

-rw------- 1 root root 0 Apr 2 02:30 mem 进程的内存被利用情况。

-r--r--r-- 1 root root 0 Apr 2 02:30 mountinfo 一些看不明白的挂载信息。

-r--r--r-- 1 root root 0 Apr 2 02:30 mounts 文件内容是当前进程加载的文件系统。

-r-------- 1 root root 0 Apr 2 02:30 mountstats 一些看不明白的挂载状态。

dr-xr-xr-x 5 root root 0 Apr 2 02:30 net 网络使用配置信息全在这里

dr-x--x--x 2 root root 0 Apr 2 02:30 ns

-rw-r--r-- 1 root root 0 Apr 2 02:30 oom_adj 特殊用途，保护某个进程不被杀死

-r--r--r-- 1 root root 0 Apr 2 02:30 oom_score
-rw-r--r-- 1 root root 0 Apr 2 02:30 oom_score_adj
-r--r--r-- 1 root root 0 Apr 2 02:30 pagemap
-r--r--r-- 1 root root 0 Apr 2 02:30 personality
lrwxrwxrwx 1 root root 0 Apr 2 02:30 root -&gt; /
-rw-r--r-- 1 root root 0 Apr 2 02:30 sched
-r--r--r-- 1 root root 0 Apr 2 02:30 schedstat
-r--r--r-- 1 root root 0 Apr 2 02:30 sessionid
-r--r--r-- 1 root root 0 Apr 2 02:30 smaps 是比maps更详细的内存映象信息。

-r--r--r-- 1 root root 0 Apr 2 02:30 stack 堆栈跟踪状态。

-r--r--r-- 1 root root 0 Mar 12 06:27 stat 进程状态。

-r--r--r-- 1 root root 0 Apr 2 02:30 statm 进程内存状态信息。

-r--r--r-- 1 root root 0 Mar 13 01:35 status 进程当前状态。

-r--r--r-- 1 root root 0 Apr 2 02:30 syscall 系统调用。不知道是啥。

dr-xr-xr-x 3 root root 0 Apr 2 02:30 task 进程的所有线程。

-r--r--r-- 1 root root 0 Apr 2 02:30 wchan 如果CONFIG_KALLSYMS被设置，将会着这里表现。

&nbsp;

参考：[https://www.kernel.org/doc/Documentation/filesystems/proc.txt](https://www.kernel.org/doc/Documentation/filesystems/proc.txt)

[http://man7.org/linux/man-pages/man5/proc.5.html](http://man7.org/linux/man-pages/man5/proc.5.html)