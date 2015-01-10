---
layout: post
category: 技术
title: 我的博客建站使用技术[1]
tags: [GitHub Pages]
published: true
description: GitHub Pages简介
<!--fullview: true-->
---


## 我的博客建站使用技术[1]

> 此[小灰@博客](http://xiaohui.me)基于GitHub Pages以及Jeklly技术编写。

----

###GitHub Pages
---

####1.Git简介

- Git是一个开源的分布式版本控制系统，用以有效、高速的处理从很小到非常大的项目版本管理。

- GitHub可以托管各种git库的站点。

- GitHub Pages免费的静态站点，三个特点：免费托管、自带主题、支持自制页面和Jekyll。

---

####2.为什么使用Github Pages

1. 搭建简单而且免费；

2. 支持静态脚本；

3. 可以绑定你的域名；

4. DIY自由发挥，动手实践一些有意思的东西git,markdown,bootstrap,jekyll；

5. 理想写博环境，git+github+markdown+jekyll；

---

####3.创建Github Pages

3.1 安装git工具

_[winddows安装](http://windows.github.com/)_

_[mac安装](http://mac.github.com/)_

3.2 两种pages模式

1.User/Organization Pages 个人或公司站点

- 使用自己的用户名，每个用户名下面只能建立一个；

- 资源命名必须符合这样的规则username/username.github.com；

- 主干上内容被用来构建和发布页面


2.Project Pages 项目站点

- gh-pages分支用于构建和发布；

- 如果user/org pages使用了独立域名，那么托管在账户下的所有project pages将使用相同的域名进行重定向，除非project pages使用了自己的独立域名；

- 如果没有使用独立域名，project pages将通过子路径的形式提供服务username.github.com/projectname；

- 自定义404页面只能在独立域名下使用，否则会使用User Pages 404；

- 创建项目站点步骤：

		$ git clone https://github.com/USERNAME/PROJECT.git PROJECT

		$ git checkout --orphan gh-pages
	
		$ git rm -rf .

		$ git add .

		$ git commit -a -m "First pages commit"

		$ git push origin gh-pages
	
3.可以通过User/Organization Pages建立主站，而通过Project Pages挂载二级应用页面。

---

点击[GitHub Pages](https://pages.github.com)查看关于GitHub Pages官网的介绍。

---

*转自：[http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html](http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html)*

---