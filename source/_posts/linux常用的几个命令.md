---
title: linux常用的几个命令
date: 2017-01-22 23:27:02
tags: [java,linux]
---

1、查看日志最后几行
```
tail -100 /access.log
```
2、进入目录相关
```
#进入一个目录
root@hisenyuan:/# cd /home/wwwlog/
#进入当前目录下的www.google.com目录
root@hisenyuan:/home/wwwlog# cd ./www.google.com
#进入父目录	
root@hisenyuan:/home/wwwlog/www.google.com# cd ../
#进入linux系统根目录
root@hisenyuan:/home/wwwlog# cd /
#根目录
root@hisenyuan:/# 
```
3、看倒数多少行
```
#看倒数10行
tail -10 /filepath/filename
#看行数外加过滤含有指定字符的行
tail -10 access.log | grep -v "yourstring"
```
4、过滤特定行，保存结果到新文件
```
cat /root/old.text | grep -v "yourstring"> /root/new.text
```