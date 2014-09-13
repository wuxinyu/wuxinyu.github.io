---
title: python ssh
layout: post
category : automatic
tagline: centos
tags : [linux]
---

    #-*- coding: utf-8 -*-  
    #!/usr/bin/python   
    import paramiko  
    import threading  
    def ssh2(ip,username,passwd,cmd):  
        try:  
            ssh = paramiko.SSHClient()  
            ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())  
            ssh.connect(ip,22,username,passwd,timeout=5)  
            for m in cmd:  
                stdin, stdout, stderr = ssh.exec_command(m)  
    #           stdin.write("Y")   #简单交互，输入 ‘Y’   
                out = stdout.readlines()  
                #屏幕输出  
                for o in out:  
                    print o,  
            print '%s\tOK\n'%(ip)  
            ssh.close()  
        except :  
            print '%s\tError\n'%(ip)  
    if __name__=='__main__':  
        cmd = ['cal','echo hello!']#你要执行的命令列表  
        username = ""  #用户名  
        passwd = ""    #密码  
        threads = []   #多线程  
        print "Begin......" 
        for i in range(1,254):  
            ip = '192.168.1.'+str(i)  
            a=threading.Thread(target=ssh2,args=(ip,username,passwd,cmd))   
            a.start() 


Thrift

    apt-get install libboost-dev libevent-dev python-dev automake pkg-config libtool flex bison sun-java6-jdk
    wget http://www.apache.org/dist//thrift/0.8.0/thrift-0.8.0.tar.gz
    tar zxvf thrift-0.8.0.tar.gz
    cd thrift-0.8.0
    ./configure
    make
    sudo make install
    sudo pip install thrift