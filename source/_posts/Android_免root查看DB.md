---
title: Android 免 root 查看数据库的内容
tags: [Android]
categories: [Android]
date: 2017-12-15 09:32:19
---



### Android 免 root 查看数据库的内容

* 在 Android 中调试时常常需要查看 SharedPreferences , database 等等的内容，但有些手机 root 又比较麻烦，或 root 不成功,今天偶尔看到这种简单的方法.

<!-- more -->



#### 1.步骤:

> 手机要先连接电脑，开启 开发者模式.

* 1.在 build.gradle 中添加

 ```Android
 dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    //查看 db 信息
    compile 'com.facebook.stetho:stetho:1.5.0'
}
 ```


* 2.在应用程序的 onCreate() 中添加 

如:

```Android
public class GreenworksApplication extends Application{
   @Override
    public void onCreate() {
        super.onCreate();

         //在 DEBUG 模式才可以用免 ROOT 查看的功能 。
        if(BuildConfig.DEBUG) {
            //查看 数据库.Chrome 浏览器打开  chrome://inspect/#devices 即可
            Stetho.initializeWithDefaults(this);
        }
	}
}
```


* 3.在电脑的 Chrome 中打开 chrome://inspect/
![](/assets/imgs/Android/Snip20171215_7.png)



> 参考1: https://stackoverflow.com/questions/23635644/how-can-i-view-the-shared-preferences-file-using-android-studio
> 
> 参考2: http://facebook.github.io/stetho/