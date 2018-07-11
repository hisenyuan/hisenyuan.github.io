---
title: 手机平板电脑原样显示html效果 - html知识
date: 2017-02-08 21:15:36
tags: [html]
---
有时候可能你会发现
在电脑上显示300x300大小的东西看起来很正常
但是用手机去访问的话，就出现等比例缩小了
但是300x300的大小完全不用缩小
直接等大不是更好？

在head加上这两行代码，可以做到原样输出
```
<meta http-equiv="X-UA-Compatible" content="IE=Edge,chrome=1">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```