---
title: Oracle性能优化常用的SQL
keywords: [Oracle性能优化常用的SQL,oracle优化]
date: 2017/3/29 10:00
tags: [oracle]
categories: oracle
---

使用如下sql能查出相应的信息，oracle博大精深要掌握得花时间

太长了，来个阅读全文吧
<!--more-->
```
--显示数据库当前连接数
select count(*) from v$process;

--显示数据库最大连接数
select value from v$parameter where name ='processes';

--修改最大Oracle最大连接数:
alter system set processes = 300 scope = spfile;

--显示当前session连接数
select count(*) from v$session;

--查看当前有哪些用户正在使用数据
SELECT osuser, a.username,cpu_time/executions/1000000||'s', sql_fulltext,machine from v$session a, v$sqlarea b where a.sql_address =b.address order by cpu_time/executions desc;

--查看连接oracle的所有机器的连接数
select machine,count(*) from v$session group by machine;

--查看连接oracle的所有机器的连接数和状态
select machine,status,count(*) from v$session group by machine,status order by status;

--查看消耗磁盘读取最多的SQL Top 5：
select disk_reads,sql_text,SQL_FULLTEXT
from (select sql_text,disk_reads,SQL_FULLTEXT,
   dense_rank() over
     (order by disk_reads desc) disk_reads_rank
   from v$sql)
where disk_reads_rank <=5;

--开始
--通过linux中消耗资源高的进程号获取oracle消耗资源的sql语句：
--1.linux中使用top命名查看oracle进程中消耗资源最高的进程号；
--2.oracle中使用命令：
select c.spid,a.p1,a.p1raw,a.p2,a.event,b.sql_text,b.SQL_FULLTEXT,b.SQL_ID 
from v$session a,v$sql b,v$process c 
where a.wait_class<>'Idle' and a.sql_id=b.sql_id and a.PADDR=c.addr 
order by event;
--3.查询结果显示出各个sql语句对应的进程号，从中找出top命令中对应消耗资源高的进程号即可找到相应的sql语句。
--结束

--判断回滚段竞争的SQL语句：（当Ratio大于2时存在回滚段竞争，需要增加更多的回滚段）
select rn.name, rs.GETS, rs.WAITS, (rs.WAITS / rs.GETS) * 100 ratio
from v$rollstat rs, v$rollname rn
where rs.USN = rn.usn;

---判断恢复日志竞争的SQL语句：（immediate_contention或wait_contention的值大于1时存在竞争）
select name,
(t.IMMEDIATE_MISSES / decode((t.IMMEDIATE_GETS + t.IMMEDIATE_MISSES),0,-1,(t.IMMEDIATE_GETS + t.IMMEDIATE_MISSES))) * 100 immediate_contention,
(t.MISSES / decode((t.GETS + t.MISSES), 0, -1, (t.GETS + t.MISSES))) * 100 wait_contention
from v$latch t
where name in ('redo copy', 'redo allocation');

--判断表空间碎片：（如果最大空闲空间占总空间很大比例则可能不存在碎片，如果比例较小，且有许多空闲空间，则可能碎片很多）
select t.tablespace_name,
sum(t.bytes),
max(t.bytes),
count(*),
max(t.bytes) / sum(t.bytes) radio
from dba_free_space t
group by t.tablespace_name
order by t.tablespace_name;

--确定命中排序域的次数：
select t.NAME, t.VALUE from v$sysstat t where t.NAME like 'sort%'

--查看当前SGA值：SGA(System Global Area)系统全局区。这是一个非常庞大的内存区间
select * from v$sga;

--确定高速缓冲区命中率：（如果命中率低于70％，则应该加大init.ora参数中的DB_BLOCK_BUFFER的值）
select 1 - sum(decode(name, 'physical reads', value, 0)) /
(sum(decode(name, 'db block gets', value, 0)) +
sum(decode(name, 'consistent gets', value, 0))) hit_ratio
from v$sysstat t
where name in ('physical reads', 'db block gets', 'consistent gets');

--确定共享池中的命中率：（如果ratio1大于1时，需要加大共享池，如果ratio2大于10％时，需要加大共享池SHARED_POOL_SIZE）
select * from
(
  select sum(pins) pins,
  sum(reloads) reloads,
  (sum(reloads) / sum(pins)) * 100 ratio1
  from v$librarycache
),
(
  select sum(gets) gets,
  sum(getmisses) getmisses,
  (sum(getmisses) / sum(gets)) * 100 ratio2
  from v$rowcache
)

--查询INIT.ORA参数：
select * from v$parameter;

--求当前会话的SID，SERIAL#
SELECT Sid, Serial# FROM V$session
WHERE Audsid = Sys_Context('USERENV', 'SESSIONID');

--查询session的OS进程ID(有输入)
SELECT p.Spid "OS Thread", b.NAME "Name-User", s.Program, s.Sid, s.Serial#,s.Osuser, s.Machine
FROM V$process p, V$session s, V$bgprocess b
WHERE p.Addr = s.Paddr
AND p.Addr = b.Paddr And (s.sid=&1 or p.spid=&1)
UNION ALL
SELECT p.Spid "OS Thread", s.Username "Name-User", s.Program, s.Sid,s.Serial#, s.Osuser, s.Machine
FROM V$process p, V$session s
WHERE p.Addr = s.Paddr
And (s.sid=&1 or p.spid=&1)
AND s.Username IS NOT NULL;

--查看锁（lock）情况
SELECT /*+ RULE */ Ls.Osuser Os_User_Name, Ls.Username User_Name,Decode(Ls.TYPE,
'RW', 'Row wait enqueue lock', 'TM', 'DML enqueue lock','TX', 'Transaction enqueue lock', 'UL', 'User supplied lock') Lock_Type,o.Object_Name OBJECT,Decode(Ls.Lmode,1, NULL, 2, 'Row Share', 3, 'Row Exclusive',
4, 'Share', 5, 'Share Row Exclusive', 6, 'Exclusive',NULL) Lock_Mode,o.Owner, Ls.Sid, Ls.Serial# Serial_Num, Ls.Id1, Ls.Id2 FROM Sys.Dba_Objects o, 
(SELECT s.Osuser, s.Username, l.TYPE, l.Lmode, s.Sid, s.Serial#, l.Id1,l.Id2 FROM V$session s, V$lock l
WHERE s.Sid = l.Sid) Ls
WHERE o.Object_Id = Ls.Id1
AND o.Owner <> 'SYS'
ORDER BY o.Owner, o.Object_Name;

--查看等待（wait）情况
SELECT Ws.CLASS, Ws.COUNT COUNT, SUM(Ss.VALUE) Sum_Value
FROM V$waitstat Ws, V$sysstat Ss
WHERE Ss.NAME IN ('db block gets', 'consistent gets')
GROUP BY Ws.CLASS, Ws.COUNT;

--求process/session的状态
SELECT p.Pid, p.Spid, s.Program, s.Sid, s.Serial#
FROM V$process p, V$session s
WHERE s.Paddr = p.Addr;

--查看表空间的名称及大小
select t.tablespace_name, round(sum(bytes/(1024*1024)),0) ts_size
from dba_tablespaces t, dba_data_files d
where t.tablespace_name = d.tablespace_name
group by t.tablespace_name;

--查看表空间物理文件的名称及大小
select tablespace_name, file_id, file_name,
round(bytes/(1024*1024),0) total_space
from dba_data_files
order by tablespace_name;

--查看回滚段名称及大小
select segment_name, tablespace_name, r.status,
(initial_extent/1024) InitialExtent,(next_extent/1024) NextExtent,

--查看控制文件
select name from v$controlfile;

--查看日志文件
select member from v$logfile;

--查看表空间的使用情况
select sum(bytes)/(1024*1024) as free_space,tablespace_name
from dba_free_space
group by tablespace_name;
SELECT A.TABLESPACE_NAME,A.BYTES TOTAL,B.BYTES USED, C.BYTES FREE,
(B.BYTES*100)/A.BYTES "% USED",(C.BYTES*100)/A.BYTES "% FREE"
FROM SYS.SM$TS_AVAIL A,SYS.SM$TS_USED B,SYS.SM$TS_FREE C
WHERE A.TABLESPACE_NAME=B.TABLESPACE_NAME AND A.TABLESPACE_NAME=C.TABLESPACE_NAME;

--捕捉运行很久的SQL
select username,sid,opname,
round(sofar*100 / totalwork,0) || '%' as progress,
time_remaining,sql_text
from v$session_longops , v$sql
where time_remaining <> 0
and sql_address = address
and sql_hash_value = hash_value

--耗资源的进程（top session）
select s.schemaname schema_name, decode(sign(48 - command), 1,
to_char(command), 'Action Code #' || to_char(command) ) action, status
session_status, s.osuser os_user_name, s.sid, p.spid , s.serial# serial_num,
nvl(s.username, '[Oracle process]') user_name, s.terminal terminal,
s.program program, st.value criteria_value from v$sesstat st, v$session s , v$process p
where st.sid = s.sid and st.statistic# = to_number('38') and ('ALL' = 'ALL'
or s.status = 'ALL') and p.addr = s.paddr order by st.value desc, p.spid asc, s.username asc, s.osuser asc

--查看有哪些实例在运行：
select * from v$active_instances;
```