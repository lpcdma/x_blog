title: "linux dd动态调整挂载img大小"
date: 2015-05-16 17:52:21
tags:
id: 787
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">sudo losetup /dev/loop0
sudo dd if=/dev/zero bs=1MiB of=/path/to/file conv=notrunc oflag=append count=xxx
sudo losetup -c /dev/loop0
sudo resize2fs /dev/loop0</pre>