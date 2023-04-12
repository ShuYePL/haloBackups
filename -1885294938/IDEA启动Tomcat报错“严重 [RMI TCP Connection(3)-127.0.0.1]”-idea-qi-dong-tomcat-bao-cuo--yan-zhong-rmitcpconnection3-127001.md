---
title: IDEA启动Tomcat报错“严重 [RMI TCP Connection(3)-127.0.0.1]”
date: 2022-07-07 09:44:31.755
updated: 2022-07-07 09:44:31.755
url: /archives/idea-qi-dong-tomcat-bao-cuo--yan-zhong-rmitcpconnection3-127001
categories: 
- JavaWeb
tags: 
- 报错
---

报错的完整信息如下
~~~
07-Jul-2022 09:32:14.775 严重 [RMI TCP Connection(3)-127.0.0.1] org.apache.tomcat.util.modeler.BaseModelMBean.invoke 调用方法[manageApp]时发生异常
	java.lang.IllegalStateException: 启动子级时出错
		at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:729)
		at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:698)
		at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:747)
		at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1873)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:78)
		at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.base/java.lang.reflect.Method.invoke(Method.java:567)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:293)
		at java.management/com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:809)
		at java.management/com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:428)
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:376)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:78)
		at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.base/java.lang.reflect.Method.invoke(Method.java:567)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:293)
		at java.management/com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:809)
		at java.management/com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at java.management/com.sun.jmx.remote.security.MBeanServerAccessController.invoke(MBeanServerAccessController.java:468)
		at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1466)
		at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1307)
		at java.base/java.security.AccessController.doPrivileged(AccessController.java:691)
		at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1406)
		at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:827)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:78)
		at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.base/java.lang.reflect.Method.invoke(Method.java:567)
		at java.rmi/sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:357)
		at java.rmi/sun.rmi.transport.Transport$1.run(Transport.java:200)
		at java.rmi/sun.rmi.transport.Transport$1.run(Transport.java:197)
		at java.base/java.security.AccessController.doPrivileged(AccessController.java:691)
		at java.rmi/sun.rmi.transport.Transport.serviceCall(Transport.java:196)
		at java.rmi/sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:587)
		at java.rmi/sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:828)
		at java.rmi/sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.lambda$run$0(TCPTransport.java:705)
		at java.base/java.security.AccessController.doPrivileged(AccessController.java:391)
		at java.rmi/sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:704)
		at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1130)
		at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:630)
		at java.base/java.lang.Thread.run(Thread.java:831)
	Caused by: org.apache.catalina.LifecycleException: 无法启动组件[StandardEngine[Catalina].StandardHost[localhost].StandardContext[/jspoa]]
		at org.apache.catalina.util.LifecycleBase.handleSubClassException(LifecycleBase.java:440)
		at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:198)
		at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:726)
		... 42 more
	Caused by: java.lang.IllegalArgumentException: servlet映射中的<url pattern>[t_student/modify]无效
		at org.apache.catalina.core.StandardContext.addServletMappingDecoded(StandardContext.java:3262)
		at org.apache.catalina.Context.addServletMappingDecoded(Context.java:905)
		at org.apache.catalina.startup.ContextConfig.configureContext(ContextConfig.java:1562)
		at org.apache.catalina.startup.ContextConfig.webConfig(ContextConfig.java:1329)
		at org.apache.catalina.startup.ContextConfig.configureStart(ContextConfig.java:986)
		at org.apache.catalina.startup.ContextConfig.lifecycleEvent(ContextConfig.java:303)
		at org.apache.catalina.util.LifecycleBase.fireLifecycleEvent(LifecycleBase.java:123)
		at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5084)
		at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:183)
		... 43 more
