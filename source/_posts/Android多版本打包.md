---
title: Android多版本打包
tags: [Android]
categories: [Android]
date: 2018-11-16 17:46:19
---



## [1.背景](#background_ifo)
<!-- more -->
## [2.多包名打包](#multi_package_name)
* 1.`build.gradle` file.
* 2.`AndroidManifest.xml` file 及 参数的使用.
* 3.导入的图片.
* 4.打包.
* 5.安装 apk 到模拟器中.
* 6.查看包名信息.
* 7.[项目的源码已上传到了 GitHub](https://github.com/ghzjtian/multiPackage_generated)

***
***
***


## 1.背景<a name="background_ifo"/>
* 1.公司有两个 app ，内部的逻辑一摸一样，就是 app 的 icon ，主题，显示的文字，另一个 app 屏蔽一些功能,遂要实现多包名打包的功能.
* 2.参考:
	* [1.Android使用Gradle玩转多渠道多包名打包](https://www.jianshu.com/p/42c0f300ffb5)	

***

## 2.多包名打包<a name="multi_package_name"/>
* 1.修改 `app/build.gradle` 文件,如下:

```
apply plugin: 'com.android.application'

android {
    //签名信息
    signingConfigs {
        release {
            keyAlias 'key0'
            keyPassword 'multi1230'
            storeFile file('/Users/glb_gz/Documents/AS_Workplace/MultiPackageAPP/debug')
            storePassword 'multi1230'
        }
    }
    compileSdkVersion 28
    defaultConfig {
        applicationId "com.example.glb_gz.multipackageapp"
        minSdkVersion 15
        targetSdkVersion 28
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            signingConfig signingConfigs.release
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }

    flavorDimensions "default"
    //配置不同包名的 APK
    productFlavors{

        GreenGuide{
            dimension "default"
            applicationId "com.example.glb_gz.multipackageapp.guide"
            manifestPlaceholders = [new_app_name:"Guide",icon:"@mipmap/ic_launcher_guide"]

            resValue("string","my_app_name","Green Guide APP")
            resValue("bool","is_cramer_app","false")
            resValue("dimen","my_app_margin","10dp")
            resValue("color","app_background_color","#0f0")

        }
        Cramer{
            dimension "default"
            applicationId "com.example.glb_gz.multipackageapp.cramer"
            //在 AndroidManifest.xml 中使用,如:
            //      android:icon="${icon}"
            //      android:label="${new_app_name}"
            manifestPlaceholders = [new_app_name:"Cramer",icon:"@mipmap/ic_launcher_cramer"]
            //在 java 代码中使用，具体的方式为: context.getResources().getString(R.string.my_app_name);
            //其他属性的设置 可以参考这篇文章:  https://www.tanelikorri.com/tutorial/android/set-variables-in-build-gradle/
            resValue("string","my_app_name","Cramer APP")
            resValue("bool","is_cramer_app","true")
            resValue("dimen","my_app_margin","30dp")
            resValue("color","app_background_color","#f00")
        }

    }


}

dependencies {
    implementation fileTree(include: ['*.jar'], dir: 'libs')
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation 'com.android.support:design:28.0.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    implementation 'com.android.support:support-vector-drawable:28.0.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}

```

* 2.相关的在 `AndroidManifest.xml` 文件的配置及 `Activity` 中的提取:

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.glb_gz.multipackageapp">

    <application
        android:allowBackup="true"
        android:icon="${icon}"
        android:label="${new_app_name}"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:label="${new_app_name}">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

```
 //测试各个属性的提取,如 boolean ,string ,dimen,color 等等
        app_info_tv.setText(getResources().getString(R.string.my_app_name)+"  isCramerApp? "+getResources().getBoolean(R.bool.is_cramer_app));
        view_ll.setBackgroundColor(getResources().getColor(R.color.app_background_color));

        Log.i("TEST","isCramerApp? "+getResources().getBoolean(R.bool.is_cramer_app));
```

* 3.导入的相关 `icon` 图片:

<img src="/assets/imgs/Android/multi_package/Snip20181116_3.png" width="60%" height="60%">

* 4.通过 `Generate signed` 去打包不同包名的包
	* 1.也试过 [用命令行](https://www.jianshu.com/p/3b38c2b110fb) 去打包,但出现错误，查找解决方法无果后就放弃了.

```
$ ./gradlew assembleRelease --stacktrace

FAILURE: Build failed with an exception.

* What went wrong:
Could not determine java version from '11.0.1'.

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.

* Get more help at https://help.gradle.org

```

<img src="/assets/imgs/Android/multi_package/Snip20181116_4.png" width="60%" height="60%">

<img src="/assets/imgs/Android/multi_package/Snip20181116_5.png" width="60%" height="60%">


* 5.通过命令行把 apk 文件安装到 模拟器上面，并看到两个 app 已经安装成功,并有不同的显示效果.

```
 glb_gz$ adb install /Users/glb_gz/Desktop/Cramer/release/app-Cramer-release.apk
Success
 glb_gz$ adb install /Users/glb_gz/Desktop/GreenGuide/release/app-GreenGuide-release.apk
Success

```

<img src="/assets/imgs/Android/multi_package/Snip20181116_2.png" width="40%" height="40%">

<img src="/assets/imgs/Android/multi_package/Snip20181116_10.png" width="40%" height="40%">

<img src="/assets/imgs/Android/multi_package/Snip20181116_9.png" width="40%" height="40%">

* 6.在 `terminal` 中用命令行 `adb shell pm list packages` 查看模拟器中包名的信息,证明已经成功设置了包名:

```
$ adb shell pm list packages

...
package:com.example.glb_gz.multipackageapp.cramer
package:com.example.glb_gz.multipackageapp.guide
...
```

