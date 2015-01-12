---
layout: post
category: 技术
title: MongoDB设计原则
tags: [MongDB,DB]
published: true
description: MongoDB设计原则
<!--fullview: true-->
---

## MongoDB设计原则

### Inserting

#### Document-Orientation

在描述中，MongoDB是面向文档的，意味着在这种数据库中主要存储单位是Collection。
 
一些常见的数据格式例如：JSON、XML、简单的键/值对。
 
储存在MongoDB中的文档是一种类JSON格式，为了得到更高的效率，使用了一种二进制表现形式且被称为BSON的格式。目标是使数据更紧凑和合理以便于扫描。
 
客户端序列化数据成BSON传送至数据库中，数据是以BSON格式被存储的。因此，读取数据的时候，数据库只需做很小的解析处理就可以传送出去，更加高效。然后客户端在反序列化BSON格式为当前语言使用的格式。

#### JSON

一段数据：

	{ author: 'joe',  
	  created : new Date('03-28-2009'),  
	  title : 'Yet another blog post',  
	  text : 'Here is the text...',  
	  tags : [ 'example', 'joe' ],  
	  comments : [ { author: 'jim', comment: 'I disagree' },  
	              { author: 'nancy', comment: 'Good post' }  
	  ]  
	}  
	
存储的实例：

	> doc = { author : 'joe', created : new Date('03-28-2009'), ... }  
	> db.posts.insert(doc);
	
#### Mongo-Friendly Schema

Mongo可以用于很多方面，第一反应或许是如何用它来编写一个使用关系数据库的应用程序。虽然这项工作非常好，但是也不能展现Mongo的真正力量。

#### Store Example

	item  
	   title  
	   price  
	   sku  
	item_features  
	   sku  
	   feature_name  
	   feature_value  
	   
不同的商品有不同的属性，但又不想在一张表中包含所有可能出现的属性。(一般关系数据库中，可能都会另建属性表，跟分类表类似)在Mongo中同样可以创建这个模型，而且更加高效。

	item : {  
	         "title" : <title> ,  
	         "price" : <price> ,  
	         "sku"   : <sku>   ,  
	         "features" : {  
	            "optical zoom" : <value> ,  
	            ...  
	         }  
	}
	
这样做有几个好处：

1、一次数据库查询可以得到整条记录。。
    
2、一条记录的所有信息都书存储在硬盘的的同一片区域，所以一次检索可以可以得到所有数据。

3、插入或更新单条属性时：  

	db.items.update( { sku : 123 } , { "$set" : { "features.zoom" : "5" } } )  

4、插入一条新属性不需要在硬盘上移动整条记录，Mongo有一个预留机制，预留出了一部分空间以适应数据对象的增长。也可以预防索引的增长等问题。

### Legal Key Names

键的命名有以下限制：

1. $不能出作为第一个字符
2. (.)点不能出现在键名中

### Schema Design(数据库设计)

#### Introduction

在Mongo里，比起设计数据库关系模式，你只需做很少的标准化工作， because there are no server-side "joins"。通常来说，都希望每个顶级对象对应一个Collection。

每一种分类都建立一个Collection，只需创建一个嵌入式对象。例如在下面的图中，我们有两个Collection，student和coureses。学生Collection中包含一个嵌入的address文档和coursesCollection有联系的score文档。

![](http://dl.iteye.com/upload/attachment/294224/40d2e57c-cf75-3509-af35-8f168a7c9ca7.png)

如果用关系数据库来设计，几乎肯定会把score分离出来单独做一张表，然后加一个外键和student相连。

#### Embed vs. Reference

在Mongo数据库设计中关键的一句话是“比起嵌入到其他Collection中做一个子对象，每个对象值得拥有自己的Collection吗？”。在关系数据库中。每个有兴趣的子项目通常都会分离出来单独设计一张表（除非为了性能的考虑）。而在Mongo中，是不建议使用这种设计的，嵌入式的对象更高效。(这句不是很确定Data is then colocated on disk; client-server turnarounds to the database are eliminated)数据是即时同步到硬盘上的，客户端与服务器不必要在数据库上做周转。所以通常来说问题就是“为什么不使用嵌入式对象呢？”
 
利用上面的例子，我们来看下为什么引用比较慢
 
	print( student.address.city );  

address是嵌入式对象，所以这个操作通常是很快速的，如果sdudent被放在内存中，那address也通常在内存中。然而下面这个例子： 

	print( student.scores[0].for_course.name );  
 
如果是第一次访问scores[0]的内容，会先执行下面这句：
 
	// pseudocode for driver or framework, not user code  
  
	student.scores[0].for_course = db.courses.findOne({_id:_course_id_to_find_});  
 
因此，每一次引用遍历都是一个数据库查询。一般来说，有问题的Collection都是默认的在_id建有索引，查询会稍微快一些。然而即使所有的数据都缓存在内存中，鉴于服务器端/客户端 的应用程序和数据库通信时仍然会有一些延迟。一般来说，期望在查询时缓存命中的境况下有1ms。因此，如果我们迭代1000 student，查找即使在有缓存的情况下仍然是很慢的，超过1m。如果我们只需要查找一条记录，时间应该在1ms左右，这对于一个网页加载来说是可以接受的。(注意：如果数据已经在缓存中,取出1000条数据也许花费时间少于1m,)

一些规则：

1. 顶级对象，一般都有自己的Collection
2. 线性细节对象，一般作为嵌入式的
3. 一个对象和另一个对象是包含关系时通常采用嵌入式设计
4. 多对多的关系通常采取引用设计
5. 只含有几个简单对象的可以单独作为一个Collection，因为整个Collection可以很快的被缓存在应用程序服务器内存中。
6. 在Collection中嵌入式对象比顶级对象更难引用。as you cannot have a DBRef to an embedded object (at least not yet).
7. It is more difficult to get a system-level view for embedded objects. For example, it would be easier to query the top 100 scores across all students if Scores were not embedded.
8. 如果将要嵌入的数据量很大（很多M），你可以限制单个对象的大小
9. 如果性能存在问题，请使用嵌入式设计

原文 写道

* "First class" objects, that are at top level, typically have their own collection.
* Line item detail objects typically are embedded.
* Objects which follow an object modelling "contains" relationship should generally be embedded.
* Many to many relationships are generally by reference.
* Collections with only a few objects may safely exist as separate collections, as the whole collection is quickly cached in application server memory.
* Embedded objects are harder to reference than "top level" objects in collections, as you cannot have a DBRef to an embedded object (at least not yet).
* It is more difficult to get a system-level view for embedded objects. For example, it would be easier to query the top 100 scores across all students if Scores were not embedded.
* If the amount of data to embed is huge (many megabytes), you may reach the limit on size of a single object.
* If performance is an issue, embed.

#### Use Cases

来看几个实例

1.客户/订单/订单项目
          
订单必须作为一个Collection,客户作为一个Collection,订单项目必须作为一个子数组嵌入到订单Collection中
 
2.博客系统

posts应该作为一个Collection,auth可以作为一个单独的Collection,或者auth包含的字段很少,比如只有email,address之类的为了更高的性能,也应该设计为嵌入式的对象.

#### Index Selection

数据库设计的第二个方面是索引的选择,一般规则:在mysql中需要的索引,在Mongo中也同样需要.
 
_id字段是自动被索引的

Fields upon which keys are looked up should be indexed.

排序字段一定建立索引.

---

*注：转自<http://kisa77.iteye.com/blog/738584>*

---