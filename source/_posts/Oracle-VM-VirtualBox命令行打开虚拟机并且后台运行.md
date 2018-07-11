---
title: Oracle VM VirtualBox命令行打开虚拟机并且后台运行
keywords: [命令行启动虚拟机,VirtualBox命令行打开ubuntu,VirtualBox隐藏到托盘]
tags: [ubuntu,linux]
date: 2017-02-19 00:00:01
categories: linux
---
这几天在折腾Ubuntu虚拟机，想着也就让他做一台服务器罢了

没想到安装之后发现一直没法让VirtualBox隐藏到托盘

按正常程序走，打开一个虚拟机会出现两个GUI界面：
1. VirtualBox界面
2. 虚拟机界面

第一个可以在打开虚拟机之后关闭，第二个不能关闭也不能隐藏到托盘

痛苦！！！

这里给出解决方案：通过cmd命令行启动并且后台运行
一、进入VirtualBox安装目录，我这里是
---
```
C:\tool\Oracle\VirtualBox>
```
二、列出所有安装的虚拟机
---
```
C:\tool\Oracle\VirtualBox>VBoxManage list vms
"centos" {162f777f-e3c4-4717-8b5b-4a4e43a8b552}
"debian" {48ebecba-77ec-483d-ab73-bf9ee777e2f6}
"ubuntu" {d8050b3a-b04b-4fe9-8a90-16086dac0ae9}
```
前面是NAME，后面是UUID，之后的**name**用这连个代替都可以
三、命令行启动虚拟机
---
<!--more-->
```
C:\tool\Oracle\VirtualBox>VBoxManage startvm "ubuntu" --type headless
Waiting for VM "ubuntu" to power on...
VM "ubuntu" has been successfully started.
```
headless是在后台运行，并且默认开启vrdp服务，可以通过远程桌面工具来访问

下面是打开ubuntu之后用Xshell链接的
```
Connecting to 127.0.0.1:2222...
Connection established.
To escape to local shell, press 'Ctrl+Alt+]'.

Welcome to Ubuntu 16.04 LTS (GNU/Linux 4.4.0-62-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

362 个可升级软件包。
0 个安全更新。

Last login: Sat Feb 18 16:25:58 2017 from 10.0.2.2
hisen@hisen-VirtualBox:~$ ls
download  公共的  模板  视频  图片  文档  下载  音乐  桌面
```

四、所有相关的命令
---
下面所有的NAME都可以用二中的name和UUID代替
```
# 列出所有安装的虚拟机
VBoxManage list vms

# 后台打开，无界面
VBoxManage startvm "NAME" --type headless 

# gui方式启动，跟桌面点击没有区别
VBoxManage startvm NAME --type gui

# 列出运行中的虚拟机
VBoxManage list runningvms

# 关闭虚拟机，等价于点击系统关闭按钮，正常关机
VBoxManage controlvm NAME acpipowerbutton

# 关闭虚拟机，等价于直接关闭电源，非正常关机
VBoxManage controlvm NAME poweroff

# 暂停虚拟机的运行
VBoxManage controlvm NAME pause

# 恢复暂停的虚拟机
VBoxManage controlvm NAME resume

# 保存当前虚拟机的运行状态
VBoxManage controlvm NAME savestate 
```