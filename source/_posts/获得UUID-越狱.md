title: 获得UUID(越狱)
date: 2017-06-06 10:58:39
categories: 学习笔记
tags: [ios,UUID]
---

###代码拷贝
```
static CFStringRef (*$MGCopyAnswer)(CFStringRef);
%hook UIDevice
-(NSString *)uniqueIdentifier {
    if (kCFCoreFoundationVersionNumber < 800) {
        void *gestalt(dlopen("/usr/lib/libMobileGestalt.dylib", RTLD_GLOBAL | RTLD_LAZY));
        $MGCopyAnswer = reinterpret_cast&lt;CFStringRef (*)(CFStringRef)&gt;(dlsym(gestalt, "MGCopyAnswer"));
        return (id)$MGCopyAnswer(CFSTR("UniqueDeviceID"));
    }
    else
        %orig;
}
%end
```

###参考文献
[libMobileGestalt.dylib](http://iphonedevwiki.net/index.php/LibMobileGestalt.dylib)
[cyida](http://gitweb.saurik.com/cydia.git/blob/90bf9a3d170ab2dc4701c76f7b3911308211f542:/MobileCydia.mm)