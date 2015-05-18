title: "ARM ShellCode编写及远程调用"
date: 2014-03-18 15:29:25
tags:
id: 572
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">D:\&gt;D:\Android\android-ndk-r9b\toolchains\arm-linux-androideabi-4.6\prebuilt\win
dows\arm-linux-androideabi\bin\as -mthumb -o thumb.o thumb.s
thumb.s: Assembler messages:
thumb.s:0: Warning: end of file not at end of a line; newline inserted

D:\&gt;D:\Android\android-ndk-r9b\toolchains\arm-linux-androideabi-4.6\prebuilt\win
dows\arm-linux-androideabi\bin\ld -mthumb -o thumb thumb.o

D:\&gt;type thumb.s
.section .text
.global _start

_start:

    .code 32
    # Thumb-Mode on
    add     r6, pc, #1
    bx  r6

    .code   16
    # _write()
    mov     r2, #16
    mov r1, pc
    add r1, #12
    mov     r0, $0x1
    mov     r7, $0x4
    svc     0

    # _exit()
    sub r0, r0, r0
    mov     r7, $0x1
    svc 0

.ascii "lpcdma.com\n"

D:\&gt;adb push thumb /data/local/tmp/thumb
1554 KB/s (4776 bytes in 0.003s)

D:\&gt;adb shell chmod 777 /data/local/tmp/thumb

D:\&gt;adb shell /data/local/tmp/thumb
lpcdma.com

IDA

.text:00008074 01 60 8F E2                             ADR     R6, (loc_807C+1)
.text:00008078 16 FF 2F E1                             BX      R6 ; loc_807C
.text:0000807C                         ; ---------------------------------------------------------------------------
.text:0000807C                                         CODE16
.text:0000807C
.text:0000807C                         loc_807C                                ;
.text:0000807C                                                                 ;
.text:0000807C 10 22                                   MOVS    R2, #0x10
.text:0000807E 79 46                                   MOV     R1, PC
.text:00008080 0C 31                                   ADDS    R1, #0xC
.text:00008082 01 20                                   MOVS    R0, #1
.text:00008084 04 27                                   MOVS    R7, #4
.text:00008086 00 DF                                   SVC     0
.text:00008088 00 1A                                   SUBS    R0, R0, R0
.text:0000808A 01 27                                   MOVS    R7, #1
.text:0000808C 00 DF                                   SVC     0
.text:0000808E 6C 70                                   STRB    R4, [R5,#1]
.text:00008090 63 64                                   STR     R3, [R4,#0x44]
.text:00008092 6D 61                                   STR     R5, [R5,#0x14]
.text:00008094 2E 63                                   STR     R6, [R5,#0x30]
.text:00008096 6F 6D                                   LDR     R7, [R5,#0x54]
.text:00008098 0A 00                                   MOVS    R2, R1

shellcode

\x01\x60\x8F\xE2\x16\xFF\x2F\xE1\x10\x22\x79\x46\x0C\x31\x01\x20\x04\x27\x00\xDF\x00\x1A\x01\x27\x00\xDF\x6C\x70\x63\x64\x6D\x61\x2E\x63\x6F\x6D\x0A\x00

Android.mk

LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)
LOCAL_LDFLAGS += -z execstack -fno-stack-protector #将堆栈设为可执行
LOCAL_MODULE    := RemoteShellcodeLauncher
LOCAL_SRC_FILES := RemoteShellcodeLauncher.c

include $(BUILD_EXECUTABLE)
#include $(BUILD_SHARED_LIBRARY)

执行shellcode

D:\libs\armeabi&gt;adb push RemoteShellcodeLauncher /data/local/tmp/Reshell
2318 KB/s (9496 bytes in 0.004s)

D:\libs\armeabi&gt;adb shell chmod 777 /data/local/tmp/Reshell

D:\libs\armeabi&gt;adb shell /data/local/tmp/Reshell 1111
Shellcode size: 18
Executing ...
lpcdma.com</pre>
参考:[http://shell-storm.org/blog/Shellcode-On-ARM-Architecture/](http://shell-storm.org/blog/Shellcode-On-ARM-Architecture/)