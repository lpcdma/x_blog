title: ds-5调试cocos2dx
date: 2015-09-18 10:53:05
tags:
---

由于对命令行情有独钟，于是乎也想试下ndk-gdb命名行调试,使用gdb无源码调试较多，使用同样的方法用于有源码调试非常麻烦，一堆错误。eclipse+adt+cdt或者exlipse+ds-5有源码调试都非常方便。

经过测试主要问题在于选对版本，例如r9d应该对应4.4.x版本，r10e对应5.1.1版本，否则会遇到一堆奇奇怪怪的错误。

主要是为了命令行调试cocos2dx,在解决一些平台兼容性问题是可能用到，cocos2dx在win32和osx上调试简单方便。修改android:minSdkVersion和android-19保持一致(这个不一致有时报错有时保持，一致没有遇到过报错情况),ABI可以在.mk中选择不影响。
设置NDK_MODULE_PATH,对应的值在build-cfg.json,如果有些找不到的在报错的Android.mk中查找cocos命令编译会设置这个值重启cmd或者shell时需要设一下。

```
set NDK_MODULE_PATH=F:\MX_L\ProRes\scut\quick-cocos2d-x\;F:\MX_L\ProRes\scut\quick-cocos2d-x\cocos\;F:\MX_L\ProRes\scut\quick-cocos2d-x\cocos\scripting\;F:\MX_L\ProRes\scut\quick-cocos2d-x\quick\player\Classes\;F:\MX_L\ProRes\scut\quick-cocos2d-x\external;
export NDK_MODULE_PATH=F:\MX_L\ProRes\scut\quick-cocos2d-x\;F:\MX_L\ProRes\scut\quick-cocos2d-x\cocos\;F:\MX_L\ProRes\scut\quick-cocos2d-x\cocos\scripting\;F:\MX_L\ProRes\scut\quick-cocos2d-x\quick\player\Classes\;F:\MX_L\ProRes\scut\quick-cocos2d-x\external;
```
不使用jdb调试

```
D:\Android\android-ndk-r10e\ndk-gdb-py.cmd --force --nowait --verbose --launch=org.cocos2dx.cpp.AppActivity
```

使用jdb

```
$ndk-gdb-py --launch=org.cocos2dx.cpp.AppActivity
```

调试

```
b fuck.cpp:38
b shit.cpp:38
```
使用网上的方法，使用file obj/local/armeabi/libcocos2dcpp.so会出现这种错误cocos2dx use ndk-gdb debug error，原因是重新load动态链接库后内存中的地址也发生了变化，所以无法访问。跟入gdb7.7源码的frv_convert_from_func_ptr_addr后发现根本没法跟，没有发现那里有调用。
不能创造价值的技术有什么用呢？