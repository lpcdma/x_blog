title: "Jenkins执行系统命令"
date: 2013-12-04 13:32:19
tags:
id: 425
comment: false
categories:
  - 学习笔记
---

百度了一下Jenkins不知道是做什么的。大概理解是做类似云编译的一个东西，可以在上面编译东西。

就是可以运行程序。

Groovy 是Java平台上设计的面向对象编程语言。这门动态语言拥有类似Python、Ruby 和 Smalltalk 中的一些特性，可以作为Java 平台的脚本语言使用。 Groovy 的语法与Java 非常相似，以至于多数的Java 代码也是正确的Groovy 代码。Groovy 代码动态的被编译器转换成Java 字节码。由于其运行在JVM 上的特性，Groovy 可以使用其他Java 语言编写的库。

运用Groovy就可以直接执行系统命令

Process p = "cmd /c whoami".execute()
println "${p.text}"
Process p="cat /etc/passwd".execute()
println "${p.text}"

这看上去就像是脚本语言。

也可通一个Grosh包[http://groovy.codehaus.org/Groosh](http://groovy.codehaus.org/Groosh)

执行系统命令，但是看上去比较复杂。