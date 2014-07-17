---
title: SqlServer 2008 行转列
layout: post
category : sqlserver
tagline: t-sql 中行转列
tags : [sqlserver]
---
###相关SQL
	select stuff((select ',' + joinColumn from subTable sub where sub.pk = master.id for xml path('')),1,1,'') joinedColumn
	from masterTable master