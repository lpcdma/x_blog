title: "ARM流水线"
date: 2014-03-18 23:17:24
tags:
id: 577
comment: false
categories:
  - 学习笔记
---

今天认认真真的读arm汇编，首先就遇到个问题，关于pc
<pre class="brush:cpp">00008390 &lt;call_gmon_start&gt;:
    8390:	e59f3014 	ldr	r3, [pc, #20]	; 83ac &lt;call_gmon_start+0x1c&gt;
    8394:	e59f2014 	ldr	r2, [pc, #20]	; 83b0 &lt;call_gmon_start+0x20&gt;
    8398:	e08f3003 	add	r3, pc, r3
    839c:	e7933002 	ldr	r3, [r3, r2]
    83a0:	e3530000 	cmp	r3, #0
    83a4:	012fff1e 	bxeq	lr
    83a8:	eaffffe0 	b	8330 &lt;_init+0x38&gt;
    83ac:	00008744 	.word	0x00008744
    83b0:	00000020 	.word	0x00000020</pre>
在8390出按照win32汇编常识，感到很奇怪，为什么pc+20会到83ac，百思不得其解，不应该啊。

google告诉了我答案。首先看了下我的cpu版本。
<pre class="brush:cpp">pi@raspberrypi:~$ cat /proc/cpuinfo
processor	: 0
model name	: ARMv6-compatible processor rev 7 (v6l)
BogoMIPS	: 2.00
Features	: swp half thumb fastmult vfp edsp java tls 
CPU implementer	: 0x41
CPU architecture: 7
CPU variant	: 0x0
CPU part	: 0xb76
CPU revision	: 7

Hardware	: BCM2708
Revision	: 000e
Serial		: 00000000902b9619</pre>
CPU是ARM7，通过google我知道了流水线这个东西，哎，android软件逆向分析技术那本书白看了。

对于流水线这个东西有点印象。

**AMR7三级流水线**

[![arm_3_tie_pipeline](http://lpcdma.com/wp-content/uploads/2014/03/arm_3_tie_pipeline-300x156.jpg)](http://lpcdma.com/wp-content/uploads/2014/03/arm_3_tie_pipeline.jpg)

[![arm_3_tie_pipeline_example](http://lpcdma.com/wp-content/uploads/2014/03/arm_3_tie_pipeline_example-300x171.jpg)](http://lpcdma.com/wp-content/uploads/2014/03/arm_3_tie_pipeline_example.jpg)

从上图，其实很容易看出，第一条指令：
add r0, r1,$5
执行的时候，此时PC已经指向第三条指令：
cmp r2,#3
的地址了，所以，是PC=PC+8.

关于ARM9,5级流水线依然是PC=PC+8

参考：http://www.crifan.com/files/doc/docbook/uboot_starts_analysis/release/htmls/why_arm7_pc_8.html

&nbsp;

&nbsp;