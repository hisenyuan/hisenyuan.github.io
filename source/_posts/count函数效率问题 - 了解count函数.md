---
title: count函数效率问题 - 了解count函数
keywords: [count(*)，count(1)]
date: 2017/3/3 14:33
tags: [mysql]
categories: java
---
## count函数的作用

想要真正的理解count函数，我们就必须明白count函数的作用。

作用一：统计某一列非空(not null)值得数量，即统计某列有值得结果数,使用count(col)。

作用二：统计结果集的行数，此时不用管某列是否为null值。即使用count(*).

明白了这点，我们就应该知道MySQL的count(*)并不是想象中的那样，统计每一列的值，而是直接忽视掉所有列，直接统计行数，那么它的效率肯定是很高的。

但是有一点，当col指定了该字段为NOT NULL时实际上，MySQL会自动将count(col)转为count(*),但是这样也同样耗费了些时间，如果col没有指定为NOT NULL的话，那么效率就更低了，MySQL就必须要判断每一行的值是否为空。

所以综上所述，如果是要统计行数最好优先使用select count(*)

当统计某一列等于多少的值得时候可以使用下面两种方法:
```
SELECT SUM(IF(id = 23,1,0)) FROM table 
SELECT COUNT(id = 23 OR NULL) FROM table 
```
