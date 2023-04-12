---
title: Maven修改setting配置中的java版本
date: 2022-08-09 21:49:38.513
updated: 2022-08-09 22:08:57.015
url: /archives/maven-xiu-gai-setting-pei-zhi-zhong-de-java-ban-ben
categories: 
- Maven
tags: 
---

Maven默认的java版本是1.5，要改成其他版本的话需要在setting.xml文件中添加以下的标签。
```
  <profiles>
    <profile>
	  <id>jdk-16</id>
	  <activation>
		<activeByDefault>true</activeByDefault>
		<jdk>16</jdk>
	  </activation>
	  <properties>
		<maven.compiler.source>16</maven.compiler.source>
		<maven.compiler.target>16</maven.compiler.target>
		<maven.compiler.compilerVersion>16</maven.compiler.compilerVersion>
	  </properties>
	</profile>
  </profiles>
```