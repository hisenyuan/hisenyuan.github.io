---
title: redis counter demo -  redis并发计数器
keywords: [redis,redis计数器]
date: 2018/1/11 22:51
tags: [redis]
categories: sql
---
# 一、说明
利用redis操作的原子性，实现java 多线程并发的情况下实现计数器。

我本机测试多个线程操作之后，结果会出现一定的延迟，但是最终数字是ok的

应该是redis内部做了一个类似于队列的功能。

需要注意的是，得使用redis连接的线程池，不然会出现异常

这里有一个：[JedisUtil](https://github.com/hisenyuan/IDEAPractice/blob/master/src/main/java/com/hisen/utils/JedisUtil.java) 下面用到了

# 二、 代码实现
## 2.1 redis操作类
```
package com.hisen.thread.count_click_by_redis;

import com.hisen.utils.JedisUtil;
import redis.clients.jedis.JedisPool;
/**
 * @author hisenyuan
 * @description 操作redis的线程类
 */
public class ClickRedis {

  /**
   * 必须使用线程池，而且线程池要大于并发数，否则会出现redis超时
   */
  private static JedisPool jedis = JedisUtil.getPool();

  public static void click() {
    jedis.getResource().incrBy("hisen", 1);
  }

  public static int getCount() {
    return Integer.parseInt(jedis.getResource().get("hisen"));
  }

  public static void declare() {
    jedis.getResource().del("hisen");
    jedis.close();
  }
}
```
## 2.2 线程类,模拟点击
```
package com.hisen.thread.count_click_by_redis;

/**
 * @author hisenyuan
 * @description 执行点击的线程类
 */
public class CountClickByRedisThread extends Thread{

  private int id;
  public CountClickByRedisThread(int id) {
    this.id = id;
  }

  @Override
  public void run() {
    super.run();
    ClickRedis.click();
    int count = ClickRedis.getCount();
    System.out.println("task:" + id + "\t 执行完毕\t线程编号：" + this.getId() + "\t当前值：" + count);
  }
}
```
2.3 主线程 - 启动类
```
package com.hisen.thread.count_click_by_redis;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

public class Main {

  public static void main(String[] args) throws InterruptedException {
    /**
     * 5 - corePoolSize：核心池的大小
     * 10 - maximumPoolSize：线程池最大线程数，它表示在线程池中最多能创建多少个线程
     * 200 - keepAliveTime：表示线程没有任务执行时最多保持多久时间会终止。
     * unit - unit：参数keepAliveTime的时间单位，有7种取值
     * workQueue：一个阻塞队列，用来存储等待执行的任务
     */
    ThreadPoolExecutor executor = new ThreadPoolExecutor(
        50,
        100,
        200,
        TimeUnit.MICROSECONDS,
        new ArrayBlockingQueue<Runnable>(50));
    // 开启50个线程
    for (int i = 0; i < 50; i++) {
      executor.execute(new CountClickByRedisThread(i));
    }
    System.out.println("已经开启所有的子线程");
    // 启动一次顺序关闭，执行以前提交的任务，但不接受新任务。
    executor.shutdown();
    // 判断所有线程是否已经执行完毕
    while (true) {
      if (executor.isTerminated()) {
        System.out.println("所有的子线程都结束了！");
        // 清除redis数据
        ClickRedis.declare();
        break;
      }
      Thread.sleep(100);
    }
  }

}
```