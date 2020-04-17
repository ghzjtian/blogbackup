---
title: Android APP基本组件
tags: [Android]
categories: [Android]
date: 2018-07-04 17:46:19
---



# Android APP基本组件
> 记录 APP 相关的组件及链接.
<!-- more -->

### 1.通用开发
### [2.Android 所用的外部库](#third_lib)
### [3.Android 自带的 View](#self_android_view)

***
***
***

### 1.通用开发



***

### 2.Android 所用的外部库<a name="third_lib"/>

##### 1.网络相关

* 1.异步http请求
>android-async-http
	* [1.GitHub 上的源码及使用文档](https://github.com/loopj/android-async-http)
 	* [2.http://www.jianshu.com/p/2fe3e305404e](http://www.jianshu.com/p/2fe3e305404e)
 	* 3.混淆代码:
 	
 		```
 		#android-async-http
		-keep class cz.msebera.android.httpclient.** { *; }
		-keep class com.loopj.android.http.** { *; }
		```

* 2.http 请求封装
>httpmime-4.1.3.jar
>
>[https://github.com/apache/httpcomponents-client](https://github.com/apache/httpcomponents-client)
>
>[http://www.jianshu.com/p/ab6a2b90ae5e](http://www.jianshu.com/p/ab6a2b90ae5e)


* 3.网络图片加载库 Fresco
>com.facebook.fresco:fresco:0.10.+
	* [1.https://github.com/facebook/fresco](https://github.com/facebook/fresco)
	* [2.https://www.fresco-cn.org/docs/index.html](https://www.fresco-cn.org/docs/index.html)
	* [3.http://www.jianshu.com/p/bb32bca8796b](http://www.jianshu.com/p/bb32bca8796b)
	* [4.官方混淆介绍](http://frescolib.org/docs/shipping.html)
	* 5.图片加载比例的显示效果 `scaleType`
		* [1.不能用 `android:scaleType ` ,要用 `fresco:actualImageScaleType ` 代替.](https://stackoverflow.com/questions/33174607/fresco-library-issues)
			

* 4.okhttp
	* 1.[GitHub 地址](https://github.com/square/okhttp)
	* 2.[Android OkHttp的基本用法](https://www.jianshu.com/p/c478d7a20d03)

* 5.Gson
	* [1.用法1](https://www.jianshu.com/p/886f7b7fca7d)
	* [2.用法2](https://www.jianshu.com/p/e740196225a4)
	* [3.GitHub 源代码](https://github.com/google/gson)
	* 4.混淆代码:
	
```
#---------------Begin: proguard configuration for Gson  ----------
# Gson uses generic type information stored in a class file when working with fields. Proguard
# removes such information by default, so configure it to keep all of it.
-keepattributes Signature
# For using GSON @Expose annotation
-keepattributes *Annotation*
# Gson specific classes
-dontwarn sun.misc.**
#-keep class com.google.gson.stream.** { *; }
# Application classes that will be serialized/deserialized over Gson
-keep class com.google.gson.examples.android.model.** { *; }
# Prevent proguard from stripping interface information from TypeAdapterFactory,
# JsonSerializer, JsonDeserializer instances (so they can be used in @JsonAdapter)
-keep class * implements com.google.gson.TypeAdapterFactory
-keep class * implements com.google.gson.JsonSerializer
-keep class * implements com.google.gson.JsonDeserializer
```



##### 2.其它相关

* 1. 滚动选择器(com.bigkoo.pickerview)
	* [1.GitHub 地址](https://github.com/Bigkoo/Android-PickerView)
	* [2.Android仿ios条件选择器pickerview](http://www.jianshu.com/p/bdbe1dd7926c)
	* [3.proguard 不用提供](https://github.com/Bigkoo/Android-PickerView/issues/544)

* 5.一键生成 View 与 Activity 的代码链接
>'com.jakewharton:butterknife:8.8.1'
>
>[https://github.com/JakeWharton/butterknife](https://github.com/JakeWharton/butterknife)
>
>[http://www.jianshu.com/p/390677e35048](http://www.jianshu.com/p/390677e35048)

* 6.简化信息订阅的方法.
>'org.greenrobot:eventbus:3.1.1'
>
>[https://github.com/greenrobot/EventBus](https://github.com/greenrobot/EventBus)
>
>[http://www.jianshu.com/p/bd645ace4f73](http://www.jianshu.com/p/bd645ace4f73)

* 8.图片的选择器
>[https://github.com/crazycodeboy/TakePhoto](https://github.com/crazycodeboy/TakePhoto)
>

* 9.Bugly Crash report 
	* [1.GitHub 地址](https://github.com/BuglyDevTeam)
	* [2.官方文档](https://bugly.qq.com/v2/)
	* [3.Bugly技术团队博客](https://buglydevteam.github.io/)
	* [4.Android 个人集成的例子](http://blog.tian.tianlovezhen.site/2018/02/08/bugly%E9%9B%86%E6%88%90/)
	* [5.混淆代码](https://bugly.qq.com/docs/user-guide/instruction-manual-android/?v=20180709165613)

```
-dontwarn com.tencent.bugly.**
-keep public class com.tencent.bugly.**{*;}
```

* 10.免 root 查看 db 信息 (facebook stetho)
	* [1.Stetho GitHub 地址](https://github.com/facebook/stetho)
	* [2.个人使用心得及教程](https://ghzjtian.github.io/2017/12/15/Android_%E5%85%8Droot%E6%9F%A5%E7%9C%8BDB/)
	* 3.混淆代码
	
```
# Stetho
-keep class com.facebook.stetho.** { *; }

```

* 11.自定义 Toast 显示(有 icon)
	* [1.GitHub 地址](https://github.com/GrenderG/Toasty)
	* 2.混淆代码

```
# Toasty
-keep class es.dmoral.toasty.** { *; }

```

* 12.自定义的 Logger 显示
	* [1.orhanobut logger GitHub 地址](https://github.com/orhanobut/logger)
	* 2.混淆代码:

```
# Logger
-keep class com.orhanobut.logger.** { *; }

```


* 13.下拉刷新控件 SmartRefreshLayout
	* [1.GitHub 地址](https://github.com/scwang90/SmartRefreshLayout)
	* [2.教程](https://segmentfault.com/a/1190000010066071)
	* [3.官方介绍说不用混淆](https://github.com/scwang90/SmartRefreshLayout#%E6%B7%B7%E6%B7%86)

* 14.DataBase
	* 1.Realm(以前用过，发现有坑,现在观望中)
		* [1.官网中文文档(不是最新)](https://realm.io/cn/docs/java/latest)
		* [2.官网英文文档](https://realm.io/docs/java/latest)
		* [3.怎样看待 Realm 这个移动数据库？](https://www.zhihu.com/question/30298585)
		* [4.Realm数据库 从入门到“放弃”](https://www.jianshu.com/p/50e0efb66bdf)
		* [5.个人的简单开发分享](http://blog.tian.tianlovezhen.site/2018/07/30/Android_Realm%E7%9A%84%E4%BD%BF%E7%94%A8/#more)
	* 2.SQLite
		* 1.sqlitebrowser(SQLite 的工具)
			* [1.GitHub 源码](https://github.com/sqlitebrowser/sqlitebrowser)
			* [2.官网(MacOS 上会闪退，暂时运行不了)](https://sqlitebrowser.org/)

* 15.Dialog
	* [1.material-dialogs](https://github.com/afollestad/material-dialogs)
		* [1.作者(Aidan Follestad)是个 95 后的牛人](https://aidanfollestad.com/)
		* 2.不用另外配置 `proguard` 的信息.
		* 3.强大的 `Dialog` 第三方包,基本上所有的 `Dialog` 用到的功能，这里都有了!!

* 16.权限申请
	* [1.RxPermissions](https://github.com/tbruyelle/RxPermissions)
		* [1.Proguard 混淆](https://github.com/tbruyelle/RxPermissions/issues/45)
		`-dontwarn com.tbruyelle.rxpermissions.**`

* [17.Android MVP Architecture Study](https://github.com/Rukey7/MvpApp)
	* 1.里面有用到大量的第三方库.
	
***

### 3.Android 自带<a name="self_android_view"/>
* 1.BottomNavigationView 
	* 1.Android 自带控件，类似于 ListView.但是有 List 和 Grid 的效果.
* [2.Android xml 字符串格式化](https://stackoverflow.com/questions/3656371/dynamic-string-using-string-xml)

* 3.SearchView 
	* [1.SearchView的使用](https://www.jianshu.com/p/00cb87a2964f)
	* [2.官方文档](https://developer.android.com/reference/android/support/v7/widget/SearchView)
	* [3.Android SearchView的高级用法，解决关于SearchView的样式与控制问题](http://tangpj.com/2016/09/11/searchview/)






