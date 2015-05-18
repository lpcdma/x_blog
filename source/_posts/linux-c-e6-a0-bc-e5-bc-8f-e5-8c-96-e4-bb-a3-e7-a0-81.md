title: "linux C 格式化代码"
date: 2014-04-14 14:20:57
tags:
id: 643
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">cat .indent.pro
-npro -kr -bad -bap -bbb -bbo -nbc -bli0 -ncdb -cdw\
 -ce -cli0 -cp2 -d0 -nbfda -di2 -i2 -ip5 -l120 -lp \
-pcs -nprs -npsl -saf -sai -saw -nsc -nsob -nss  -ts4 -ut

indent sample.c</pre>