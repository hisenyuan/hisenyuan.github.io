---
title: hexo新建文章时候默认带上categories，tags
date: 2017-02-09 21:42:36
tags: [hexo]
categories: hexo
---
在博客的 **scaffolds** 文件夹里有个**post.md** 添加上需要的配置就行

这里是创建post的模板。

我的默认设置成这样：
```
---
title: {{ title }}
date: {{ date }}
tags: []
categories:
---
```
tags: [关键词1,关键词2]
