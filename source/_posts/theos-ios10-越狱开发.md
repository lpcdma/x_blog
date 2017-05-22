title: theos(ios10)越狱开发
date: 2017-05-22 10:16:07
categories: 学习笔记
tags: [arm theos]
---

## 初识
最早一次接触theos应该是在完美宿舍,拿着越狱的ipad mimi2在ios7上很2
的搭建和敲一些测试代码.没有真正做个项目,所以基本上完全淡忘。

## 安装
- [查看官方kiwi](https://github.com/theos/theos/wiki/Installation)
- [查看IphoneDev](http://iphonedevwiki.net/index.php/Theos/Setup)

## 测试
我只用到hook方面的我就只测试hook
```
#import <Foundation/Foundation.h>
#include "nslog_to_os_log.h"
%hook SpringBoard

-(void)applicationDidFinishLaunching:(id)application
{
    %orig;
    HBLogDebug(@"lpcdma Debug hblog debug");
    HBLogInfo(@"lpcdma Debug hblog info");
    HBLogWarn(@"lpcdma Debug hblog warn");
    HBLogError(@"lpcdma Debug hblog error");
}
%end
```
为了打印日志,我在ios10上折腾了一阵,记忆中直接使用NSLog就可以打印日志.
但是ios10上死活打印不出来,经过几番搜索。得到上面的代码。和查看日志的
姿势:
- [无法输入日志的原因](https://www.reddit.com/r/jailbreakdevelopers/comments/5t4a0s/ios_10_cant_see_nslog_messages_or_printf_messages/)
- [读取日志的姿势](https://www.reddit.com/r/jailbreakdevelopers/comments/5qt7r2/102_nslog_and_hblogdebug_not_showing_in_system_log/)
- [ios上hexdump参考](http://www.septicus.com/SSCrypto/trunk/SSCrypto.m)
这里接触到了两个新的工具[deviceconsole](https://github.com/MegaCookie/deviceconsole)和[oslog](https://github.com/limneos/oslog)
前者是mac上工具,稳定性极差.

## 调试
- [签名及开始调试](http://iphonedevwiki.net/index.php/Debugserver)
- [gdb&lldb初步帮助文档](http://lldb.llvm.org/lldb-gdb.html)
```
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>com.lpcdma.test.launchapplications</key>
	<true/>
	<key>com.lpcdma.test.debugapplications</key>
	<true/>
	<key>com.apple.springboard.debugapplications</key>
	<true/>
	<key>get-task-allow</key>
	<true/>
	<key>task_for_pid-allow</key>
	<true/>
	<key>run-unsigned-code</key>
	<true/>
</dict>
</plist>
```
签名用的*xml*中需要填写别调试程序的**Bundle identifier**,试过**lipo -S**伪签名运行程序可以用,无法调试.

## 未完成事项
没有找到好的ide,iphoneopendev+xcode印象中试过,可感觉不太好.
代码量不多,使用vim和subl凑合.

有同学说不使用theos,使用xcode创建静态库,命令行打包这个估计需要试下才知道这么做.
学习的方式使用**make -n**查看theos构建流程,应该能找到合适的方式.