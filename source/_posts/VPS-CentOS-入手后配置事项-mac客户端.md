---
title: VPS(CentOS)入手后配置事项(mac客户端)
date: 2016-10-09 17:44:15
category: VPS总结笔记
archives: VPS
tags:
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
8. 配置ssh-RSA 登陆
8. 修改ssh的配置文件
9. 重启ssh服务

# 配置SS(R)服务器端

# 加速
 ## 单边加速
  ### 锐速
 ## 双边加速
  ### KCP
# 测速