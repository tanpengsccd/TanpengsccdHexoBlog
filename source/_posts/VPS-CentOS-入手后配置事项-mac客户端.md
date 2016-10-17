---
title: VPS(CentOS)入手后配置事项(mac客户端)
date: 2016-10-09 17:44:15
category:
- VPS总结笔记
archives: VPS
tags:
- VPS
---
参考:
https://ttt.tt/104/  
https://mozillazg.com/2013/01/linux-vps-first-things-need-to-do.html

>常用命令

1. rm [-r] 文件/文件夹
	移除文件[夹]
2. sudo
	暂时使用最高权限
3. sudo su
	使用最高权限
4. [sudo] su [-l] 用户名
	切换用户
5. CTRL + C 按键
	中断当前操作
6. ssh [-l用户名] [-p端口] 用户名（常用root）@ip（如192.168.1.1）

7. top
	
8. ps  -ef

# 简单配置环境（方便后续操作，可以不做）
1. 安装 git  

	```
	yum install git
	
	```
2. 安装 zsh

	```
	yum install zsh
	
	```
3. 安装 oh my zsh  
	如果登陆其它账户没有高亮语法可再次运行以下命令  
	
	```
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
  ```

# ssh安全设置

1. 修改root密码  

	```
	passwd
	```
2. 新建用户  

	```
	useradd 用户名
	```
3. 修改新用户密码  

	```
	passwd  用户名
	```
4. 授予新用户权限
	
	```
	echo -e "\n用户名 ALL=(ALL) ALL\n" >> /etc/sudoers
	tail -3 /etc/sudoers
	```
	也可以 `visudo` 自己末行自己添加 `用户名 ALL=(ALL) ALL` 到末行
5. 配置ssh-keygen 登陆（不用输入密码登录VPS）
	客户端本地运行（非VPS）
	`ssh-keygen` 一路回车获得一对公私钥
 	`cat  /Users/‘whoami’/.ssh/id_rsa.pub` 这就是你的公钥  
  	`cat  /Users/‘whoami’/.ssh/id_rsa` 这就是你的私钥    
	`ssh-copy-id -i ~/.ssh/id_rsa.pub username@198.51.100.100`或者``scp  [-P 非默认端口] /Users/	`whoami`/.ssh/id_rsa.pub username@IP地址:~/.ssh/authorized_keys `` 
	将客户端的公钥复制到VPS上
	  
	>简单说一下原理，可以不看,VPS为了验证你是合法的登陆者（客户端）,在你ssh登陆VPS时，VPS会发来一段字符串，你用生成的私	钥加密这串字符串生成密文，又发回给VPS，VPS用你上传的公钥解密密文，如果解密得到字符串和发出字符串一致则验证通过。这样就	可以保证发送的信息即使暴露，只要公私钥安全就可以保证验证的安全性

6. 修改ssh的配置文件  
	`vi /etc/ssh/sshd_config`
	
	>每行前的 “#”表示这行配置被注释了，不生效,需要的配置去掉“#”才可生效

	PermitRootLogin yes 改为 PermitRootLogin no ，不允许root账户登录 建议设置为no更安全  
	PasswordAuthentication yes 改为 PasswordAuthentication no。允许密码登录，如果设置为NO，就只能通过ssh公私钥登录了
	Port 22 改为 Port 端口号数字(比如： Port 7564)。默认为22建议自己修改其他端口
1. 重启ssh服务
	`sudo service sshd restart`

# 端口安全设置

## iptables配置
>关闭多余端口防止一些DDoS攻击  
>iptables一般系统已经集成

1. 清楚默认规则  

	```
	iptables -F
	```
2. 允许ssh 默认22端口 进入和返回  

	```
	iptables -A INPUT -p tcp --dport 30501 -j ACCEPT  
	iptables -A OUTPUT -p tcp --sport 30501 -m state --state ESTABLISHED -j ACCEPT
	```
3. 允许 53 端口，一般作为 DNS 服务使用

	```
	iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
	iptables -A INPUT -p udp --sport 53 -j ACCEPT
	```
4. 允许本机访问本机

	```
	iptables -A INPUT -s 127.0.0.1 -d 127.0.0.1 -j ACCEPT
	iptables -A OUTPUT -s 127.0.0.1 -d 127.0.0.1 -j ACCEPT
	```
5. 允许所有 IP 访问 80 和 443 端口，一般作为 http 和 https 用途  

	```
	iptables -A INPUT -p tcp -s 0/0 --dport 80 -j ACCEPT
	iptables -A OUTPUT -p tcp --sport 80 -m state --state ESTABLISHED -j ACCEPT
	iptables -A INPUT -p tcp -s 0/0 --dport 443 -j ACCEPT
	iptables -A OUTPUT -p tcp --sport 443 -m state --state ESTABLISHED -j ACCEPT
	```
6. 保存配置  

	```
	service iptables save 
	or 
	iptables-save > /etc/sysconfig/iptables
	```
7. 开机自启动 iptables 

	```
	chkconfig iptables on
	```
8.  服务重启
	
	```
	service iptables restart
	```
9. 查看配置  
	[-n]代表新增
	
	```
	iptables -L [-n]
	```
	
	> 具体使用iptables 详解 看一看这里http://blog.csdn.net/reyleon/article/details/12976341

## 防扫描（fail2ban）

1. 安装EPLE源
 
	```
	yum install epel-release.noarch
	```
2. 安装 fail2ban

	```
	yum install fail2ban.noarch
	```
	安装时会自动加入自启

3. 	拷贝默认配置文件

	```
	cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
	```
	通过查看 /var/log/fail2ban.log 文件即可知道有哪些精力过剩的家伙在整天扫描你的 SSH 了。
	>fail2ban github主页 https://github.com/fail2ban/fail2ban  
	>了解更多  http://www.voidcn.com/blog/cmdschool/article/p-5575852.html

# 配置SS(R)服务器端
>具体看这里 [shadowsocks-rss](https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup)
	
	
# 加速
 ## 单边加速
  ### 锐速
 ## 双边加速
  ### KCP
# 测速
# 性能测试
>https://www.91yun.org/zh/archives/157
