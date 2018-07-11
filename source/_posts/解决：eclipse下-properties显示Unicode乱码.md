---
title: 解决：eclipse下*.properties显示Unicode乱码
date: 2017-02-08 20:53:30
tags: [java,乱码,properties]
---

eclipse的*.properties文件，默认的编码方式是iso-8859-1

Window -> preferences -> general -> Contents Types -> Text(展开)
 -> Java Aroperties File(点击) -> *.properties(locked)(点击)
 -> 把iso-8859-1改为 UTF-8 -> Update -> OK

然后就可以正常显示中文了