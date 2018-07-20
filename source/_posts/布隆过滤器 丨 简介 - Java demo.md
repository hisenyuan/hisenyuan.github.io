---
title: 布隆过滤器 丨 简介 - Java demo
keywords: [布隆过滤器,java布隆过滤器]
date: 2017/9/7 10:48
tags: [算法]
categories: java
---
## 概述
布隆过滤器的作用是加快判定一个元素是否在集合中出现的方法。

因为其主要是过滤掉了大部分元素间的精确匹配，故称为过滤器。

---

其应用场景为需要频繁在一个海量的集合中查找某个元素是否存在。

并且通常，这个值不在集合中。

比如Google chrome用此方法检查一个url是否在恶意url库中。

## 简单的例子
假设有一些字符串，假设有一个字符串a，要在集合B中查找其是否在集合B中。最笨的方法是遍历集合B中的每个元素bi，精确匹配a是否等于bi。若集合B中有N个元素，则最坏情况下需要执行N次精确匹配。

一个改进的方法是将a和B中每个字符串按照特定规则映射为数字，称为hash值。规则可以任意设置。比如取各字符串的首字母和尾字母的编码之乘积，取奇数个字符的编码执行异或，等。将比较字符串问题变成一个比较数字的问题。比较字符串需要从头到尾比较，而数字的比较会快很多。

需要注意的是，当两个字符串相同时，采用相同的映射规则得到的数字一定相同。但当两个字符串不同时，得到的字符串不一定不同。所以，当我们发现两个字符串的hash值相同时，两个字符串不一定相同，所以需要进一步去精确匹配两个字符串是否相同。但采用hash值方法已经能够过滤掉一部分以前需要精确匹配的计算量。仅当hash值相同（假设hash值通过字符串首尾字母计算得来，则当两个字符串首尾字母相同时hash值相同）时才去比较字符串本身。若选择hash值合理，则性能将大幅提高。

布隆过滤器通过将一个字符串使用多个不同的hash值计算方法，映射为多个不同的hash值，当所有这些hash值完全相同时，才认为两个字符串相同。从而进一步降低了放生hash值相同的可能性，从而进一步提高了过滤的性能。
## Java代码实现
算法使用了md5值来生成n个不同的hash值
<!--more-->
```java
package com.hisen.interview;

import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.ArrayList;

/**
 * 布隆过滤器 - 测试某个元素不存在集合中
 * Created by hisenyuan on 2017/9/7 at 8:50.
 */
public class BloomFilter {
  public static final int NUM_SLOTS = 1024 * 1024 * 8;
  public static final int NUM_HASH = 8;
  private BigInteger bitmap = new BigInteger("0");

  public static void main(String[] args) {
    BloomFilter bf = new BloomFilter();
    ArrayList<String> contents = new ArrayList<>();
    contents.add("sldkjelsjf");
    contents.add("ggl;ker;gekr");
    contents.add("wieoneomfwe");
    contents.add("sldkjelsvrnlkjf");
    contents.add("ksldkflefwefwefe");
    for (int i = 0; i < contents.size(); i++) {
      bf.adElement(contents.get(i));
    }
    System.out.println(bf.check("sldkjelsvrnlkjf"));
    System.out.println(bf.check("sldkjelsvrnkjf"));
  }
  private void adElement(String message) {
    for (int i = 0; i < NUM_HASH; i++) {
      int hashCode = getHash(message, i);
      if (!bitmap.testBit(hashCode)) {
        bitmap = bitmap.or(new BigInteger("1").shiftLeft(hashCode));
      }
    }
  }
  private boolean check(String message) {
    for (int i = 0; i < NUM_HASH; i++) {
      int hashCode = getHash(message,i);
      if (this.bitmap.testBit(hashCode)){
        return false;
      }
    }
    return true;
  }

  private int getHash(String message, int i) {
    try {
      MessageDigest md5 = MessageDigest.getInstance("md5");
      message = message + String.valueOf(i);
      byte[] bytes = message.getBytes();
      md5.update(bytes);
      BigInteger bi = new BigInteger(md5.digest());
      return Math.abs(bi.intValue()) % NUM_SLOTS;
    } catch (NoSuchAlgorithmException e) {
      e.printStackTrace();
    }
    return -1;
  }
}
```
