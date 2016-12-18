---
title: UpImageMacApp(U图床)-markdown写作工具推荐
categories:
  - Mac
tags:
  - Mac
comments: true
date: 2016-8-18 20:31:51
update: 2016-12-18 20:31:51
archives:
description:
---
# 前言
   感觉markdown 很好用，做笔记和贴代码都很优雅，而且很多软件业支持（主要是 有道云笔记），但是也有个不方便的地方：多媒体附件 不方便存储，主要是图片不方便贴。所以在Google找了一天，找了个通用解决方法，就是找个云图床，提供此服务的主要有七牛、又拍、Imgur 和 Flickr（被墙）。我一一注册了并寻找相关Mac软件。软件比较少，只发现了iPic和UpImageMacApp。iPic 因为收费😢，功能限制了。所以找到了chenxtdo的UpImageMacApp开源项目，这个开源项目是仿iPic做的，而且是swift写的，恰好我也想学swift，于是 fork 后 新增一些部分功能，现在可以如果只用七牛云图床的话，功能应该已经够用了。  
   关于主流云图床介绍：https://www.tipga.com/essay/55dac137777777100300002a
# 功能
可以将 系统或者三方截图工具的截图 上传至云服务器（暂时只支持七牛）。  
1. 支持自动上传，快捷键上传（CMD + U），拖拽上传  
2. 支持系统截图上传，以及三方截图工具上传   
3. 可以查看历史上传图片  
4. 有上传进度  
![功能.jpg](http://oet7vjedr.bkt.clouddn.com/20161218148206861980134.jpg?imageView2/0/format/jpg)

# 配置 

一张图就能看懂了

![201612187198620161218148203151941359.png](http://oet7vjedr.bkt.clouddn.com/201612187198620161218148203151941359.png)
# 下载地址
##我fork的版本：  
https://github.com/tanpengsccd/UPImageMacApp/releases

##chenxtdo主分支原版本
https://github.com/chenxtdo/UPImageMacApp 

