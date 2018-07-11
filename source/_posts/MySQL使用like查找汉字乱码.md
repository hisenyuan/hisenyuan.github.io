---
title: MySQL使用like查找汉字乱码
date: 2017-02-08 21:03:34
tags: [mysql,like]
---
第一种解决办法：BINARY
---
在关键字之前加上：BINARY，会使关键字强制转换为二进制字符串
```
select id form t where chinese like **BINARY** %汉字%
```
第二种解决办法：改关键字类型
---
把关键字的类型改成：BINARY


这两种办法都可以解决乱码问题
---