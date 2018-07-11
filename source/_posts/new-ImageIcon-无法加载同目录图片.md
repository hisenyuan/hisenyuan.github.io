---
title: new ImageIcon()无法加载同目录图片
date: 2017-02-08 20:59:16
tags: [java,jframe]
---
错误：
```
new ImageIcon("1.jpg")
```
正确：
```
new ImageIcon("src/com/hisen/thread/progressbar/1.jpg")
```
图片路径：
```
test\src\com\hisen\thread\progressbar\1.jpg
```
所谓的相对路径，是相对于这个工程而言的，而不是当前文件夹而言。
