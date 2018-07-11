---
title: MySQL decode - MySQL中类似oracle的decode实现方法
keywords: [MySQL decode,MySQL decode函数,decode函数]
date: 2017/4/27 9:06
tags: [mysql,sql]
categories: mysql
---
在oracle中直接有decode函数 decode(cola,null,0)

表示如果cola为空，赋值为0

在mysql中的具体实现如下，
```
mysql> describe book;
+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra          |
+---------+--------------+------+-----+---------+----------------+
| book_id | bigint(20)   | NO   | PRI | NULL    | auto_increment |
| name    | varchar(100) | NO   |     | NULL    |                |
| number  | int(11)      | NO   |     | NULL    |                |
+---------+--------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)

mysql> select * from book;
+---------+-------------------------+--------+
| book_id | name                    | number |
+---------+-------------------------+--------+
|     123 | 123                     |    122 |
|    1000 | Java程序设计             |      5 |
|    1001 | 数据结构                 |      9 |
|    1002 | 设计模式                 |     10 |
|    1003 | 编译原理                 |     10 |
|    1004 | MySQL从删库到跑路         |    100 |
|    1005 | 活着                     |     10 |
|    1232 | 11111                    |    124 |
|    2001 | 测试                     |   2001 |
|   10064 | 老鼠爱大米               |  10088 |
|   10066 | 老鼠爱大米               |   1008 |
|   10088 | 测试宝典                |   1008 |
|   10096 | maven实战               |  10096 |
|   11111 | 111111                  |  11111 |
|   12311 | 都懂得                  |   1222 |
|  123222 | 222                     |   1111 |
+---------+-------------------------+--------+
16 rows in set (0.00 sec)

mysql> select if(count(b.book_id)=16,"十六","不是十六") '结果' from book b;
+--------+
| 结果   |
+--------+
| 十六   |
+--------+
1 row in set (0.00 sec)

mysql> select case count(b.book_id) when 16 then '十六' else '其他' end as '结果' from book b;
+--------+
| 结果   |
+--------+
| 十六   |
+--------+
1 row in set (0.00 sec)
```