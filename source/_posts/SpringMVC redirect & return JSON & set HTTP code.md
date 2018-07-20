---
title: SpringMVC redirect & return JSON & set HTTP code
keywords: SpringMVC,java
categories: java
date: 2018-07-13 09:06:02
tags: java
---

springmvc正常情况下redirect并且设置指定响应码，异常情况下返回json数据
## 背景介绍
需求就是正常情况下能redirect到指定的页面
异常的情况下，能够返回JSON格式的错误信息
正常情况和异常情况都需要设置HTTP Code

## 代码实现
```java
    @RequestMapping(value = "/test", method = RequestMethod.POST)
    public String webCharge(HttpServletRequest request, HttpServletResponse response) {
        if (1==1) {
            response.setStatus(200);
            return "redirect:https://github.com/hisenyuan";
        } else {
            try {
                response.setStatus(405);
                response.getWriter().write(JSON.toJSONString("hisenyuan"));
                return null;
            } catch (IOException e) {
                e.printStackTrace();
                return "";
            }
        }
    }
```
