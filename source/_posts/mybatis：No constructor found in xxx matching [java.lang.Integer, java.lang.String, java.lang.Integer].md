---
title: mybatis：No constructor found in xxx matching
keywords: [mybatis，No constructor found in xxx matching]
date: 2017/3/17 9:54
tags: [java]
categories: java
---

如下错误提示：

mybatis：No constructor found in xxx matching [java.lang.Integer, java.lang.String, java.lang.Integer]

原因：xxx 这个bean缺少一个默认的构造方法！

---

解决：加上默认的构造方法即可

---

我是在单元测试的时候遇到这个问题