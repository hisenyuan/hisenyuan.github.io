---
title: powerdesigner Oracle mysql comment 缺少右括号
keywords: [powerdesigner Oracle mysql comment 缺少右括号]
date: 2017/6/22 0:11
tags: [mysql,oracle]
categories: sql
---
powerdesigner Oracle mysql comment 缺少右括号

今天人家给我个powerdesigner设计好的表

要我去间数据库，直接复制出来去oracle执行，结果报错。

在comment附近，如果把comment去掉则可以正确执行

最后找到罪魁祸首：powerdesigner没有设置对数据库

解决办法，在powerdesigner页面
```
database-->change curren DBMS 
```
上面是设置你需要的数据库

下面是为更改前的数据库