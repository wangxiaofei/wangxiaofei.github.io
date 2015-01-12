---
layout: post
category: 技术
title: 初识MongoDB
tags: [MongDB,DB]
published: true
description: 初识MongoDB
<!--fullview: true-->
---

## 初识MongoDB

> MongoDB是一种文件导向数据库管理系统，由C++撰写而成，以此来解决应用程序开发社区中的大量现实问题。2007年10月，MongoDB由10gen团队所发展。2009年2月首度推出。

---

### 1 安装与部署

MongoDB可以从开放源代码来建构与安装，更常见的是安装binary文件，目前有Windows, Linux, OS X和Solaris版本。许多Linux包管理系统现在已包含了MongoDB的包，包括CentOS和Fedora, Debian和Ubuntu, Gentoo以及Arch Linux。 同样可从官方网站获取。

MongoDB使用内存映射文件, 32位系统上限制大小为2GB的数据 (64-比特支持更大的数据). MongoDB服务器只能用在小端序系统，虽然大部分公司会同时准备小端序和大端序系统。

#### 1.1 语言支持

MongoDB有官方的驱动如下：

- C
- C++
- C# / .NET
- Erlang
- Haskell
- Java
- JavaScript
- Lisp
- node.JS
- Perl
- PHP
- Python
- Ruby
- Scala

目前还有许多非官方式的驱动，在ColdFusion, Delphi, Erlang, Factor, Fantom, Go, JVM languages (Clojure, Groovy , Scala, etc.), Lua, HTTP REST, Racket, and Smalltalk.

#### 1.2 复制

MongoDB的开发人员可以保证一个操作已被复制到至少_N_个服务器上每个运行的基础.

#### 1.3 主从式

由于操作都是在主机，从机将复制任何更改的数据。

例如：starting a master/slave pair locally:

	$ mkdir -p ~/dbs/master ~/dbs/slave
	$ ./mongod --master --port 10000 --dbpath ~/dbs/master
	$ ./mongod --slave --port 10001 --dbpath ~/dbs/slave --source localhost:10000
	
#### 1.4 副本集

副本集类似于主从式架构，但他们结合的能力为副机，如果当前一直迟缓时，选出新的主机。

---

### 2 管理与图形化接口

#### 2.1 监视

支持MongoDB的监视插件：

- munin
- ganglia
- scout
- cacti

#### 2.2 GUIs

目前较受欢迎的UI有：

- Fang of Mongo –这是一个网页式的接口，由Django和jQuery所构成.
- Futon4Mongo – a clone of the CouchDB Futon web interface for MongoDB.
- Mongo3 – Ruby写成的接口.
- MongoHub –一个OS X应用程序.
- Opricot – a browser-based MongoDB shell,由PHP撰写而成.
- Database Master MongoDB Tool for Windows
- RockMongo Best PHP MongoDB Administrator轻量级，支持多国语言.
- MongoVUE Download CS，图形界面，封装较好。


*注：以上内容摘自[维基百科](http://zh.wikipedia.org/wiki/MongoDB).*

---

### 3 MongoDB特点

特点：高性能、易部署、易使用，存储数据非常方便。

主要功能特性有：

- 面向集合存储，易存储对象类型的数据

- 模式自由

- 支持动态查询

- 支持完全索引，包含内部对象

- 支持查询

- 支持复制和故障恢复

- 使用高效的二进制数据存储，包括大型对象（如视频等）

- 自动处理碎片，以支持云计算层次的扩展性

- 支持RUBY，PYTHON，JAVA，C++，PHP等多种语言

- 文件存储格式为BSON（一种JSON的扩展）

- 可通过网络访问

使用原理

所谓“面向集合”（Collenction-Oriented），意思是数据被分组存储在数据集中，被称为一个集合（Collenction)。每个集合在数据库中都有一个唯一的标识名，并且可以包含无限数目的文档。集合的概念类似关系型数据库（RDBMS）里的表（table），不同的是它不需要定义任何模式（schema)。

模式自由（schema-free)，意味着对于存储在mongodb数据库中的文件，我们不需要知道它的任何结构定义。如果需要的话，你完全可以把不同结构的文件存储在同一个数据库里。

存储在集合中的文档，被存储为键-值对的形式。键用于唯一标识一个文档，为字符串类型，而值则可以是各种复杂的文件类型。我们称这种存储形式为BSON（Binary JSON）。

注：转自<http://www.cnblogs.com/hoojo/archive/2011/06/01/2066119.html>

---

### 4 MongoDB设计初识[后续会继续更新独立文章]

MongoDB和传统SQL schema设计上最大的区别就是关于模型关系用什么方法表示比较好(在MongoDB里即可以用Link,又可以用Embedded)

简单总结下：

1. FirstClass （比如“User”这种） 应该用独立的Collection
2. "条目类型"的，应该 embedded
3. 两个模型之间如果是包含关系，用 embedded
4. 多对多关系，用 link(类似sql里面的foregin key)
5. 如果一个模型，其可能存的对象很少，那么就用独立的collection，这样有助于mongodb server做缓存
6. embedded方式不利于做复杂的关联，复杂的查询
7. embedded方式性能很有优势，如果你有“性能”方面的要求，可以考虑用embbed

---










