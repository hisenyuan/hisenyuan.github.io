---
title: Unix文件超过大小限制移动到某目录
keywords: [linux 文件 超过 移动,移动文件,linux判断文件大小]
date: 2019/4/8 22:00
tags: [linux]
categories: linux
---
某个文件夹下面有n个文件，需要移动大于10M的文件到/tmp/目录下

实现命令
```
find . -type f -size +100M -exec mv {} /tmp/ \;
```
find 查找
. 当前目录
-type 文件类型：f 文件，d 目录
-size 文件大小：+100M +：大于 -：小于 空：等于
-exec 管道命令，将前面的查询结果传递给后面的命令
{} 指前面传递过来的的查询结果
\; 结束管道命令
