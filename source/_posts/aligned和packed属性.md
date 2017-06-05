title: aligned和packed属性
date: 2017-06-05 14:57:09
categories: 学习笔记
tags: [yunos,clang,gcc,packed,编译器]
---

### 问题描述
这是一个结构体buffer,用hexdump出来的数据.
![buffer dump](http://lpcdma.com/images/aligned_packed/1.png)
结构体的定义
![define struct](http://lpcdma.com/images/aligned_packed/2.png)
结构体值打印
![struct print](http://lpcdma.com/images/aligned_packed/2.png)
测试代码
![test code](http://lpcdma.com/images/aligned_packed/0.png)
正常情况下.accessFlags的值应该是0x00000001,
但是不知道为什么,
格式化这个buffer的时候直接跳过了 01 00 这两个字节

猜想是对齐的问题,但不知道怎么解.
后咨询了下某高人,搜索的到编译器相关结果:
```
struct space {
    u4 a;
    u2 b;
    u4 accessFlags;
}__attribute__((packed));
```
packed告诉编译器不对其.
此问题在.yunos 3.0.5上首次遇到.

### 参考文献
[GCC中的aligned和packed属性](http://blog.shengbin.me/posts/gcc-attribute-aligned-and-packed)