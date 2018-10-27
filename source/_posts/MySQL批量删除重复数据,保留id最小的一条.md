---
title: MySQL批量删除重复数据,保留id最小的一条
keywords: [mysql,mysql删除重复数据,mysql去重]
date: 2018/10/16 10:08
tags: [mysql批量删除重复数据]
categories: sql
---
stu数据如下：

id | name
---|---
1 | hisen
2 | hisen
3 | hisen
4 | hisenyuan
5 | hisenyuan

删除重复的name，保留id最小的。

思路就是先找出重复数据，然后再找出需要保留的数据(重复中id最小的)

然后删除id不在需要保留的id中的所有数据

```sql
delete
from stu
where id not in (select t.minId
                 from (select min(id) minId from stu group by name having count(name) > 1) t)
```
