title: "lua三元表达式"
date: 2014-07-20 20:43:43
tags:
id: 687
comment: false
categories:
  - 学习笔记
---

C语言中的三元运算符

a ? b : c

在Lua中可以这样实现：

(a and b) or c