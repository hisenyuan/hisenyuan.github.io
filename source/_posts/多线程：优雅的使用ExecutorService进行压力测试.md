---
title: 多线程：优雅的使用ExecutorService进行压力测试
keywords: 多线程,ExecutorService,线程池,java
categories: java
date: 2018-07-14 01:44:02
tags: java
---


很多时候写功能或者接口需要进行压力测试，
今天发现jwt在生成token的时候，如果输入都是一样的
仅有一个签发时间不一样，生成的token是有可能是一样的

```java
public void testCreate() {
        ThreadFactory namedThreadFactory = new ThreadFactoryBuilder().setNameFormat("hisenyuan").build();
        ExecutorService pool = new ThreadPoolExecutor(
                20,
                50,
                10000L,
                TimeUnit.MILLISECONDS,
                new LinkedBlockingQueue<>(10240),
                namedThreadFactory,
                new ThreadPoolExecutor.AbortPolicy());
        for (int i = 0; i < 50; i++) {
            // 需要提交的内容
            pool.execute(this::createTokenTest);
        }
        pool.shutdown();
        try {
            while (!pool.awaitTermination(500, TimeUnit.MILLISECONDS)) {
                LOGGER.debug("Waiting for terminate");
            }
        } catch (InterruptedException e) {
            LOGGER.error(e);
        }
    }
```
