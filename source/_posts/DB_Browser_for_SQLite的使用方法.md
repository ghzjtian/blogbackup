---
title: DB_Browser_for_SQLite的使用方法
tags: [Mac]
categories: [Mac]
date: 2018-07-29 23:46:19
---



# DB_Browser_for_SQLite的使用方法

>在网上看到有个开源的查看 SQLite 中数据的软件 DB_Browser_for_SQLite ，所以下载来看看,没想到下载下来，一打开就 Crash.特意在这里记录下解决的方法

<!-- more -->

### 1.相关机器及软件的配置

<img src="/assets/imgs/Mac/ScreenShot_2018-07-29_23.08.14.png">

<img src="/assets/imgs/Mac/Snip20180729_1.png">


### 2.解决的步骤

* 1.在官网下载最新的 Release 版本(Version 3.10.1 released)
	* [1.GitHub 源码](https://github.com/sqlitebrowser/sqlitebrowser)
	* [2.官网(MacOS 上会闪退，暂时运行不了)](https://sqlitebrowser.org/)

* 2.下载下来，安装好后，一打开就 Crash.

<img src="/assets/imgs/Mac/ScreenShot_2018-07-29_23.07.56.png">

<img src="/assets/imgs/Mac/ScreenShot_2018-07-29_23.20.31.png">

* [3.在 GitHub 中看到好像是 APP 联网的问题](https://github.com/sqlitebrowser/sqlitebrowser/issues/1311)
	* 1.遂尝试把 Mac 的网络断掉，然后再打开，可以了!!!
	* 2.然后再打开 APP，然后打开 `偏好设置`,把里面的 `自动更新`取消勾选，就 OK ,联网再打开也没事.

<img src="/assets/imgs/Mac/Snip20180729_2.png">

<img src="/assets/imgs/Mac/Snip20180729_3.png">

<img src="/assets/imgs/Mac/Snip20180729_5.png">

