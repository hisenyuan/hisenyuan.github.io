---
title: hexo安装过程
date: 2017-01-20 00:23:06
tags: [hexo,安装,github]
---

## 准备工作 ##
1. Node.js：<a href="https://nodejs.org/en/" target="_blank">点击下载</a>
2. git：<a href="https://git-for-windows.github.io/" target="_blank">点击下载</a>
3. MarkdownPad：<a href="http://markdownpad.com/" target="_blank">点击下载</a>

安装好上面三个工具
可能会遇到的问题：

**1、**Git Bash执行`node -v`提示无效 或者 `npm install` 报 command not found

解决办法：在环境变量 - 用户变量中 - 新建用户变量 - 添加nodejs安装路径

如：C:\tool\nodejs

**2、**ERROR Deployer not found : github

解决办法：

1. 配置文件有问题，冒号后面都有一个空格的
1. 执行：npm install hexo-deployer-git --save （这命令是为了解决hexo新版本的部署问题）

**3**使用淘宝镜像加快安装速度
安装cnpm，使用命令：
```
npm install cnpm -g --registry=https://registry.npm.taobao.org
```

## 安装过程 ##
1. 打开Git Bash
2. 进入nodejs安装目录
3. 开始安装hexo，输入下面代码
4. `npm install -g hexo`#等待安装完成，这个过程可能会快也可能很慢，耐心等待
6. `mkdir blog && cd blog` #上面这个代码是创建一个博客存放的目录
8. `hexo init`#初始化
9. `cnpm install` #安装依赖包
10. 完成之后，本地博客就搭建完成
11. `hexo g` #生成静态页面
12. `hexo s` #启动服务器，打开http://localhost:4000 就是本地博客

本地博客安装完成，下面介绍发布到github上
1. 登陆github，没有就注册
2. 点击右上角加号+
3. Create a new repository
4. 名字写：`yourgithubname.github.io`
5. 创建完成
6. 点击`Setting`
7. 选择一个主题，然后就好了
8. 编辑blog文件夹里面的`_config.yml`配置文件
9. 最后面添加
```
deploy: 
  type: git
  repository: http://github.com/yourname/yourname.github.io.git
  branch: master
```

最后执行

1. `hexo g`#重新生成静态博客
2. `hexo d`#将本地静态博客部署到github

现在你在浏览器打开：http://yourname.github.io就可以访问你的博客了
到此为止就搭建完了一个博客

开始写第一篇文章：
执行：hexo new "你的文章标题"
然后你在`blog/source/_posts`文件夹下面有文件，用markdownpad打开编辑
执行：
1. `hexo g`#重新生成
2. `hexo s`#本地查看效果
3. `hexo d`#上传到github
4. 或者不预览，直接一步上传到github：`hexo d -g`