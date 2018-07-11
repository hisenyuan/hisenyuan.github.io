---
title: Mysql Employees Database练习题&答案
keywords: [Mysql Employees,Mysql Employees题目,Mysql Employees练习]
date: 2017/4/10 19:07
tags: [mysql]
categories: sql
---
Employees数据库是mysql官方提供的一个测试用数据库

里面含有几十万条数据。

找了好久也没有找到比较匹配的题目

就找了个匹配度比较高的题来练习，如果你还没有导入Employees Sample Database

**请参考：<a href="http://hisen.me/20170401-mysql%E6%95%B0%E6%8D%AE%E5%BA%93%E5%AE%89%E8%A3%85%E5%AE%98%E6%96%B9%E8%87%AA%E5%B8%A6employees%E6%B5%8B%E8%AF%95%E8%A1%A8/" target="_blank">点击导入Employees</a>**

本次操作在Xshell中完成，也就是mysql命令行。

简单的操作
---

```
#登陆数据库
hisen@ubuntu:/$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 5.7.17-0ubuntu0.16.04.2 (Ubuntu)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| employees          |
| log4j              |
| mysql              |
| performance_schema |
| ssm                |
| sys                |
+--------------------+
7 rows in set (0.00 sec)

mysql> use employees
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+----------------------+
| Tables_in_employees  |
+----------------------+
| current_dept_emp     |
| departments          |
| dept_emp             |
| dept_emp_latest_date |
| dept_manager         |
| employees            |
| salaries             |
| titles               |
+----------------------+
8 rows in set (0.00 sec)

mysql> select * from employees limit 10;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
|  10001 | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
|  10002 | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |
|  10003 | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |
|  10004 | 1954-05-01 | Chirstian  | Koblick   | M      | 1986-12-01 |
|  10005 | 1955-01-21 | Kyoichi    | Maliniak  | M      | 1989-09-12 |
|  10006 | 1953-04-20 | Anneke     | Preusig   | F      | 1989-06-02 |
|  10007 | 1957-05-23 | Tzvetan    | Zielinski | F      | 1989-02-10 |
|  10008 | 1958-02-19 | Saniya     | Kalloufi  | M      | 1994-09-15 |
|  10009 | 1952-04-19 | Sumant     | Peac      | F      | 1985-02-18 |
|  10010 | 1963-06-01 | Duangkaew  | Piveteau  | F      | 1989-08-24 |
+--------+------------+------------+-----------+--------+------------+
10 rows in set (0.00 sec)
#统计各部门曾经拥有的员工数量
mysql> select dept_no,count(*) emp_sum
    -> from dept_emp
    -> group by dept_no
    -> order by emp_sum desc;
+---------+---------+
| dept_no | emp_sum |
+---------+---------+
| d005    |   85707 |
| d004    |   73485 |
| d007    |   52245 |
| d009    |   23580 |
| d008    |   21126 |
| d001    |   20211 |
| d006    |   20117 |
| d003    |   17786 |
| d002    |   17346 |
+---------+---------+
9 rows in set (0.63 sec)
#创建视图
mysql> CREATE VIEW test AS
    -> SELECT dept_no, COUNT(*) AS emp_sum
    -> FROM dept_emp
    -> GROUP BY dept_no
    -> ORDER BY emp_sum DESC
    -> ;
Query OK, 0 rows affected (0.16 sec)
mysql> select * from test;
+---------+---------+
| dept_no | emp_sum |
+---------+---------+
| d005    |   85707 |
| d004    |   73485 |
| d007    |   52245 |
| d009    |   23580 |
| d008    |   21126 |
| d001    |   20211 |
| d006    |   20117 |
| d003    |   17786 |
| d002    |   17346 |
+---------+---------+
9 rows in set (0.11 sec)
#联合查询，加上部门名称
mysql> select test.dept_no,emp_sum,dept_name
    -> from test,departments
    -> where test.dept_no = departments.dept_no;
+---------+---------+--------------------+
| dept_no | emp_sum | dept_name          |
+---------+---------+--------------------+
| d009    |   23580 | Customer Service   |
| d005    |   85707 | Development        |
| d002    |   17346 | Finance            |
| d003    |   17786 | Human Resources    |
| d001    |   20211 | Marketing          |
| d004    |   73485 | Production         |
| d006    |   20117 | Quality Management |
| d008    |   21126 | Research           |
| d007    |   52245 | Sales              |
+---------+---------+--------------------+
9 rows in set (0.16 sec)
```

