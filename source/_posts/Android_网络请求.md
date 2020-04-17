---
title: Android 网络请求
tags: [Android]
categories: [Android]
date: 2019-01-11 17:46:19
---

> 记录 Android 网络请求的问题点.

<!-- more -->


## 1.开发中遇到的问题点.

> 在用 HttpURLConnection 请求 Http 的 Get 请求时,出现 `InputStream.read` 方法在差不多末尾时一直返回 `java.net.ProtocolException: unexpected end of stream ` 的问题.

* 1.不是 HTTP 的问题. 
	* 1.用 PostMan 的 http 请求，可以返回期待的值.
* 2.不是请求方法的问题.
	* 1.同样的方法，就运行那个 URL 有问题.
* 3.问题.
	* 1.同样的内容返回，同样的方法和 http 请求， PostMan 的返回中没有 Transfer 就可以返回期待的值.
	* 2.同样的内容返回，同样的方法和 http 请求，同样的链接，用纯 java 代码的返回结果就没有问题.
	* 3.定位到是 `bufferedReader.readLine()` 有问题，但是纯 java 代码又没有问题.
	* 4.不是  Transfer-Encoding 为 chunked 的问题
	* 5.定位到是后面的几位 byte 取值过大的问题.
* 4.显示的错误:
```
java.net.ProtocolException: unexpected end of stream
```
* 5.参考文献:

```
看了很多文章都没找到办法:
https://github.com/lingochamp/FileDownloader/issues/775
https://stackoverflow.com/questions/50798241/java-net-protocolexception-unexpected-end-of-stream-android

```	
	
* 6.还没找到解决的方法.

