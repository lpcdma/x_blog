title: "SWF文件XSS总结"
date: 2013-10-29 11:27:18
tags:
id: 358
comment: false
categories:
  - 学习笔记
---

工具:Flash Decompiler Trillix|UltraEdit|tommsearch

查找漏洞关键词：'ExternalInterface.call', 'getURL', 'navigateToURL', 'javascript:',‘loadMovie'.'loadVariables‘,'loadMovieNum','loadScrollContent','loadSound','NetStream.play','htmlText'等。