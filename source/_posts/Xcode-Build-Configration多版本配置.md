---
title: Xcode-Build-Configration多版本配置
categories:
  - iOS
tags:
  - iOS
  - Xcode
comments: true
date: 2016-10-16 17:37:17
update: 2016-12-16 17:50:17
archives: 
description:
---



写在前面： 
多套 build configration 可以方便我们编译成不同的版本，其中可以 不同bundle id 不同 Displa namey，并以此 分化功能上的差异。
常见用处：
1. 调试版 可能会需要加入 Reveal SDK, 方便调试视图 调试版可能需要需要使用测试的API接口,而正式版是不能含有此SDK，且使用正式版的API。
2. Lite，pro 版本 功能上的分化。大部分核心功能相同但仍有部分不同


此处以我的项目为例。需要新建一个内部测试版（Alpha）和一个企业版（Enterprise），相对于正式版（release），内部测试版需要可以使用测试API地址和新增使用自动更新功能，企业版需要新增使用自动更新功能。
|版本|测试版地址|更新提示|
|---|:---:|:---:|
|Release|X|X|
|Debug|O|X|
|Enterprise|X|O|


# project 配置

1. 新增build configration
 ![2016121614818742423966.jpg](http://oet7vjedr.bkt.clouddn.com/2016121614818742423966.jpg?imageView2/0/format/jpg)
这样便新增一套配置文件 例如我自己命名为“Alpha” 和 “Enterprise”。

2. 新建自定义设置项
![20161216148187614526138.jpg](http://oet7vjedr.bkt.clouddn.com/20161216148187614526138.jpg?imageView2/0/format/jpg)
比如第一个设置项我对应不同版本设置了 不同的 bundle id （这样就可以几个版本的app 同时共存），并把设置项命名为“BUNDLE_IDENTIFIER” 。

3. 设置 bundle ID   
如下填入 $(BUNDLE_IDENTIFIER) ，即 $(设置项名称)。即可完成 Build Configration 中 bundle id 的差异化
![20161216148187675240155.jpg](http://oet7vjedr.bkt.clouddn.com/20161216148187675240155.jpg?imageView2/0/format/jpg)
其它配置项的差异化 配置方法雷同：如Display name，info plist  path，app icon。

# cocoapods 配置
其实配置很简单，只是一部分童鞋不知道语法  
`pod 三方库名称 , :configurations => [' build名称']`

![20161216148187879197159.jpg](http://oet7vjedr.bkt.clouddn.com/20161216148187879197159.jpg?imageView2/0/format/jpg)


# 代码控制

1. 配置cocoapods前需要先配置 project的预处理宏
![20161216148187920028262.jpg](http://oet7vjedr.bkt.clouddn.com/20161216148187920028262.jpg?imageView2/0/format/jpg)
2. 代码  
宏编译知识看[这里](http://www.cnblogs.com/rusty/archive/2011/03/27/1996806.html)  
    1. 友盟统计 区别设置渠道
    ```
    #if defined(AlPHA)
        UMConfigInstance.channelId = @"ALPHA";
    #elif defined(ENTERPRISE)
     UMConfigInstance.channelId = @"ENTERPRISE";
    #endif
    ```

    2. 内部测试版 和 企业版 使用 蒲公英SDK 的更新服务
    ```
    #if defined(ALPHA) ||  defined(ENTERPRISE)

        [[PgyManager sharedPgyManager]; startManagerWithAppId:PGY_APP_ID];
        [[PgyManager sharedPgyManager] setEnableFeedback:NO];
        [[PgyManager sharedPgyManager] setShakingThreshold:3];
        [[PgyManager sharedPgyManager] setThemeColor:COLOUR_RED];
        [[PgyUpdateManager sharedPgyManager]; startManagerWithAppId:PGY_APP_ID];
        [[PgyUpdateManager sharedPgyManager] checkUpdate];
   
    #endif
    ```

# 一些效果
![20161216148188009825013.jpg](http://oet7vjedr.bkt.clouddn.com/20161216148188009825013.jpg?imageView2/0/format/jpg)