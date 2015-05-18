title: "os x批量替换"
date: 2014-10-29 00:57:45
tags:
id: 745
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">sed -i "" "s|3.3|3.5|g" `grep 3.3 -rl ./`</pre>