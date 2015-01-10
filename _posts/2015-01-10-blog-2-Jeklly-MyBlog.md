---
layout: post
category: 技术
title: 我的博客建站使用技术[2]
tags: [Jeklly,Ruby]
published: true
description: Jeklly简介
<!--fullview: true-->
---


## 我的博客建站使用技术[2]

> 此[小灰@博客](http://xiaohui.me)基于GitHub Pages以及Jeklly技术编写。

---

### Jeklly

---

#### 1.什么是jekyll

Jekyll是一种简单的、适用于博客的、静态网站生成引擎。它使用一个模板目录作为网站布局的基础框架，支持Markdown、Textile等标记语言的解析，提供了模板、变量、插件等功能，最终生成一个完整的静态Web站点。说白了就是，只要安装Jekyll的规范和结构，不用写html，就可以生成网站。[*[jekyll介绍]*](http://jekyllbootstrap.com/lessons/jekyll-introduction.html)[*[jekyll on github]*](https://github.com/mojombo/jekyll)   [*[jekyllbootstrap]*](http://jekyllbootstrap.com/)。

Jekyll使用Liquid模板语言，`{{page.title}}`表示文章标题，`{{content}}`表示文章内容。我们可以用两种Liquid标记语言：输出标记（output markup）和标签标记 (tag markup)。输出标记会输出文本（如果被引用的变量存在），而标签标记不会。输出标记是用双花括号分隔，而标签标记是用花括号-百分号对分隔。[*[Liquid模板语言]*](https://github.com/shopify/liquid/wiki/liquid-for-designers) [*[Liquid模板变量参考]*](https://github.com/mojombo/jekyll/wiki/Template-Data)。

jekyll与github的关系：GitHub Pages一个由 GitHub 提供的用于托管项目主页或博客的服务，jekyll是后台所运行的引擎。

---

#### 2.Jeklly安装

查看[官方安装文档](http://jekyllrb.com/docs/installation/)。

---

#### 3.Jeklly启动

启动命令：

	jeklly serve
	
当看到如下信息时表明启动成功


	XxxMBP:xxxx.github.io xxx$ jekyll serve
	Configuration file: /Users/xxx/xxxx/xxxx.github.io/_config.yml
                Source: /Users/xxx/xxxx/xxxx.github.io
           Destination: /Users/xxx/xxxx/xxxx.github.io/_site
          Generating... 
                        done.
 	 Auto-regeneration: enabled for '/Users/xxx/xxxx/xxxx.github.io'
	Configuration file: /Users/xxx/xxxx/xxxx.github.io/_config.yml
    	Server address: http://0.0.0.0:4000/
 	  Server running... press ctrl-c to stop.
 	  
---
 	  
#### 4.Jeklly调试

该服务为自动热重启，当有文件改变时自动重启，刷新页面即可生效。

---

具体详细请点击[Jeklly](http://jekyllrb.com)查看关于Jeklly的介绍。

---