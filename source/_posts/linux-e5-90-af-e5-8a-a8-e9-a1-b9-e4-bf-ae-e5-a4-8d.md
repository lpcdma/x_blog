title: "linux启动项修复"
date: 2012-06-13 13:38:12
tags:
id: 23
comment: false
---

由于分区不够了，对linux从新进行了分区，结果也就破坏了原有系统的启动项。
使用 LiveCD/LiveUSB 或者硬盘安装的方法，进入一个 live 环境，挂载上原来的根分区，
比如挂载为 /mnt/temp，运行命令： 创建 /mnt/temp： 
```
sudo mkdir /mnt/temp
```

挂载原来的根目录： 
```
sudo mount /dev/sda5 /mnt/temp
```

如果是单独分的 /boot (以 sda6 为例)，则挂载它，如没有则跳过此步： 
```
sudo mount /dev/sda6 /mnt/temp/boot
```

挂载系统目录： 
```
for i in /dev /dev/pts /proc /sys; do sudo mount -B $i /mnt/temp$i; done
```

 chroot 进入原系统： 
```
sudo chroot /mnt/temp
```

重新安装 Grub2 到 MBR 并更新启动项： 
```
grub-install /dev/sda
    update-grub
```

退出环境： 
```
exit
```

还原系统目录： 
```
for i in /dev/pts /dev /proc /sys / ; do sudo umount /mnt/temp$i ; done
```
