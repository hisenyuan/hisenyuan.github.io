---
title: MySQL update语句
keywords: [MySQL update语句]
date: 2017/8/1 10:37
tags: [mysql]
categories: sql
---
刚在一个群里，有人有这么一个需求。

表A：id,name

表B：id,其他,name(新增字段)

A，B表通过id关联，要把A的name给对应的B的name

以前也没有写过这种update语句
```
update 表名 set 字段名=字段值 where 条件
如 update A set name='xiaoming' where id='';
如果是多表查询
update 表1 a inner join 表2 b on  ab表的关联 set a.字段=b.字段
如 update A a inner join B b on a.id=b.id set a.name=b.name
就是在table1表和table2表id相同时 把table2的name值赋给table1的name
```