---
layout: post
category: 技术
title: Apache Maven
tags: [Java]
published: true
description: Apache Maven 简介
<!--fullview: true-->
---

## Apache Maven

> Apache Maven，是一个软件（特别是Java软件）项目管理及自动构建工具，由Apache软件基金会所提供。基于项目对象模型（缩写：POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤。曾是Jakarta项目的子项目，现为独立Apache项目。

### 示例

Maven项目使用称为项目对象模型（Project Object Model，POM）来配置的。

项目对象模型存储在命名为 pom.xml 的文件中。

以下是一个简单的示例：

	<project>
	  <!-- model version is always 4.0.0 for Maven 2.x POMs -->
	  <modelVersion>4.0.0</modelVersion>
	 
	  <!-- project coordinates, i.e. a group of values which
	       uniquely identify this project -->
 
	  <groupId>com.mycompany.app</groupId>
	  <artifactId>my-app</artifactId>
	  <version>1.0</version>
 
	  <!-- library dependencies -->
 
	  <dependencies>
	    <dependency>
 
	      <!-- coordinates of the required library -->
 
	      <groupId>junit</groupId>
	      <artifactId>junit</artifactId>
	      <version>3.8.1</version>
 
	      <!-- this dependency is only used for running and compiling tests -->
 
	      <scope>test</scope>
 
	    </dependency>
	  </dependencies>
	</project>
	
### 常用Maven命令

Maven2 的运行命令为 ： ```mvn```

常用命令为 ：

	mvn archetype:generate	#创建 Maven 项目
    mvn compile				#编译源代码
    mvn test-compile		#编译测试代码
    mvn test				#运行应用程序中的单元测试
    mvn site				#生成项目相关信息的网站
    mvn clean				#清除目标目录中的生成结果
    mvn package				#依据项目生成 jar 文件
    mvn install				#在本地 Repository 中安装 jar
    mvn eclipse:eclipse		#生成 Eclipse 项目文件
    
生成项目

建一个 JAVA 项目 ： 

	mvn archetype:generate-DgroupId=com.demo -DartifactId=App
          
建一个 web 项目 ： 

	mvn archetype:generate-DgroupId=com.demo -DartifactId=web-app -DarchetypeArtifactId=maven-archetype-webapp
	
简单解释一下：

archetype  是一个内建插件，他的create任务将建立项目骨架

archetypeArtifactId   项目骨架的类型

DartifactId 项目名称

---