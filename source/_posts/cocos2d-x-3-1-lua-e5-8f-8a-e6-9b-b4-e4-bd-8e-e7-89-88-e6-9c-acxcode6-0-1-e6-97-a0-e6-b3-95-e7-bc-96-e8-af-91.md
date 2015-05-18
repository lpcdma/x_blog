title: "cocos2d-x-3.1 lua及更低版本xcode6.0.1无法编译"
date: 2014-09-23 01:19:33
tags:
id: 736
comment: false
categories:
  - 学习笔记
---

解决办法：

复制cocos2d-x-3.2版本中cocos2d-x-3.2/external/lua/lua/prebuilt/ios/liblua.a，

cocos2d-x-3.2/external/curl/prebuilt/ios/libcurl.a到3.1对应目录下。

可解决：

Undefined symbols for architecture x86_64:
"_opendir$INODE64", referenced from:
_OPENSSL_DIR_read in libcocos2dx iOS.a(o_dir.o)
"_readdir$INODE64", referenced from:
_OPENSSL_DIR_read in libcocos2dx iOS.a(o_dir.o)
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)