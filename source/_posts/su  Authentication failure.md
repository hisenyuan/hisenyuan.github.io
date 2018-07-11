---
title: su  Authentication failure
keywords: [su:Authentication failure]
date: 2017/4/1 16:44
tags: [linux]
categories: linux
---
想要获取root权限，提示如下
```
hisen@ubuntu:/var/lib$ su
Password: 
su: Authentication failure
```
解决办法
```
hisen@ubuntu:$ sudo passwd root
Enter new UNIX password: 
Retype new UNIX password: 
passwd: password updated successfully
hisen@ubuntu:$ su
Password: 
root@ubuntu:# cd mysql
```
重新设置一下密码即可，我这边装的时候设置的用户是：hisen

刚刚重新设置的密码就是你装系统的时候设置的用户密码。