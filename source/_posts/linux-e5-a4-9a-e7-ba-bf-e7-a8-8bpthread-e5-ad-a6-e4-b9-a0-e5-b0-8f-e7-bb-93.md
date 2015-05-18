title: "Linux多线程Pthread学习小结"
date: 2013-11-04 13:58:20
tags:
id: 377
comment: false
categories:
  - 学习笔记
---

**简介**

POSIX thread 简称为pthread，Posix线程是一个**[POSIX](http://baike.baidu.com/view/209573.htm)**标准线程.该标准定义内部API创建和操纵线程.

&nbsp;

**作用**

线程库实行了POSIX线程标准通常称为pthreads.pthreads是最常用的POSIX系统如Linux和Unix,而微软Windowsimplementations同时存在.举例来说,pthreads-w32可支持MIDP的pthread

Pthreads定义了一套 C程序语言类型、函数与常量，它以 pthread.h 头文件和一个线程库实现。

&nbsp;

**数据类型**

pthread_t：线程句柄

pthread_attr_t：线程属性

线程操纵函数（简介起见，省略参数）

pthread_create()：创建一个线程

pthread_exit()：终止当前线程

pthread_cancel()：中断另外一个线程的运行

pthread_join()：阻塞当前的线程，直到另外一个线程运行结束

pthread_attr_init()：初始化线程的属性

pthread_attr_setdetachstate()：设置脱离状态的属性（决定这个线程在终止时是否可以被结合）

pthread_attr_getdetachstate()：获取脱离状态的属性

pthread_attr_destroy()：删除线程的属性

pthread_kill()：向线程发送一个信号

&nbsp;

**同步函数**

用于 mutex 和条件变量

pthread_mutex_init() 初始化互斥锁

pthread_mutex_destroy() 删除互斥锁

pthread_mutex_lock()：占有互斥锁（阻塞操作）

pthread_mutex_trylock()：试图占有互斥锁（不阻塞操作）。当互斥锁空闲时将占有该锁；否则立即返回

pthread_mutex_unlock(): 释放互斥锁

pthread_cond_init()：初始化条件变量

pthread_cond_destroy()：销毁条件变量

pthread_cond_wait(): 等待条件变量的特殊条件发生

pthread_cond_signal(): 唤醒第一个调用pthread_cond_wait()而进入睡眠的线程

Thread-local storage（或者以Pthreads术语，称作 _线程特有数据_）:

pthread_key_create(): 分配用于标识进程中线程特定数据的键

pthread_setspecific(): 为指定线程特定数据键设置线程特定绑定

pthread_getspecific(): 获取调用线程的键绑定，并将该绑定存储在 value 指向的位置中

pthread_key_delete(): 销毁现有线程特定数据键

&nbsp;

**与一起工作的工具函数**

pthread_equal(): 对两个线程的线程标识号进行比较

pthread_detach(): 分离线程

pthread_self(): 查询线程自身线程标识号

&nbsp;

**详细请参见：**

Linux多线程pthread：     [http://blog.csdn.net/Sunboy_2050/archive/2010/10/04/5920936.aspx](http://blog.csdn.net/Sunboy_2050/archive/2010/10/04/5920936.aspx)

Pthread多线程学习小结： [http://blog.csdn.net/Sunboy_2050/archive/2010/10/04/5921003.aspx](http://blog.csdn.net/Sunboy_2050/archive/2010/10/04/5921003.aspx)

来自：[http://blog.csdn.net/ithomer/article/details/6063067](http://blog.csdn.net/ithomer/article/details/6063067)