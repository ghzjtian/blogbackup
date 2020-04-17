---
title: Google Map 在 google play 中不显示的问题
tags: [Android]
categories: [google]
date: 2017-12-27 17:46:19
---


>最近在开发 google map ,发现在本地下很正常(debug 和 release)(可以正常地显示),但是把 release 的 apk 上传到 google play 上面就一片空白，现在记录下解决的过程.

<!-- more -->

### [1.这个是 google play 推出的 Google Play App Signing 服务所导致的.](#issues)
### [2.解决](#solves)


***
***
***


## 1.这个是 google play 推出的 Google Play App Signing 服务所导致的.<a name="issues"/>


>开始发布应用时，自己手贱点击了 
`启用Google Play App Signing`

* 参考

> 1.https://stackoverflow.com/questions/44671778/published-app-on-play-store-cant-communicate-with-google-maps-api-and-facebook
> 
> 2.https://support.google.com/googleplay/android-developer/answer/7384423

***

## 2.解决<a name="solves"/>

* 1.在 google play 中 APP 的 应用签名 处查看 google 新增的 SHA1

![](/assets/imgs/Android/Snip20171227_5.png)

* 2.把 上面中的 SHA1 添加到 google map 的 API 密钥 的管理台中，然后保存即可.

![](/assets/imgs/Android/Snip20171227_6.png)

* 3.前后的效果对比图.

![](/assets/imgs/Android/ScreenShot2017-12-27_15.41.29.png)
![](/assets/imgs/Android/ScreenShot2017-12-27_15.41.18.png)

