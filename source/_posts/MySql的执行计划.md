---
title: MySql的执行计划
keywords: [mysql执行计划,mysql explain]
date: 2017/11/23 17:04
tags: [sql,mysql,explain]
categories: sql
---
###命令介绍
MySQL的EXPLAIN命令用于SQL语句的查询执行计划(QEP)。

这条命令的输出结果能够让我们了解MySQL 优化器是如何执行

执行下面的SQL
```
explain select * FROM book where name like '活%'
```
得到下面的数据

| id | select_type | table | partitions | type | possible_keys | key | key_len | ref | rows | filered | Extra |
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
| 编号 | 查询方式 | 表名 | 分区 | 连接方式 | 索引(可能) | 索引 | 索引长度 | 作用列 | 行数 | 百分比 | 额外 |
| 1 | SIMPLE | book | null | ALL | null | null | null | null | 114 | 11.11 | Using where |

###建表语句
<!--more-->
```
#创建个人信息表
CREATE TABLE hisen_test_explain_people
(
  id bigint auto_increment primary key,
  zipcode char(32) not null default '',
  address varchar(128) not null default '',
  lastname char(64) not null default '',
  firstname char(64) not null default '',
  birthdate char(10) not null default ''
);
#创建索引
alter table hisen_test_explain_people add key(zipcode,firstname,lastname);

#插入个人信息数据
insert into hisen_test_explain_people
(zipcode,address,lastname,firstname,birthdate)
values
  ('230031','anhui','zhan','jindong','1989-09-15'),
  ('100000','beijing','zhang','san','1987-03-11'),
  ('200000','shanghai','wang','wu','1988-08-25');
#创建汽车信息表
CREATE TABLE hisen_test_explain_people_car(
  people_id bigint,
  plate_number varchar(16) not null default '',
  engine_number varchar(16) not null default '',
  lasttime timestamp
);
#插入汽车信息
insert into hisen_test_explain_people_car
(people_id,plate_number,engine_number,lasttime)
values
  (1,'A121311','12121313','2017-11-23 :21:12:21'),
  (2,'B121311','1S121313','2016-11-23 :21:12:21'),
  (3,'C121311','1211SAS1','2015-11-23 :21:12:21');
```
###执行计划样例
```
explain select zipcode,firstname,lastname from hisen_test_explain_people;
+----+-------------+---------------------------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
| id | select_type | table                     | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------------------------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | hisen_test_explain_people | NULL       | index | NULL          | zipcode | 160     | NULL |    3 |   100.00 | Using index |
+----+-------------+---------------------------+------------+-------+---------------+---------+---------+------+------+----------+-------------+

explain select zipcode from (select * from hisen_test_explain_people a) b;
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key     | key_len | ref  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | a     | NULL       | index | NULL          | zipcode | 160     | NULL |    3 |   100.00 | Using index |
+----+-------------+-------+------------+-------+---------------+---------+---------+------+------+----------+-------------+


id是用来顺序标识整个查询中SELELCT 语句的，通过上面这个简单的嵌套查询可以看到id越大的语句越先执行。
该值可能为NULL，如果这一行用来说明的是其他行的联合结果，比如UNION语句：

explain select * from hisen_test_explain_people where zipcode = 100000 union select * from hisen_test_explain_people where zipcode = 200000;
+----+--------------+---------------------------+------------+------+---------------+------+---------+------+------+----------+-----------------+
| id | select_type  | table                     | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra           |
+----+--------------+---------------------------+------------+------+---------------+------+---------+------+------+----------+-----------------+
|  1 | PRIMARY      | hisen_test_explain_people | NULL       | ALL  | zipcode       | NULL | NULL    | NULL |    3 |    33.33 | Using where     |
|  2 | UNION        | hisen_test_explain_people | NULL       | ALL  | zipcode       | NULL | NULL    | NULL |    3 |    33.33 | Using where     |
|NULL| UNION RESULT | <union1,2>                | NULL       | ALL  | NULL          | NULL | NULL    | NULL | NULL |     NULL | Using temporary |
+----+--------------+---------------------------+------------+------+---------------+------+---------+------+------+----------+-----------------+
```
###参考
1. <a href="http://blog.csdn.net/u012990533/article/details/45643509" target="_blank">参数解释</a>
2. <a href="http://blog.csdn.net/u012721013/article/details/54965844" target="_blank">实战演练</a>

