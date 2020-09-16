---
title: MySQL分页查询
keywords: [mysql,分页查询,page query,分页]
date: 2020/07/12 23:18
tags: [mysql]
categories: mysql
---
# 零、分页查询
分页查询一般都会想到 offset limit

## 问题1
当不停机需要全量同步数据时
有可能会漏掉或者重复处理很多数据
因为中途会有数据改动
每次分页内的数据与预期的会有出入

## 问题2
offset 会进行全表扫描

# 一、推荐方案
通过不变的字段进行排序
最好是使用递增的主键索引

```mysl
select *
  from hisen
where id > ${lastId}
  order by id
limit ${pageSize};
```

# 二、参考
<!--more-->
[请不要将OFFSET和LIMIT用于分页](https://hackernoon.com/please-dont-use-offset-and-limit-for-your-pagination-8ux3u4y)