07-Jul-2022 09:32:14.777 严重 [RMI TCP Connection(3)-127.0.0.1] org.apache.tomcat.util.modeler.BaseModelMBean.invoke 调用方法[createStandardContext]时发生异常
	javax.management.RuntimeOperationsException: 调用方法[manageApp]时发生异常
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:303)
		at java.management/com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:809)
		at java.management/com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:428)
		at org.apache.catalina.mbeans.MBeanFactory.createStandardContext(MBeanFactory.java:376)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:78)
		at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.base/java.lang.reflect.Method.invoke(Method.java:567)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:293)
		at java.management/com.sun.jmx.interceptor.DefaultMBeanServerInterceptor.invoke(DefaultMBeanServerInterceptor.java:809)
		at java.management/com.sun.jmx.mbeanserver.JmxMBeanServer.invoke(JmxMBeanServer.java:801)
		at java.management/com.sun.jmx.remote.security.MBeanServerAccessController.invoke(MBeanServerAccessController.java:468)
		at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl.doOperation(RMIConnectionImpl.java:1466)
		at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl$PrivilegedOperation.run(RMIConnectionImpl.java:1307)
		at java.base/java.security.AccessController.doPrivileged(AccessController.java:691)
		at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl.doPrivilegedOperation(RMIConnectionImpl.java:1406)
		at java.management.rmi/javax.management.remote.rmi.RMIConnectionImpl.invoke(RMIConnectionImpl.java:827)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:78)
		at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.base/java.lang.reflect.Method.invoke(Method.java:567)
		at java.rmi/sun.rmi.server.UnicastServerRef.dispatch(UnicastServerRef.java:357)
		at java.rmi/sun.rmi.transport.Transport$1.run(Transport.java:200)
		at java.rmi/sun.rmi.transport.Transport$1.run(Transport.java:197)
		at java.base/java.security.AccessController.doPrivileged(AccessController.java:691)
		at java.rmi/sun.rmi.transport.Transport.serviceCall(Transport.java:196)
		at java.rmi/sun.rmi.transport.tcp.TCPTransport.handleMessages(TCPTransport.java:587)
		at java.rmi/sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run0(TCPTransport.java:828)
		at java.rmi/sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.lambda$run$0(TCPTransport.java:705)
		at java.base/java.security.AccessController.doPrivileged(AccessController.java:391)
		at java.rmi/sun.rmi.transport.tcp.TCPTransport$ConnectionHandler.run(TCPTransport.java:704)
		at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1130)
		at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:630)
		at java.base/java.lang.Thread.run(Thread.java:831)
[2022-07-07 09:32:14,807] Artifact jspoa:war exploded: Error during artifact deployment. See server log for details.
	Caused by: java.lang.IllegalStateException: 启动子级时出错
		at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:729)
		at org.apache.catalina.core.ContainerBase.addChild(ContainerBase.java:698)
		at org.apache.catalina.core.StandardHost.addChild(StandardHost.java:747)
		at org.apache.catalina.startup.HostConfig.manageApp(HostConfig.java:1873)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:78)
		at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.base/java.lang.reflect.Method.invoke(Method.java:567)
		at org.apache.tomcat.util.modeler.BaseModelMBean.invoke(BaseModelMBean.java:293)
		... 34 more
	Caused by: org.apache.catalina.LifecycleException: 无法启动组件[StandardEngine[Catalina].StandardHost[localhost].StandardContext[/jspoa]]
		at org.apache.catalina.util.LifecycleBase.handleSubClassException(LifecycleBase.java:440)
		at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:198)
		at org.apache.catalina.core.ContainerBase.addChildInternal(ContainerBase.java:726)
		... 42 more
	Caused by: java.lang.IllegalArgumentException: servlet映射中的<url pattern>[t_student/modify]无效
		at org.apache.catalina.core.StandardContext.addServletMappingDecoded(StandardContext.java:3262)
		at org.apache.catalina.Context.addServletMappingDecoded(Context.java:905)
		at org.apache.catalina.startup.ContextConfig.configureContext(ContextConfig.java:1562)
		at org.apache.catalina.startup.ContextConfig.webConfig(ContextConfig.java:1329)
		at org.apache.catalina.startup.ContextConfig.configureStart(ContextConfig.java:986)
		at org.apache.catalina.startup.ContextConfig.lifecycleEvent(ContextConfig.java:303)
		at org.apache.catalina.util.LifecycleBase.fireLifecycleEvent(LifecycleBase.java:123)
		at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5084)
		at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:183)
		... 43 more
07-Jul-2022 09:32:23.751 信息 [Catalina-utility-2] org.apache.catalina.startup.HostConfig.deployDirectory 把web 应用程序部署到目录 [D:\dev\apache-tomcat-10.0.20\webapps\manager]
07-Jul-2022 09:32:23.963 信息 [Catalina-utility-2] org.apache.catalina.startup.HostConfig.deployDirectory Web应用程序目录[D:\dev\apache-tomcat-10.0.20\webapps\manager]的部署已在[212]毫秒内完成
~~~
在这里可以看到有一行的内容是
~~~
Caused by: java.lang.IllegalArgumentException: servlet映射中的<url pattern>[t_student/modify]无效
~~~
所以这个的原因是因为我把 servlet 映射地址配错了，正确的地址前面要加上一个 / ，也就是
~~~
/t_student/modify
~~~