title: "android ndk 编译方式"
date: 2014-03-31 23:33:43
tags:
id: 614
comment: false
categories:
  - 学习笔记
---

**1\. 编译可执行程序**

&nbsp;

LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)
LOCAL_MODULE    := hello-jni
LOCAL_SRC_FILES := hello-jni.c
include $(BUILD_EXECUTABLE)
<div></div>
&nbsp;

**2\. 编译静态库**

文件Android.mk:

LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)
LOCAL_MODULE    := hello-jni
LOCAL_SRC_FILES := hello-jni.c
include $(**BUILD_STATIC_LIBRARY**)

&nbsp;

文件Application.mk:

**APP_MODULES**:=hello-jni

&nbsp;

**3\. 编译动态库**

文件Android.mk:

LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)
LOCAL_MODULE    := hello-jni
LOCAL_SRC_FILES := hello-jni.c
include $(**BUILD_SHARED_LIBRARY**)

&nbsp;

&nbsp;

**4\. 编译动态库+静态库**

文件Android.mk:

LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)
LOCAL_MODULE    := mylib_static
LOCAL_SRC_FILES := src.c
include $(**BUILD_STATIC_LIBRARY**)

include $(CLEAR_VARS)
LOCAL_MODULE    := mylib_shared
LOCAL_SRC_FILES := src2.c

**LOCAL_STATIC_LIBRARIES**:= mylib_static

include $(**BUILD_SHARED_LIBRARY**)