---
title: oracle取微秒 - 记录点滴
keywords: [oracle,java]
date: 2017/9/23 10:36
tags: [oracle,sql]
categories: java
---
昨天在群里看到有人问怎么通过sql在oracle中取微秒

以前没有遇到过，就搜索了一下，找了一会给找到了
```
select to_char(systimestamp, 'yyyy-mm-dd hh24:mi:ss ff') from dual;
```
输出:年-月-日 时:分:秒 微秒
```
2017-9-24 10:38:27 129368
```
很少问题是搜索引擎找不到的，学会如何描述问题才是关键