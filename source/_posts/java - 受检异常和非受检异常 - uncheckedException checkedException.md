---
title: Java受检异常和非受检异常 - uncheckedException checkedException
keywords: [java异常,java uncheckedException,java checkedException,java unchecked checked,java 受检非受检异常]
date: 2019/4/29 13:31
tags: [java,异常]
categories: java
---
# 一、简单介绍
除了runtimeException以外的异常，都属于checkedException。

CheckedException(受检异常)：编译器会检查这类异常，需要强制捕获，否则无法编译通过。
UnCheckedException(非受检异常)：编译器不会检查这类异常，可以不用捕获，可以编译通过。

# 二、常见异常
受检：IOException、SQLException、NumberFormatException、IllegalArgumentException
非受检：OutOfMemoryError、StackOverflowError、NullPointerException、IndexOutOfBoundsException

[点击查看所有jdk8 api文档](https://docs.oracle.com/javase/8/docs/api/)
进去可以看到java.lang.RuntimeException下有很多子类异常

暂时写这些，日后再完善
