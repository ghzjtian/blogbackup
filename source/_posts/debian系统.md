---
title: debian系统
tags: [Linux]
categories: [Linux]
date: 2018-09-05 22:08:19
---


> 介绍与 debian 系统相关的命令

<!-- more -->

## [1.系统升级](#system_update)
## [2.相关的操作命令](#some_operation)
## [3.设置系统时间](#set_time)
## [4.设置中文环境](#set_language)
## [5.设置 ssh 免密登录](#no_password_login)

***
***
***

## 1.系统升级<a name="system_update"/>
##### 1.相关的参考
>1.debian8(jessie) -> debian9(stretch)

* 1.[How to Upgrade Debian 8 (Jessie) to 9 (Stretch) safely](https://www.howtoforge.com/tutorial/how-to-upgrade-debian-8-jessie-to-9-stretch/)

***
## 2.相关的操作命令<a name="some_operation"/>
* 1.查看版本号:
`# cat /etc/os-release`
* 2.查看端口的使用情况:
`# netstat -ntlp`


***

## 3.设置系统时间<a name="set_time"/>

>[1.Debian 官方方法:](https://wiki.debian.org/DateTime)
>
>[2.How to config Time and Date on Debian 8 (NTP)](https://www.hugeserver.com/kb/config-time-date-debian-8-ntp/)
>
>使用 NTP 服务器去自动纠正.


##### 1.设置自动纠正时间

* 1.更新源.
```
# apt-get update
```
* 2.安装 ntp .
```
# apt-get install ntp
```
* 3.备份 localtime .
```
# mv /etc/localtime /etc/localtime.back
```
* 4.建立链接.
```
# ln -s /usr/share/zoneinfo/PRC /etc/localtime
```
* 5.重启 ntp.

```
#service ntp restart

```

##### 2.查看当前的时间
```
root@Tim_s_Server2:/etc# date
Wed Nov 15 13:52:41 CST 2017
```

***

## 4.设置中文环境<a name="set_language"/>
>背景:在安装debian 时选择了中文的安装环境，然后在安装完成后发现有一些文字会显示菱形.
![ScreenShot2017-10-18_09.42.32.png](http://upload-images.jianshu.io/upload_images/2079545-83a305a0ea246acb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 在网络上找到解决的方法:
	* 1.[debian添加中文支持](http://www.cnblogs.com/pengdonglin137/p/3280663.html)
	* 2.[Perl: warning: Setting locale failed in Debian and Ubuntu](https://www.cyberciti.biz/faq/perl-warning-setting-locale-failed-in-debian-ubuntu/)
	* 3.[Linux之Debian夸平台时文件名乱码](https://my.oschina.net/mickelfeng/blog/95871)
	* 4.[Mac 上 FTP 工具 Transmit 不能显示中文 中文显示成了乱码 解决方法](https://www.macapp.so/tips/Transmit%E4%B8%8D%E8%83%BD%E6%98%BE%E7%A4%BA%E4%B8%AD%E6%96%87%20%E4%B8%AD%E6%96%87%E6%98%BE%E7%A4%BA%E6%88%90%E4%BA%86%E4%B9%B1%E7%A0%81%20%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/)
		* 解决方法:`将Transmit菜单VIEW中的text encoding里设置为UTF-8就可以显示中文文件名了。`

### 1.步骤:
* 1.在 sudo 下运行 dpkg-reconfigure locales，选择上以下选项：

```
en_US ISO-8859-1
zh_CN GB2312
zh_CN.GBK GBK
zh_CN.UTF-8 UTF-8
zh_TW BIG5
zh_TW.UTF-8 UTF-8
```

* 2.然后安装字体(前面两个是简体的，后面两个是繁体的)

```
# apt-get install fonts-arphic-gbsn00lp
# apt-get install fonts-arphic-gkai00mp
# apt-get install fonts-arphic-bsmi00lp
# apt-get install fonts-arphic-bkai00mp
```

* 3. 重新启动服务器: `reboot` .

> 完成后相关截图:

![ScreenShot2017-10-18_10.14.05.png](http://upload-images.jianshu.io/upload_images/2079545-deefad65e2deac17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![ScreenShot2017-10-18_10.12.47.png](http://upload-images.jianshu.io/upload_images/2079545-2e178829e9602b29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 4.设置 LANG 为 中文.

```
# LANG=en_US
# echo $LANG
en_US
//开始设置.
# LANG=zh_CN.UTF-8
# echo $LANG
```


***

## 5.设置 ssh 免密登录<a name="no_password_login"/>

#### 1.参考:

* [1.Linux 下 的ssh免密登录](https://www.jianshu.com/p/84b506ae2201)
* [2.ssh免密登录配置](https://www.jianshu.com/p/0922095f69f3)

#### 2.相关的步骤:
	
* 1.在 `客户端(如 Mac)` 生成私钥密钥对.
	* 1.生成方法:[Generating a new SSH key pair](https://gitlab.com/help/ssh/README#generating-a-new-ssh-key-pair)
* 2.在 `服务器端(如 Debian)` 的用户的主目录下的 `.ssh/` 下新建一个文件 `authorized_keys` 
	* 1.如果是 `root` 用户,相应的 `.ssh` 目录路径为 `/root/.ssh`,如果是 `tian` 用户,相应的 `.ssh` 目录路径为 `/home/tian/.ssh`.
	* 2.如果没有 `.ssh` 目录,需要自己新建.
	* 3.然后把需要免密登录的客户端的 `公钥` 添加到 `authorized_keys ` 文件后面,如:
	
```
root@debian:~/.ssh# cat authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDBhaX9ZlQtaunrVjNBDcmRIcinu8d9FkXahiXwBPu6vn7TQXfmb7ozRAp5OD7/OYXlIF2L9MBCMynIyysIzcAIX0tJ47Q9pIjAEM1z89KFx2w58/U95wWZ0dyO1D2dkMa0bJCNoGt2EimgElUxCAi3Svr/ygOScGdF7TiALL/EQR0A6dIeWEw/OziCI1Smds4HKe82FegQ7Ywj5Bm/LknvGF8tmehZJ/0Tf4I4wgSdjT0GH8tAnSbzb3ZuNUBlUOaRfnBkNjApsPClFAqbp29IQ4WcWea3MdMiQAU4s9S22Yuc5Dv97oGfhcEE9vLmRGY5NsCM6cIDQYYineVvNq3F683oYfMy3bgbFiWwOtTB2bWLgDgG0cCoLt9Z9Rgh0bJsvI2fyBHc7WFOrumaK/hTb+fkv85q8+NR9XKiH3t1eSflJzqSpkthqjT8wDZ8HYLk0G4ktXg7qCFK5aMyycjO675/anygXe/mnGW5iOCDXnWSkXerKYcnf0RUx+V9JpBjezbgRmNUEXmQfC6IJeVoaAM7WcuwnXiAgfQGYKY1JdQMEzyj5CcvWYxiQsU8NWTGcwHqsQkszW6rRW86Z3HVlB2SJZzS1NAyTMh6GMFGE61pm17KNBh8nHskgy/AhRSPdgUxDqXEblVdnF74DsGrx+8DVAIcuhS0dTT7gFB8rw== abcd@163.com(GLB MBP)
```

* 3.在登录时，只要在客户端的 `terminal` 上输入命令 `ssh yourUserName@ip_address` ,即可远程免密登录.



