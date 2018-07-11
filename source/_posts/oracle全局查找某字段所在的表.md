---
title: oracle全局查找某字段所在的表
keywords: [oracle全局查找某字段所在的表,oracle定位字段所在的表,定位某字段值所在的表]
date: 2017/6/12 14:13
tags: [oracle]
categories: sql
---
下面这个存储过程是查找数据库中某个字段值为：hisen 的表
```
declare
l_cnt    varchar2(20);
v_sql  varchar2(4000);
v_tablename varchar(200);
  cursor cursor_jsdx is select 'select count(*) from ' || table_name || ' where NAME=''hisen''',table_name from user_tab_columns where column_name='NAME';
  --注：这里的字段名要大写 
begin
     open cursor_jsdx;
     Loop
            fetch cursor_jsdx into v_sql,v_tablename;
            exit when cursor_jsdx%notfound;
            execute immediate v_sql       into l_cnt;           
         if l_cnt >0 then   
         ---如果该表有那内容的就打印那个表的名字。
            dbms_output.put_line(v_tablename);
         end if;
     end loop;
    Close cursor_jsdx;
end;
```