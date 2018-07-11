---
title: MySQL下A库的a表导入到B库的b表
keywords: [mysql,数据导出]
date: 2017/5/16 11:44
tags: [mysql]
categories: mysql
---
| 数据库名称 | 表名 |
|:-----|:-----|
| employees | employees |
| hisen | employee |

现在想把第一行的数据导入到第二行，具体如下
```
mysql> describe employees;
+------------+---------------+------+-----+---------+-------+
| Field      | Type          | Null | Key | Default | Extra |
+------------+---------------+------+-----+---------+-------+
| emp_no     | int(11)       | NO   | PRI | NULL    |       |
| birth_date | date          | NO   |     | NULL    |       |
| first_name | varchar(14)   | NO   |     | NULL    |       |
| last_name  | varchar(16)   | NO   |     | NULL    |       |
| gender     | enum('M','F') | NO   |     | NULL    |       |
| hire_date  | date          | NO   |     | NULL    |       |
+------------+---------------+------+-----+---------+-------+
6 rows in set

mysql> select count(*) from employees;
+----------+
| count(*) |
+----------+
|   300024 |
+----------+
1 row in set
```
employees数据库中的employees表有30万数据

我想把这个数据导出到另外一个数据库hisen中的employee表中

具体操作如下
<!--more-->
```
mysql> create table hisen.employee as(
    -> select * from employees.employees
    -> );
Query OK, 300024 rows affected
Records: 300024  Duplicates: 0  Warnings: 0

mysql> use hisen;
Database changed
mysql> select count(*) from employee;
+----------+
| count(*) |
+----------+
|   300024 |
+----------+
1 row in set
```
这种操作是针对所有字段都导出的情形，创建表跟插入数据合二为一

如果导出部分字段或者有其他限制条件写sql即可

这应该是最简单的方法~