练习题目和答案：
---
建议看看输出的结果自己写下sql，不要单纯的复制粘贴。

因为数据量比较大， 很多时候我都加了5条数据的限制。
<!--more-->
```
#1.查找整个职员表的所有内容。
mysql> select * from employees limit 5;
+--------+------------+------------+-----------+--------+------------+
| emp_no | birth_date | first_name | last_name | gender | hire_date  |
+--------+------------+------------+-----------+--------+------------+
|  10001 | 1953-09-02 | Georgi     | Facello   | M      | 1986-06-26 |
|  10002 | 1964-06-02 | Bezalel    | Simmel    | F      | 1985-11-21 |
|  10003 | 1959-12-03 | Parto      | Bamford   | M      | 1986-08-28 |
|  10004 | 1954-05-01 | Chirstian  | Koblick   | M      | 1986-12-01 |
|  10005 | 1955-01-21 | Kyoichi    | Maliniak  | M      | 1989-09-12 |
+--------+------------+------------+-----------+--------+------------+
5 rows in set (0.00 sec)

#2.查看雇员名字(last_name)。
mysql> select last_name from employees limit 5;
+-----------+
| last_name |
+-----------+
| Facello   |
| Simmel    |
| Bamford   |
| Koblick   |
| Maliniak  |
+-----------+
5 rows in set (0.00 sec)

#3.查看雇员编号、名字和部门
mysql> select e.last_name,e.emp_no,d.dept_name from employees e,departments d,dept_emp de where e.emp_no = de.emp_no and de.dept_no = d.dept_no limit 5;
+-------------+--------+------------------+
| last_name   | emp_no | dept_name        |
+-------------+--------+------------------+
| Sluis       |  10011 | Customer Service |
| Lortz       |  10038 | Customer Service |
| Tramer      |  10049 | Customer Service |
| Billingsley |  10060 | Customer Service |
| Syrzycki    |  10088 | Customer Service |
+-------------+--------+------------------+
5 rows in set (0.00 sec)

#4.显示所有雇员的工号、姓名、工资
mysql> select e.emp_no,concat(e.last_name,' ',e.first_name) name,s.salary from employees e,salaries s where e.emp_no=s.emp_no limit 5;
+--------+----------------+--------+
| emp_no | name           | salary |
+--------+----------------+--------+
|  10001 | Facello Georgi |  60117 |
|  10001 | Facello Georgi |  62102 |
|  10001 | Facello Georgi |  66074 |
|  10001 | Facello Georgi |  66596 |
|  10001 | Facello Georgi |  66961 |
+--------+----------------+--------+
5 rows in set (0.00 sec)

#5.查找在d005号部门工作的雇员
mysql> select e.emp_no,concat(e.last_name,' ',e.first_name) name,d.dept_no,d.dept_name from employees e,dept_emp de,departments d where e.emp_no = de.emp_no and de.dept_no = d.dept_no and d.dept_no = 'd005' limit 5;
+--------+--------------------+---------+-------------+
| emp_no | name               | dept_no | dept_name   |
+--------+--------------------+---------+-------------+
|  10001 | Facello Georgi     | d005    | Development |
|  10006 | Preusig Anneke     | d005    | Development |
|  10008 | Kalloufi Saniya    | d005    | Development |
|  10012 | Bridgland Patricio | d005    | Development |
|  10014 | Genin Berni        | d005    | Development |
+--------+--------------------+---------+-------------+
5 rows in set (0.05 sec)

#6.要求查找职位为Engineer和Senior Engineer的雇员姓名(last_name)
mysql> select e.last_name from employees e,titles t where t.emp_no=e.emp_no and t.title in('Engineer','Senior Engineer') limit  5;
+-----------+
| last_name |
+-----------+
| Facello   |
| Bamford   |
| Koblick   |
| Koblick   |
| Preusig   |
+-----------+
5 rows in set (0.00 sec)

#7.查找职位不是Engineer和Senior Engineer的部门编号，雇员部门及姓名。将姓名显示为(first_name+last_name命名为”Name”)
mysql> select d.dept_no,d.dept_name,concat(e.first_name,' ',e.last_name) name from employees e,titles t,dept_emp de,departmentss d where e.emp_no=de.emp_no and de.dept_no = d.dept_no and t.emp_no = e.emp_no and t.title not in('Engineer','Senior Engineer'') limit 5;
+---------+------------------+--------------+
| dept_no | dept_name        | name         |
+---------+------------------+--------------+
| d009    | Customer Service | Mary Sluis   |
| d009    | Customer Service | Huan Lortz   |
| d009    | Customer Service | Huan Lortz   |
| d009    | Customer Service | Basil Tramer |
| d009    | Customer Service | Basil Tramer |
+---------+------------------+--------------+
5 rows in set (0.00 sec)

#8.查找哪些雇员的工资在60000到90000之间
mysql> select concat(e.first_name,' ',e.last_name) name,s.salary from employees e,salaries s where s.salary between 60000 and 90000 and e.emp_no = s.emp_no limit 5;
+----------------+--------+
| name           | salary |
+----------------+--------+
| Georgi Facello |  60117 |
| Georgi Facello |  62102 |
| Georgi Facello |  66074 |
| Georgi Facello |  66596 |
| Georgi Facello |  66961 |
+----------------+--------+
5 rows in set (0.00 sec)

#9.查找哪些雇员的工资不在60000到90000之间
mysql> select concat(e.first_name,' ',e.last_name) name,s.salary from employees e,salaries s where s.salary not between 60000 and 90000 and e.emp_no = s.emp_no limit 10;
+-------------------+--------+
| name              | salary |
+-------------------+--------+
| Parto Bamford     |  40006 |
| Parto Bamford     |  43616 |
| Parto Bamford     |  43466 |
| Parto Bamford     |  43636 |
| Parto Bamford     |  43478 |
| Parto Bamford     |  43699 |
| Parto Bamford     |  43311 |
| Chirstian Koblick |  40054 |
| Chirstian Koblick |  42283 |
| Chirstian Koblick |  42542 |
+-------------------+--------+
10 rows in set (0.00 sec)

#10.查找first_name以P开头，后面仅有四个字母的雇员信息
mysql> select e.emp_no,concat(e.first_name,' ',e.last_name) name from employees e where e.first_name like 'P____' and e.first_name not like 'p__' limit 5;
+--------+---------------------+
| emp_no | name                |
+--------+---------------------+
|  10003 | Parto Bamford       |
|  10101 | Perla Heyers        |
|  10138 | Perry Shimshoni     |
|  10353 | Phule Hammerschmidt |
|  10387 | Parto Wrigley       |
+--------+---------------------+
5 rows in set (0.00 sec)

#11.查找last_name以K开头的雇员信息
mysql> select e.emp_no,concat(e.first_name,' ',e.last_name) name from employees e where e.first_name like 'K%' limit 5; 
+--------+----------------------+
| emp_no | name                 |
+--------+----------------------+
|  10005 | Kyoichi Maliniak     |
|  10016 | Kazuhito Cappelletti |
|  10018 | Kazuhide Peha        |
|  10031 | Karsten Joslin       |
|  10066 | Kwee Schusler        |
+--------+----------------------+
5 rows in set (0.00 sec)

#12.查找名字以字母K开头，以i结尾，并且第三个字母为o的雇员名字(First_name)、职位和所在部门号
mysql> select concat(e.first_name,' ',e.last_name) name,t.title,de.dept_no from employees e,dept_emp de,titles t where e.first_name like 'K_o%i' and e.emp_no = t.emp_no and e.emp_no = de.emp_no limit 5;
+------------------+--------------+---------+
| name             | title        | dept_no |
+------------------+--------------+---------+
| Kyoichi Maliniak | Senior Staff | d003    |
| Kyoichi Maliniak | Staff        | d003    |
| Kyoichi Wossner  | Staff        | d007    |
| Kyoichi Flexer   | Senior Staff | d007    |
| Kyoichi Flexer   | Staff        | d007    |
+------------------+--------------+---------+
5 rows in set (0.00 sec)

#13.查找哪些雇员的职位名不以Se开头
mysql> select concat(e.first_name,' ',last_name) name,t.title from employees e,titles t where e.emp_no = t.emp_no and t.title not like 'Se%' limit 5;
+-------------------+--------------------+
| name              | title              |
+-------------------+--------------------+
| Bezalel Simmel    | Staff              |
| Chirstian Koblick | Engineer           |
| Kyoichi Maliniak  | Staff              |
| Tzvetan Zielinski | Staff              |
| Saniya Kalloufi   | Assistant Engineer |
+-------------------+--------------------+
5 rows in set (0.00 sec)

#14.查找d005号部门里不是Staff的雇员信息
mysql> select concat(e.first_name,' ',last_name) name,t.title from employees e,dept_emp de,titles t where e.emp_no = de.emp_no  and e.emp_no = t.emp_no and t.title != 'Staff' limit 5;
+-------------------+-----------------+
| name              | title           |
+-------------------+-----------------+
| Georgi Facello    | Senior Engineer |
| Parto Bamford     | Senior Engineer |
| Chirstian Koblick | Engineer        |
| Chirstian Koblick | Senior Engineer |
| Kyoichi Maliniak  | Senior Staff    |
+-------------------+-----------------+
5 rows in set (0.00 sec)

#15.查找d005号部门工资大于100000的员工的信息
mysql> select concat(e.first_name,' ',last_name) name,t.title,s.salary from employees e,dept_emp de,titles t,salaries s where e.emp_no = de.emp_no and e.emp_no = t.emp_no and e.emp_no = s.emp_no and de.dept_no = 'd005' and s.salary > 100000 limit 5;
+---------------+--------------------+--------+
| name          | title              | salary |
+---------------+--------------------+--------+
| Kwee Schusler | Assistant Engineer | 102425 |
| Kwee Schusler | Assistant Engineer | 102674 |
| Kwee Schusler | Assistant Engineer | 103672 |
| Kwee Schusler | Engineer           | 102425 |
| Kwee Schusler | Engineer           | 102674 |
+---------------+--------------------+--------+
5 rows in set (0.00 sec)

#16.按字母顺序显示雇员的名字(last_name)
mysql> select concat(e.first_name,' ',last_name) name from employees e order by e.last_name limit 5;
+------------------+
| name             |
+------------------+
| Aluzio Aamodt    |
| Sachem Aamodt    |
| Sreenivas Aamodt |
| Mokhtar Aamodt   |
| Bartek Aamodt    |
+------------------+
5 rows in set (0.24 sec)

#17.按部门编号降序显示雇员信息
mysql> select concat(e.first_name,' ',last_name) name,de.dept_no from employees e,dept_emp de where e.emp_no = de.emp_no order by de.dept_no desc limit 5;
+-------------------+---------+
| name              | dept_no |
+-------------------+---------+
| Pohua Sichman     | d009    |
| Uri Juneja        | d009    |
| Mohammed Pleszkun | d009    |
| Chiranjit Kuzuoka | d009    |
| Ronghao Morrow    | d009    |
+-------------------+---------+
5 rows in set (0.00 sec)

#18.计算每个部门的平均工资和工资总和
mysql> select de.dept_no,sum(s.salary),avg(s.salary) from employees e,dept_emp de,salaries s where e.emp_no = de.emp_no and e.emp_no = s.emp_no group by de.dept_no;
+---------+---------------+---------------+
| dept_no | sum(s.salary) | avg(s.salary) |
+---------+---------------+---------------+
| d001    |   13725425266 |    71913.2000 |
| d002    |   11650834677 |    70489.3649 |
| d003    |    9363811425 |    55574.8794 |
| d004    |   41554438942 |    59605.4825 |
| d005    |   48179456393 |    59478.9012 |
| d006    |   10865203635 |    57251.2719 |
| d007    |   40030089342 |    80667.6058 |
| d008    |   11969730427 |    59665.1817 |
| d009    |   13143639841 |    58770.3665 |
+---------+---------------+---------------+
9 rows in set (7.14 sec)

#19.查询每个部门的每个职位的雇员数
mysql> select de.dept_no,t.title,sum(e.emp_no) from employees e,dept_emp de,titles t where e.emp_no = de.emp_no and e.emp_no = t.emp_no group by de.dept_no,t.title limit 5;
+---------+--------------+---------------+
| dept_no | title        | sum(e.emp_no) |
+---------+--------------+---------------+
| d001    | Manager      |        220061 |
| d001    | Senior Staff |    3561178455 |
| d001    | Staff        |    4142508539 |
| d002    | Manager      |        220199 |
| d002    | Senior Staff |    3090249864 |
+---------+--------------+---------------+
5 rows in set (3.13 sec)

#20.请算出employees表中所有雇员的平均工资
mysql> select avg(s.salary) from employees e,salaries s where e.emp_no = s.emp_no;
+---------------+
| avg(s.salary) |
+---------------+
|    63810.7448 |
+---------------+
1 row in set (4.48 sec)

#21.请查询出employees表中的最低工资的员工信息
mysql> select concat(first_name,' ',last_name) name from employees e,salaries s where e.emp_no = s.emp_no and s.salary = (selecct min(s.salary) salary from salaries s);
+--------------+
| name         |
+--------------+
| Olivera Baek |
+--------------+
1 row in set (1.99 sec)

#22.请计算出每个部门的平均工资、最高工资和最低工资
mysql> select de.dept_no,avg(s.salary),max(s.salary),min(s.salary) from employees e,dept_emp de,salaries s where e.emp_no = de.emp_no and e.emp_no = s.emp_no group by de.dept_no;
+---------+---------------+---------------+---------------+
| dept_no | avg(s.salary) | max(s.salary) | min(s.salary) |
+---------+---------------+---------------+---------------+
| d001    |    71913.2000 |        145128 |         39127 |
| d002    |    70489.3649 |        142395 |         38812 |
| d003    |    55574.8794 |        141953 |         38735 |
| d004    |    59605.4825 |        138273 |         38623 |
| d005    |    59478.9012 |        144434 |         38849 |
| d006    |    57251.2719 |        132103 |         38786 |
| d007    |    80667.6058 |        158220 |         39169 |
| d008    |    59665.1817 |        130211 |         38851 |
| d009    |    58770.3665 |        144866 |         38836 |
+---------+---------------+---------------+---------------+
9 rows in set (7.06 sec)

#23.查询薪水发放时间在1986-06-26 ~ 1987-06-25薪水高于46145号雇员并且工种与他相同的雇员情况。
select e.*  from employees e,titles t,salaries s  where e.emp_no = t.emp_no  and e.emp_no = t.emp_no  and t.title = (select title from titles where emp_no = 46135)  and s.salary > (select s.salary from salaries s where s.from_date > str_to_date('1986-06-26','%Y-%m-%d') and s.to_date < str_to_date('1987-06-2 25','%Y-%m-%d') and s.emp_no = '46135')  and s.from_date > str_to_date('1986-06-26','%Y-%m-%d') and s.to_date < str_to_date('1987-06-2 25','%Y-%m-%d') limit 10;
+--------+------------+------------+------------+--------+------------+
| emp_no | birth_date | first_name | last_name  | gender | hire_date  |
+--------+------------+------------+------------+--------+------------+
|  10004 | 1954-05-01 | Chirstian  | Koblick    | M      | 1986-12-01 |
|  10009 | 1952-04-19 | Sumant     | Peac       | F      | 1985-02-18 |
|  10010 | 1963-06-01 | Duangkaew  | Piveteau   | F      | 1989-08-24 |
|  10012 | 1960-10-04 | Patricio   | Bridgland  | M      | 1992-12-18 |
|  10014 | 1956-02-12 | Berni      | Genin      | M      | 1987-03-11 |
|  10018 | 1954-06-19 | Kazuhide   | Peha       | F      | 1987-04-03 |
|  10020 | 1952-12-24 | Mayuko     | Warwick    | M      | 1991-01-26 |
|  10022 | 1952-07-08 | Shahaf     | Famili     | M      | 1995-08-22 |
|  10023 | 1953-09-29 | Bojan      | Montemayor | F      | 1989-12-17 |
|  10026 | 1953-04-03 | Yongqiao   | Berztiss   | M      | 1995-03-20 |
+--------+------------+------------+------------+--------+------------+
10 rows in set, 2 warnings (0.05 sec)


#24.查询工资在10000到50000之间的雇员所在部门的所有人员的信息。
mysql> select e.*,d.dept_no,d.dept_name from employees e,dept_emp de,departments d where e.emp_no = de.emp_no and de.dept_no =d.dept_no and e.emp_no in (select distinct s.emp_no from salaries s where s.salary between 10000 and 50000) limit 10;
+--------+------------+-------------+-----------+--------+------------+---------+------------------+
| emp_no | birth_date | first_name  | last_name | gender | hire_date  | dept_no | dept_name        |
+--------+------------+-------------+-----------+--------+------------+---------+------------------+
|  10011 | 1953-11-07 | Mary        | Sluis     | F      | 1990-01-22 | d009    | Customer Service |
|  10038 | 1960-07-20 | Huan        | Lortz     | M      | 1989-09-20 | d009    | Customer Service |
|  10049 | 1961-04-24 | Basil       | Tramer    | F      | 1992-05-04 | d009    | Customer Service |
|  10098 | 1961-09-23 | Sreekrishna | Servieres | F      | 1985-05-13 | d009    | Customer Service |
|  10112 | 1963-08-13 | Yuichiro    | Swick     | F      | 1985-10-08 | d009    | Customer Service |
|  10115 | 1964-12-25 | Chikara     | Rissland  | M      | 1986-01-23 | d009    | Customer Service |
|  10126 | 1954-08-07 | Kayoko      | Valtorta  | M      | 1985-09-08 | d009    | Customer Service |
|  10128 | 1958-02-15 | Babette     | Lamba     | F      | 1988-06-06 | d009    | Customer Service |
|  10137 | 1959-07-30 | Maren       | Hutton    | M      | 1985-02-18 | d009    | Customer Service |
|  10154 | 1957-01-17 | Abdulah     | Thibadeau | F      | 1990-12-12 | d009    | Customer Service |
+--------+------------+-------------+-----------+--------+------------+---------+------------------+
10 rows in set (1.28 sec)

#25.查询出在薪水发放时间在1986-06-26 ~ 1987-06-25的员工信息
###（工号，姓名，性别，薪水，职位）
mysql> select e.emp_no,concat(e.first_name," ",e.last_name) name,e.gender,s.salary,t.title from employees e,salaries s,titles t where e.emp_no = s.emp_no and e.emp_no = t.emp_no and s.from_date > str_to_date('1986-06-26','%Y-%m-%d') and s.to_date < str_to_date('1987-06-2 25','%Y-%m-%d') limit 10;
+--------+-------------------+--------+--------+-----------------+
| emp_no | name              | gender | salary | title           |
+--------+-------------------+--------+--------+-----------------+
|  13543 | Tua Garigliano    | F      |  78646 | Staff           |
|  13589 | Arfst Munck       | F      |  44306 | Staff           |
|  14085 | Jiafu Constantine | F      |  43575 | Staff           |
|  14613 | Kien Herath       | M      |  54124 | Staff           |
|  19083 | Nobuyoshi Asmuth  | M      |  40000 | Engineer        |
|  19135 | Leaf Soicher      | M      |  40000 | Senior Engineer |
|  19761 | Vasiliy Niizuma   | M      |  65098 | Staff           |
|  19769 | Bezalel Holburn   | M      |  53118 | Engineer        |
|  22892 | Giao Monkewich    | M      |  79554 | Senior Staff    |
|  24101 | Yakichi Manderick | F      |  45092 | Engineer        |
+--------+-------------------+--------+--------+-----------------+
10 rows in set, 1 warning (0.07 sec)
```