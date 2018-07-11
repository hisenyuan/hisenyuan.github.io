---
title: try redis - redis官方教程练习
keywords: [redis,sql]
date: 2017/5/19 14:58
tags: [java,sql]
categories: sql
---
这里主要是介绍了几种redis支持的数据结构，以及操作方法

官网地址：<a href="http://try.redis.io/" target="_blank">http://try.redis.io/</a>

我的redis是安装在linux虚拟机，通过Xshell操作，显示可能跟cmd不大一样

但是操作都是一样的

具体操作如下：
<!--more-->
```
#连接redis客户端
hisen@ubuntu:~$ redis-cli
#存放数据
127.0.0.1:6379> set 'connections' '10'
OK
127.0.0.1:6379> get connections
"10"
#自增1方法：incr
127.0.0.1:6379> incr connections
(integer) 11
127.0.0.1:6379> incr connections
(integer) 12
#删除
127.0.0.1:6379> del conncetions
(integer) 0
#不存在才存放数据 setnx：SET-if-not-exists
127.0.0.1:6379> SETNX hisen hello
(integer) 1
127.0.0.1:6379> get hisen
"hello"
127.0.0.1:6379> SETNX hisen hello
(integer) 0
#添加数据
127.0.0.1:6379> set resource:lock "redis demo"
OK
#设置超时时间，单位秒
127.0.0.1:6379> expire resource:lock 120
(integer) 1
#查看剩余时间
127.0.0.1:6379> ttl resource:lock
(integer) 85
127.0.0.1:6379> ttl resource:lock
(integer) 81
127.0.0.1:6379> ttl resource:lock
(integer) 57

#List集合
#放在list最后（right 右边）
127.0.0.1:6379> rpush friends "Alice"
(integer) 1
127.0.0.1:6379> rpush friends "Bob"
(integer) 2
#放在最前（left 左边）
127.0.0.1:6379> lpush friends "Sam"
(integer) 3
#获取所有数据
127.0.0.1:6379> lrange friends 0 -1
1) "Sam"
2) "Alice"
3) "Bob"
#获取下标0-1的数据
127.0.0.1:6379> lrange friends 0 1
1) "Sam"
2) "Alice"
#获取下标1-2的数据
127.0.0.1:6379> lrange friends 1 2
1) "Alice"
2) "Bob"
#获取长度
127.0.0.1:6379> llen friends
(integer) 3
#删除第一个数据（左边）
127.0.0.1:6379> lpop friends
"Sam"
#删除最后一个数据（右边）
127.0.0.1:6379> rpop friends
"Bob"
127.0.0.1:6379> llen friends
(integer) 1
#输出所有
127.0.0.1:6379> lrange friends 0 -1
1) "Alice"

#Set集合
#添加
127.0.0.1:6379> sadd superpowers "flight"
(integer) 1
127.0.0.1:6379> sadd superpowers "x-ray vision"
(integer) 1
127.0.0.1:6379> sadd superpowers "reflexes"
(integer) 1
#删除
127.0.0.1:6379> srem superpowers "reflexes"
(integer) 1
#判断数据是否存在set中
127.0.0.1:6379> sismember superpowers "flight"
(integer) 1
#输出所有
127.0.0.1:6379> smembers superpowers
1) "flight"
2) "x-ray vision"

127.0.0.1:6379> sadd birdpowers "pecking"
(integer) 1
127.0.0.1:6379> sadd birdpowers "flight"
(integer) 1
#合并两个SET，会过滤重复
127.0.0.1:6379> sunion superpowers birdpowers
1) "pecking"
2) "flight"
3) "x-ray vision"

#有序集合，按照数字排序
127.0.0.1:6379> zadd hackers 1940 "Alan Kay"
(integer) 1
127.0.0.1:6379> zadd hackers 1906 "Grace Hopper"
(integer) 1
127.0.0.1:6379> zadd hackers 1953 "Richard Stallman"
(integer) 1
127.0.0.1:6379> zadd hackers 1965 "Yukihiro Mastsumoto"
(integer) 1
127.0.0.1:6379> zadd hackers 1916 "Claude Shannon"
(integer) 1
127.0.0.1:6379> zadd hackers 1969 "Linus Torvalds"
(integer) 1
127.0.0.1:6379> ZADD hackers 1957 "Sophie Wilson"
(integer) 1
127.0.0.1:6379> ZADD hackers 1912 "Alan Turing"
(integer) 1
#输出
127.0.0.1:6379> zrange hackers 2 4
1) "Claude Shannon"
2) "Alan Kay"
3) "Richard Stallman"
127.0.0.1:6379> zrange hackers 0 -1
1) "Grace Hopper"
2) "Alan Turing"
3) "Claude Shannon"
4) "Alan Kay"
5) "Richard Stallman"
6) "Sophie Wilson"
7) "Yukihiro Mastsumoto"
8) "Linus Torvalds"
127.0.0.1:6379> 

#哈希集合
127.0.0.1:6379> hset user:1000 name "hisen"
(integer) 1
127.0.0.1:6379> hset user:1000 email "hisen@hisen.com"
(integer) 1
127.0.0.1:6379> hset user:1000 pwassword "pswd"
(integer) 1
127.0.0.1:6379> hgetall user:1000
1) "name"
2) "hisen"
3) "email"
4) "hisen@hisen.com"
5) "pwassword"
6) "pswd"

#自增
27.0.0.1:6379> hset user:1000 visits 10
(integer) 1
127.0.0.1:6379> hset user:1000 visits 1
(integer) 0
127.0.0.1:6379> hincrby user:1000 visits 1
(integer) 2
127.0.0.1:6379> hincrby user:1000 visits 1
(integer) 3
127.0.0.1:6379> hincrby user:1000 visits 10
(integer) 13
127.0.0.1:6379> hdel user:1000 visits
(integer) 1
127.0.0.1:6379> hincrby user:1000 visits 1
(integer) 1
```