---
title: FindLucklyGirl - 找出值班的幸运儿
keywords: [hashCode,list差集,随机筛选,大转盘]
date: 2018/07/20 22:40
tags: java
categories: java
---

### 一、背景简介
今天头给我们开会，说到团队对外沟通的问题。

谈到对外需要积极给人解决问题，而不是各种推脱，即使自己不知道，也可以给个眼神找到对的人。

继而谈到需要安排人轮流负责跟外部接洽

由于这个活呢，大伙儿认为不是什么好差事，那就抓阄决定吧

于是乎就感觉可以写一个简单的排班系统小bug，不过我这里只是提供一个简单的思路

### 二、程序代码
主要的逻辑在这，当然并没有考虑数据持久化的问题

性能等其他的问题，纯粹是一个思路，用hashCode取模主要是打的比较散，很均匀

加上日期什么的，就一个排班表出来了。
```Java
    /**
     *
     * @param person 目前所有的人
     * @param lucklys 这一轮已经值班了的人
     */

    private static void findLucklyOneByHashCode(List<String> person, ArrayList<String> lucklys) {
        List<String> result = new ArrayList<>(person);
        result.removeAll(lucklys);
        if (result.size() == 0) {
            lucklys.clear();
            result = new ArrayList<>(person);
        }
        String code = System.nanoTime() + UUID.randomUUID().toString();
        int index = Math.abs(code.hashCode()) % result.size();
        String lucklyOne = result.get(index);
        System.out.println("lucklyOne:" + lucklyOne);
        lucklys.add(lucklyOne);
    }
    /**
     * hashCode随机取出来的数据
     * lucklyOne:G
     * lucklyOne:B
     * lucklyOne:C
     * lucklyOne:D
     * lucklyOne:H
     * lucklyOne:F
     * lucklyOne:E
     * lucklyOne:A
     */
```
