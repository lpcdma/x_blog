title: "编译android版luajit"
date: 2015-05-18 22:02:41
tags:
---
```bash
git clone https://github.com/LuaDist/luajit.git

make HOST_CC="gcc -m32" CROSS=/Users/lpcdma/sandbox/android-ndk-r10d/toolchains/arm-linux-androideabi-4.6/prebuilt/darwin-x86_64/bin/arm-linux-androideabi- TARGET_SYS=Linux TARGET_FLAGS="--sysroot /Users/lpcdma/sandbox/android-ndk-r10d/platforms/android-14/arch-arm"
```