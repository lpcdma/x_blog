title: "内存|hexedit文本内容转换成字符串"
date: 2014-04-10 23:21:35
tags:
id: 633
comment: false
categories:
  - 工具收集
---

<pre class="brush:py">import re

file = open('re.txt','r')
arrE = []
for line in file.readlines():
    arrH = re.findall(u'\s+(\w\w)',line)
    if len(arrH) &gt; 16:
        for i in range(16):
            arrE.append(arrH[i])
    else:
        for i in range(len(arrH)):
            arrE.append(arrH[i])
print len(arrE)
print arrE
print ''.join(arrE)</pre>