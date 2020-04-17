---
title: Android BottomNavigationView 的用法
tags: [Android]
categories: [Android]
date: 2018-07-18 17:46:19
---



# Android BottomNavigationView 的用法

* 1.用 BottomNavigationView + flagment 实现 Tabbar 切换效果.

<!-- more -->

## 1. BottomNavigationView
* 1.改变底栏 icon 和 文字 的颜色
	* 1.参考1:[https://stackoverflow.com/questions/42346899/bottomnavigationbar-change-the-tab-icon-color](https://stackoverflow.com/questions/42346899/bottomnavigationbar-change-the-tab-icon-color)
	* 2.相关的代码
	
```
//activity_brake.xml
app:itemBackground="@color/colorYellow"
app:itemIconTint="@drawable/bottom_icon_selector"
app:itemTextColor="@color/colorAccent"
```

```
//bottom_icon_selector.xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_checked="true" android:color="@color/colorWhite" />
    <item android:color="@color/colorBlack"  />
</selector>
```


* 3.相关的图片
<img src="/assets/imgs/Android/ScreenShot_2018-07-18_14.37.11.png.png">



***

