---
title: linux命令统计出现次数前10的次数和IP | awk sort unip等组合
keywords: [linux统计,linux排序,linux统计ip]
date: 2019/4/10 16:00
tags: [linux]
categories: linux
---
# 一、命令组合
```
$ more sort.log | awk '{print $1}' | sort | uniq -c | sort -k1nr | head -3
   4 5
   3 1
   2 2
```

# 二、命令详解
more：一次读取少量的数据，避免一次性载入大文件，比cat好些
awk '{print $1}'：以awk默认的分隔符，并且打印第一列
sort：把上一步的结果按ASCII排序
uniq -c：如果重复那么计数，uniq命令可以组合很多其它参数
sort -k1nr：根据第一列的数据进行排序，n是按数值大小排序，r是倒序排序
head -3：显示前面三行数据

# 三、原始数据
<!--more-->
```
$ cat sort.log
1
2
3
4
5
6
1
1
2
3
4
5
5
5
6
```
