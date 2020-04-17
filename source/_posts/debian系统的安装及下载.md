---
title: debian系统的安装及下载
tags: [Linux]
categories: [linux]
date: 2018-09-07 17:46:19
---


# debian系统的安装及下载

> 在本地的旧机器上面安装 debian 系统的过程记录.

<!-- more -->

## [1.下载](#download)
## [2.U 盘安装盘的制作](#make_u_installer)
## [3.debian 系统的安装](#debian_install)


***
***
***

## 1.下载<a name="download"/>
* 1.在 [http://cdimage.debian.org/debian-cd/](http://cdimage.debian.org/debian-cd/) 上下载最新的 debian 系统.
	* 1.要根据计算机的 CPU 支持的位数去下载相应的系统安装包.
		* [1.cpu处理器架构小知识](https://www.opsdev.cn/post/chuliqijiagou.html)
		* [2.怎样查看计算机是32位还是64位操作系统](https://jingyan.baidu.com/article/09ea3edec9caa4c0aede392e.html)

***

## 2.U 盘安装盘的制作<a name="make_u_installer"/>
* 1.Mac 系统上.
	* 1.参考: [Mac制作Linux启动盘](https://www.jianshu.com/p/8f99d190e6c3)
	* 2.操作步骤:

```
debianinstaller user$ sudo diskutil unmountDisk /dev/disk2
Password:
Unmount of all volumes on disk2 was successful

debianinstaller user$ sudo dd if=/Users/user/Downloads/debian-8.8.0-amd64-netinst.iso/debian-8.8.0-amd64-netinst.iso of=/dev/disk2 bs=1m
Password:
247+0 records in
247+0 records out
258998272 bytes transferred in 49.859634 secs (5194548 bytes/sec)
```

***

## 3.debian 系统的安装<a name="debian_install"/>
* 1.在bios中选择USB启动
* 2.按照提示安装 debian install(最小化安装)



