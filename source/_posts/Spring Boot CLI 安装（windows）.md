---
title: Spring Boot CLI 安装（windows）
keywords: [Spring Boot CLI 安装,SpringBootCLI,windows]
date: 2017/7/13 0:35
tags: [java]
categories: java
---
SpringBootCLI是一个命令行工具，可用于快速搭建基于spring的原型。

它支持运行Groovy脚本，这也就意味着你可以使用类似Java的语法，但不用写很多的模板代码。

Spring Boot不一定非要配合CLI使用，但它绝对是Spring应用取得进展的最快方式.

下载分发包
---
1. 地址:<a href="https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-installing-the-cli" target="blank">getting-started-installing-the-cli<a/>
2. 下载：spring-boot-cli-x.x.x.RELEASE-bin.zip (例如：spring-boot-cli-1.5.4.RELEASE-bin.zip)
3. 解压
4. 设置环境变量：C:\hisenwork\soft\spring-1.5.4.RELEASE\bin （在path中添加）
5. cmd测试：spring --version
```
spring --version
Spring CLI v1.5.4.RELEASE
```

运行一个简单程序
---
<!--more-->
文件名称：HelloController.groovy

文件内容：
```
@RestController
class HelloController{
	@RequestMapping("/")
	def hello(){
		return "Hello World"
	}
}
```
运行程序：cmd进入文件所在文件夹，执行:spring run HelloController.groovy

提醒：Resolving dependencies第一次初始化时间会久一点，耐心等待
```
spring run HelloController.groovy

Resolving dependencies..........................

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.5.4.RELEASE)
```
之后在浏览器中输入：http://localhost:8080/

就能看到：Hello World