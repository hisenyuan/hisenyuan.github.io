---
title: 浅谈volatile
keywords: [volatile,java,内存可见性]
date: 2019/6/12 21:00
tags: [java]
categories: java
---
# 一、volatile是什么
volatile是Java提供的一种轻量级的同步机制
保证变量的修改其它线程立马可见，解决部分并发问题

# 二、volatile局限性
无法解决复合操作，例如 i++ i--这种操作
原因：i++操作分三步
1. 读取i=1；
2. i=i+1=2；
3. i=2写入主存
有可能在3写入之前，其它线程已经对i进行了修改，比如改为100了，结果覆盖写入了2

# 三、volatile原理
volatile底层依靠指令重排序来实现内存可见性的
具体的规则如下
1. 当第二个操作是voaltile写时，无论第一个操作是什么，都不能进行重排序
2. 当第一个操作是volatile读时，不管第二个操作是什么，都不能进行重排序
3. 当第一个操作是volatile写时，第二个操作是volatile读时，不能进行重排序

<!--more-->
# 四、 参考
https://www.cnblogs.com/chengxiao/p/6528109.html
