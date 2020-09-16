---
title: Java GC - 理论与实践 - 附优化案例
keywords: [java gc,java垃圾回收,jvm,垃圾回收器,Parallel Scavenge,Parallel Old]
date: 2019/4/29 19:00
tags: [java]
categories: java
---
# 一、理论知识
常见参数：
1. -Xms 堆初始化    例如：-Xms256m
2. -Xmx 堆最大值    例如：-Xmx512m
3. -Xmn 堆新生代    例如：-Xmn100m
4. -XX:NewRatio   新生代与老年代的比例
5. -XX:SurvivorRatio 新生代区域比例，默认8,代表Eden:From Survivor:To Survivor = 8:1:1

垃圾回收器：
1. Serial/Serial Old 新生代/老年代，古老，单线程，暂停所有用户线程，复制算法/标记整理算法
2. ParNew 1的多线程版本
3. Parallel Scavenge 新生代，多线程，不需要暂停用户线程，复制算法
4. Parallel Old 老年代，多线程，不需要暂停用户线程，标记整理算法
5. CMS（Current Mark Sweep，[详情](http://www.importnew.com/2782.html)）老年代，与ParNew配合使用，牺牲吞吐量获得最短停顿，标记整理算法
6. G1 并行与并发收集器，可预测的停顿时间

# 二、实践案例
1. Full GC 之前进行 Minor GC 避免扫描过多的对象， 配置：CMSScavengeBeforeRemark
2. Xms和Xmx设置为相同，这样可以减少内存自动扩容和收缩带来的性能损失
3. JVM调优是最后的稻草，进行JVM调优之前应该先优化架构和代码
4. 调优是一个复杂的过程，根据具体的场景结合对垃圾回收器的深入理解进行调优，才可能事半功倍。

各个区大小比例建议
```
# 活跃空间大小：Full GC后堆中老年代占用空间的大小
空间      倍数
总大小    3-4倍活跃空间大小
新生代    1-1.5倍活跃空间大小
老年代    2-3倍活跃空间大小
```
<!--more-->
# 三、参考连接
1.1 https://www.cnblogs.com/honey01/p/9475726.html
1.2 https://www.cnblogs.com/dolphin0520/p/3783345.html
2.1 https://tech.meituan.com/2017/12/29/jvm-optimize.html
