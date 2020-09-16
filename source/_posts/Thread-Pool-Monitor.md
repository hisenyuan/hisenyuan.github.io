---
title: Thread Pool Monitor - 线程池监控
keywords: [ThreadPoolExecutor,ThreadPool,Thread Pool Monitor,线程池监控]
date: 2019/9/5 19:50
tags: [java]
categories: java
---
# 一、背景
业务当中多处用到线程池进行异步处理;
为了得知线程池设置是否合理，故需要增加线程池监控;
常见的实现方式：
1. org.springframework.scheduling.concurrent.ScheduledExecutorTask
2. org.springframework.scheduling.annotation.Scheduled

本文使用1的方式实现，主要是方便进行配置，可以托管多个任务;
# 二、效果预览
```
taskName:pool1-monitor. taskCount:820, completedTaskCount:820, largestPoolSize:30, poolSize:30, activeCount:0, corePoolSize:30, maximumPoolSize:50, queueSize:0
taskName:pool2-monitor. taskCount:1703, completedTaskCount:1703, largestPoolSize:30, poolSize:30, activeCount:0, corePoolSize:30, maximumPoolSize:50, queueSize:0
taskName:pool3-monitor. taskCount:820, completedTaskCount:448, largestPoolSize:30, poolSize:30, activeCount:30, corePoolSize:30, maximumPoolSize:50, queueSize:342
```

# 三、监控逻辑
代码如下:
<!-- more -->
```java
package com.hisen.thread.monitor.threadpool;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

import java.util.List;
import java.util.concurrent.ThreadPoolExecutor;

/**
 * @author hisenyuan
 * @date 2019-09-05 19:45
 */
public class ThreadPoolMonitor implements Runnable {
    private static final Logger LOGGER = LoggerFactory.getLogger(ThreadPoolMonitor.class);

    // xml中注入
    private List<ThreadPoolTaskExecutor> pools;

    @Override
    public void run() {
        monitorHandle(pools);
    }

    private void monitorHandle(List<ThreadPoolTaskExecutor> pools) {
        pools.forEach(this::handleExecutor);
    }

    private void handleExecutor(ThreadPoolTaskExecutor p) {
        ThreadPoolExecutor threadPoolExecutor = p.getThreadPoolExecutor();
        // 线程池需要执行的任务数
        long taskCount = threadPoolExecutor.getTaskCount();

        // 线程池在运行过程中已完成的任务数
        long completedTaskCount = threadPoolExecutor.getCompletedTaskCount();

        // 曾经创建过的最大线程数
        long largestPoolSize = threadPoolExecutor.getLargestPoolSize();

        // 线程池里的线程数量
        long poolSize = threadPoolExecutor.getPoolSize();

        // 线程池里活跃的线程数量
        long activeCount = threadPoolExecutor.getActiveCount();

        // 配置的核心线程数
        int corePoolSize = threadPoolExecutor.getCorePoolSize();

        // 配置的最大线程数
        int maximumPoolSize = threadPoolExecutor.getMaximumPoolSize();

        // 当前线程池队列的个数
        int queueSize = threadPoolExecutor.getQueue().size();

        // 线程池前缀名称
        String threadNamePrefix = p.getThreadNamePrefix();

        LOGGER.info("taskName:{}monitor. taskCount:{}, completedTaskCount:{}, largestPoolSize:{}, poolSize:{}, activeCount:{}, corePoolSize:{}, maximumPoolSize:{}, queueSize:{}"
                , threadNamePrefix
                , taskCount
                , completedTaskCount
                , largestPoolSize
                , poolSize
                , activeCount
                , corePoolSize
                , maximumPoolSize
                , queueSize);
    }

    public List<ThreadPoolTaskExecutor> getPools() {
        return pools;
    }

    public void setPools(List<ThreadPoolTaskExecutor> pools) {
        this.pools = pools;
    }
}
```

# 四、spring xml配置

```xml
<!-- ### 线程池监控 开始 ### -->
<bean id="threadPoolMonitorRunnable" class="com.hisen.thread.monitor.threadpool.ThreadPoolMonitor">
    <property name="pools">
        <list>
            <ref bean="pool1" />
            <ref bean="pool2" />
            <ref bean="pool3" />
        </list>
    </property>
</bean>

<!-- 定时执行的线程池 -->
<bean id="threadPoolMonitorTask" class="org.springframework.scheduling.concurrent.ScheduledExecutorTask">
    <property name="runnable" ref="threadPoolMonitorRunnable"/>
    <!-- 延迟时间，单位ms -->
    <property name="delay" value="150000"/>
    <!-- 间隔时间，单位ms -->
    <property name="period" value="5000"/>
</bean>

<!-- 任务调度 -->
<bean id="scheduledExecutorFactoryBean" class="org.springframework.scheduling.concurrent.ScheduledExecutorFactoryBean">
    <!-- 执行任务的线程池个数 -->
    <property name="poolSize" value="1" />
    <!-- 任务列表 -->
    <property name="scheduledExecutorTasks">
        <list>
            <ref bean="threadPoolMonitorTask"/>
        </list>
    </property>
</bean>
<!-- ### 线程池监控 结束 ### -->
```
