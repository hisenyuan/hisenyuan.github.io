---
title: MySQL一些简单的语句
date: 2017-01-20 01:23:17
tags: mysql
---
emlog_ad字段
id
status
position
title
weight
content

```+sql
--找出重复
SELECT a.*
FROM emlog_ad a
WHERE a.id IN
    (SELECT b.id id
     FROM emlog_ad b
     GROUP BY b.title
     HAVING count(b.id)>1);

--删除重复留下id最小的
SELECT a.*
FROM emlog_ad a
WHERE a.id IN
    (SELECT b.id
     FROM emlog_ad b
     GROUP BY b.title
     HAVING count(b.id)>1)
  AND a.id NOT IN
    (SELECT min(c.id)
     FROM emlog_ad c
     GROUP BY c.title
     HAVING count(c.id)>1);

--一句sql把所有AA改为BB，CC改为DD
UPDATE emlog_ad a
SET a.`status`=(
CASE
	WHEN a.`status` = '1' THEN '11'
	WHEN a.`status`='2' THEN '22'
	END
);
```