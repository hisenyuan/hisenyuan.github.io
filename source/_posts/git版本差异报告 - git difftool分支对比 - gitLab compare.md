---
title: git版本差异报告 - git difftool分支对比 - gitLab compare
keywords: [git,git diff,git版本差异报告]
date: 2019/3/9 18:08
tags: [java,gitLab,git]
categories: java
---
# 一、初衷
有时候上线会出现合错代码，比如功能，或者maven的pom文件；

人都不是十全十美的人，只要是人就会犯错
关键是要想办法去避免，只有工具才不犯错

所以尽量想办法利用工具来防止我们犯错，git来说有很多办法；
简单的命令，或者gitlab的compare报告；
每次上线之前，开发自己看一遍当前版本与线上版本的区别，确认每一个点都是正确的改动；

# 二、gitlab compare功能(推荐)
2.1 入口：工程首页 -> Repository -> Compare
2.2 选择：上一个版本，当前版本，点击Compare
2.2 结果：一次能看到两个版本的所有commit、改动点
# 三、git命令
```
# 显示版本之间改动的文件名
git difftool master 1.2.4 --stat

# 在命令行挨个显示版本某个文件具体差异
git difftool master 1.2.4
```
