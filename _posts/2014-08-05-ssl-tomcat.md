---
title: ssl webserver
layout: post
category : dev
tagline: dev
tags : [java,tomcat]
---
	keytool -import -alias startsslca -file ca.cer \
	  -keystore checkthecrowd.jks -trustcacerts
	keytool -import -alias startsslca1 -file sub.class1.server.ca.pem \
	  -keystore checkthecrowd.jks -trustcacerts
	keytool -import -alias webserver -file ssl.crt \
	  -keystore checkthecrowd.jks