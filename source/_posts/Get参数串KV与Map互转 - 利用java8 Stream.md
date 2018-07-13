---
title: Get参数串KV与Map互转 - 利用java8 Stream
keywords: map,get,stream
categories: java
date: 2018-07-13 00:25:02
tags: java
---



## 一、背景
在各种系统需要加签的时
一般都会把参与签名的数据以get请求参数拼接起来
并且要求有序，这个方法会比较方便

## 二、实现
### 2.1  拼接为有序的get请求类字符串
```
    public String getSortedStr(Map<String, String> unSortedStr) {
        String sortedStr= unSortedStr
                .entrySet()
                .stream()
                .filter(entry -> !StringUtil.isEmpty(entry.getValue()))
                .sorted(Map.Entry.comparingByKey())
                .map(entry -> entry.getKey() + "=" + entry.getValue())
                .collect(Collectors.joining("&"));
        return sortedStr;
    }
```
### 2.2 把get类参数字符串转为map
```
    private Map<String,String> getMapData(String getStr){
        String[] strs = getStr.split("&");
        HashMap<String, String> dataMap = new HashMap<>(16);
        for (int i = 0; i < strs.length; i++) {
            String[] str = strs[i].split("=");
            dataMap.put(str[0], str[1]);
        }
        return dataMap;
    }
```

对于get类字符串没有发现比较好的方法转换为map