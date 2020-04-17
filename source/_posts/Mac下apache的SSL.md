---
title: Mac下apache的SSL
tags: [Mac]
categories: [Mac]
date: 2018-04-11 17:46:19
---



###  Mac下apache的SSL

<!-- more -->

#### 1.相关参考:
* 参考1: [https://www.jianshu.com/p/b2a9655fe687](https://www.jianshu.com/p/b2a9655fe687)
* 参考2: [https://getgrav.org/blog/macos-sierra-apache-ssl](https://getgrav.org/blog/macos-sierra-apache-ssl)
* 参考3: [https://gist.github.com/jonathantneal/774e4b0b3d4d739cbc53](https://gist.github.com/jonathantneal/774e4b0b3d4d739cbc53)
* 参考4: [https://discussions.apple.com/thread/3184919](https://discussions.apple.com/thread/3184919)



#### 2.过程
* 1.在参照 `参考1` 配置无效的情况下，参照 `参考2` 才配置好了(Mac 的系统版本为 10.12.6 (16G29),macOS Sierra)!
* 2.手机，电脑 通过 `https` 也可以正常地访问.
* 3.不知道为什么，本地的电脑始终无法被外网访问 443 端口 ，明明已经做了映射.
* 4.参照了 `参考3` 后，还是不能正常地访问，而且发现了新的问题:
	* 1.SSL 的 `DocumentRoot` 的路径不能被修改，已修改就会出现 `Forbidden,You don't have permission to access / on this server.`

* 5.在关闭 `httpd` 时，发现一些奇怪的问题:
	* 1.运行命令 `sudo apachectl -k stop` , http 没反应，但 https 已经关闭了.
	* 2.在参照了 `参考4`后，运行命令 ` sudo killall httpd`,才把 http 关闭了.  


