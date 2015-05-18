title: "valgrind"
date: 2014-04-14 15:02:33
tags:
id: 648
comment: false
categories:
  - 工具收集
---

## Valgrind 概述

### 体系结构

Valgrind是一套Linux下，开放源代码（GPL V2）的仿真调试工具的集合。Valgrind由内核（core）以及基于内核的其他调试工具组成。内核类似于一个框架（framework），它模拟了一个CPU环境，并提供服务给其他工具；而其他工具则类似于插件 (plug-in)，利用内核提供的服务完成各种特定的内存调试任务。

[http://valgrind.org/docs/manual/mc-manual.html](http://valgrind.org/docs/manual/mc-manual.html)

[https://www.ibm.com/developerworks/cn/linux/l-cn-valgrind/](https://www.ibm.com/developerworks/cn/linux/l-cn-valgrind/)