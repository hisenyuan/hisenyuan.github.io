---
title: 利用IDEA写Hexo博客的一些技巧
keywords: [idea,hexo]
date: 2017/3/3 10:51
tags: [idea,hexo]
categories: hexo
---
今天偶然看到有人说用idea写博客

刚开始我觉得这样会很麻烦，后来想想以前写博客也是醉了

先新建一个 _post 的快捷方式

进去，然后到博客根目录

打开Git Bash，然后执行
```
hexo n "你要写的文章题目"
```
然后在 _post 快捷方式打开刚刚新建的markdown文件，用markdownpad打开编辑。。。

编辑完了回到Git Bash。。。。想想就很麻烦

---

于是乎用IDEA打开博客根目录
```
sources -> _post -> new -> Edit File Templates
```
<!--more-->
name：markdown extension：md
内容：
```
---
title: ${NAME}
keywords: []
date: ${DATE} ${TIME}
tags: []
categories:
---
```
接下来apply -> *.md -> 下面选择markdown

以后新建markdown文件就会默认带上这个模版

效果
```
title: 利用IDEA写Hexo博客的一些技巧
keywords: []
date: 2017/3/3 10:51
tags: []
categories: hexo
```

接下来到了发布的时间，于是我们可以设置一下Terminal（在setting里面），设置为bash（git目录下）

设置完了之后Alt + F12 调出 Terminal 即可进行git操作

到此，大功告成，我要去更新博客了