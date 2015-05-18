title: "IDA Pro dump memory"
date: 2015-02-03 19:34:04
tags:
id: 767
comment: false
categories:
  - 编程学习
---

<pre class="brush:cpp">#include &lt;idc.idc&gt;
static main(){
	auto fp,xAddress;
	fp = fopen("d:\\fuck.dll","wb");
	for(xAddress=0xa1515000;xAddress&lt;(0xa1515000+0xccb);xAddress++){
		fputc(Byte(xAddress),fp);
	}
}</pre>