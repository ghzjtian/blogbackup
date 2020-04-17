---
title: Android bugly集成
tags: [Android]
categories: [Android]
date: 2018-02-08 17:46:19
---



### bugly集成
>根据 [https://bugly.qq.com/docs/user-guide/instruction-manual-android/?v=20180206175744](https://bugly.qq.com/docs/user-guide/instruction-manual-android/?v=20180206175744) 去集成.
>步骤文字很多为复制，为方便自己而已.

<!-- more -->

##### 详细步骤

* 1.在Module的build.gradle文件中添加依赖和属性配置：
```
 //Bugly Crash report
    compile 'com.tencent.bugly:crashreport:2.6.6'
```

* 2.在AndroidManifest.xml中添加权限：
```
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.READ_LOGS" />
```
* 3.请避免混淆Bugly，在Proguard混淆文件中增加以下配置：
```
-dontwarn com.tencent.bugly.**
-keep public class com.tencent.bugly.**{*;}
```

* 4.获取APP ID并将以下代码复制到项目Application类onCreate()中，Bugly会为自动检测环境并完成配置：

```
CrashReport.initCrashReport(getApplicationContext(), "4ece1b11736", BuildConfig.DEBUG);

```

* 5.制造一个 Crash

```
  //测试 bugly crash
 CrashReport.testJavaCrash();
```

* Crash 后台收到效果截图:
![](/assets/imgs/Android/ScreenShot2018-02-08_14.55.11.png)