---
title: 前端_css_调试
tags: [frontend]
categories: [frontend]
date: 2018-08-13 17:46:19
---



# 前端_css_调试
>这几天在做一个前端的页面，发现因为 墙 的问题，运行不了，在这里特地记录解决的方法.


<!-- more -->



## 1.背景
## 2.解决方法


***
***
***

## 1.背景
* 1.之前在网页上 css 写的是:

```
<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Raleway">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">

```
在自己的电脑上跑没问题，但是有小伙伴运行发现在他们的电脑上跑，一直 LOAD 不出来，很久后 LOAD 出来了，但是版面乱七八糟的，经过排查，发现是 墙的问题，我的电脑一直在 翻，所以没有察觉到.

## 2.解决方法
* 1.到各个网站下载对应的 CSS 代码
	* 1.特别注意 `font-awesome.min.css` 的 CSS，要到 [官网](https://fontawesome.com/v4.7.0/get-started/) 去下载完整的包导入才行,因为它同时还关联其它的 CSS/JS/FONT 包的，如果只是导入一个，显示的效果会不完整.
* 2.导入到代码中，以 [LARAVEL 为例](https://stackoverflow.com/questions/13433683/using-css-in-laravel-views):
	* 1.把下载好的 CSS 放到 `public/css` 文件夹中.
	* 2.在 `blade.php` 中导入,导入方法可以参考以下:
	
	```
	<link rel="stylesheet" href="<?php echo asset('css/w3.css')?>">
<link rel="stylesheet" href="<?php echo asset('css/raleway.css')?>">
<link rel="stylesheet" href="{{ URL::asset('css/font-awesome.min.css')}}">
<link rel="stylesheet" href="<?php echo asset('css/bootstrap.min.css')?>">
	```

