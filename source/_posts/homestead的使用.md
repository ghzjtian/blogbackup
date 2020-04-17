---
title: Homestead 的使用
tags: [php]
categories: [php]
date: 2018-11-12 17:46:19
---

> 主要记录下 `Homestead` 的使用方法

<!-- more -->

## [1.参考](#referances)
## [2.ssh 连接](#ssh_connect)
## [3.上传文件到服务器](#upload_file)
## [4.mysql 的操作](#mysql_usage)
## [5.MongoDB 的安装](#mongoDB_install)
## [6.相关的问题](#related_issues)
***
***
***

## 1.参考<a name="referances"/>
* [1.Laravel Homestead 5.7 中文文档](https://laravel-china.org/docs/laravel/5.7/homestead/2245#8875b6)

***

## 2.ssh 连接<a name="ssh_connect"/>
* 1.在项目的目录下面，可以用 `vagrant ssh` 去进入到 Linux 虚拟机下面.
* 2.在其他的路径的时候，通过 `ssh vagrant@192.168.10.10` 去连接,[详细参考这里](https://stackoverflow.com/questions/29450404/is-there-a-default-password-to-connect-to-vagrant-when-using-homestead-ssh-for).

```
$ ssh vagrant@192.168.10.10
Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-32-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Nov 12 11:58:32 UTC 2018

  System load:  0.0               Processes:           131
  Usage of /:   9.6% of 61.80GB   Users logged in:     1
  Memory usage: 21%               IP address for eth0: 10.0.2.15
  Swap usage:   0%                IP address for eth1: 192.168.10.10


285 packages can be updated.
96 updates are security updates.


Last login: Mon Nov 12 11:32:02 2018 from 10.0.2.2
vagrant@httpdocs:~$ exit
logout
Connection to 192.168.10.10 closed.

```

***

## 3.上传文件到服务器<a name="upload_file"/>
* [1.利用 `scp` 去上传文件](https://www.cnblogs.com/no7dw/archive/2012/07/07/2580307.html):

```
$ scp /Users/glb_gz/Desktop/smarttec.sql vagrant@192.168.10.10:/tmp
smarttec.sql                                  100% 1556KB  54.1MB/s   00:00    
glb-gzdeMacBook-Pro:~ glb_gz$ 
```

*** 

## 4.mysql 的操作<a name="mysql_usage"/>
* 1.用户名和密码分别为 `homestead`/`secret`,详见 [连接数据库](https://laravel-china.org/docs/laravel/5.7/homestead/2245#connecting-via-ssh),相关的操作例子:

```
vagrant@httpdocs:~$ mysql -u homestead -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.23-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

***

## 5.MongoDB 的安装<a name="mongoDB_install"/>

#### 1.参考(目前主要用 homestead vagrant 去安装 mongodb):

* [1.HomeStead 中安装 MongoDB](https://laravel-china.org/docs/laravel/5.7/homestead/2245#installing-mongodb)
* [2.Installing MongoDB on Homestead](https://medium.com/@williamvicary/installing-mongodb-on-homestead-3eb750dcc891)
* [3.PHP7 MongDB 安装与使用](http://www.runoob.com/mongodb/mongodb-intro.html)
* [4.MongoDB on Laravel Homestead with PHP 7(主要参考这个)](https://silviu-s.com/mongodb-on-laravel-homestead-with-php-7/)

* 5.其他的一些信息
	* [1.Install MongoDB Enterprise on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-ubuntu/)
	* [2.SQL 到 Mongo 的对应表 ](http://php.net/manual/zh/mongo.sqltomongo.php)
	* [3.MongoDB driver ¶](http://php.net/manual/zh/set.mongodb.php)
	* [4.Using the PHP Library for MongoDB (PHPLIB) ¶](http://php.net/manual/en/mongodb.tutorial.library.php#mongodb.tutorial.library)
	* [5.MongoDB PHP tutorial](http://zetcode.com/db/mongodbphp/)
	* [6.How to Install and Secure MongoDB 3.6 on Ubuntu 17.10](https://medium.com/@matthagemann/how-to-install-mongodb-3-6-on-ubuntu-17-10-ac0bc225e648)




#### 2.在 `Homestead.yaml` 中安装 `mongodb` :
* 1.在 `Homestead.yaml` 中修改文件的配置,如:
	
```
ip: 192.168.10.10
memory: 2048
cpus: 1
provider: virtualbox
authorize: ~/.ssh/id_rsa.pub
keys:
    - ~/.ssh/id_rsa
folders:
    -
        map: /Users/glb_gz/Documents/php_workplace/Test
        to: /home/vagrant/code
sites:
    -
        map: homestead.test
        to: /home/vagrant/code/public

databases:
    - homestead

name: test
hostname: test
mongodb : true
```


***

#### 3.SSH 进入 `vagrant ` 后，执行下面的操作,就可以顺利安装好 `MongoDB` 了:
```
$ ssh vagrant@192.168.10.10
vagrant@test:/usr/local$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
Executing: /tmp/apt-key-gpghome.raLrEy2Bnr/gpg.1.sh --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
gpg: key 5F8F93707F0CEB10: public key "Totally Legit Signing Key <mallory@example.org>" imported
gpg: key 9ECBEC467F0CEB10: 1 signature not checked due to a missing key
gpg: key 9ECBEC467F0CEB10: public key "Richard Kreuter <richard@10gen.com>" imported
gpg: Total number processed: 2
gpg:               imported: 2

vagrant@test:/usr/local$ echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list
deb http://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/3.0 multiverse
vagrant@test:/usr/local$ sudo apt-get update
Hit:1 https://deb.nodesource.com/node_8.x bionic InRelease
vagrant@test:/usr/local$ sudo apt-get install -y mongodb-org
Reading package lists... Done
Building dependency tree       
Reading state information... Done
mongodb-org is already the newest version (3.6.9).
0 upgraded, 0 newly installed, 0 to remove and 221 not upgraded.
vagrant@test:/usr/local$ sudo pecl install mongodb
WARNING: channel "pecl.php.net" has updated its protocols, use "pecl channel-update pecl.php.net" to update
downloading mongodb-1.5.3.tgz ...
...
Build complete.
Don't forget to run 'make test'.

running: make INSTALL_ROOT="/tmp/pear/temp/pear-build-rootLH757y/install-mongodb-1.5.3" install
Installing shared extensions:     /tmp/pear/temp/pear-build-rootLH757y/install-mongodb-1.5.3/usr/lib/php/20170718/
running: find "/tmp/pear/temp/pear-build-rootLH757y/install-mongodb-1.5.3" | xargs ls -dils
539331    4 drwxr-xr-x 3 root root    4096 Nov 16 07:00 /tmp/pear/temp/pear-build-rootLH757y/install-mongodb-1.5.3
539739    4 drwxr-xr-x 3 root root    4096 Nov 16 07:00 /tmp/pear/temp/pear-build-rootLH757y/install-mongodb-1.5.3/usr
539740    4 drwxr-xr-x 3 root root    4096 Nov 16 07:00 /tmp/pear/temp/pear-build-rootLH757y/install-mongodb-1.5.3/usr/lib
539741    4 drwxr-xr-x 3 root root    4096 Nov 16 07:00 /tmp/pear/temp/pear-build-rootLH757y/install-mongodb-1.5.3/usr/lib/php
539742    4 drwxr-xr-x 2 root root    4096 Nov 16 07:00 /tmp/pear/temp/pear-build-rootLH757y/install-mongodb-1.5.3/usr/lib/php/20170718
539738 5796 -rwxr-xr-x 1 root root 5934296 Nov 16 07:00 /tmp/pear/temp/pear-build-rootLH757y/install-mongodb-1.5.3/usr/lib/php/20170718/mongodb.so

Build process completed successfully
Installing '/usr/lib/php/20170718/mongodb.so'
install ok: channel://pecl.php.net/mongodb-1.5.3
configuration option "php_ini" is not set to php.ini location
You should add "extension=mongodb.so" to php.ini
Segmentation fault
#然后在 php.ini 中写入 extension = mongodb.so
vagrant@test:/usr/local$ sudo find / -name 'php.ini'
/etc/php/7.1/fpm/php.ini
/etc/php/7.1/cli/php.ini
/etc/php/5.6/fpm/php.ini
/etc/php/5.6/cli/php.ini
/etc/php/7.3/fpm/php.ini
/etc/php/7.3/cli/php.ini
/etc/php/7.2/fpm/php.ini
/etc/php/7.2/cli/php.ini
/etc/php/7.0/fpm/php.ini
/etc/php/7.0/cli/php.ini
vagrant@test:/usr/local$ php -v
PHP 7.2.12-1+ubuntu18.04.1+deb.sury.org+1 (cli) (built: Nov 12 2018 09:55:44) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.12-1+ubuntu18.04.1+deb.sury.org+1, Copyright (c) 1999-2018, by Zend Technologies
    with blackfire v1.22.0~linux-x64-non_zts72, https://blackfire.io, by Blackfire
vagrant@test:/usr/local$ sudo vi /etc/php/7.2/fpm/php.ini
vagrant@test:/usr/local$ sudo vi /etc/php/7.2/cli/php.ini
vagrant@test:/usr/local$ sudo service php7.2-fpm restart && sudo service nginx restart

```




## 6.相关的问题<a name="related_issues"/>

* 1.在 Mac 上运行 `$ composer require mongodb/mongodb` ,出现以下问题:

```
  Problem 1
    - mongodb/mongodb 1.4.2 requires ext-mongodb ^1.5.0 -> the requested PHP extension mongodb is missing from your system.
    - mongodb/mongodb 1.4.1 requires ext-mongodb ^1.5.0 -> the requested PHP extension mongodb is missing from your system.
    - mongodb/mongodb 1.4.0 requires ext-mongodb ^1.5.0 -> the requested PHP extension mongodb is missing from your system.
    - Installation request for mongodb/mongodb ^1.4 -> satisfiable by mongodb/mongodb[1.4.0, 1.4.1, 1.4.2].
```

* 1.1 解决方法 [Composer can't find mongodb extension](https://github.com/jenssegers/laravel-mongodb/issues/797),运行 `$ composer require jenssegers/mongodb --ignore-platform-reqs`