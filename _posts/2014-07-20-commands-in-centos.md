---
title: command on centos
layout: post
category : automatic
tagline: centos
tags : [linux]
---

Development Tools安装

	yum groupinstall "Development Libraries"
	yum groupinstall "Development Tools"
	yum install ncurses-devel zlib-devel texinfo gtk+-devel gtk2-devel qt-devel tcl-devel tk-devel libX11-devel kernel-headers kernel-devel

ssh无密码登陆

	$ssh-keygen -t rsa
	$ssh-copy-id -i root@serverip

网卡udev目录

	/etc/udev/rules.d/70-persistent-net.rules 

复制虚拟机导致mac地址错误

	修改udev名称 从eth1为eth0 并删除错误的配置节
	修改eth0配置文件的mac地址为新的地址 重启

查询网关

	route -n
