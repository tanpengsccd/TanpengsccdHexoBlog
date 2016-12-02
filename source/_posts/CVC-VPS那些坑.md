---
title: CVC-VPS那些坑
tags: 
  - VPS
  - CVC
comments: true  
date: 2016-10-17 10:50:15
update:
category:
- VPS总结笔记
archives:
description:
---

# 购买后官方不发送panel面板的随机登陆密码
1. https://panel.cloudatcost.com/ 自行Reset会发送密码  

# 管理面板不能登录或者忘记密码
1. https://panel.cloudatcost.com/ 自行Reset会发送密码 

# can‘t get ip .............
1. 故名思议没有IP给你了，等半天再创建吧。  
	
# 创建VPS后 ping不通且SSH无法连接
1. 首先在控制面板 Modify下点击Netwoking ，这样测试2次，如果2次全为fail看2.1，如果第二次为succes则尝试再次ping 和ssh连接,若还是无法连接看2.2  
2. 
	1. 可能是CVC抽风 删除后再新建，如果3次后依然如此，就只能等半天到一天再试了 
	2. 点击面板的Console 如果能链接，就说明是你自己的设置防火墙之类的软件有问题。

