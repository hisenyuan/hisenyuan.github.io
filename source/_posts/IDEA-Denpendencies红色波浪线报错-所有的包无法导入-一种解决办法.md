---
title: 'IDEA Denpendencies红色波浪线报错,所有的包无法导入 - 一种解决办法'
keywords: [java,idea,IDEA Denpendencies报错,IDEA Denpendencies红色波浪线]
tags: [java,idea,Maven]
date: 2017-02-28 20:19:22
categories: java
---
```
failed to read artifact descriptor for xxx：jar
```
一下午那代码里面是各种报错

凡是引入的大部分都报错

原因就是maven管理的jar没有添加上依赖

最后在stackoverflow找到了良药

上面有图片，错误会详细一点，如果你的也相同，可以试一试
```
maven project -> Execute Maven Goal -> mvn -U clean install
```
执行以上命令之后等待完成，应该就好了

参考自stackoverflow：<a href="http://stackoverflow.com/questions/11454822/import-maven-dependencies-in-intellij-idea" target="_blank">点击查看</a>
