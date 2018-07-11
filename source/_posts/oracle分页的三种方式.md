---
title: oracle分页的三种方式
keywords: [oracle分页查询,oracle分页]
date: 2017/4/11 18:48
tags: [oracle]
categories: sql
---

第一种：
```
select *
  from (select t.*, rownum rn
          from (select * from EW_AUTH_FLOW) t
         where rownum <= 5)
 where rn > 2;
```
第二种：
```
select *
  from (select t.*, rownum rn from ew_auth_flow t where rownum <= 5)
 where rn > 2;
```
第三种：
```
select *
  from (select t.*, rownum rn from ew_auth_flow t)
 where rn between 2 and 5;
```