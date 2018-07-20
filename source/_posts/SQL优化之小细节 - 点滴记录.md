---
title: SQL优化之小细节 - 点滴记录
keywords: [sql,sql优化]
date: 2017/3/9 10:38
tags: [sql,java]
categories: sql
---
不知道现在是不是还很多人首先就把关联的id放在where的第一位

这里有一个简单的对比，情况相同的时候，两个sql的时间相差八倍

优：0.077s
```sql
SELECT ew.all_amt ,
       ew.customer_id,
       cf.id,
       cf.cert_id,
       cf.acct_type
FROM ew_quota_info ew,
     cf_customer cf
WHERE cf.acct_type in(2,3)
  AND ew.customer_id = cf.id
```
劣：0.630s
```sql
SELECT ew.all_amt ,
       ew.customer_id,
       cf.id,
       cf.cert_id,
       cf.acct_type
FROM ew_quota_info ew,
     cf_customer cf
WHERE ew.customer_id = cf.id
  AND cf.acct_type in(2,3)
```
上述原因：where子句从后往前执行，应该把大的过滤条件放在后面
记录时间：2017年3月9日 10:44:59

---
