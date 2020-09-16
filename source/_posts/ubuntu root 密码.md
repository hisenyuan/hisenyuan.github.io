---
title: ubuntu root su 密码
keywords: [ubuntu root密码,su密码,Authentication failure]
date: 2017/7/4 21:08
tags: [linux]
categories: linux
---
在安装完ubuntu的时候

只有自己设置的非root的帐号和密码

但是又要用root密码怎么办呢？
```
hisen@ubuntu-1:~$ sudo passwd
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```
上面输入的密码就是你的root密码

检测一下
```
hisen@ubuntu-1:~$ su
Password:
root@ubuntu-1:/home/hisen#
```
通过，至此结束
