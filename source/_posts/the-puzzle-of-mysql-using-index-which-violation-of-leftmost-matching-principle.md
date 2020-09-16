---
title: MySQL查询违反最左匹配原则，但 explain 显示走索引的疑惑
keywords: [索引,mysql,index,最左匹配,mysql using index,explain]
date: 2020/9/15 21:45
tags: [mysql,index]
categories: 数据库
---
# 零、本文背景
有个朋友抛出一个问题，*明显不符合最左匹配原则的 SQL，居然走索引了*
兜兜转转，嘀咕了好几天，期间也和几个朋友讨论了一下
都没有结果，最后还是在 MySQL 的官方文档中找到了原因
记录下，也算是一次不错的探索。

# 一、问题描述
## 1.1 表结构
```sql
CREATE TABLE `people_new` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `last_name` varchar(255) NOT NULL,
  `first_name` varchar(255) NOT NULL,
  `bob` date NOT NULL,
  PRIMARY KEY (`id`),
  KEY `index_union` (`last_name`,`first_name`,`bob`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8 COMMENT='人员-新'
```
## 1.2 数据
```
mysql> select * from people_new;
+----+-----------+------------+------------+
| id | last_name | first_name | bob        |
+----+-----------+------------+------------+
|  1 |  hisen    | yuan       | 2008-08-08 |
+----+-----------+------------+------------+
1 row in set (0.00 sec)
```
## 1.3 SQL 分析
可以看到 **Using index**
但是 **possible_keys** null 而 **key** 显示 index_union
```
mysql> explain select * from people_new  where bob = '2008-08-08' and first_name = 'yuan';
+----+-------------+------------+------------+-------+---------------+-------------+---------+------+------+----------+--------------------------+
| id | select_type | table      | partitions | type  | possible_keys | key         | key_len | ref  | rows | filtered | Extra                    |
+----+-------------+------------+------------+-------+---------------+-------------+---------+------+------+----------+--------------------------+
|  1 | SIMPLE      | people_new | NULL       | index | NULL          | index_union | 1537    | NULL |    1 |   100.00 | Using where; Using index |
+----+-------------+------------+------------+-------+---------------+-------------+---------+------+------+----------+--------------------------+
1 row in set, 1 warning (0.00 sec)
```

# 二、原因
<!--more-->
1.1 的表结构显示，除了 id，其余三个属性都在 **联合索引** 中
所以通过任何字段查询，返回的字段都被索引覆盖了，构成 **覆盖索引** ，
由于扫描全部索引会快于全表扫描，所以这时候 sql 不管是不是最左匹配都会走索引(应该是『全量索引』)。

3.1 的表结构中，除了 id ，还有 area 不在联合索引当中，此时破坏了 **覆盖索引** ，故不走索引。
```
If the index is a covering index for the queries and can be used to satisfy all data required from the table,
only the index tree is scanned. In this case, the Extra column says Using index.
An index-only scan usually is faster than ALL because the size of the index usually is smaller than the table data.
```
详情：[dev.mysql.com](https://dev.mysql.com/doc/refman/8.0/en/explain-output.html#explain-extra-information)

# 三、验证
## 3.1 修改后的表结构
```sql
CREATE TABLE `people_new` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `last_name` varchar(255) NOT NULL,
  `first_name` varchar(255) NOT NULL,
  `bob` date NOT NULL,
  `area` varchar(256) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `index_union` (`last_name`,`first_name`,`bob`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8 COMMENT='人员-新'
```
## 3.2 修改表结构后的数据
```
mysql> select * from people_new;
+----+-----------+------------+------------+------+
| id | last_name | first_name | bob        | area |
+----+-----------+------------+------------+------+
|  1 |  hisen    | yuan       | 2008-08-08 | NULL |
+----+-----------+------------+------------+------+
1 row in set (0.00 sec)

```
## 3.2 SQL 分析
```
mysql> explain select * from people_new  where bob = '2008-08-08' and first_name = 'yuan';
+----+-------------+------------+------------+------+---------------+------+---------+------+------+----------+-------------+
| id | select_type | table      | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+------------+------------+------+---------------+------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | people_new | NULL       | ALL  | NULL          | NULL | NULL    | NULL |    1 |   100.00 | Using where |
+----+-------------+------------+------------+------+---------------+------+---------+------+------+----------+-------------+
1 row in set, 1 warning (0.00 sec)
```

# 四、总结
有时候多和同行交流也能学到很多。
带着问题去看官方文档收获会比较大。
遇到问题，找到原因，解决问题，印象会深刻。
