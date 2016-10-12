---
title: VPS(CentOS)入手后配置事项(mac客户端)
date: 2016-10-09 17:44:15
category:
- VPS总结笔记
archives: VPS
tags:
- VPS
---

>常用命令
	1. rm [-r] 文件/文件夹 
		移除文件夹
    2. sudo
    	暂时使用最高权限
    3. sudo su 
    	使用最高权限
    4. sudo su -l 用户名
    	切换用户
    5. CTRL + C 按键
    	中断当前操作
    6. ssh [-l用户名] [-p端口] 用户名（常用root）@ip（如192.168.1.1）

1. 安装 git  
	yum install git
2. 安装 zsh
	yum install zsh
3. 安装 oh my zsh
	```
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
    ```
4. 修改root密码
	passwd
5. 新建用户
		useradd -l 用户名 
6. 修改新用户密码
	passwd -l 用户名
7. 授予新用户权限（可选）
	```
	echo -e "\n用户名 ALL=(ALL) ALL\n" >> /etc/sudoers
	tail -3 /etc/sudoers
    ``` 
8. 配置ssh-keygen 登陆（不用输入密码登录VPS）
	客户端运行
	`ssh-keygen` 一路回车获得一对公私钥
 	`cat  /Users/‘whoami’/.ssh/id_rsa.pub` 这就是你的公钥
    `cat  /Users/‘whoami’/.ssh/id_rsa` 这就是你的私钥
	`scp  [-P 非默认端口] /Users/‘whoami’/.ssh/id_rsa.pub root@IP地址:~/.ssh/authorized_keys ` 将客户端的公钥复制到VPS上
 	>简单说一下原理，可以不看,VPS为了验证你是合法的登陆者（客户端）,在你ssh登陆VPS时，VPS会发来一段字符串，你用生成的私钥加密这串字符串生成密文，又发回给VPS，VPS用你上传的公钥解密密文，如果解密得到字符串和发出字符串一致则验证通过。这样就可以保证发送的信息即使暴露，只要公私钥安全就可以保证验证的安全性
    
8. 修改ssh的配置文件
	>每行前的 “#”表示这行配置被注释了，不生效
	#PermitRootLogin yes 改为 PermitRootLogin no ，不允许root账户登录 建议设置为no更安全
	#PasswordAuthentication yes 改为 PasswordAuthentication no。允许密码登录，如果设置为NO，就只能通过ssh公私钥登录了
    #Port 22 改为 Port 端口号数字(比如： Port 7564)。默认为22建议自己修改其他端口
    
9. 重启ssh服务
	`sudo service sshd restart` 
# 配置SS(R)服务器端

# 加速
 ## 单边加速
  ### 锐速
 ## 双边加速
  ### KCP
# 测速