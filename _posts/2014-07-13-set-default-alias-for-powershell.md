---
title: 在powershell设置默认工具应用程序别名
layout: post
category : automatic
tagline: 在powershell设置默认工具应用程序别名
tags : [powershell]
---
###相关命令
修改执行策略 

	set-executionpolicy remotesigned
 
检查配置文件

	test-path $profile

创建适用于所有用户和所有外壳程序的配置文件：

	new-item -path C:/Windows/System32/WindowsPowerShell/v1.0/profile.ps1 -itemtype file -forc
 
编辑配置文件并添加相关工具执行别名

    new-item alias:st -value "C:\Tools\Sublime Text 2\sublime_text.exe" | Out-Null

重启powershell即可.

###profile.ps1简单例子
	$gitroot = "E:\git-repo"
	$blog = Join-Path $gitroot "wuxinyu.github.io"
	function XYCdBlog
	{
		Set-Location $blog
	}

	function XYRunBlog
	{
		Set-Location $blog
		jekyll serve --watch
	}

	set-alias cd-blog XYCdBlog
	set-alias run-blog XYRunBlog
	new-item alias:st -value "C:\Tools\Sublime Text 2\sublime_text.exe" | Out-Null