---
title: iOS多包名打包
tags: [iOS]
categories: [iOS]
date: 2018-11-18 17:46:19
---


## [1.背景](#background_ifo)
<!-- more -->
## [2.在 workspace 下建立多个 Projects](#multi_project)
## [3.根据环境的不同，打包多个版本的 app](#multi_configuration)
## [4.根据环境不同，新建多个 Target](#multi_target)

## 5.相关的源码地址为:
* 1.[https://github.com/ghzjtian/generated_multi_iOS](git@github.com:ghzjtian/generated_multi_iOS.git)

## [6.多人共用一个证书](#multi_user_public)


***
***
***

## 1.背景<a name="background_ifo"/>
* 1.相同的 `Project` 源码 ,想要打包多个 APP , APP 间有 icon,主题，相关的 label 的不同，其他的逻辑代码都一样.

***

## 2.在 workspace 下建立多个 Projects<a name="multi_project"/>
#### 1.参考:
* [1.iOS用workspace和cocoapods管理多个项目](https://www.jianshu.com/p/e3cfae830985)

***

## 3.根据环境的不同，打包多个版本的 app<a name="multi_configuration"/>
#### 1.参考:
* 1.[Xcode Build Settings — User Defined Settings — iOS Manage multiple configuration and environments with Single target](https://medium.com/@kavithakumarasamy89/xcode-build-settings-user-defined-settings-manage-multiple-environments-with-single-target-3e5c1a307999)

#### 2.详细的过程:
* 1.先添加一个 `配置环境` 
	* 1.如这里就在  `Project/Info` 的 `Configurations` 下,添加了一个 `Testing` 测试环境的配置.

<img src="/assets/imgs/iOS/multi_package/Snip20181119_11.png" width="70%" height="70%">

* 2.在 `Target/BuildSettings` 下添加用户自定义的信息.

	* 1.在 `APP/TARGETS/Build Settings` 中，添加一个 `User-Defined Settings`,如这里就添加了 `mAPP_BUILD_NUMBER`,`mAPP_BUNDLE_IDNETIFIER`,`mAPP_SERVER_URL` 等等的信息资料.

<img src="/assets/imgs/iOS/multi_package/Snip20181119_10.png" width="70%" height="70%">

* 3.在 `Info.plist` 中，把相关的在 `BuildSettings` 下的设置做关联:

<img src="/assets/imgs/iOS/multi_package/Snip20181119_13.png" width="70%" height="70%">

* 4.同理在 `TARGETS/General/Identity ` 下也一样要与 `BuildSettings ` 关联:
	
<img src="/assets/imgs/iOS/multi_package/Snip20181119_5.png" width="70%" height="70%">

	* 注意 `Bundle Identifier` 在这里关联没用，要在 `Build Settings/Product Bundle Identifier` 做关联才行 !! 如下图:
<img src="/assets/imgs/iOS/multi_package/Snip20181119_2.png" width="70%" height="70%">
<img src="/assets/imgs/iOS/multi_package/Snip20181119_14.png" width="70%" height="70%">

	* 可以看到有所改变，如果没有的话，点去其他页面再点回来，这样刷新一下即可
<img src="/assets/imgs/iOS/multi_package/Snip20181119_8.png" width="70%" height="70%">

* 5.在 `ViewController` 中，获取设置的值:

```
   NSBundle *bundle = [NSBundle mainBundle];
    //bundle identifier
    NSString *bundleIdentifier = [bundle bundleIdentifier];
    //display name
    NSString *displayName = [bundle objectForInfoDictionaryKey:@"CFBundleDisplayName"];
    //Version String
    NSString *versionStr = [bundle objectForInfoDictionaryKey:@"CFBundleShortVersionString"];
    //Version Build number
    NSString *buildNumber = [bundle objectForInfoDictionaryKey:@"CFBundleVersion"];
    //APP SERVER URL
    NSString *serverUrl = [bundle objectForInfoDictionaryKey:@"ServerURL"];
  
    _titleLb.text=[NSString stringWithFormat:@"%@",displayName];
    _showLb.text = [NSString stringWithFormat:@"bundle ID:%@,\nversionStr:%@,\nbuildNumber:%@,\nserverUrl:%@",bundleIdentifier,versionStr,buildNumber,serverUrl];
    
    NSLog(@"bundle ID:%@,\ndisplayName:%@,\nversionStr:%@,\nbuildNumber:%@,\nserverUrl:%@",bundleIdentifier,displayName,versionStr,buildNumber,serverUrl);
    

```

* 6.在运行前，先到 `Edit Scheme` 中，选择不同的 `Build Configuration`,然后运行查看效果图:

<img src="/assets/imgs/iOS/multi_package/Snip20181119_6.png" width="70%" height="70%">
<img src="/assets/imgs/iOS/multi_package/Snip20181119_7.png" width="70%" height="70%">
	* 在手机上的效果图
<img src="/assets/imgs/iOS/multi_package/Snip20181119_19.png" width="70%" height="70%">
		*  ~
<img src="/assets/imgs/iOS/multi_package/Snip20181119_16.png" width="70%" height="70%">
		*  ~
<img src="/assets/imgs/iOS/multi_package/Snip20181119_17.png" width="70%" height="70%">
		*  ~
<img src="/assets/imgs/iOS/multi_package/Snip20181119_18.png" width="70%" height="70%">



***
	
## 4.根据环境不同，新建多个 Target<a name="multi_target"/>
>上一步基本结决了不同的环境下，释放出不同的 APP (如 Debug,Test,Release) 版本，但是如果是想要改变不同的 icon,device orientation 等等的参数，可能就后劲不足了,下面主要讲解通过新建 `Target` 解决这种问题.

#### 1.参考:
* [1.Xcode “Targets” with multiple build configurations](https://medium.com/@andersongusmao/xcode-targets-with-multiples-build-configuration-90a575ddc687)
	* 1.[相关的 Source code](https://github.com/heuristisk/hkScopum)

#### 2.步骤:
* 1.右键点击,新建一个 `Target` 为 `Green`

<img src="/assets/imgs/iOS/multi_package/Snip20181119_22.png" width="70%" height="70%">

* 2.设置 `Green` 的 `General` 和  `Build Settings` 的信息:

<img src="/assets/imgs/iOS/multi_package/Snip20181119_13.png" width="70%" height="70%">
<img src="/assets/imgs/iOS/multi_package/Snip20181119_30.png" width="70%" height="70%">
<img src="/assets/imgs/iOS/multi_package/Snip20181119_31.png" width="70%" height="70%">

* 3.效果图:

<img src="/assets/imgs/iOS/multi_package/Snip20181119_29.png" width="70%" height="70%">

***

<img src="/assets/imgs/iOS/multi_package/Snip20181119_26.png" width="70%" height="70%">

***

<img src="/assets/imgs/iOS/multi_package/Snip20181119_27.png" width="70%" height="70%">

***

<img src="/assets/imgs/iOS/multi_package/Snip20181119_28.png" width="70%" height="70%">

* 4.如果需要在代码中判断是哪个 APP ,可以设置一个 `Macro` ,然后在需要的地方判断即可,如:

```
//定义一个判断的 宏
#define IS_GREEN_APP [[[NSBundle mainBundle] bundleIdentifier] containsString:@"com.glb.Green"]

//在有需要的地方判断
if(IS_GREEN_APP){
    NSLog(@"The APP is Green Guide APP");
}else{
    NSLog(@"The APP is Cramer APP");
}

```

## 6.多人共用一个证书<a name="multi_user_public"/>

* 1.参考 [如何导出 p12 文件](https://www.jianshu.com/p/837d64356dac)
* 2.通过安装了导出来的 `.p12` 文件，即可在本电脑打包并发布 APP. 

