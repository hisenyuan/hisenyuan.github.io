---
title: ubuntu重装系统 - 在ubuntu系统下重新安装
keywords: [ubuntu重装系统,ubuntu系统下重装ubuntu]
date: 2017/3/5 16:24
tags: [linux]
categories: linux
---

有时候在linux环境下需要重新安装一下系统

这里我就说一下今天我安装的方法。

下载好ubuntu的镜像，随便放在一个非系统盘的根目录下

改名为：ubuntu.iso
```
sudo chmod 777 /boot/grub/grub.cfg
sudo vi  /boot/grub/grub.cfg
#在 export linux_gfx_mode 下面添加如下内容
menuentry "install  ubuntu powered by hisen" {
  search --set -f /ubuntu.iso
  loopback loop /ubuntu.iso
  set root=(loop)
  linux /casper/vmlinuz.efi boot=casper iso-scan/filename=/ubuntu.iso
  initrd /casper/initrd.lz
  boot
}
```
保存退出，重启就会进入系统。

桌面上点击那个安装的图标即可完成重装