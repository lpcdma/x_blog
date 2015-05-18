title: "android native调试记录"
date: 2014-03-29 16:52:54
tags:
id: 593
comment: false
categories:
  - 学习笔记
---

1.c/cpp,函数不通用。

2.ABI错误是因为android:minSdkVersion与android:targetSdkVersion版面不一致造成。

3.不用设置c/c++ general--&gt;paths and symbols,在第一次编译时会自动添加。

4.需要修改c/c++ general--&gt;Code Analysis--&gt;method cannot be resolved.不然编译成也不能运行。

5.调试时，要启动debug后，调用前下断点，才会触发，不然不会触发。

&nbsp;

太狗血了。