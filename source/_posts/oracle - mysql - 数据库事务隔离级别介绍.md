---
title: oracle - mysql - 数据库事务隔离级别介绍
keywords: [数据库事物隔离级别,oracle事物默认隔离级别,mysql数据库隔离级别]
date: 2017/11/14 22:44
tags: [sql,java]
categories: sql
---

名词解释
---
1. 脏读: 对于两个事物 T1, T2, T1 读取了已经被 T2 更新但还没有被提交的字段. 之后, 若 T2 回滚, T1读取的内容就是临时且无效的.  
2. 不可重复读: 对于两个事物 T1, T2, T1 读取了一个字段, 然后 T2 更新了该字段. 之后, T1再次读取同一个字段, 值就不同了.  
3. 幻读: 对于两个事物 T1, T2, T1 从一个表中读取了一个字段, 然后 T2 在该表中插入了一些新的行. 之后, 如果 T1 再次读取同一个表, 就会多出几行.

数据库的4个事物隔离级别
---
√：可能会出现
×：为不会出现

| name | 名称 | 级别 | 脏读 | 不可重复读 | 幻读 |
|:-----|:-----|:-----|:-----|:-----|:-----|
| Read uncommitted |  读未提交 | 1 | √ | √ | √ |
| Read committed | 读提交 | 2 | × | √ | √  |
| Repeatable read | 重复读 | 3 | × | × | √  |
| Serializable | 序列化 | 4 | × | × | × |

oracle
---
Oracle 支持的 2 种事务隔离级别：READ COMMITED, SERIALIZABLE.

Oracle 默认的事务隔离级别为: READ COMMITED

mysql
---
Mysql 支持 4 中事务隔离级别.

Mysql 默认的事务隔离级别为: REPEATABLE READ

延伸
---
参考：https://www.cnblogs.com/andy6/p/6045679.html