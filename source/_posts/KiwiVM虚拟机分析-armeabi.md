title: KiwiVM虚拟机分析(armeabi)
date: 2017-05-19 16:34:51
categories: 学习笔记
tags: [arm,虚拟机,代码保护]
---
*本文是初次分析,简单记录部分分析结果.*
## BL指令跳转计算
```
.text:00034608 21 3B 01 EB                 BL      sub_83294
```
**0x83294 = 0x13b21*4+(0x34608+8)**
## 笔记
据说这个demo是一个什么算法，拿到跑了下完全茫然,不知从何处下手。
经过几分钟的摸索,我看到一个函数表:
```
.data:000C092C AC 45 03 00 off_C092C       DCD sub_345AC           ; DATA XREF: .data:off_C3408
.data:000C0930 E0 45 03 00                 DCD sub_345E0
.data:000C0934 14 46 03 00                 DCD sub_34614
...
...
.data:000C33FC 3C 72 05 00                 DCD sub_5723C
.data:000C3400 70 72 05 00                 DCD sub_57270
.data:000C3404 00 00 00 00                 ALIGN 8
.data:000C3408 2C 09 0C 00 off_C3408       DCD off_C092C           ; DATA XREF: sub_26310+31C
.data:000C3408                                                     ; sub_26310+70C
```
这个表统计了下,有2741个函数,目前看不出是不是他们说的handler,暂且在我这里就叫函数.
这些函数是有特征的:
```
.text:000345AC 00 48 2D E9                 STMFD   SP!, {R11,LR}
.text:000345B0 0D B0 A0 E1                 MOV     R11, SP
.text:000345B4 10 D0 4D E2                 SUB     SP, SP, #0x10
.text:000345B8 01 20 A0 E1                 MOV     R2, R1
.text:000345BC 00 30 A0 E1                 MOV     R3, R0
.text:000345C0 04 00 0B E5                 STR     R0, [R11,#var_4]
.text:000345C4 08 10 8D E5                 STR     R1, [SP,#0x10+var_8]
.text:000345C8 04 00 1B E5                 LDR     R0, [R11,#var_4]
.text:000345CC 04 20 8D E5                 STR     R2, [SP,#0x10+var_C]
.text:000345D0 00 30 8D E5                 STR     R3, [SP,#0x10+var_10]
.text:000345D4 55 11 01 EB                 BL      sub_78B30
.text:000345D8 0B D0 A0 E1                 MOV     SP, R11
.text:000345DC 00 88 BD E8                 LDMFD   SP!, {R11,PC}
...
...
.text:00078B30             sub_78B30                               ; CODE XREF: sub_345AC+28p
.text:00078B30
.text:00078B30             var_10          = -0x10
.text:00078B30             var_C           = -0xC
.text:00078B30             var_8           = -8
.text:00078B30             var_4           = -4
.text:00078B30
.text:00078B30 00 48 2D E9                 STMFD   SP!, {R11,LR}
.text:00078B34 0D B0 A0 E1                 MOV     R11, SP
.text:00078B38 10 D0 4D E2                 SUB     SP, SP, #0x10
.text:00078B3C 01 20 A0 E1                 MOV     R2, R1
.text:00078B40 00 30 A0 E1                 MOV     R3, R0
.text:00078B44 04 00 0B E5                 STR     R0, [R11,#var_4]
.text:00078B48 08 10 8D E5                 STR     R1, [SP,#0x10+var_8]
.text:00078B4C 04 00 1B E5                 LDR     R0, [R11,#var_4]
.text:00078B50 20 10 9F E5                 LDR     R1, =(aKvm3333 - 0x78B5C)
.text:00078B54 01 10 8F E0                 ADD     R1, PC, R1      ; "KVM$3333$"
.text:00078B58 E2 C0 A0 E3+                MOV     R12, #0x1BE2
.text:00078B60 04 20 8D E5                 STR     R2, [SP,#0x10+var_C]
.text:00078B64 0C 20 A0 E1                 MOV     R2, R12
.text:00078B68 00 30 8D E5                 STR     R3, [SP,#0x10+var_10]
.text:00078B6C 87 AC FF EB                 BL      sub_63D90
.text:00078B70 0B D0 A0 E1                 MOV     SP, R11
.text:00078B74 00 88 BD E8                 LDMFD   SP!, {R11,PC}
```
sub_63D90这个函数f5后是return 1;
统计发现有2300+这样的结构.
这部分代码也看不出什么东西，先把重点放到剩下的400多个函数上。
经过最终的过滤。我只发现4个函数有内容。
分析过滤代码:
```
import idc

def my_range(start, end, step):
    while start <= end:
        yield start
        start += step
handler_count = 0
handler_arr = []
for x in my_range(0xC092C, 0xC3408, 0x04):
	bladdr = idc.Dword(x) + (0x04 * 10)
	blcode = idc.Dword(bladdr);
	#print hex(blcode)
	blcode = blcode & 0x00ffffff
	#print hex(blcode)
	blfunc = blcode * 4 + bladdr + 8;
	#print hex(blfunc)
	target_addr = blfunc + 0x3c;
	for x in my_range(blfunc, target_addr, 0x04):
		xx = idc.Dword(x) & 0xff000000
		if xx == 0xeb000000:
			target_addr = x
			break
	blcode0 = idc.Dword(target_addr)
	if not (blcode0 & 0xff000000) == 0xeb000000:
		# print '++++++++++++++'
		# print hex(blfunc)
		continue
	#print hex(blcode0)
	blcode0 = blcode0 & 0x00ffffff
	#print hex(blcode0)
	blfunc0 = blcode0 * 4 + target_addr + 8;
	#print hex(blfunc0)
	blfunc0 &= 0x000fffff
	# if not 0x4063d90 == blfunc0:
	if blfunc0 not in handler_arr:
		handler_arr.append(blfunc0)
		print '------------------'
		print hex(target_addr)
		print hex(blfunc0)
# 	if idc.Dword(x + 0x04) - idc.Dword(x) == 0x34:
# 		handler_count += 1
# 		print hex(idc.Dword(x))
handler_count = len(handler_arr)
print handler_count

# start_addr = 0x0007C530
# target_addr = start_addr + 0x3c
# for x in my_range(start_addr, target_addr, 0x04):
# 	xx = idc.Dword(x) & 0xff000000
# 	if xx == 0xeb000000:
# 		target_addr = x
# 		print hex(x)
# 		break
```