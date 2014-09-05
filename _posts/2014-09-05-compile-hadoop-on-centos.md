---
title: compile hadoop2.5.0 on centos
layout: post
category : automatic
tagline: centos
tags : [linux]
---


	#!/bin/bash

	echo -e "\033[31m"
	echo -e "Program : Hadoop-2.5.0_CentOS-7.0-x86_64-Compile.sh "
	echo -e "Hadoop 2.5.0 Compile Shell Script (CentOS 7.0 x86_64) "
	echo -e "by Shau-Rong Lu 2014-08-25 "
	echo -e "\033[0m"

	cd /usr/local/src
	#yum -y groupinstall  "Development tools"
	yum -y install gcc  gcc-c++  svn  cmake git zlib zlib-devel openssl openssl-devel rsync java-1.7.0-openjdk.x86_64 java-1.7.0-openjdk-devel.x86_64  make  wget

	# echo "********** Install OpenJDK **********"

	yum -y install  java
	# or
	#yum -y install java-1.7.0-openjdk

	yum -y install  java-1.7.0-openjdk-devel

	#export JAVA_HOME=/usr
	#echo 'export JAVA_HOME=/usr' >> /etc/profile
	#echo 'export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.65-2.5.1.2.el7_0.x86_64' >> /etc/profile
	echo 'export JAVA_HOME=/usr/lib/jvm/java' >> /etc/profile
	echo 'export PATH=$PATH:$JAVA_HOME/bin' >> /etc/profile
	echo 'export CLASSPATH=$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar' >> /etc/profile

	source /etc/profile
	java -version
	#export | grep JAVA
	#export | grep jdk

	echo "********** Install Apache Maven 3.0.5 (yum) **********"

	yum -y install maven

	# mvn -version

	echo "********** Install FindBugs 3.0.0 **********"

	cd  /usr/local/src
	if [ ! -s findbugs-3.0.0.tar.gz ]; then
	  wget  http://jaist.dl.sourceforge.net/project/findbugs/findbugs/3.0.0/findbugs-3.0.0.tar.gz
	fi
	tar zxvf findbugs-3.0.0.tar.gz -C /usr/local/
	ln -s /usr/local/findbugs-3.0.0/bin/findbugs  /usr/bin/findbugs
	echo 'export FINDBUGS_HOME=/usr/local/findbugs-3.0.0' >> /etc/profile
	echo 'export PATH=$PATH:$FINDBUGS_HOME/bin' >> /etc/profile
	source /etc/profile

	#export | grep FINDBUGS_HOME
	#export | grep PATH
	#read -n 1 -p "Press Enter to continue..."

	echo "********** Install Protoc 2.5.0 **********"
	# https://code.google.com/p/protobuf/
	# https://code.google.com/p/protobuf/downloads/list

	cd  /usr/local/src
	if [ ! -s protobuf-2.5.0.tar.gz ]; then
	  wget https://protobuf.googlecode.com/files/protobuf-2.5.0.tar.gz
	fi
	tar zxvf protobuf-2.5.0.tar.gz -C /usr/local/src
	cd /usr/local/src/protobuf-2.5.0
	./configure
	make
	make install
	ln -s /usr/local/bin/protoc /usr/bin/protoc
	echo 'export PROTO_HOME=/usr/local/' >> /etc/profile
	echo 'export PATH=$PATH:$PROTO_HOME/bin' >> /etc/profile
	source /etc/profile
	#read -n 1 -p "Press Enter to continue..."

	echo "********** Compile Hadoop **********"

	cd  /usr/local/src
	if [ ! -s hhadoop-2.5.0-src.tar.gz ]; then
	  wget  http://ftp.tc.edu.tw/pub/Apache/hadoop/common/hadoop-2.5.0/hadoop-2.5.0-src.tar.gz
	fi
	tar zxvf hadoop-2.5.0-src.tar.gz -C /usr/local/src
	cd  /usr/local/src/hadoop-2.5.0-src/

	#mvn clean
	mvn package -Pdist,native -DskipTests -Dtar
	#read -n 1 -p "Press Enter to continue..."
