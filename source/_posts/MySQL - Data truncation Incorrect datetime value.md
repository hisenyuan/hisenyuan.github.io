---
title: MySQL - Data truncation:Incorrect datetime value
keywords: [mysql,Incorrect datetime value]
date: 2019/2/1 18:13
tags: [sql,mysql]
categories: sql
---
### 一、问题
```
com.mysql.jdbc.MysqlDataTruncation: Data truncation: Incorrect datetime value: '2039-01-07 12:58:20.625' for column
```
### 二、原因
由于程序没有控制好，计算下一次更新时间失误，造成数值过大。
```
update table_a
SET UPDATE_TIME = '2039-01-07 12:58:20.625'
WHERE ID = 6241
```
以下是MySQL官方的说法，就是时间超过了范围。
```
The TIMESTAMP data type is used for values that contain both date and time parts.
TIMESTAMP has a range of '1970-01-01 00:00:01' UTC to '2038-01-19 03:14:07' UTC.
```
