---
title: eclipse无法链接github
date: 2017-02-08 20:56:02
tags: [eclipse,github]
---
浏览器什么的都能打开github.com
就是eclipse无法提交到github，每次都是连接超时
然后就直接修改host了，目前有效
2017年1月14日 18:01:34

host位置：
```
C:\Windows\System32\drivers\etc
```
host文件最后一行加上下面内容即可
```
192.30.253.112       github.com
```