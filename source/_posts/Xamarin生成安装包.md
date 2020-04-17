---
title: Xamarin 安装包的生成
tags: [Xamarin]
categories: [Xamarin]
date: 2018-12-14 17:46:19
---

> 记录 Android 与 iOS 的安装包的生成方式与打包发布.

<!-- more -->

## [1.Android 打包压缩](#Android_compress)
## [2.iOS TestFlight 相关](#iOS_testflight)


***
***
***

## 1.Android 打包压缩<a name="Android_compress"/>

#### 1.步骤:

* 1.右击 `BLE_APP.Android` 中的 `Options`, `Android Build`-> `Linker` -> `Linker Behaviour`
	* 1.选择 `Don't Link` 选项，然后打包
		* 1.发现打包出来的 apk 有 `67.51` MB 大小.
		* 2.华为手机测试打开没问题
	* 2.选择 `Link all assemblies` 选项,然后打包
		* 1.发现打包出来的 apk 有 `14.3` MB 大小.
		* 2.华为手机测试打开就 Crash .
	* 2.选择 `Link SDK assemblies only` 选项,然后打包
		* 1.发现打包出来的 apk 有 `14.3` MB 大小.
		* 2.华为手机测试打开没问题.

![](http://pic.pgyjz.cn/blog/Xamarin/screenshot%202018-12-14%20at%2017.47.03.png)

***

## 2.iOS TestFlight 相关<a name="iOS_testflight"/>

#### 1.参考:
* 1.[TestFlight 使用指南](https://www.jianshu.com/p/a7669d4b2217)
* 2.[iOS如何使用TestFlight进行App Beta版测试](https://www.jianshu.com/p/684e4b56b99a)
* 3.[Using TestFlight to Distribute Xamarin.iOS Apps](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/testflight?tabs=macos)
* 4.[Publishing Xamarin.iOS apps to the App Store](Publishing Xamarin.iOS apps to the App Store)

#### 2.具体步骤
* 1.在 `Info.plist` 中填写好相应的图片信息.
* 2.选择 `Build/Archive for Publishing` ,准备开始打包.

![](http://pic.pgyjz.cn/blog/Xamarin/screenshot 2019-01-07 at 17.08.18.png)

* 3.注意 `Info.plist/Version` 的值不要大于两个点,如 `1.1.01071653` 会出现如下的错误:

```
ERROR ITMS-90060: "This bundle is invalid. The value for key CFBundleShortVersionString '1.1.0.01071653' in the Info.plist file must be a period-separated list of at most three non-negative integers."
```

* 4.打包完成后，通过 `Application Loader` 上传到 `App Store` 然后在对应的 app 中选择 `TestFlight`,然后添加测试人员即可.

* 5.内部测试相关:
	* 1.内部开发人员后台添加相关
![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190108_2.png)
	* 6.内部测试人员收到的邮件信息及点击按钮后网页的显示.

![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190108_3.png)

![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190108_1.png)

