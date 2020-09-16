---
title: WeightAlgorithm - 权重算法 - Random、HashCode对比
keywords: [WeightAlgorithm,Weight,Algorithm,权重算法]
date: 2019/3/22 20:01
tags: [java,算法]
categories: java
---
# 一、权重算法
权重算法一般在路由里面用的比较多，分布式环境下对等的服务有多个，加权随机选出一个服务来调用；

可能还有其他方面的用途，下面的代码简单的实现了这个权重，本质上就用到了数组，随机下标；

# 二、代码概览
```java
public static void main(String[] args) {
    String[] weight = {"A", "A", "A", "A", "A", "B", "B", "B", "C", "C"};
    final int times = 500000;
    final long hashStart = System.currentTimeMillis();
    List<String> hashRes = getList4Hash(weight, times);
    printRes("Hash", hashStart, hashRes);

    final long randomStart = System.currentTimeMillis();
    List<String> randomRes = getList4Random(weight, times);
    printRes("Random", randomStart, randomRes);
//        Hash          use millis: 931
//        A:49.92%
//        B:29.99%
//        C:20.09%
//        Random        use millis: 50
//        A:50.08%
//        B:29.93%
//        C:19.99%
}
```

# 三、完整代码
<!--more-->
github:<a href="https://github.com/hisenyuan/IDEAPractice/blob/master/src/main/java/com/hisen/algorithms/WeightAlgorithm.java" target="_blank">show all code</a>

```java
package com.hisen.algorithms;

import com.google.common.collect.Lists;

import java.text.NumberFormat;
import java.util.List;
import java.util.Map;
import java.util.Random;
import java.util.UUID;
import java.util.function.Function;
import java.util.stream.Collectors;

/**
 * @Author hisenyuan
 * @Description $end$
 * @Date 2019/3/21 21:04
 */
public class WeightAlgorithm {
    public static void main(String[] args) {
        String[] weight = {"A", "A", "A", "A", "A", "B", "B", "B", "C", "C"};
        final int times = 500000;
        final long hashStart = System.currentTimeMillis();
        List<String> hashRes = getList4Hash(weight, times);
        printRes("Hash", hashStart, hashRes);

        final long randomStart = System.currentTimeMillis();
        List<String> randomRes = getList4Random(weight, times);
        printRes("Random", randomStart, randomRes);
//        Hash          use millis: 931
//        A:49.92%
//        B:29.99%
//        C:20.09%
//        Random        use millis: 50
//        A:50.08%
//        B:29.93%
//        C:19.99%
    }

    private static void printRes(String method, long start, List<String> resList) {
        final Map<String, Long> collect = resList.stream().collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
        final double a = collect.get("A");
        final double b = collect.get("B");
        final double c = collect.get("C");
        final double sum = a + b + c;

        NumberFormat nt = NumberFormat.getPercentInstance();
        nt.setMinimumFractionDigits(2);

        System.out.println(method + "\t use millis: " + (System.currentTimeMillis() - start));
        System.out.println("A" + ":" + nt.format(a / sum));
        System.out.println("B" + ":" + nt.format(b / sum));
        System.out.println("C" + ":" + nt.format(c / sum));
    }

    private static List<String> getList4Hash(String[] weight, int times) {
        List<String> result = Lists.newArrayList();
        for (int i = 0; i < times; i++) {
            final String a = UUID.randomUUID().toString() + System.currentTimeMillis();
            final int hash = a.hashCode();
            final int index = hash > 0 ? hash : -hash;
            final String res = weight[index % weight.length];
            result.add(res);
        }
        return result;
    }

    private static List<String> getList4Random(String[] weight, int times) {
        List<String> result = Lists.newArrayList();
        Random random = new Random();
        for (int i = 0; i < times; i++) {
            final int index = random.nextInt(weight.length);
            final String res = weight[index];
            result.add(res);
        }
        return result;
    }
}
```
