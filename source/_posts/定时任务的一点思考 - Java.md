---
title: 定时任务的一点思考 - Java
keywords: [java,定时任务,分布式调度]
date: 2018/10/27 13:52
tags: [java,定时任务]
categories: java
---
## 一、背景交代
1. 部署方式：多点
2. 业务概要：处理一批数据，可以失败重试

## 二、实现方式
1. 主表入一条控制数据，两个控制字段：next，process(0:空闲,1:处理中)，子表具体处理业务
2. 定时任务只扫描主表，通过其他业务字段，加上主表两个控制字段(now>netx,process=0)，捞出需要处理的数据
3. 当开始处理数据时候，先更新process=1，如果更新成功，说明拿到锁(假装是一个分布式锁，因为两台机器)
4. 如果拿到3的锁，则开始处理子表的业务数据，一次查100条(limit 0,100 desc update_time)，直到子表全部处理完成
5. 如果遇到错误，把错误数据的id存起来，更新子表重试次数，和更新时间，如果达到最大失败次数，更改状态(让4查不出这条数据)
6. 如果遇到当前处理数据的id存在List中，那么说明当前所有的数据都处理过一次了，退出(如果不判断，失败后会无限循环，直到全部成功)

## 三、伪代码
```java
    public void process(MainTable mainTable) {

        // 1. 锁控制表
        boolean lock = service.lock(mainTable);
        LOGGER.info("lockFlag:,", lock, ",mainTableId:", mainTable.getId());
        if (!lockFlag) {
            return;
        }
        List<Long> repeatIds = new ArrayList<>();
        // 2. 根据主表id，下次扫描时间，每次查询100条
        List<SubTable> subTables;
        do {
          //扫描子表中的数据
            subTables = service.querySub(mainTable.getId(), 100);
            for (SubTable sub : subTables) {
                if (repeatIds.contains(batch.getId())) {
                    subTables = null;
                    break;
                }
                // 处理业务
                handle(mainTable, subTables, repeatIds);
            }
        } while (!CollectionUtils.isEmpty(subTables));

        // 5. 检查是否全部终态
        checkHandleDone(mainTable);
        // 7. 释放锁
        boolean unLock = service.unLock(mainTable);
        LOGGER.info("lockFlag:,", unLock, ",mainTableId:", mainTable.getId());
    }
```
