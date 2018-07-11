---
title: SQL语句的执行顺序 - 关键字的顺序
keywords: [sql语句的执行顺序,sql中关键字的执行顺序]
date: 2017/11/14 23:01
tags: [sql,java]
categories: sql
---
sql执行顺序
---
1. from 
2. join 
3. on 
4. where 
5. group by(开始使用select中的别名，后面的语句中都可以使用)
6. avg,sum.... 
7. having 
8. select 
9. distinct 
10. order by

参考资料
---
http://blog.csdn.net/u014044812/article/details/51004754

http://blog.csdn.net/bitcarmanlee/article/details/51004767 (含流程图)