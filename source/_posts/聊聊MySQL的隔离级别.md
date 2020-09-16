---
title: 聊聊MySQL的隔离级别 | MySQL隔离级别原理
keywords: [mysql,mysql隔离级别,mysql隔离级别原理,脏读,不可重复读,幻读]
date: 2019/4/13 16:32
tags: [mysql]
categories: sql
---
这是之前的一个帖子：<a href="https://hisen.me/20171114-oracle%20-%20mysql%20-%20%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB%E4%BB%8B%E7%BB%8D/" target="_blank">oracle - mysql - 数据库事务隔离级别介绍</a>
写的不全面， 按现在的理解，重新写一个；

# 一、名词解释
脏读：在一个查询事务过程中，读到了其它事务没有提交的数据；
不可重复读：一个事务查询过程中，多次查询得到了不一致的结果，原因是：有别的更新事务提交了；
幻读：一个事务查询过程中，多次查询得到了不一致的结果，原因是：有别的删除事务/插入事务提交了；

# 二、数据库隔离级别
| name | 名称 | 脏读 | 不可重复读 | 幻读 | 加锁读 |
|:-----|:-----|:-----|:-----|:-----|:-----| :-----|
| Read uncommitted |  读未提交 | Yes | Yes | Yes | No |
| Read committed | 读已提交 | No | Yes | Yes  | No |
| Repeatable read | 可重复读 | No | No | Yes  | No |
| Serializable | 序列化 | No | No | No | Yes |

默认的隔离级别为：RR，原因：5.1之后版本，如果Binlogog开启语句级别，必须为RR，RC可能会导致Binlog数据错误([详情](https://blog.csdn.net/shudaqi2010/article/details/79958222));

# 三、控制方式
读未提交：每次都是读数据最新的版本(包括事务未提交的数据)；
读已提交：MVCC控制；
可重复读：MVCC控制；
序列化：在读取的每一行上加锁，只能按顺序进行读写；

# 四、InnoDB MVCC(多版本并发控制)原理
<!--more-->
InnoDB引擎下的MVCC，是通过在每一行上增加两个隐藏列实现的；
每发生一个新的事务，系统版本都会递增；
当UPDATE数据的时候，都是先copy出来一行操作，不影响原表数据，提交之后才影响；

一个保存创建时间(非时间值，而是系统版本号)；
一个保存删除时间(非时间值，而是系统版本号)；

## 4.1 SELECT
当前查询事务系统ID：5
结果条件：
1. CREATE VERSION <= 5(RR级别为 <= 5,RC级别应该是直接取最大值)；
2. DELETE VERSION == null or DELETE VERSION > 5 (RR级别应该只有为空，RC级别可能有 > 5的情况)；

1可以保证读取到的行，在事务开始之前就已经存在，要么是事务自身插入或者修改的；
2可以保证读取到的行，在事务开始之前未被删除；

## 4.2 INSERT
为新插入的每一行保存当前系统版本号作为CREATE VERSION；

## 4.3 DELETE
为删除的每一行保存当前系统版本号作为DELETE VERSION；

## 4.4 UPDATE
插入一行新的记录，保存当前系统版本号作为CREATE VERSION；
同事保存当前系统版本号到原来的行上作为DELETE VERSION；

# 五、参考：
1. 《高性能MySQL》
2. <a href="https://www.cnblogs.com/huanongying/p/7021555.html" target="_blank">MySQL的四种事务隔离级别</a>(ps:此处有测试截图，比较直观)
