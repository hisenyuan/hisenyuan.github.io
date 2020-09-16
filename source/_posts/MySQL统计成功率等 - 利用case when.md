---
title: MySQL统计成功率等 - 利用case when
keywords: [mysql,case when,成功率]
date: 2019/2/18 10:13
tags: [sql,mysql]
categories: sql
---
CONCAT:连接字符串,CONCAT(55.00,'%')->55.00%
truncate:处理小数点位数,truncate(10.1111,2)->10.11

### 统计sql
```sql
select res.*, CONCAT(truncate((res.succ / res.`all`) * 100, 2), '%') as 'success rate'
from (select tpc.USER_NO,
             tpc.TYPE,
             count(*)                                                            as 'all',
             sum(case when tpc.STATUS = 0 then 1 else 0 end)                     as 'succ',
             sum(case when tpc.STATUS = 1 then 1 else 0 end)                     as 'fail',
             sum(case when tpc.STATUS != 1 && tpc.STATUS != 0 then 1 else 0 end) as 'other'
      from t_hisen tpc
      where tpc.DATE between '20190216' and '20190217'
      group by tpc.USER_NO, tpc.TYPE
      order by tpc.USER_NO) res
```
### 统计结果
| USER_NO | TYPE | all | succ | fail | other | success rate |
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
| 456 | 10 | 1 | 0 | 0 | 1 | 0.00% |
| 123 | 10 | 29 | 23 | 0 | 6 | 79.31% |
