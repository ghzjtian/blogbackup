---
title: iOS_APP基本组件
tags: [iOS]
categories: [iOS]
date: 2018-08-14 17:46:19
---



# iOS_APP基本组件
> 整理以前用到的  iOS APP 常见的基本组件.

<!-- more -->

## 0.参考
* [1.Top 10 Libraries for iOS Developers](https://www.raywenderlich.com/259-top-10-libraries-for-ios-developers)

## [1.第三方组件](#third_component)

***
***
***

## 1.第三方组件<a name="third_component"/>

* 1.状态提示框
	* [1.MBProgressHUD 弹框组件](https://github.com/jdg/MBProgressHUD)
	* [2.SVProgressHUD](https://github.com/SVProgressHUD/SVProgressHUD)
* [2.ATNavBarButton,Bar 上的 Button](https://github.com/emotality/ATNavBarButton)
* [3.等待转圈图 JQIndicatorView](https://gitee.com/yybz/JQIndicatorView)

* [4.SDWebImage](https://github.com/rs/SDWebImage)
	* 1.图片加载框架.

* [5.Flipboard/FLAnimatedImage](https://github.com/Flipboard/FLAnimatedImage)
	* 1.GIF 动画加载框架.

* [6.MJRefresh](https://github.com/CoderMJLee/MJRefresh)

* [7.Bugly]

* [8.自定义 通知栏 信息: CWStatusBarNotification](https://github.com/cezarywojcik/CWStatusBarNotification)

* [9.软键盘弹出，智能调整与 TextField 的距离.IQKeyboardManager](https://github.com/hackiftekhar/IQKeyboardManager)

* [10.检测网络状态,Reachability](https://github.com/tonymillion/Reachability)

* 11.FMDB 数据库处理
	* [1.GitHub 地址](https://github.com/ccgus/fmdb)
	* [2.iOS FMDB详解](http://www.imlifengfeng.com/blog/?p=512)
	* [3.简单的 DEMO](https://github.com/ghzjtian/FMDB_TEST)

* 12.网络请求
	* 1.AFNetworking (Object-C)
		* [1.GitHub 地址+DEMO(但是 DEMO 的 URL 有问题.)](https://github.com/AFNetworking/AFNetworking) 
		* [2.Weather APP(一个使用 AFNetWorking 的例子.From raywenderlich)](https://www.raywenderlich.com/2516-afnetworking-2-0-tutorial)
	* 2.Alamofire/Alamofire(Switch)
		* [1.GitHub 地址](https://github.com/Alamofire/Alamofire)

* [13.CoderMJLee/MJExtension](https://github.com/CoderMJLee/MJExtension)
	* 1.字典模型相互转换.

* 14.CocoaLumberjack/CocoaLumberjack
	* [0.GitHub 源代码(包括文档)](https://github.com/CocoaLumberjack/CocoaLumberjack)
		* 1.可以把 log 保存到 文件/数据库.
		* [2.在 Xcode 用不同的颜色显示.](https://github.com/CocoaLumberjack/CocoaLumberjack/blob/master/Documentation/XcodeColors.md)
		* 3.Debug 和 Release 的 Log 有不同的输出.
	* 1.一个 Logging 框架,相关教程参考:
		* [1.CocoaLumberjack：简单好用的Log库](https://www.jianshu.com/p/7b799bef0107)
		* [2.深入解析iOS日志库CocoaLumberjack](https://www.jianshu.com/p/d8bad0e2683c)
	* 2.打印保存的 log 的路径的命令为 : `DDLogVerbose(@"%@",[[[NSFileManager defaultManager]URLsForDirectory:NSLibraryDirectory inDomains:NSUserDomainMask] lastObject]);`
		* 4.保存的 file 的路径为: `/Users/user/Library/Developer/CoreSimulator/Devices/A656FEC2-922B-4D92-BB56-F42EDDD0BD18/data/Containers/Data/Application/E1BE1827-1DEA-48E8-B5E6-7F76E0837FE8/Library/Caches/Logs/com.deusty.iOSLibStaticTest 2018-09-13--14-53-34-390.log`

* 15.CocoaLumberjack/CocoaLumberjack
	* 0.让 Xcode 的 Log 输出不同的颜色.
	* [1.GitHub 源码	](https://github.com/robbiehanson/XcodeColors)
	* [2.Can't work Xcode9?](https://github.com/robbiehanson/XcodeColors/issues/91)
	* [3.Making Xcode-Colors work with Xcode 8.1 — Xcode 9](https://medium.com/gk-blog/making-xcode-colors-work-with-xcode-8-1-f67da74ad83f)

* 16.代码实现 AutoLayout 复杂的布局效果.
	* [1.SnapKit/Masonry](https://github.com/SnapKit/Masonry)

* 17.[MGJRouter 路由](https://github.com/meili/MGJRouter)
* 18.[LGAlertView 状态提示 View](https://github.com/Friend-LGA/LGAlertView)
* 19.[ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)
	* 1.[最快让你上手ReactiveCocoa之基础篇](https://www.jianshu.com/p/87ef6720a096)
	* 2.[iOS ReactiveCocoa框架的简单使用](https://www.jianshu.com/p/148075efc2c9)



