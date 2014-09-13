---
title: hadoop scm server on centos 6
layout: post
category : automatic
tagline: centos
tags : [linux]
---

1.修改yum.conf
	
	cachedir=/var/cache/yum
	keepcache=1 //缓存所有rpm 形成repo 供所有子节点使用

2.安装必须的rpm
	
	yum groupinstall -y "Development Tools"
	yum install -y wget openssh-clients unzip 

3.安装web.py 并安装
	wget http://webpy.org/static/web.py-0.37.tar.gz
	python setup.py install

4.准备Hadoop2.5.0 zookeeper3.4.6 jdk7 mysql