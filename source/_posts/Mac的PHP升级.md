---
title: Mac的PHP升级
tags: [Mac]
categories: [Mac]
date: 2018-11-13 17:46:19
---



## [1.参考](#references)
<!-- more -->
## [2.具体的过程](#detail)



***
***
***

## 1.参考<a name="references"/>
* [1.How to upgrade your version of PHP to 7.0 on macOS Sierra](https://medium.com/zenchef-tech-and-product/how-to-upgrade-your-version-of-php-to-7-0-on-macos-sierra-e1bfdea55a63)
* [2.Upgrade to PHP 7.3 or 7.2 on macOS Mojave, Sierra or on OSX 10.6 – 10.11](https://coolestguidesontheplanet.com/upgrade-php-on-osx/)

***

## 2.具体的过程<a name="detail"/>
#### 1.升级前
```
$ php -v
PHP 5.6.30 (cli) (built: Oct 29 2017 20:30:32) 
Copyright (c) 1997-2016 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies

```
* 1.然后输入 `curl -s https://php-osx.liip.ch/install.sh | bash -s 7.2` 进行升级

* 2.提示升级完毕后，重启 `terminal`,输入 `php -v` ,即可看到新版本:

```
 php -v
PHP 7.2.12 (cli) (built: Nov  9 2018 11:03:05) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.12, Copyright (c) 1999-2018, by Zend Technologies
```

## 3.简单的 php 服务器

* 1.在 terminal 运行以下命令
```
php -S 10.100.1.172:8080 -t .
```