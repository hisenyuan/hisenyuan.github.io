---
title: >-
  java.lang.IllegalArgumentException: addChild:  Child name '/hisen' is not
  unique
keywords: []
tags: []
date: 2017-02-27 15:00:46
categories:
---
报错如下：
```
27-Feb-2017 12:53:31.268 严重 [RMI TCP Connection(13)-127.0.0.1] org.apache.tomcat.util.modeler.BaseModelMBean.invoke Exception invoking method manageApp
 java.lang.IllegalArgumentException: addChild:  Child name '/hisen' is not unique
	at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:738)
	at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:728)
	at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:734)
	at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1702)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:300)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:482)
	at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:431)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:300)
	at com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:819)
	at com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
	at javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1487)
	at javax.management.remote.rmi.RMIConnectionImpl.access$300(RMIConnectionImpl.java:97)
	at javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1328)
	at javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1420)
	at javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:848)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:606)
	at sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:322)
	at sun.rmi.transport.Transport$1.run(Transport.java:177)
	at sun.rmi.transport.Transport$1.run(Transport.java:174)
	at java.security.AccessController.doPrivileged(Native Method)
	at sun.rmi.transport.Transport.serviceCall(Transport.java:173)
	at sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:556)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:811)
	at sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:670)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
	at java.lang.Thread.run(Thread.java:744)
```
重要的是：
```
org.apache.tomcat.util.modeler.BaseModelMBean.invoke Exception invoking method manageApp
 java.lang.IllegalArgumentException: addChild:  Child name '/hisen' is not unique
```
## IDEA解决办法 ##


Project Structure -> Artifacts

查看里面是否有配置相同的Artifacts

删除即可