---
title: Windows10开机bat启动VirtualBox虚拟机并且后台运行
keywords: [bat开启VirtualBox虚拟机,bat打开VirtualBox虚拟机,Windows10开机启动VirtualBox虚拟机]
tags: [VirtualBox,bat,linux]
date: 2017-02-20 09:51:57
categories: 软件
---

1.制作启动脚本

新建一个start.bat文件，内容如下
```
@echo off

echo 本命令可让us在后台运行
echo 启动之后可以关闭本窗口

::进入虚拟机目录
cd C:\"Program Files"\Oracle\VirtualBox

::执行相关命令 同时启动两台虚拟机
VBoxManage startvm "us" --type headless
::VBoxManage startvm "ubuntu" --type headless

::执行完之后按回车退出窗口
pause
```
2.设置开机启动

把start.bat文件复制到[启动]文件夹里面

[启动]文件夹路径
```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
```
文件管理器地址栏显示大概是这样
```
Windows > [开始]菜单 > 程序 > 启动
```
放进去之后就可以开机启动了！

启动之后Xshell连接即可