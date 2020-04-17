---
title: 代码混淆
tags: [Android]
categories: [Android]
date: 2018-02-01 17:46:19
---



>记录 Android Minify 代码混淆相关

<!-- more -->


# Android Minify 代码混淆
### [1.参考:](#references)
### [2.APP 中 build.gradle 的配置.](#setting)
### [3.proguard-rules.pro 混淆的代码(cramer app)](#proguard_code)
### [4.混淆后的反编译](#check_uncode)
### [5.注意事项](#attertian)

***
***
***

### 1.参考:<a name="references"/>
* 1.[Android Studio混淆打包](https://www.jianshu.com/p/d7b7e903cfa7)
* 2.[Android Studio Apk 打包 混淆](https://www.jianshu.com/p/f140a967b789)
* 3.[https://www.jianshu.com/p/b471db6a01af](https://www.jianshu.com/p/b471db6a01af)
* 
* 4.[官方文档:](https://www.guardsquare.com/en/proguard/manual/examples#library)
* 5.[https://developer.android.com/studio/build/shrink-code.html](https://developer.android.com/studio/build/shrink-code.html)
* 6.[http://blog.csdn.net/qq_33689414/article/details/69630673](http://blog.csdn.net/qq_33689414/article/details/69630673)

***

### 2.APP 中 build.gradle 的配置.<a name="setting"/>

```
 buildTypes {
        release {
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.config
        }

        //暂时测试都为 true,如果发现影响构建速度可以设置为 false
        debug {
            minifyEnabled true
            shrinkResources true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

        }
    }
```


### 3.proguard-rules.pro 混淆的代码(cramer APP )<a name="proguard_code"/>


```

#############################################
#
# 对于一些基本指令的添加
#
#############################################

-ignorewarnings

# 代码混淆压缩比，在0~7之间，默认为5，一般不做修改
-optimizationpasses 5

# 混合时不使用大小写混合，混合后的类名为小写
-dontusemixedcaseclassnames

# 指定不去忽略非公共库的类
-dontskipnonpubliclibraryclasses

# 这句话能够使我们的项目混淆后产生映射文件
# 包含有类名->混淆后类名的映射关系
-verbose

# 指定不去忽略非公共库的类成员
-dontskipnonpubliclibraryclassmembers

# 不做预校验，preverify是proguard的四个步骤之一，Android不需要preverify，去掉这一步能够加快混淆速度。
-dontpreverify

# 保留Annotation不混淆
-keepattributes *Annotation*,InnerClasses

# 避免混淆泛型
-keepattributes Signature

# 抛出异常时保留代码行号
-keepattributes SourceFile,LineNumberTable

# 指定混淆是采用的算法，后面的参数是一个过滤器
# 这个过滤器是谷歌推荐的算法，一般不做更改
-optimizations !code/simplification/cast,!field/*,!class/merging/*


#############################################
#
# Android开发中一些需要保留的公共部分
#
#############################################

# 保留我们使用的四大组件，自定义的Application等等这些类不被混淆
# 因为这些子类都有可能被外部调用
-keep public class * extends android.app.Activity
-keep public class * extends android.app.Appliction
-keep public class * extends android.app.Service
-keep public class * extends android.content.BroadcastReceiver
-keep public class * extends android.content.ContentProvider
-keep public class * extends android.app.backup.BackupAgentHelper
-keep public class * extends android.preference.Preference
-keep public class * extends android.view.View
-keep public class com.android.vending.licensing.ILicensingService


# 保留support下的所有类及其内部类
-keep class android.support.** {*;}

# 保留继承的
-keep public class * extends android.support.v4.**
-keep public class * extends android.support.v7.**
-keep public class * extends android.support.annotation.**

# 保留R下面的资源
-keep class **.R$* {*;}

# 保留本地native方法不被混淆
-keepclasseswithmembernames class * {
    native <methods>;
}

# 保留在Activity中的方法参数是view的方法，
# 这样以来我们在layout中写的onClick就不会被影响
-keepclassmembers class * extends android.app.Activity{
    public void *(android.view.View);
}

# 保留枚举类不被混淆
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

# 保留我们自定义控件（继承自View）不被混淆
-keep public class * extends android.view.View{
    *** get*();
    void set*(***);
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
}

# 保留Parcelable序列化类不被混淆
-keep class * implements android.os.Parcelable {
    public static final android.os.Parcelable$Creator *;
}

# 保留Serializable序列化的类不被混淆
-keepnames class * implements java.io.Serializable
-keepclassmembers class * implements java.io.Serializable {
    static final long serialVersionUID;
    private static final java.io.ObjectStreamField[] serialPersistentFields;
    !static !transient <fields>;
    !private <fields>;
    !private <methods>;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}

# 对于带有回调函数的onXXEvent、**On*Listener的，不能被混淆
-keepclassmembers class * {
    void *(**On*Event);
    void *(**On*Listener);
}

# webView处理，项目中没有使用到webView忽略即可
-keepclassmembers class fqcn.of.javascript.interface.for.webview {
    public *;
}
-keepclassmembers class * extends android.webkit.webViewClient {
    public void *(android.webkit.WebView, java.lang.String, android.graphics.Bitmap);
    public boolean *(android.webkit.WebView, java.lang.String);
}
-keepclassmembers class * extends android.webkit.webViewClient {
    public void *(android.webkit.webView, jav.lang.String);
}

# 移除Log类打印各个等级日志的代码，打正式包的时候可以做为禁log使用，这里可以作为禁止log打印的功能使用
# 记得proguard-android.txt中一定不要加-dontoptimize才起作用
# 另外的一种实现方案是通过BuildConfig.DEBUG的变量来控制
#-assumenosideeffects class android.util.Log {
#    public static int v(...);
#    public static int i(...);
#    public static int w(...);
#    public static int d(...);
#    public static int e(...);
#}

#############################################
#
# 项目中特殊处理部分
#
#############################################

#-----------处理反射类---------------



#-----------处理js交互---------------



#-----------处理实体类---------------
# 在开发的时候我们可以将所有的实体类放在一个包内，这样我们写一次混淆就行了。
#-keep class com.blankj.data.bean.**{ *; }

#保护 Application 类
-keep class com.greenworks.guide.GreenworksApplication { *; }

#保护 HttpManage 下的 bean 类
-keep class com.greenworks.guide.net.HttpManage$ResultCallback { *; }
-keep class com.greenworks.guide.net.HttpManage$Error { *; }
-keep class com.greenworks.guide.net.HttpManage$ErrorEntity { *; }

# keep 其它 bean 类
-keep class com.greenworks.guide.eventbus.ChargeFinishEvent { *; }
-keep class com.greenworks.guide.upgrade.** { *; }


#-----------处理第三方依赖库---------
#项目所用到的第三方的库,不要混淆

#jar 包

#android-async-http
-keep class com.loopj.android.http.** { *; }

#hiflying-iots-android-smartlink
-keep class com.hiflying.smartlink.** { *; }

#httpclient, httpmime
-keep class org.apache.http.entity.mime.** { *; }

#xlink-wifi-official-sdk,云智易 wifi  sdk
-keep class io.xlink.wifi.sdk.** { *; }


#自定义view

#pickerview
-keep class com.bigkoo.pickerview.** { *; }

#bean,如果使用了Gson之类的工具要使被它解析的JavaBean类即实体类不被混淆。
-keep class com.greenworks.guide.bean.** { *; }

#网络自动下载 lib 包

#android 包
-keep class android.** { *; }

#apache 包
-keep class org.apache.http.** { *; }

#facebook.fresco
-keep class com.facebook.** { *; }

#nordicsemi , DFU,
-keep class no.nordicsemi.android.dfu.** { *; }

#okhttp
-keep class okhttp3.** { *; }

#Event Bus
# https://github.com/greenrobot/EventBus/issues/503
-keepattributes *Annotation*
-keepclassmembers class ** {
    @org.greenrobot.eventbus.Subscribe <methods>;
}
-keep enum org.greenrobot.eventbus.ThreadMode { *; }

# Only required if you use AsyncExecutor
-keepclassmembers class * extends org.greenrobot.eventbus.util.ThrowableFailureEvent {
    <init>(java.lang.Throwable);
}

#butterknife
-keep class butterknife.** { *; }
-dontwarn butterknife.internal.**
-keep class **$$ViewBinder { *; }
-keepclasseswithmembernames class * {
    @butterknife.* <fields>;
}
-keepclasseswithmembernames class * {
    @butterknife.* <methods>;
}

#takephoto
-keep class com.jph.takephoto.** { *; }

#google map
-keep class com.google.** { *; }

#barteksc/AndroidPdfViewer
-keep class com.shockwave.**

#orhanobut logger
-keep class com.orhanobut.logger.** { *; }

#nrf logger
-keep class no.nordicsemi.android.log.** { *; }

```

***

### 4.混淆后的反编译<a name="check_uncode"/>
>参考： [https://www.jianshu.com/p/a21edfa632cf](https://www.jianshu.com/p/a21edfa632cf)
>
>所用到的软件的下载: 链接:[https://pan.baidu.com/s/1kWySit5](https://pan.baidu.com/s/1kWySit5)  密码:81u7

***

### 5.注意事项<a name="attertian"/>

* 5.1发现查看反编译的代码时,每次都需要重启 JD-GUI.app ,否则显示的还是上一次查看的结果.

* 5.2 要注意 bean 不能被混淆.
* 5.3 所有用到的第三方的 lib ,自定义的 view ，最好都添加到 -keep 里面.
