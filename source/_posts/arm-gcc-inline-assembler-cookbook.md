title: "ARM GCC Inline Assembler Cookbook"
date: 2013-12-12 13:42:41
tags:
id: 440
comment: false
categories:
  - 学习笔记
---

C需要嵌入ARM汇编时

这么做
<pre class="brush:cpp">#include &lt;stdio.h&gt;

unsigned int get_sp(void){
	asm(
	"mov r0,sp\n\t"
	"bx lr");
}
int main(){
	printf("Stack pointer (sp):0x%x\n",get_sp());
}</pre>

参考文献：《ARM GCC Inline Assembler Cookbook》
http://www.ethernut.de/en/documents/arm-inline-asm.html