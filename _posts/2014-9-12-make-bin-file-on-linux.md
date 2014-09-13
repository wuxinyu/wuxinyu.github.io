---
title: make bin file on linux
layout: post
category : automatic
tagline: centos
tags : [linux]
---
bin的制作
<p>
linux 下制作二进制 .bin  的文件
制做方法是使用cat 命令将执行脚本和打包文件同事放到一个.bin的文件里
这样安装的时候只要使用一个包，直接执行该包即可安装完毕，简单方便。
例：制作安装apache、mysql的安装脚本包
<p>
1.将源码包先打包

	#tar zcvf packages.tar.gz httpd-2.0.63.tar.bz2 mysql-5.0.33.tar.gz

2.编写脚本如下：

	#cat install.sh packages.tar.gz >install.bin

这样就生成install.bin的安装文件，改文件是由shell脚# cat install.sh

	#!/bin/bash
	dir_tmp=/root/installapache
	mkdir $dir_tmp
	sed -n -e ‘1,/^exit 0$/!p’ $0 > “${dir_tmp}/packages.tar.gz” 2>/dev/null
	cd $dir_tmp
	tar zxf packages.tar.gz
	tar jxf httpd-2.0.63.tar.bz2
	cd  httpd-2.0.63
	./configure –prefix=/tmp/apache2
	make
	make install
	cd $dir_tmp
	tar zxf mysql-5.0.33.tar.gz
	cd mysql-5.0.33
	./configure –with-charset=gbk –with-extra-charsets=binary,latin1,gb2312 \
	–localstatedir=/home/db –with-mysqld-ldflags=-all-static -enable-assembler –with-innodb –prefix=/tmp/mysql5
	make
	make install
	exit 0本和二进制合成的。

前半部分是脚本后半部分是二进制文件，用strings等二进制查看命令可以看到
最主要的是下面这句，是将二进制文件从.bin文件里分离出来

	sed -n -e ‘1,/^exit 0$/!p’ $0 > “${dir_tmp}/packages.tar.gz” 2>/dev/null

##上面一句的意思为打印除从第一行到所在exit 0的行的所有行到${dir_tmp}/packages.tar.gz,如果过程中有错误则输出到/dev/null
安装的时候直接执行sh install.bin
安装这个方法可以将我们平时常使用的安装脚本化，然后打包。以后使用就方便了。
