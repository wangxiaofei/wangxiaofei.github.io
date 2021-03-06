---
layout: post
category: [技术]
title: xiaohui.me正式启用
tags: [域名]
published: true
description: xiaohui.me注册使用介绍
<!--fullview: true-->
---


## xiaohui.me正式启用

> 2015年1月8日，我的域名*`xiaohui.me`*正式启用，访问请点击*[小灰@博客](http://xiaohui.me)*。

---

### 1. 域名选择

在注册`xiaohui.me`域名之前我曾经注册过个人域名`wangxiaofei.net.cn`，当时是有优惠，6RMB/年，因为便宜，所以就注册了，但时候后来一直没怎么用。

现在发现自己需要一个个人博客的域名，所以放弃`wangxiaofei.net.cn`域名选择新域名，至于放弃域名主要有以下原因：

1. 域名太长
2. 域名带有真实名字

而`.me`域名虽然贵，但是是国际域名而且更加简短，所以我选择`xiaohui.me`顶级域名作为我个人域名。

---

### 2. 域名注册

域名从[新网][xinwang]购买，首次注册150RMB，续费290RMB/年，但是后来发现[DNSPod][dnspod]注册的域名首次注册`.me`域名需要200RMB，续费也是200RMB/年。不过[DNSPod][dnspod]将来会有域名导入

---

### 3. 修改域名DNS

因为域名在[新网][xinwang]购买，所以默认的DNS解析是用[新网][xinwang]的默认DNS解析，为了方便使用`GitHub Pages`，域名DNS解析换成了[DNSPod][dnspod]来解析

如果想把自己的域名更换DNSPod来解析，则修改自己域名的DNS为：

	f1g1ns1.dnspod.net
	f1g1ns2.dnspod.net
	
---

### 4. 增加域名A指向

要想把自己的域名指向固定的IP地址，则需要对域名添加`A记录`，记录值为指向IP地址。

如何获取目标IP地址，可以使用`dig`命令，比如CMD命令行中输入：

	dig wangxiaofei.github.com
	
则会出现以下信息：

	localhost:~ shawn$ dig wangxiaofei.github.com

	; <<>> DiG 9.8.3-P1 <<>> wangxiaofei.github.com
	;; global options: +cmd
	;; Got answer:
	;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 10184
	;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 0

	;; QUESTION SECTION:
	;wangxiaofei.github.com.		IN	A

	;; ANSWER SECTION:
	wangxiaofei.github.com.	30	IN	CNAME	github.map.fastly.net.
	github.map.fastly.net.	30	IN	A	103.245.222.133

	;; Query time: 299 msec
	;; SERVER: 192.168.1.1#53(192.168.1.1)
	;; WHEN: Thu Jan  8 15:21:40 2015
	;; MSG SIZE  rcvd: 91

其中`ANSWER SECTION`下面有IP`103.245.222.133`则是`wangxiaofei.github.com`指向的IP地址，把此地址添加到`A记录`当中即可。同时也可以通过向Github Pages中的`CNAME`添加`xiaohui.me`来反指，这样当访问<http://wangxiaofei.github.com>时则域名会变成<http://xiaohui.me>

我的域名指向我的[GitHub][github]博客[http://wangxiaofei.github.com](http://wangxiaofei.github.com)，此链接已经通过`CNAME`指向<http://xiaohui.me>

---

[xiaohui.me]:http://xiaohui.me
[github]:http://github.com
[xinwang]:http://www.xinnet.com "新网"
[dnspod]:http://www.dnspod.cn "DNSPod"




