title: elf函数提取及利用
date: 2017-05-31 14:30:47
categories: 学习笔记
tags: [elf]
---

验证并测试elf文件函数提取后用于同架构的其他可执行文件的可行性.

t.c
```
int foo() { return 42; }
int main() { return foo(); }
```
提取编译后的foo函数体.
```
[root@localhost tmp]# gcc t.c
[root@localhost tmp]# gdb -q ./a.out
Reading symbols from /tmp/a.out...(no debugging symbols found)...done.
(gdb) disas/r foo
Dump of assembler code for function foo:
   0x00000000004004ed <+0>:	55	push   %rbp
   0x00000000004004ee <+1>:	48 89 e5	mov    %rsp,%rbp
   0x00000000004004f1 <+4>:	b8 2a 00 00 00	mov    $0x2a,%eax
   0x00000000004004f6 <+9>:	5d	pop    %rbp
   0x00000000004004f7 <+10>:	c3	retq   
End of assembler dump.
(gdb) dump memory foo.o 0x00000000004004ed 0x00000000004004f7+1
(gdb) quit
[root@localhost tmp]# od -tx1 foo.o
0000000 55 48 89 e5 b8 2a 00 00 00 5d c3
0000013
```
生成.s文件
```
{ echo -e ".globl\tfoo\nfoo:\n\t.byte\t\c";
  od -tx1 foo.o | cut -c9- |
    sed -e '/^ *$/d' -e 's/^/0x/' -e 's/ /,0x/g'; } > foo1.s
```
将提前的函数体编译到foo.c中
```
#include "foo.h"
#include <stdio.h>
int main() { int x = foo(); printf("====%d", x);}
```
foo.h
```
int foo();
```
编译流程
```
[root@localhost tmp]# gcc -c foo1.s -o foo1.o
[root@localhost tmp]# ar rcs libfoo.a foo1.o
[root@localhost tmp]# gcc foo.c -L . -lfoo
[root@localhost tmp]# ./a.out 
====42
```