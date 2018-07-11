---
title: MySQL一个group by查询出最大值和非group by所在字段的值
keywords: [mysql,group by]
date: 2017/4/18 15:33
tags: [mysql]
categories: mysql
---
表结构，数据内容如下。

需求是：查找出每个年级，年纪最大的人的名字。

个人的思维只停留在
```
mysql> select name, max(age),grade from  stu group by grade;
ERROR 1055 (42000): Expression #1 of SELECT list is not in GROUP BY clause and contains nonaggregated column 'hisen.stu.name' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by

mysql> SELECT A.* FROM stu A INNER JOIN (SELECT MAX(AGE) AS MAX_AGE,GRADE FROM stu GROUP BY GRADE ) B ON A.GRADE = B.GRADE AND A.AGE= B.MAX_AGE;
+----+------+------+-------+
| id | name | age  | grade |
+----+------+------+-------+
|  3 | c    |   13 |     1 |
|  6 | f    |   13 |     2 |
+----+------+------+-------+
2 rows in set (0.00 sec)
```

折腾了一会自己不知道怎么解决，后来倒是解决了。

具体过程如下：
```
mysql> describe stu;
+-------+---------+------+-----+---------+----------------+
| Field | Type    | Null | Key | Default | Extra          |
+-------+---------+------+-----+---------+----------------+
| id    | int(11) | NO   | PRI | NULL    | auto_increment |
| name  | char(1) | YES  |     | NULL    |                |
| age   | int(11) | YES  |     | NULL    |                |
| grade | int(11) | YES  |     | NULL    |                |
+-------+---------+------+-----+---------+----------------+
4 rows in set (0.00 sec)

mysql> select * from stu;
+----+------+------+-------+
| id | name | age  | grade |
+----+------+------+-------+
|  1 | a    |   11 |     1 |
|  2 | b    |   12 |     1 |
|  3 | c    |   13 |     1 |
|  4 | d    |   11 |     2 |
|  5 | e    |   12 |     2 |
|  6 | f    |   13 |     2 |
+----+------+------+-------+
6 rows in set (0.00 sec)

mysql> select max(name), max(age),grade from  stu group by grade;
+-----------+----------+-------+
| max(name) | max(age) | grade |
+-----------+----------+-------+
| c         |       13 |     1 |
| f         |       13 |     2 |
+-----------+----------+-------+
2 rows in set (0.01 sec)
```