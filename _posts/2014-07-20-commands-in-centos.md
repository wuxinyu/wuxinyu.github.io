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

修改机器名
	
	/etc/sysconfig/network
	/etc/hosts
	hostname your-hostname

安装scp host wget等命令

	yum install openssh-clients ind-utils wget

关闭Selinux

	1      vi /etc/selinux/config
	2      #SELINUX=enforcing     #注释掉
	3      #SELINUXTYPE=targeted  #注释掉
	4      SELINUX=disabled  #增加
	5      :wq  #保存，关闭。
	6      shutdown -r now   #重启系统

查看SELinux的状态：

	getenforce

监听日志变化
	
	tail -f
