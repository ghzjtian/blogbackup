---
title: Mac相关的信息
tags: [Mac]
categories: [Mac]
date: 2017-12-19 18:46:19
---



# Mac相关的信息

* 主要记录在 Mac 相关的命令及配置的信息 .

<!-- more -->

## [1.修改 DNS 信息.](#modify_DNS)
## [2.Web 服务器](#Web_server)
## [3.Mysql 相关](#mysql_server)
## [4.MAMP搭建](#mamp_install)
## [5.Web(Nginx) 服务器](#web_server_nginx)
## [6.解压文件](#unrar_folder)


***
***
***

## 1.修改 DNS 信息.<a name="modify_DNS"/>
>修改本地的 DNS 信息,直接用 域名访问指定的 IP 地址的信息.

>如访问 127.0.0.1
>访问局域网内的其它机器.

* 修改 /etc/hosts 文件.


```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1	localhost
255.255.255.255	broadcasthost
::1             localhost
127.0.0.1	www.glbmac.com
10.100.1.217	www.glbgz.com
```

## 2.Web 服务器<a name="Web_server"/>
>Mac(macOS Sierra,Ver 10.12.6) 自带 apache2 服务器.

##### 1.修改 httpd.conf 目录.

> /private/etc/apache2/httpd.conf

```
#去除前面的#号,使得 apache2 支持 php
LoadModule php5_module libexec/apache2/libphp5.so

#修改Directory
<Directory />
AllowOverride none
Require all granted
</Directory>

#把目录改为自己的目录(Mac 限制了只能在 user 中的 Sites 下建立主目录)
DocumentRoot "/Users/tianzeng/Sites"
<Directory "/Users/tianzeng/Sites">
#发现也可以在 /private/etc/apache2/extra/httpd-userdir.conf 下修改主目录，但没验证

```

***

##### 2.修改 httpd-vhosts.conf
>/private/etc/apache2/extra/httpd-vhosts.conf

```
#可以用域名代替 localhost
<VirtualHost *:80>
DocumentRoot "/Users/tianzeng/Sites"
ServerName www.mysites.com
ErrorLog "/private/var/log/apache2/sites-error_log"
CustomLog "/private/var/log/apache2/sites-access_log" common
<Directory />
Options Indexes FollowSymLinks MultiViews
AllowOverride None
Order deny,allow
Allow from all
</Directory>
</VirtualHost>


```

##### 3.控制的命令
```
sudo apachectl start
sudo apachectl stop
```

*** 


## 3.Mysql 相关<a name="mysql_server"/>
#### 1.相关控制命令
```
#启动／停止命令

On macOS Sierra & OS to start/stop/restart MySQL post 5.7  from the command line:
start: sudo launchctl load -F /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
stop: sudo launchctl unload -F /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist

```

***

## 4.MAMP搭建<a name="mamp_install"/>

> 详细教程: http://www.jianshu.com/p/d560ce2318c5
> 
> http://www.jianshu.com/p/a665a6372e42
> 
> Macintosh、Apache、MySQL和PHP



#### 1.下载 MAMP
* 到 百度云 下载破解版(有钱请支持正版),然后
	* 1.需要先打开 mamp
	* 2.再运行 mamp pro 

#### 2.关闭原先系统的开发环境
* 1.Mysql
```
sudo launchctl unload -F /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
```
* 2.Apache2
```
sudo apachectl stop
```

#### 3.MAMP 各种文件路径
1.配置文件:
```
/Library/Application Support/appsolute/MAMP PRO/conf
```


#### 4.Mac 连接 MAMP 的内置 mysql 
>1.修改 mamp 的 mysql 的默认端口为 3307.
>
>2.连接教程: [https://stackoverflow.com/questions/43157632/mysql-command-line-with-mamp](https://stackoverflow.com/questions/43157632/mysql-command-line-with-mamp)

* 1.相关的步骤:
	* 1.Command Line 进入 ```/Applications/MAMP/Library/bin ```
	* 2.连接 mysql : ``` ./mysql -u root -p```

***

## 5.Web(Nginx) 服务器<a name="web_server_nginx"/>
### 1.参考:
* 1.[更换 brew 镜像](https://xu3352.github.io/mac/2018/09/06/mac-homebrew-update-slowly)
* 2.[Mac下用brew安装nginx](https://www.jianshu.com/p/6c7cb820a020)
* 3.[Installing Nginx in Mac OS X Maverick With Homebrew](https://medium.com/@ThomasTan/installing-nginx-in-mac-os-x-maverick-with-homebrew-d8867b7e8a5a)

### 2.步骤

* 1.安装 nginx `brew install nginx`,安装完成后会出现:

```
Docroot is: /usr/local/var/www

The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
nginx can run without sudo.

nginx will load all files in /usr/local/etc/nginx/servers/.

To have launchd start nginx now and restart at login:
  brew services start nginx
Or, if you don't want/need a background service you can just run:
  nginx
```

> 我们从上面的返回可以看出， nginx 的配置文件在 `/usr/local/etc/nginx/nginx.conf` 中, 网站目录在 `/usr/local/var/www` 中.

* 2.控制的命令:

```
brew services start nginx
brew services stop nginx
brew services restart nginx

```

以上命令可能会出现 `Error: Unknown command: services`, 可以以一下命令去控制:

```
sudo nginx
sudo nginx -s stop

```

## 6.命令行解压

## 1.参考
* 1.[How can I extract .rar files on the Mac?](https://superuser.com/questions/52124/how-can-i-extract-rar-files-on-the-mac#comment2010675_52126)
* 2.[Unzip fail when zip contains chinese char on macOS 10.13](https://github.com/CocoaPods/CocoaPods/issues/7711)


## 2.解压
* 1.unrar `.rar`文件

```
Using Homebrew, in a terminal type:

brew install unrar

to use it just navigate to your file and type

unrar x <filename>

Or list files via unrar l archive.rar and extract single file: unrar e archive.rar folder/file.exe desired_location/
```

* 2.unzip `zip` 文件

```
unzip xxx.zip

or 

ditto -V -x -k --sequesterRsrc --rsrc FILENAME.ZIP DESTINATIONDIRECTORY
```


