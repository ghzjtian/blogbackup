---
title: Android_Realm的使用
tags: [Android]
categories: [Android]
date: 2018-07-30 18:46:19
---



# Android_Realm的使用
>1.Android 中使用 Realm 数据库


<!-- more -->


## 1.参考资料
* 1.参考资料
	* [1.官网中文文档(不是最新)](https://realm.io/cn/docs/java/latest)
	* [2.官网英文文档](https://realm.io/docs/java/latest)
	* [3.怎样看待 Realm 这个移动数据库？](https://www.zhihu.com/question/30298585)
	* [4.Realm数据库 从入门到“放弃”](https://www.jianshu.com/p/50e0efb66bdf)
	* [5.Realm for Android详细教程](https://www.jianshu.com/p/28912c2f31db)


## 2.初步使用教程
* 1.在项目的 `build.gradle` 中，写入以下代码 ` classpath "io.realm:realm-gradle-plugin:5.4.0"`
* 2.在 `app` 的 `build.gradle` 中，写入 `apply plugin: 'realm-android'` 和 

``` 
realm {
    syncEnabled = true;
} 
```

* 3.在 `Application` 中的 `onCreate` 方法中加入 ` Realm.init(this);`

* 4.简单的数据操作
	* 1.增加
	 
```
//添加
        Realm realm = Realm.getDefaultInstance();
        realm.beginTransaction();

        SearchHistoryBean history = realm.createObject(SearchHistoryBean.class);
        history.setId("1");
        history.setType("Brake");
        history.setValue(query);
        history.setSearchTime(System.currentTimeMillis());

        realm.commitTransaction();
```

* 2.查询

```
   //查询
Realm mRealm = Realm.getDefaultInstance();
RealmResults<SearchHistoryBean> historys = mRealm.where(SearchHistoryBean.class).findAll();
QLLog.i(mRealm.copyFromRealm(historys).toString());
```


* 5.导出 APP 中 `realm` 的数据库:
	* 1.在 `AndroidStudio/Device File Explorer` 中，打开 `data/data/app_package/files/realm-object-ser` 中，右键，保存到桌面.
	* 2.用 `Realm Studio` 打开上一步下载的 数据库文件。

<img src="/assets/imgs/Android/ScreenShot_2018-07-30_16.54.11.png">

