---
title: laravel项目的部署
tags: [php]
categories: [php]
date: 2018-07-23 17:46:19
---



# laravel项目的部署

* 1.记录把服务器部署在 远程服务器的过程.

<!-- more -->


## [1.参考](#references)
## [2.安装过程](#install_step)
* 1.mysql 的安装(安装最新版)

## [3.遇到的问题](#issues)
* 1.`Specified key was too long error`错误
* 2.`public/index.php`主页显示没问题, 但 `login`,`register` 等等就显示 `404` 错误

## [4.项目的部署](#project_install)

***
***
***
	
## 1.参考:<a name="references"/>
* 0.服务器(配置支持中文)需要安装的软件(nginx+php+mysql+composer+git+PhpMyAdmin)
* 1.[LAMP环境下的Laravel项目部署](https://www.jianshu.com/p/6038191ef4fe)
* [2.请问laravel5项目部署到生产环境的最佳实践？](https://www.zhihu.com/question/35537084)
* [3.How to Install Laravel on Debian 9](https://www.rosehosting.com/blog/how-to-install-laravel-on-debian-9/)
* [4.A step by step guide to setup PHP (Laravel) environment (Linux).](https://hackernoon.com/a-step-by-step-guide-to-setup-php-laravel-environment-linux-50b55a4fd15e)
* [5.【GitLab】：Webhooks 实现自动化服务器项目部署](https://laravel-china.org/articles/5012/gitlab-webhooks-implements-automated-server-project-deployment)
* [6.Deploying a Laravel 5.6 Web App on Ubuntu](https://medium.com/@grmcameron/deploying-your-laravel-web-app-1faaa66f3302)
* [7.How To Install and Use Composer on Debian 8](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-composer-on-debian-8)
***

## 2.安装过程<a name="install_step"/>

#### 1.mysql 的安装(安装最新版).
* [1.How to Set Up MariaDB on Debian 9](https://www.linode.com/docs/databases/mariadb/mariadb-setup-debian/)
* [2.How to Install MariaDB 10 on Debian and Ubuntu](https://www.tecmint.com/install-mariadb-in-ubuntu-and-debian/)
* [3.uninstall MariaDB](https://www.londonappdeveloper.com/how-to-completely-uninstall-mariadb-from-a-debian-7-server/)
* [4.开启远程访问的权限](https://www.jianshu.com/p/8fc90e518e2c)
	* 1.在服务器端进入 mysql ,然后运行命令 `grant all privileges on *.* to 'root'@'%' identified by 'remove1112300';` , `flush privileges;`
	* 2.注释文件 ` vi /etc/mysql/my.cnf`  中的 `bind-address = 127.0.0.1`
		* 1.如果是 MariaDB, `bind-address` 在 `/etc/mysql/mariadb.conf.d/50-server.cnf`处
		* 2.参考: [Where is my bind address?](https://dba.stackexchange.com/questions/164929/where-is-my-bind-address/164932)
		* 3.记得提前查看 mysql 端口的状态是否在监听 3306,`netstat -tulpen`.
	* 3.重启服务. `service mysql restart`

#### 2.PHP 的安装.
* 0.参考: 
	*	1.[How-to Install PHP 7.2.x, NGINX 1.10.x & Laravel 5.6](https://medium.com/@asked_io/how-to-install-php-7-2-x-nginx-1-10-x-laravel-5-6-f9e30ee30eff)
	* 2.[Installing Php 7.2 On Debian 8 Jessie And Debian 9 Stretch](https://www.chris-shaw.com/blog/installing-php-7.2-on-debian-8-jessie-and-debian-9-stretch)
* 1.需要安装的 php 的插件有:
	* 1.`php7.0-zip`
	* 2.`php7.2-gd`
	* 3.`php7.2-fpm`


***

## 3.遇到的问题<a name="issues"/>
#### 1.服务器做 `php artisan migrate` 时，出现 `Specified key was too long error` 错误
* [解决方法1:Laravel 5.4: Specified key was too long error](https://laravel-news.com/laravel-5-4-key-too-long-error)
* [同样的方法2:](https://segmentfault.com/a/1190000008416200)
* 3.安装新版的 mariaDB(经测试，10.3.8-MariaDB 可以)




#### 2.`public/index.php`主页显示没问题, 但 `login`,`register` 等等就显示 `404` 错误
* [1.配置链接](https://laravel-china.org/docs/laravel/5.6/installation/1352)
	* 1.需要把 `/etc/nginx/sites-available/default` 中的 `root`路径设置到 `public` 中.	

#### 3.执行 `composer install ` 时，遇到 `composer the zip extension and unzip command are both missing skipping` 的问题
* [1.解决方法`(sudo) apt install zip unzip php7.0-zip`](https://stackoverflow.com/questions/41274829/php-error-the-zip-extension-and-unzip-command-are-both-missing-skipping)

#### 4.只能显示首页，其它页面显示 `404 ERROR`。
* [1.需要配置 nginx 的 `/etc/nginx/sites-available/default` 文件(先 cp 备份好)](https://laravel-china.org/docs/laravel/5.6/deployment/1357)
	* 1.配置完成后，重启 `nginx`.
		* 1.检查是否有错误,`nginx -t`.
		* 2.重启 `nginx`,`service nginx restart`

#### 5.在后台增加图片时，显示`Impossible to create the root directory "/home/tian/www/html/frontend/storage/app/public/users/August2018".`
* 1.在 项目的上级目录下，如 `/home/tian/www/html` , 运行一下命令: `chown -R www-data:root .`

#### 6.在运行 `composer install` 时，出现 `mmap() failed: [12] Cannot allocate memory` 错误
* 1.[PHP Composer update “cannot allocate memory” error ](https://stackoverflow.com/questions/18116261/php-composer-update-cannot-allocate-memory-error-using-laravel-4)
	* 解决的方法，运行 
	```
	/bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
	/sbin/mkswap /var/swap.1
	/sbin/swapon /var/swap.1
	```
	
	

***

## 4.项目的部署<a name="project_install"/>
* 1.把代码放到 `GitLab` 中(因为 `GitLab` 可以创建私人的仓库,而且日本服务器 `clone` 的速度比国内的 `Gitee` 快 n 倍)
* 2.把 `nginx` 的 `root` 路径改为当前 laravel 项目的 public 目录.
	* 1.`vi /etc/nginx/sites-available/default`
	* 2.修改 `root /var/www/html/FastCloud_Server/public/`
	* 3.重启 `nginx`(记得 nginx 要有 phpXX-fpm 的支持才行)
		* 1.`nginx -t`
		* 	2.`service nginx restart`
* 3.到项目路径下，执行 `composer install`,安装依赖.
	* 1.国内的服务器，最好是更改 `composer` 的源:
		* 1.[Packagist 镜像使用方法](https://pkg.phpcomposer.com/)
			* 1.运行 `composer config -g repo.packagist composer https://packagist.phpcomposer.com`
		* 2.[Composer 中文镜像 / Packagist 中国全量镜像正式发布！](https://laravel-china.org/composer)
			* 1.运行: `composer config -g repo.packagist composer https://packagist.laravel-china.org`
	* 2.如果是在本地 Mac 下运行出现 `In PDOConnection.php line 50:SQLSTATE[HY000] [2002] Connection refused ` ,就记得打开 Mac 上的 `Mysql`,否则后续的操作可能会出现问题.

* 4.部署配置
	* 1.生成 `.env` 文件	,命令: `cp .env.example.product .env`
	* 2.生成 `APP_KEY` ,命令: `php artisan key:generate`
	* 3.设置文件夹的权限:

		`chmod -R 777 storage`
		`chmod -R 777 bootstrap/cache`
	* 4.运行数据库的迁移:
		* 1.`php artisan migrate`
	* 5.安装 voyager
		* 1.安装`php artisan voyager:install --with-dummy`
	* 6.设置 `storage` 连接.
		* 0.到服务器项目目录运行: `php artisan storage:link`
		* 1.或手动链接: `ln -s /var/www/html/FastCloud_Server/storage/app/public /var/www/html/FastCloud_Server/public/storage` 
		
	* 7.[生成 JWT 认证的 KEY](http://jwt-auth.readthedocs.io/en/develop/laravel-installation/) ,`php artisan jwt:secret`
		* 1.如果不生成，在取得 `API` 数据的时候，会有 500 错误弹出.
	* 8.暂时如果服务器有更新的步骤:
		* 1.先在本地 push 到 GitLab
		* 2.ssh 登录到服务器，执行 `php artisan down (--message="Upgrading Database" --retry=60)`，命令，让服务器[进入 维护 模式](https://laravel-china.org/docs/laravel/5.6/configuration/1353).
		* 3.`git pull origin master` ,把源码下载下来.
		* 4.修改好一些依赖后，就执行 `php artisan up` 上线.

	* 9.数据的迁移及保存
		* 1.Mysql 数据库的内容就用 Sequel Pro 把数据导出或导入.
			* 1.一般需要导出的数据库为:`abouts,events,permission_role,permissions,pictures,products,users`
		* 2.其它的 `public` 中的数据就通过 [`scp`](https://unix.stackexchange.com/questions/188285/how-to-copy-a-file-from-a-remote-server-to-a-local-machine) 命令去导出和导入.
			* 1.下载,如在本地电脑运行以下的命令: `scp -r tian@10.100.0.1:/home/tian/www/html/frontend/storage/app/public /Users/tianzeng/Desktop`
			* 2.Logs 的下载,`scp -r tian@10.100.1.217:/home/tian/www/html/frontend/storage/logs/laravel.log /Users/tianzeng/Desktop`
			* 3.数据的上传,在本地电脑运行一下命令: `scp -r /Users/tianzeng/Desktop/public tian@10.100.1.217:/home/tian/www/html/GLB/storage/app`
			
			
			
***
***
***



