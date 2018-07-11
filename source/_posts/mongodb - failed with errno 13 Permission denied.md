---
title: mongodb - failed with errno13 Permission denied 
keywords: [failed with errno:13 Permission denied,mongodb]
date: 2017/7/12 0:18
tags: [mongodb]
categories: sql
---
在命令行输入：mongo报错
```
hisen@ubuntu:~$ mongo
MongoDB shell version: 3.2.13
connecting to: test
2017-07-11T23:11:23.827+0800 I STORAGE  [main] In File::open(), ::open for '/home/hisen/.mongorc.js' failed with errno:13 Permission denied
The ".mongorc.js" file located in your home folder could not be executed
```
看倒数第二行，应该是权限的问题，于是
```
hisen@ubuntu:~$ sudo chown -R hisen /home/hisen/.mongorc.js
hisen@ubuntu:~$ mongo
MongoDB shell version: 3.2.13
connecting to: test
Server has startup warnings: 
2017-07-11T23:11:15.845+0800 I CONTROL  [initandlisten] 
2017-07-11T23:11:15.845+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2017-07-11T23:11:15.845+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2017-07-11T23:11:15.845+0800 I CONTROL  [initandlisten] 
2017-07-11T23:11:15.845+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2017-07-11T23:11:15.845+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2017-07-11T23:11:15.845+0800 I CONTROL  [initandlisten] 
```
完美解决，用户权限的问题。