---
title: synolygy dsm 安装aria2(c)简单教程
categories:
  - nas
tags:
  - nas
  - dsm
  - aria2
  - 下载
comments: true
date: 2016-12-02 15:43:32
update:
archives:
description:
---

# 条件  
 1. DSM 系统可以重启进入系统
 2. ssh 可以通
# 步骤
1. nas 打开ssh
![20161128148030246747542.jpg](http://oet7vjedr.bkt.clouddn.com/20161128148030246747542.jpg?imageView2/0/format/jpg)
让nas可以ssh 联通  
```ruby
ssh -p端口(默认20) -lroot ip地址
```  

![20161128148030238819327.jpg](http://oet7vjedr.bkt.clouddn.com/20161128148030238819327.jpg?imageView2/0/format/jpg)

2. 新建下载文件夹 
    ![20161202148064735642784.jpg](http://oet7vjedr.bkt.clouddn.com/20161202148064735642784.jpg?imageView2/0/format/jpg)  
    设置权限
    ![20161202148064749830927.jpg](http://oet7vjedr.bkt.clouddn.com/20161202148064749830927.jpg?imageView2/0/format/jpg)
    ok。   
    tips：命令行创建的文件夹 dsm系统是看不到的。所以老老实实这样创建吧  
2. 安装Entware 以支持安装opkg 软件  
    具体见 https://github.com/Entware-ng/Entware-ng  
    ```uname -a```  可查看自己linux 系统版本,根据系统版本安装对应版本Entware
2. 安装 git-http 和 aria2  
        opkg install git-http
        opkg install aria2
3. 调试与配置
    1. 配置aira2.conf 配置文件  
        1. 具体看移步 http://aria2c.com/usage.html 
        2. 或直接用我自己的配置文件
             1. 新建个aria2.session 会话文件  
                ```touch  /volume1/downloads/aria2.session ``` 
             2. 下载 我的aria2.conf  
                
                ```wget -O  /volume1/downloads/aria2.conf  https://raw.githubusercontent.com/tanpengsccd/aria2conf/master/dsm/aria2.conf```  

                这个文件不能删掉哦！/volume1/downloads/aria2.conf 不然启动会失败
    2. 调试  
        ```/opt/bin/aria2c --enable-rpc --rpc-listen-all=true ``` 
        
        查看log  
        登陆  http:/aria2c.com/ 在这个在线管理界面自行配置，添加下载任务，看log信息自己调试
        先检查内网ip是否ok，再检查外网ip。
    3. 配置aria2启动项
        vi /etc/rc.local  
        添加  
        
        #aria2c 安装后会自启（偶不知道自启的配置文件在哪...,没法改），要先关闭再重新以配置文件启动  
         ```
        killall aria2c
        /opt/bin/aria2c --conf-path=/volume1/downloads/aria2.conf -D
        ``` 
        
    4. 测试 aria2c 正常否  
    netstat -apn|grep <端口号>   
    killall 进程  
   
3.[选作] 安装本地nas的aria2 web客户端
    首先需要打开nas的 web服务器，5.2的DSM系统可以直接打开，6.0需要自己再套件商城下载
    外网访问可能需要其他端口,因为80端口一般都是被电信封了的
    ![20161202148064674515880.jpg](http://oet7vjedr.bkt.clouddn.com/20161202148064674515880.jpg?imageView2/0/format/jpg)
    
    1. webui-aria2 界面丰富  
    git clone https://github.com/ziahamza/webui-aria2.git  
    2. YAAW 界面简单，占用小  
    git clone https://github.com/binux/yaaw.git
---
以下比较过时，仅做参考
1. 安装bootstrap 以 支持安装ipkg包  教程http://www.vspecialist.co.uk/2014/09/how-to-install-ipkg-on-a-synology-nas/  
2. DSM6.0 安装aria2（arm构架CPU）的教程 https://www.chiphell.com/thread-1558683-1-1.html  
3. DSM5.0 http://post.smzdm.com/p/49402/
