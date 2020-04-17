---
title: xdebug集成
tags: [php]
categories: [php]
date: 2018-02-09 17:46:19
---



## 在 phpStorm 中集成 xdebug
>参考: [https://www.jianshu.com/p/8fb9ad0719c2](https://www.jianshu.com/p/8fb9ad0719c2)

<!-- more -->

##有两种方法(在用第二种方法)
#### [1.系统自带的 php + phpstorm + firefox 集成.](#sys_php)
#### [2.MAMP + phpstorm + firefox 集成.](#mamp)

***
***
***

## 1.系统自带的 php + phpstorm + firefox 集成<a name="sys_php"/>

### 1.确定要下载的 xdebug 的版本

> 在 [https://xdebug.org/wizard.php](https://xdebug.org/wizard.php) 中复制你的 phpinfo() 的信息进去，它就会自动判断(下面为我复制我的 PHPINFO 后，系统为我解析的结果).

```
Summary
Xdebug installed: no
Server API: CGI/FastCGI
Windows: no
Zend Server: no
PHP Version: 7.1.1
Zend API nr: 320160303
PHP API nr: 20160303
Debug Build: no
Thread Safe Build: no
Configuration File Path: /Applications/MAMP/bin/php/php7.1.1/conf
Configuration File: /Library/Application Support/appsolute/MAMP PRO/conf/php7.1.1.ini
Extensions directory: /Applications/MAMP/bin/php/php7.1.1/lib/php/extensions/no-debug-non-zts-20160303
Instructions
Download xdebug-2.6.0.tgz
Unpack the downloaded file with tar -xvzf xdebug-2.6.0.tgz
Run: cd xdebug-2.6.0
Run: phpize (See the FAQ if you don't have phpize.

As part of its output it should show:

Configuring for:
...
Zend Module Api No:      20160303
Zend Extension Api No:   320160303
If it does not, you are using the wrong phpize. Please follow this FAQ entry and skip the next step.

Run: ./configure
Run: make
Run: cp modules/xdebug.so /Applications/MAMP/bin/php/php7.1.1/lib/php/extensions/no-debug-non-zts-20160303
Edit /Library/Application Support/appsolute/MAMP PRO/conf/php7.1.1.ini and add the line
zend_extension = /Applications/MAMP/bin/php/php7.1.1/lib/php/extensions/no-debug-non-zts-20160303/xdebug.so
Restart the webserver
If you like Xdebug, and thinks it saves you time and money, please have a look at the donation page.
```

### 2.下载 xdebug.

### 3.


***

## 2.MAMP + phpstorm + firefox 集成.<a name="mamp"/>
>参考: 
>
>1.[https://www.jianshu.com/p/9de52bf4fc47](https://www.jianshu.com/p/9de52bf4fc47)
>2.[http://blog.csdn.net/LiuMiao1128/article/details/68060678](http://blog.csdn.net/LiuMiao1128/article/details/68060678)

#### 2.1 所用的软件:
* 1.phpstorm
* 2.mamp pro
* 3.chrome

#### 2.2详细步骤:
* 1.mamp 自带 xdebug ,直接启动 
* [http://localhost/MAMP/index.php?language=English&page=phpinfo](http://localhost/MAMP/index.php?language=English&page=phpinfo)

	* 1.2.mamp 配置的相关截图:
![](/assets/imgs/web/ScreenShot2018-02-09_10.59.35.png)
![](/assets/imgs/web/ScreenShot2018-02-09_10.59.55.png)
	* 1.3.在 phpinfo 中,可以看到php的相关配置

![](/assets/imgs/web/ScreenShot2018-02-09_11.05.33.png)

![](/assets/imgs/web/ScreenShot2018-02-09_11.05.48.png)

* 2.配置项目的运行环境(实际的操作步骤请以 [参考1](https://www.jianshu.com/p/9de52bf4fc47) 的步骤为准,这里只是贴出我自己的配置截图)


![](/assets/imgs/web/ScreenShot2018-02-09_11.08.07.png)

![](/assets/imgs/web/ScreenShot2018-02-09_11.08.32.png)

![](/assets/imgs/web/ScreenShot2018-02-09_11.08.54.png)


![](/assets/imgs/web/ScreenShot2018-02-09_11.09.03.png)


![](/assets/imgs/web/ScreenShot2018-02-12_10.01.30.png)

![](/assets/imgs/web/ScreenShot2018-02-09_11.09.23.png)

![](/assets/imgs/web/ScreenShot2018-02-09_11.12.02.png)

