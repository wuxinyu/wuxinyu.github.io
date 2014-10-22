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
	yum -y install python-devel
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

基于ssh自动部署

	#!/bin/sh
	
	hosts=`cat ~/host.txt`
	 
	for x in $hosts
	do
	        echo "===== $x ====="
	        ssh $x "mkdir -p /home/baoniu/hadoop_data/local; mkdir -p /home/baoniu/hadoop_data/logs"
	        scp -r ~/hadoop-0.23.1/  $x:~/
	done

storm依赖

	git clone https://github.com/nathanmarz/jzmq.git
	cd jzmq
	./autogen.sh
	./configure
	make
	sudo make install

storm submitter

	remote: storm jar storm-starter.jar storm.starter.RollingTopWords production-topology remote
	local:  storm jar storm-starter.jar storm.starter.RollingTopWords

如通过copy部署supervisor后 需清空strom.local.dir内容后再启动supervisor

hadoop 2.5

	yum -y install  lzo-devel  zlib-devel  gcc autoconf automake libtool gcc-c++  openssl-devel ncurses-devel cmake

	Protobuf、Ant、maven、findbugs jdk1.7

echo “export JAVA_HOME=/usr/java/jdk1.7.0_40/” >> /etc/profile

服务器端启动

usr/bin/rsync --daemon --config=/etc/rsyncd/rsyncd.conf
可能需要root权限运行.
/etc/rsyncd/rsyncd.conf 是你刚才编辑的rsyncd.conf的位置.
也可以在/etc/rc.d/rc.local里加入让系统自动启动等.
客户端同步

rsync -参数 用户名@同步服务器的IP::rsyncd.conf中那个方括号里的内容 本地存放路径 如:
rsync -avzP nemo@192.168.10.1::nemo /backup
说明：
-a 参数，相当于-rlptgoD，-r 是递归 -l 是链接文件，意思是拷贝链接文件；-p 表示保持文件原有权限；-t 保持文件原有时间；-g 保持文件原有用户组；-o 保持文件原有属主；-D 相当于块设备文件；
-z 传输时压缩；
-P 传输进度；
-v 传输时的进度等信息