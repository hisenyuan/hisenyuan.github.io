---
title: Oracle中exists在update中无法限制住条件，在select中可以
keywords: [oracle,exists]
date: 2017/3/23 17:12
tags: [oracle,mysql]
categories: sql
---

这个问题是自己写的一个bug，标示不知道原因是什么

现在暂时使用 in 代替解决了

下面的查询是能限制住acct_type
```
SELECT ew.customer_id,cf.acct_type
FROM ew_quota_info ew,
     cf_customer cf
WHERE cf.acct_type in(2,3)
  AND ew.customer_id = cf.id
```
但是在update的时候,会把acct_type=1的也更新了
```
update ew_quota_info ew
set ew.all_amt = 100000
where exists(
SELECT ew.customer_id
FROM ew_quota_info ew,
     cf_customer cf
WHERE cf.acct_type in(2,3)
  AND ew.customer_id = cf.id
)
```