title: "Eclipse 写Javascript卡死问题"
date: 2014-01-23 14:38:39
tags:
id: 495
comment: false
categories:
  - 学习笔记
---

打开项目的.project文件，将
&lt;buildCommand&gt;
&lt;name&gt;org.eclipse.wst.jsdt.core.javascriptValidator&lt;/name&gt;
&lt;arguments&gt;
&lt;/arguments&gt;
&lt;/buildCommand&gt;

&lt;nature&gt;org.eclipse.wst.jsdt.core.jsNature&lt;/nature&gt;

删除