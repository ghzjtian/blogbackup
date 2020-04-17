---
title: Mac上七牛云的用法
tags: [Mac]
categories: [Mac]
date: 2018-11-20 17:46:19
---



### 记录下在 Mac 上怎样使用七牛云去存储图片


<!-- more -->

## [1.新建一个 存储空间](#new_store)
## [2.添加一个域名](#add_domain)
## [3.使用 `chrome/qiniu upload files` 插件快速上传图片](#use_plugin)


***
***
***

## 1.新建一个 存储空间<a name="new_store"/>
* 1.这里一定要选择 华东地区，因为 `Chrome` 上的插件 `qiniu upload files` 只支持 华东地区.

![](http://pic.pgyjz.cn/blog/Mac/qiNiuYun/Snip20181120_2.png)

***

## 2.添加一个域名<a name="add_domain"/>
* 1.因为自动分配的域名会 30 天后过期，所以需要添加一个经过 `ICP` 备案的域名,我这里为 `pic.pgyjz.cn`.

* 2.添加后，七牛云 会分配一个域名给你，你把它添加到域名管理后台的 CNAME 的记录值那里就可以了(我这里为 阿里云 的域名管理后台).

![](http://pic.pgyjz.cn/blog/Mac/qiNiuYun/Snip20181120_4.png)

***

## 3.使用 `chrome/qiniu upload files` 插件快速上传图片<a name="use_plugin"/>

* 1.到 `个人中心`->`密钥管理` 处,取得相应的 `密钥`.

![](http://pic.pgyjz.cn/blog/Mac/qiNiuYun/Snip20181120_5.png)

* 2.下载 `chrome/qiniu upload files` .

![](http://pic.pgyjz.cn/blog/Mac/qiNiuYun/Snip20181120_6.png)

* 3.拖拉图片，即可上传

![](http://pic.pgyjz.cn/blog/Mac/qiNiuYun/Snip20181120_7.png)
