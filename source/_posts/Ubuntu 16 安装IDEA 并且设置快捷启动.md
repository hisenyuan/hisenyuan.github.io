---
title: Ubuntu 16 安装IDEA 并且设置快捷启动
keywords: [idea]
date: 2017/3/4 0:23
tags: [idea,java]
categories: java
---

安装简单，下载官网的文件(with java的比较方便)

解压之后在bin目录下执行
```
sudo sh idea.sh
```
就会进入安装程序，接下来会跳出图形界面，跟windows差不多的步骤

没有激活码可以看之前的文章

关键的一个是我发现网上说的建立桌面快捷方式不行

就这样弄个方便的
```
cd ~
ln -s /idea home/bin/idea.sh idea
#接下来执行 idea & 就可以打开，如果提示权限不够
#就执行 sudo idea &
sudo idea &
#后面的 & 代表后台运行的意思，不影响控制台
```
