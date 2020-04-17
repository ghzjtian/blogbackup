---
title: Laravel学习
tags: [php]
categories: [php]
date: 2018-06-03 17:46:19
---



# Laravel学习
>1.记录下对 PHP 框架 Laravel 的学习。


<!-- more -->

### [1.基本的 Laravel 命令](#basic_command)
* 1.Laravel 5.6 开发文档
* 2.相关的 Valet 的命令
* 3.Homestead 的相关的命令


### [2.Voyager 后台框架的学习](#voyager_learn)
> Voyager 是在 Laravel 的基础上的后台框架.

### [3.Composer 本地路径加载第三方扩展包](https://laravel-china.org/topics/1999/composer-local-path-loading-third-party-extension-pack)
* [1.正确的 Composer 扩展包安装方法](https://laravel-china.org/topics/1901/correct-method-for-installing-composer-expansion-pack)

### [4.数据的模拟插入](#data_insert)

### [5.数据库的迁移](#database_migrate)

### [6.相关的 cache 清除](#cache_clear)

### [7.邮件发送功能](#email_send)

***
***
***

### 1.基本的 Laravel 命令<a name="basic_command"/>

#### 1.Laravel 5.6 开发文档
	* [1.中文](https://laravel-china.org/docs/laravel/5.6/installation/1352)
	* [2.英文](https://laravel.com/docs/5.6)


#### 2.相关的 Valet 的命令,[参考自:laravel.com](https://laravel.com/docs/5.6/valet):

	* 1.新建一个 laravel 的项目
	
		`
		laravel new project
		`

	* 2.valet 的启动/停止 等等:
	
		`
		valet start
		valet stop
		`
	
	* 3.mysql 的启动/停止
	
		`
		brew services start mysql
		brew services stop mysql
		`

#### [3.Homestead 的相关的命令](https://laravel-china.org/docs/laravel/5.6/homestead/1355#a29c5a)

* 1.`vagrant up` 启动 Homestead
* 2.`vagrant -h` 启动帮助页面.
* 3.[解决 `No input file specified` 的问题](https://laracasts.com/discuss/channels/laravel/how-can-access-my-laravel-project-from-my-homestead?page=1#)
* 4.做 mysql 数据库的  migrate 时， `.env` 文件的配置为:
	
```java
DB_CONNECTION=mysql
DB_HOST=192.168.10.10
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```


* [5.PhpStorm+Homestead+Xdebug调试Laravel](https://juejin.im/post/5a614681518825732646de42)
	* 1.在 Chrome 上，要那图标变为绿色才表示监听中

	 <img src="/assets/imgs/laravel/ScreenShot_2018-06-11_11.05.11.png" >

* 6.本机的 `Sequel Pro` 连接 Homestead 数据库的配置

	<img src="/assets/imgs/laravel/ScreenShot_2018-06-09_21.27.51.png" width="70%" height="70%">
	
* 7.Tinker 的使用
		* 1.[使用 Tinker 来调试 Laravel 应用程序的数据以及使用 Tinker 一些总结](https://laravel-china.org/articles/8575/debugging-laravel-application-data-with-tinker-and-using-some-tinker-summaries)
		
	```php
	#在终端中打开 tingker
	php artisan tinker
	#在数据库中生成一个 User 的数据
	factory(App\User::class)->create();
	#查看 User 表的所有数据.
	App\User::all();
	```

* 8.[在 Homestead 中加入 phpmyadmin ,How to setup phpmyadmin on a Laravel Homestead box?](https://stackoverflow.com/questions/23788096/how-to-setup-phpmyadmin-on-a-laravel-homestead-box/36640591)
		* 1.成功添加后，访问的地址为: [http://phpmyadmin.test:8000/](http://phpmyadmin.test:8000/)

* 9.如果之前项目的 `HomeStead` 开着没有关闭,又开另外一个项目的 `HomeStead` ,浏览器显示的还会是之前的项目的显示.

* 10.在 `voyager` 的后台主页显示 `Missing storage symlink` 的问题，点击了 `Fix it` 也没有反应.

	* 1.原因:是因为 `homestead` 把 Mac 上链接文件复制到了 linux 上,但双方的路径不一样所导致的.通过 `vagrant ssh` 链接到主机，然后查看就知道了:
		
		
		```
		vagrant@hooks:~/code/public$ ls -al
		total 12
		drwxr-xr-x 1 vagrant vagrant  320 Jun 20 02:28 .
		drwxr-xr-x 1 vagrant vagrant 1024 Jun 20 03:25 ..
		drwxr-xr-x 1 vagrant vagrant   96 Jun 20 02:09 css
		-rw-r--r-- 1 vagrant vagrant    0 Jun 20 02:09 favicon.ico
		-rw-r--r-- 1 vagrant vagrant  593 Jun 20 02:09 .htaccess
		-rw-r--r-- 1 vagrant vagrant 1823 Jun 20 02:09 index.php
		drwxr-xr-x 1 vagrant vagrant   96 Jun 20 02:09 js
		-rw-r--r-- 1 vagrant vagrant   24 Jun 20 02:09 robots.txt
		lrwxr-xr-x 1 vagrant vagrant   79 Jun 20 02:28 storage -> /Users/tianzeng/Documents/php_Workplace/GitHub/Laravel/hooks/storage/app/public
		drwxr-xr-x 1 vagrant vagrant   96 Jun 20 02:28 vendor
		```
		
* [2.解决方法:](https://github.com/ghzjtian/yoyager/tree/dev2#truely_path)
	* 1.导航到 远程主机的 code 目录下,`vagrant ssh` -> `cd code`	
	
	* 2.运行命令 `sudo ln -s /home/vagrant/code/storage/app/public /home/vagrant/code/public/storage`
	* 3.查看结果，已修改成功.
	
		```
		vagrant@hooks:~/code$ ls -al public 
		total 12
		drwxr-xr-x 1 vagrant vagrant  320 Jun 20 03:52 .
		drwxr-xr-x 1 vagrant vagrant 1024 Jun 20 03:25 ..
		drwxr-xr-x 1 vagrant vagrant   96 Jun 20 02:09 css
		-rw-r--r-- 1 vagrant vagrant    0 Jun 20 02:09 favicon.ico
		-rw-r--r-- 1 vagrant vagrant  593 Jun 20 02:09 .htaccess
		-rw-r--r-- 1 vagrant vagrant 1823 Jun 20 02:09 index.php
		drwxr-xr-x 1 vagrant vagrant   96 Jun 20 02:09 js
		-rw-r--r-- 1 vagrant vagrant   24 Jun 20 02:09 robots.txt
		lrwxr-xr-x 1 vagrant vagrant   37 Jun 20 03:52 storage -> /home/vagrant/code/storage/app/public
		drwxr-xr-x 1 vagrant vagrant   96 Jun 20 02:28 vendor
		vagrant@hooks:~/code$ 
		```

* 11.多站点的支持,只需在 `Homestead.yaml` 文件中增加如下的站点信息,并且在 `hosts` 文件中增加 `homestead2.test` 的信息,然后运行 `vagrant reload --provision` 重新加载即可.
	
```
	folders:
    -
        map: /Users/tianzeng/Documents/php_Workplace/FastCloud/FastCloud
        to: /home/vagrant/code

    - map: /Users/tianzeng/Documents/php_Workplace/testPHP
      to: /home/vagrant/code2

sites:
    -
        map: homestead.test
        to: /home/vagrant/code/public

    -   map: homestead2.test
        to: /home/vagrant/code2
```


		
***

### 2.Voyager 后台框架的学习<a name="voyager_learn"/>
* [1.Voyager v1.1 开发文档.](https://voyager.readme.io/docs/installation)


### 4.数据的模拟插入<a name="data_insert"/>
* 1.参考 [Seeders](https://laravel-china.org/docs/laravel/5.6/seeding/1401) , [Factory](https://laravel-china.org/docs/laravel/5.6/database-testing/1412#writing-factories)
* 2.相关的步骤:
	* 1.生成一个 `factory`,如: `php artisan make:factory SparkFactory --model=Spark` ,并填写相关的模拟数据的生成:
	
```
<?php

use Faker\Generator as Faker;

$factory->define(App\Spark::class, function (Faker $faker) {
return [
'name' => '产品名_'.str_random(10),
'pic' => 'http://file.mancando.cn/resource/images/product/10020001/201609091354591425789.jpg',
'number' => '产品编号_'.str_random(10),
'supplier_number' => '供应商编号_'.str_random(10),
'category_name' => '产品种类_'.str_random(10),
'brand_name' => '品牌名_'.str_random(10),
'package_size' => random_int(0,10),
'unit' => '支',
'status' => random_int(0,2),
'description' => '描述_'.str_random(20),
'sale_price' => random_int(10,20),
'purchase_price' => random_int(5,10),
'sales' => random_int(100,200),
'stock' => random_int(50,100),
];
});

```

* 2.生成一个对应的 `Seeder` ,如: `php artisan make:seeder SparksTableSeeder
` ,并写入生成模拟数据的逻辑(这里为 50 条模拟数据的生成):

```
 public function run()
    {

        factory(App\Spark::class,50)->create();

    }
```
* 3.运行 `composer dump-autoload` 命令，[否则会有 `Class XXXSeeder does not exist` 发生.](https://stackoverflow.com/questions/26143315/laravel-5-artisan-seed-reflectionexception-class-songstableseeder-does-not-e)
* 4.运行命令 `php artisan db:seed --class=SparksTableSeeder` ,数据就已经生成了.


***

### 5.数据库的迁移<a name="database_migrate"/>
* 1.参考: [数据库的迁移](https://laravel-china.org/docs/laravel/5.6/migrations/1400)
* 2.步骤:
	* 0.生成一个 Model,如 Setting: `php artisan make:Model Setting`.
	* 1.运行命令`php artisan make:migration create_new_sparks_table --create=my_sparks
` ,将会有文件 `database/migrations/2018_07_31_065118_create_new_sparks_table.php` 生成.
	* 2.往 `2018_07_31_065118_create_new_sparks_table.php `的 up 方法中填入数据库的字段数据, 如:

```
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateNewSparksTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('my_sparks', function (Blueprint $table) {
            $table->increments('id');

            $table -> string('name') -> nullable();
            $table -> string('pic')-> nullable();
            $table -> string('number')-> nullable();
            $table -> string('supplier_number')-> nullable();
            $table -> string('category_name')-> nullable();
            $table -> string('brand_name')-> nullable();
            $table -> unsignedInteger('package_size')-> nullable();
            $table -> string('unit')-> nullable();
            $table -> tinyInteger('status')-> nullable();
            $table -> text('description')-> nullable();
            $table -> unsignedInteger('sale_price')-> nullable();
            $table -> unsignedInteger('purchase_price')-> nullable();
            $table -> unsignedInteger('sales')-> nullable();
            $table -> unsignedInteger('stock')-> nullable();

            $table->nullabletimestamps();
            $table-> softDeletes()-> nullable();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('my_sparks');
    }
}


```

* 3.运行迁移 `php artisan migrate`. 即可生成指定字段的数据表.

* 4.想要在代码里删除刚刚新增的数据表(发现有错误发生!!!)
	* 1.修改文件名,如 把 `2018_07_31_065119_create_new_sparks_table.php` 改为 `2018_07_31_065120_create_new_sparks_table.php`,只有这样在运行 `php artisan migrate` 时才可以运行这个文件.
	* 2.在 up 方法中 写入
	
```
	$this->down();
        return;	
```

* 5.更新数据库，添加一个数据库的字段
	* 1.先运行 `php artisan make:migration update_products_table`,在 `migrations`生成一个文件。
	* 2.然后在 run 方法中添加所需要添加的字段，如
	```
	 Schema::table('products',function (Blueprint $table){
            $table->string('image_large')->nullable()->after('image');
        });
	```

	* 3.运行 `php artisan migrate` 即可.


***

## 6.相关的 cache 清除<a name="cache_clear"/>
* 1.清除 view 缓存. `php artisan view:clear`
	* 1.但是在 `PHP Storm` 中，用这条命令清除了后，一刷新 浏览器的网页，旧的页面又出来了.
	* 2.没有找到解决的方法!!!
		* [1.Option to disable cache](https://github.com/laravel/framework/issues/2501)
		* [2.How I can disable templates caching in development mode?
](https://stackoverflow.com/questions/16971445/how-i-can-disable-templates-caching-in-development-mode)
		* [3.Changing blade files never change when refreshing/clearing cache](https://laracasts.com/discuss/channels/laravel/changing-blade-files-never-change-when-refreshingclearing-cache?page=1)

***

### 7.邮件发送功能<a name="email_send"/>
##### 1.用 [mailtrap](https://mailtrap.io/inboxes/399408/messages/887456554) 去代发邮件:
* 1.参考(发现都没效果，从 mailtrap 后台可以看到有发送的 EMAIL 数据，但目标邮箱没收到任何的邮件):
	* [1.How To Send Email In Laravel Tutorial](https://appdividend.com/2018/03/05/send-email-in-laravel-tutorial/)
	* [2.Laravel - Sending Email](https://www.tutorialspoint.com/laravel/laravel_sending_email.htm)

##### 2.用 163 邮箱去做发送邮箱.(亲测有效)
* 1.用 163 普通的免费邮,进去网页版，然后按 设置->客户端授权密码(设置一个密码)-开启,就可以开启 laravel 客户端登录发送邮件了.
	* 2.发现用 163 企业邮找不到
* 2.在 `.env`中做如下的配置,[163免费邮客户端设置的POP3、SMTP、IMAP地址](http://help.163.com/09/1223/14/5R7P3QI100753VB8.html):

```
MAIL_DRIVER=smtp
MAIL_HOST=smtp.163.com
MAIL_PORT=994
MAIL_USERNAME=test@163.com
MAIL_PASSWORD=上一步的客户端授权码
MAIL_ENCRYPTION=ssl
MAIL_FROM_ADDRESS= test@163.com
MAIL_FROM_NAME=test
```

* 3.发送的实际代码参考:
	* [1.使用 Laravel 基于 SMTP 驱动实现发送邮件](https://www.jianshu.com/p/15ea81c9d781)
	* [2.How To Send Email In Laravel Tutorial](https://appdividend.com/2018/03/05/send-email-in-laravel-tutorial/)
	* [3.Laravel - Sending Email](https://www.tutorialspoint.com/laravel/laravel_sending_email.htm)
	* [4.利用Laravel自带SMTP邮件组件实现发送邮件](https://www.jianshu.com/p/8ccb2820df23)
	* [5.Laravel5.5 新特性~Markdown 邮件模板的显示](https://laravel-china.org/articles/5455/laravel55-new-feature-markdown-mail-template-display)







