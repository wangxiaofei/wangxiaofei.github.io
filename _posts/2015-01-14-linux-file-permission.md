---
layout: post
category: 技术
title: Linux文件权限
tags: [Linux]
published: true
description: Linux文件权限详解
<!--fullview: true-->
---


## Linux "ls -l"文件列表权限详解

### 1. 使用 ls -l 命令 执行结果如下（/var/log） ：

	drwxr-x--- 2 root              adm    4096 2013-08-07 11:03 apache2  
	drwxr-xr-x 2 root              root   4096 2013-08-07 09:43 apparmor  
	drwxr-xr-x 2 root              root   4096 2013-08-07 09:44 apt  
	-rw-r----- 1 syslog            adm   16802 2013-08-07 14:30 auth.log  
	-rw-r--r-- 1 root              root    642 2013-08-07 11:03 boot.log  
	drwxr-xr-x 2 root              root   4096 2013-08-06 18:34 ConsoleKit  
	drwxr-xr-x 2 root              root   4096 2013-08-07 09:44 cups  
	-rw-r----- 1 syslog            adm   10824 2013-08-07 11:08 daemon.log  
	drwxr-xr-x 2 root              root   4096 2013-08-07 09:45 dbconfig-common  
	-rw-r----- 1 syslog            adm   21582 2013-08-07 11:03 debug  
	drwxr-xr-x 2 root              root   4096 2013-08-07 09:45 dist-upgrade  
	-rw-r--r-- 1 root              adm   59891 2013-08-07 11:03 dmesg  
	
展示结果大体分为七列（部分） ：

以第一条记录为例

- 第一列： `drwxr-x---` 表识文件的类型 和文件权限   
  
- 第二列： `2`是纯数字 ，表示 文件链接个数  
  
- 第三列： `root` 表示文件的所有者   
  
- 第四列： `adm` 表示为文件的所在群组   
  
- 第五列： `4096`，表示为文件长度（大小）  
  
- 第六列： `2013-08-07 11:03`，表示文件最后更新（修改）时间  
  
- 第七列： `apache2` 表示文件的名称  

详见下图：

![](http://img.blog.csdn.net/20130807144329984?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSmVuTWluWmhhbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 2. 文件类型和文件权限 ，即为列表第一列内容：（以第一条记录为例 ）

“drwxr-x---” 含义：有两部分组成 ，一部分是第一列即为“d” ,表示文件类型（目录或文件夹），另一部分是“rwxr-x---” 表示文件权限，权限有分为三段：即为 “ rwx ”,“  r-x  ”和 “ ---  ”分别表示 ，文件所有者的权限，文件所属组的权限 和其他用户对文件的权限。

####2.1 文件类型，大体分为如下几类 ：

	d ：目录   
	- ：文件   
	l ：链接   
	s ：socket   
	p ：named pipe   
	b ：block device   
	c ：character device  
	
####2.2 文件权限 ：

	r:含义为 “可读”，用数字 4 表示   
  
	w:含义为 “可写”用数字 2 表示  
  
	x:含义为“可执行”用数字 1 表示  
  
	-:含义为“无权限”用数字0 表示
  
	X:含义为只有目标文件对某些用户是可执行的或该目标文件是目录时才追加x 属性。   
	
	s:含义为 在文件执行时把进程的属主或组ID置为该文件的文件属主。方式“u＋s”设置文件的用户ID位，“g＋s”设置组ID位。 
	  
	t:含义为保存程序的文本到交换设备上  
	
### 3. 文件权限的更改

使用命令 :chmod  文件权限 文件名称 [-R]

命令两种用法 ：

#### 3.1 直接给文件赋相应的权限即为 :

使用符号模式可以设置多个项目：who(用户类型），operator(操作符）和permission(权限）,每个项目的设置可以用逗号隔开。 命令chmod将修改who指定的用户类型对文件的访问权限，用户类型由一个或者多个字母在who的位置来说明

如who的符号模式表所示:

|who	|用户类型|	说明
|||
|u	|user	|文件所有者
|g	|group	|文件所有者所在组
|o	|others	|所有其他用户
|a	|all	|所用用户, 相当于 ugo

operator的符号模式表:

|Operator	|说明
||
|+	|为指定的用户类型增加权限
|-	|去除指定用户类型的权限
|=	|设置指定用户权限的设置，即将用户类型的所有权限重新设置

permission的符号模式表:

|模式	|名字	|说明
|||
|r	|读	|设置为可读权限
|w	|写	|设置为可写权限
|x	|执行权限		|设置为可执行权限
|X	|特殊执行权限	|只有当文件为目录文件，或者其他类型的用户有可执行权限时，才将文件权限设置可执行
|s	|setuid/gid	|当文件被执行时，根据who参数指定的用户类型设置文件的setuid或者setgid权限
|t	|粘贴位	|设置粘贴位，只有超级用户可以设置该位，只有文件所有者u可以使用该位

符号模式实例:

- 对目录的所有者u和关联组g增加读r和写w权限:

		$ chmod ug+rw mydir
		$ ls -ld mydir
		drw-rw----   2 unixguy  uguys  96 Dec 8 12:53 mydir

- 对文件的所有用户ugo删除写w权限:

		$ chmod a-w myfile
		$ ls -l myfile
		-r-xr-xr-x   2 unixguy  uguys 96 Dec 8 12:53 myfile
	
- 对mydir的所有者u和关联组g设置成读r和可执行x权限:

		$ chmod ug=rx mydir
		$ ls -ld mydir
		dr-xr-x---   2 unixguy  uguys 96 Dec 8 12:53 mydir
	
#### 3.2 使用数字方式代替权限 ：   

chmod的八进制语法的数字说明：

- r 4

- w 2

- x 1

- \- 0

所有者的权限用数字表达：属主的那三个权限位的数字加起来的总和。如rwx ，也就是4+2+1 ，应该是7。

用户组的权限用数字表达：属组的那个权限位数字的相加的总和。如rw- ，也就是4+2+0 ，应该是6。

其它用户的权限数字表达：其它用户权限位的数字相加的总和。如r-x ，也就是4+0+1 ，应该是5。

数字含义详见如下列表：

|所有者|群组|其他|三位代表权限的数字|
|---|---|---|---|
|r w x|r w x|r w x|r w x|
|4 2 1|4 2 1|4 2 1| 777|
|4 2 1|0 0 0|4 0 1|705|

例如修改文件myfile的权限

	$ chmod 664 myfile
	$ ls -l myfile
	-rw-rw-r--  1   57 Jul  3 10:13  myfile


### 4. 文件所有者/群组的更改

命令 chown 用户名:群组名称 文件，例如 ：

	chown -R mysql:mysql auth.log #含义为 把 文件 auth.log 的所有者更改为 mysql -R为递归操作

---