---
title: resteasy fastjson版本冲突问题
keywords: [resteasy,fastjson,RESTeasy Provider Factory is null,ResteasyBootstrap]
date: 2019/8/4 23:50
tags: [java]
categories: java
---
# 零、相关介绍
关键错误信息：
java.lang.RuntimeException: RESTeasy Provider Factory is null, do you have the ResteasyBootstrap listener configured?
java.lang.RuntimeException: Illegal to inject a message body into a singleton into public com.alibaba.fastjson.support.jaxrs.FastJsonProvider(java.lang.String)

resteasy:JBoss的一个开源项目，提供一套完整的框架帮助开发人员构建RESTful Web Service和RESTful Java应用程序。
fastjson:由阿里开发的一个性能很好的Java JSON 解析器和生成器。

引起错误的依赖
```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.58</version><!--改为：1.1.34.sec01相安无事-->
</dependency>
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>resteasy-jaxrs</artifactId>
    <version>2.2.1.GA</version>
</dependency>
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>jaxrs-api</artifactId>
    <version>2.2.1.GA</version>
</dependency>
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>resteasy-spring</artifactId>
    <version>2.2.1.GA</version>
</dependency>
```

# 一、错误日志
本地启动Server报错
<!--more-->
```
2019-08-03 11:51:22,491 ERROR - [RMI TCP Connection(2)-127.0.0.1] - Context initialization failed
java.lang.RuntimeException: RESTeasy Provider Factory is null, do you have the ResteasyBootstrap listener configured?
	at org.jboss.resteasy.plugins.spring.SpringContextLoaderSupport.customizeContext(SpringContextLoaderSupport.java:53)
	at org.jboss.resteasy.plugins.spring.SpringContextLoader.customizeContext(SpringContextLoader.java:30)
	at org.springframework.web.context.ContextLoader.configureAndRefreshWebApplicationContext(ContextLoader.java:382)
	at org.springframework.web.context.ContextLoader.initWebApplicationContext(ContextLoader.java:283)
	at org.springframework.web.context.ContextLoaderListener.contextInitialized(ContextLoaderListener.java:112)
	at org.jboss.resteasy.plugins.spring.SpringContextLoaderListener.contextInitialized(SpringContextLoaderListener.java:44)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:5197)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5720)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:145)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:1018)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:994)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:662)
	at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1899)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:301)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:619)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:566)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:301)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at com.sun.jmx.remote.security.MBeanServerAccessController.invoke(MBeanServerAccessController.java:468)
	at javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1487)
	at javax.management.remote.rmi.RMIConnectionImpl.access$300(RMIConnectionImpl.java:97)
	at javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1328)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1427)
	at javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:848)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:322)
	at sun.rmi.transport.Transport$2.run(Transport.java:202)
	at sun.rmi.transport.Transport$2.run(Transport.java:199)
	at java.security.AccessController.doPrivileged(Native Method)
	at sun.rmi.transport.Transport.serviceCall(Transport.java:198)
	at sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:567)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:828)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.access$400(TCPTransport.java:619)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler$1.run(TCPTransport.java:684)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler$1.run(TCPTransport.java:681)
	at java.security.AccessController.doPrivileged(Native Method)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:681)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
	at java.lang.Thread.run(Thread.java:745)
```
tomcat localhost long
```
八月 03, 2019 11:51:22 上午 org.apache.catalina.core.ApplicationContext log
信息: No Spring WebApplicationInitializer types detected on classpath
八月 03, 2019 11:51:22 上午 org.apache.catalina.core.StandardContext listenerStart
严重: Exception sending context initialized event to listener instance of class org.jboss.resteasy.plugins.server.servlet.ResteasyBootstrap
java.lang.RuntimeException: java.lang.RuntimeException: Unable to instantiate MessageBodyReader
	at org.jboss.resteasy.plugins.providers.RegisterBuiltin.register(RegisterBuiltin.java:35)
	at org.jboss.resteasy.spi.ResteasyDeployment.start(ResteasyDeployment.java:211)
	at org.jboss.resteasy.plugins.server.servlet.ResteasyBootstrap.contextInitialized(ResteasyBootstrap.java:28)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:5197)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5720)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:145)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:1018)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:994)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:662)
	at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1899)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:301)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:619)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:566)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:301)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at com.sun.jmx.remote.security.MBeanServerAccessController.invoke(MBeanServerAccessController.java:468)
	at javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1487)
	at javax.management.remote.rmi.RMIConnectionImpl.access$300(RMIConnectionImpl.java:97)
	at javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1328)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1427)
	at javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:848)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:322)
	at sun.rmi.transport.Transport$2.run(Transport.java:202)
	at sun.rmi.transport.Transport$2.run(Transport.java:199)
	at java.security.AccessController.doPrivileged(Native Method)
	at sun.rmi.transport.Transport.serviceCall(Transport.java:198)
	at sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:567)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:828)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.access$400(TCPTransport.java:619)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler$1.run(TCPTransport.java:684)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler$1.run(TCPTransport.java:681)
	at java.security.AccessController.doPrivileged(Native Method)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:681)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
	at java.lang.Thread.run(Thread.java:745)
Caused by: java.lang.RuntimeException: Unable to instantiate MessageBodyReader
	at org.jboss.resteasy.spi.ResteasyProviderFactory.registerProvider(ResteasyProviderFactory.java:760)
	at org.jboss.resteasy.plugins.providers.RegisterBuiltin.registerProviders(RegisterBuiltin.java:70)
	at org.jboss.resteasy.plugins.providers.RegisterBuiltin.register(RegisterBuiltin.java:31)
	... 51 more
Caused by: java.lang.RuntimeException: Illegal to inject a message body into a singleton into public com.alibaba.fastjson.support.jaxrs.FastJsonProvider(java.lang.String)
	at org.jboss.resteasy.core.MessageBodyParameterInjector.inject(MessageBodyParameterInjector.java:209)
	at org.jboss.resteasy.core.ConstructorInjectorImpl.injectableArguments(ConstructorInjectorImpl.java:63)
	at org.jboss.resteasy.core.ConstructorInjectorImpl.construct(ConstructorInjectorImpl.java:129)
	at org.jboss.resteasy.spi.ResteasyProviderFactory.getProviderInstance(ResteasyProviderFactory.java:1038)
	at org.jboss.resteasy.spi.ResteasyProviderFactory.addMessageBodyReader(ResteasyProviderFactory.java:478)
	at org.jboss.resteasy.spi.ResteasyProviderFactory.registerProvider(ResteasyProviderFactory.java:756)
	... 53 more

八月 03, 2019 11:51:22 上午 org.apache.catalina.core.ApplicationContext log
信息: Initializing Spring root WebApplicationContext
八月 03, 2019 11:51:22 上午 org.apache.catalina.core.StandardContext listenerStart
严重: Exception sending context initialized event to listener instance of class org.jboss.resteasy.plugins.spring.SpringContextLoaderListener
java.lang.RuntimeException: RESTeasy Provider Factory is null, do you have the ResteasyBootstrap listener configured?
	at org.jboss.resteasy.plugins.spring.SpringContextLoaderSupport.customizeContext(SpringContextLoaderSupport.java:53)
	at org.jboss.resteasy.plugins.spring.SpringContextLoader.customizeContext(SpringContextLoader.java:30)
	at org.springframework.web.context.ContextLoader.configureAndRefreshWebApplicationContext(ContextLoader.java:382)
	at org.springframework.web.context.ContextLoader.initWebApplicationContext(ContextLoader.java:283)
	at org.springframework.web.context.ContextLoaderListener.contextInitialized(ContextLoaderListener.java:112)
	at org.jboss.resteasy.plugins.spring.SpringContextLoaderListener.contextInitialized(SpringContextLoaderListener.java:44)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:5197)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5720)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:145)
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:1018)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:994)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:662)
	at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1899)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:301)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:619)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:566)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:301)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at com.sun.jmx.remote.security.MBeanServerAccessController.invoke(MBeanServerAccessController.java:468)
	at javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1487)
	at javax.management.remote.rmi.RMIConnectionImpl.access$300(RMIConnectionImpl.java:97)
	at javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1328)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1427)
	at javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:848)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:322)
	at sun.rmi.transport.Transport$2.run(Transport.java:202)
	at sun.rmi.transport.Transport$2.run(Transport.java:199)
	at java.security.AccessController.doPrivileged(Native Method)
	at sun.rmi.transport.Transport.serviceCall(Transport.java:198)
	at sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:567)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:828)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.access$400(TCPTransport.java:619)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler$1.run(TCPTransport.java:684)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler$1.run(TCPTransport.java:681)
	at java.security.AccessController.doPrivileged(Native Method)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:681)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
	at java.lang.Thread.run(Thread.java:745)
```
# 三、解决办法
```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.1.34.sec01</version><!--改为：1.1.34.sec01相安无事-->
</dependency>
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>resteasy-jaxrs</artifactId>
    <version>2.2.1.GA</version>
</dependency>
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>jaxrs-api</artifactId>
    <version>2.2.1.GA</version>
</dependency>
<dependency>
    <groupId>org.jboss.resteasy</groupId>
    <artifactId>resteasy-spring</artifactId>
    <version>2.2.1.GA</version>
</dependency>
```

# 四、细节说明
暂时不知道引起的原因，后期补充
