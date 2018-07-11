---
title: mysql数据库安装官方自带employees测试库
keywords: [mysql数据库安装官方自带employees测试库,mysql测试库]
date: 2017/4/1 17:12
tags: [mysql]
categories: mysql
---
mysql官方是有一个自带的数据库，名为：Employees Sample Database

官网介绍：<a href="https://dev.mysql.com/doc/employee/en/" target="_blank">Employees Sample Database</a>

| 表名 | 中文 | 
|:-----|:-----|
| department | 部门表 |
| dept_emp | 部门员工任职期表(按部门&时期) | 
| dept_manager | 部门经理任职期表(按时期) | 
| employees | 员工详情表 | 
| salaries | 员工薪资表(按时期) | 
| title | 员工职称表(按时期) | 

<img src="http://wx2.sinaimg.cn/mw690/b2e389b6ly1fe7b71pwupj20m80muabj.jpg" />
导入的操作过程，在ubuntu上进行操作
<!--more-->

一、导入数据库操作过程
---

```
#ubuntu apt-get 安装的mysql默认的配置文件
#添加一行：default-storage-engine=InnoDB
hisen@ubuntu:/$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
#获取root权限
hisen@ubuntu:/var/lib$ su
Password: 
#进数据库目录
root@ubuntu:# cd /var/lib/mysql
#删除两个文件
root@ubuntu:/var/lib/mysql# rm ib_logfile0 
root@ubuntu:/var/lib/mysql# rm ib_logfile1
#重启数据库
root@ubuntu:/# service mysql restart
#下载Employees database
hisen@ubuntu:~/dl$ wget https://codeload.github.com/datacharmer/test_db/zip/master
#解压
hisen@ubuntu:~/dl$ unzip master
#查看
hisen@ubuntu:~/dl$ ll
total 35840
drwxrwxr-x  3 hisen hisen     4096 Apr  1 16:50 ./
drwxr-xr-x 10 hisen hisen     4096 Apr  1 16:40 ../
-rw-rw-r--  1 hisen hisen 36687570 Apr  1 16:30 master
drwxrwxr-x  4 hisen hisen     4096 Oct 14  2015 test_db-master/
#进目录
hisen@ubuntu:~/dl$ cd test_db-master/
#导入数据库
hisen@ubuntu:~/dl/test_db-master$ mysql -u root -p < employees.sql
Enter password: 
INFO
CREATING DATABASE STRUCTURE
INFO
storage engine: InnoDB
INFO
LOADING departments
INFO
LOADING employees
INFO
LOADING dept_emp
INFO
LOADING dept_manager
INFO
LOADING titles
INFO
LOADING salaries
data_load_time_diff
00:01:11
#导入成功
#验证是否导入成功
hisen@ubuntu:~/dl/test_db-master$ mysql -u root -p < test_employees_md5.sql
Enter password: 
INFO
TESTING INSTALLATION
table_name	expected_records	expected_crc
employees	300024	4ec56ab5ba37218d187cf6ab09ce1aa1
departments	9	d1af5e170d2d1591d776d5638d71fc5f
dept_manager	24	8720e2f0853ac9096b689c14664f847e
dept_emp	331603	ccf6fe516f990bdaa49713fc478701b7
titles	443308	bfa016c472df68e70a03facafa1bc0a8
salaries	2844047	fd220654e95aea1b169624ffe3fca934
table_name	found_records   	found_crc
employees	300024	4ec56ab5ba37218d187cf6ab09ce1aa1
departments	9	d1af5e170d2d1591d776d5638d71fc5f
dept_manager	24	8720e2f0853ac9096b689c14664f847e
dept_emp	331603	ccf6fe516f990bdaa49713fc478701b7
titles	443308	bfa016c472df68e70a03facafa1bc0a8
salaries	2844047	fd220654e95aea1b169624ffe3fca934
table_name	records_match	crc_match
employees	OK	ok
departments	OK	ok
dept_manager	OK	ok
dept_emp	OK	ok
titles	OK	ok
salaries	OK	ok
computation_time
00:00:22
summary	result
CRC	OK
count	OK
```

二、数据库练习
---

**练习题**：<a href="http://hisen.me/20170410-Mysql%20Employees%20%20Database%E7%BB%83%E4%B9%A0%E9%A2%98&%E7%AD%94%E6%A1%88/
" target="_blank">**点击查看练习题**</a>