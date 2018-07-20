---
title: sql行转列，列转行
keywords: [mysql行转列,mysql列转行,oracle行转列,oracle列转行]
date: 2017/6/13 20:51
tags: [oracle,mysql]
categories: sql
---
有时候需要行转列或者列转行

在Oracle 11g中，Oracle 又增加了2个查询：pivot（行转列） 和unpivot（列转行）

下面是在mysql中的操作：
```SQL
mysql> select * from test;
+------+------+--------+
| id   | age  | weigth |
+------+------+--------+
|    1 |    1 |      3 |
|    1 |    2 |      2 |
|    1 |    3 |      1 |
|    2 |    5 |      6 |
+------+------+--------+
4 rows in set (0.00 sec)

#根据ID分组
mysql> select group_concat(age) from test group by id;
+-------------------+
| group_concat(age) |
+-------------------+
| 1,2,3             |
| 5                 |
+-------------------+
2 rows in set (0.01 sec)

#不分组
mysql> select group_concat(age) from test;
+-------------------+
| group_concat(age) |
+-------------------+
| 1,2,3,5           |
+-------------------+
1 row in set (0.00 sec)
```
