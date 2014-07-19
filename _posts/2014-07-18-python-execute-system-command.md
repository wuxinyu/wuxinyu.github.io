---
title: python 执行系统命令
layout: post
category : automatic
tagline: python o执行系统命令经常使用的语句
tags : [linux,python,shell]
---

摘自：http://www.2cto.com/kf/201009/74896.html

###os相关 
	os.chdir(dirname)                             #变更当前目录
	os.listdir(dirname)                           #遍历当前目录
	os.system                                     #命令执行 无返回
	os.popen                                      #命令执行 返回file 可用readline遍历或read
	os.getcwd()                                   #返回当前的工作目录路径
    os.chroot(dirname)                            #把dirname作为进程的根目录。和*nix下的chroot命令类似    
	os.chown(path,uid,gid)                        #改变文件的属主。uid和gid为-1的时候不改变原来的属主。
	os.link(src,dst)                              #创建硬连接
	os.mkdir(path,[mode])                         #创建目录。mode的意义参见os.chmod()，默认是0777
	os.makedirs(path,[mode])                      #和os.mkdir()类似，不过会先创建不存在的父目录。
    os.readlink(path)                             #返回path这个符号链接所指向的路径
    os.remove(path)                               #删除文件，不能用于删除目录
    os.rmdir(path)                                #删除文件夹，不能用于删除文件
    os.symlink(src,dst)                           #创建符号链接
    os.environ                                    #环境变量
	os.chmod(path,mode)                           #更改path的权限位。mode可以是以下值(使用or)的组合：
	os.path.isfile                                #文件判断
	os.path.isdir                                 #目录判断
	os.path.split(path) -> (dirname,basename)     #这个函数会把一个路径分离为两部分，比如：os.path.split(”/foo /bar.dat”)会
	                                               返回(”/foo”,”bar.dat”)
	os.path.join(dirname,basename)                #这个函数会把目录名和文件名组合成一个完整的路径名，比如：
	                                               os.path.join(”/foo”,”bar.dat”)会返回”/foo/bar.dat”。这个函数
	                                               和os.path.split()刚好相反。
	os.path.abspath(path)                         #把path转成绝对路径
	os.path.expanduser(path)                      #把path中包含的”~”和”~user”转换成用户目录
	os.path.expandvars(path)                      #根据环境变量的值替换path中包含的”$name”和”${name}”，比如环境变量FISH=nothing，
	                                               那os.path.expandvars(”$FISH/abc”)会返回”nothing/abc”
	os.path.normpath(path)                        #去掉path中包含的”.”和”..”
	os.path.splitext(path)                        #把path分离成基本名和扩展名。比如：os.path.splitext(”/foo /bar.tar.bz2″)返回
	                                                (’/foo/bar.tar’, ‘.bz2′)。要注意它和os.path.split()的区别
	os.path.exists(path)                          #判断文件或者目录是否存在
	os.path.isfile(                               #判断path所指向的是否是一个普通文件，而不是目录
	os.path.isdir(path)	                          #判断path所指向的是否是一个目录，而不是普通文件
	os.path.islink(path)                          #判断path所指向的是否是一个符号链接
	os.path.ismount(path)                         #判断path所指向的是否是一个挂接点(mount point)
	os.path.getatime(path)                        #返回path所指向的文件或者目录的最后存取时间。
	os.path.getmtime(path)                        #返回path所指向的文件或者目录的最后修改时间
	os.path.getctime(path)                        #返回path所指向的文件的创建时间
	os.path.getsize(path                          #返回path所指向的文件的大小

### os.mode
R代表读,W代表写，X代表执行权限。USR 代表用户，GRP代表组，OTH代表其它。

	os.S_ISUID
	os.S_ISGID
	os.S_ENFMT
	os.S_ISVTX
	os.S_IREAD
	os.S_IWRITE
	os.S_IEXEC
	os.S_IRWXU
	os.S_IRUSR
	os.S_IWUSR
	os.S_IXUSR
	os.S_IRWXG
	os.S_IRGRP
	os.S_IWGRP
	os.S_IXGRP
	os.S_IRWXO
	os.S_IROTH
	os.S_IWOTH
	os.S_IXOTH



###shutil相关

	shutil.copytree
	shutil.move(src,dst)
	shutil.rmtree(path[,ignore_errors[,onerror]])
	shutil.copy(src,dest)
	shutil.copy2(src,dest)

###commands相关
	commands.getstatusoutput  #返回(status,output)

应用python编写shell脚本经常要用到os,shutil,glob(正则表达式的文件名),tempfile(临时文件),pwd(操作/etc/passwd文件),grp(操作/etc/group文件),commands(取得一个命令的输出)。前面两个已经基本上介绍完了，后面几个很简单，看一下文档就可以了。
sys.argv是一个列表，保存了python程序的命令行参数。其中 sys.argv[0]是程序本身的名字。
不能光说不练，接下来我们就编写一个用于复制文件的简单脚本。前两天叫我写脚本的同事有个几万个文件的目录，他想复制这些文件到其它的目录，又不能直接复制目录本身。他试了一下”cp src/* dest/”结果报了一个命令行太长的错误，让我帮他写一个脚本。操起python来：

	import sys,os.path,shutil
	for f in os.listdir(sys.argv[1]):
		shutil.copy(os.path.join(sys.argv[1],f),sys.argv[2])

