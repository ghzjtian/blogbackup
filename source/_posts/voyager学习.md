---
title: voyager学习
tags: [php]
categories: [php]
date: 2018-08-02 17:46:19
---



# voyager学习
>1.laravel 中 voyager 框架的学习和使用.
>
>2.voyager 是一个后台管理系统，功能强大.
>

<!-- more -->

## [1.voyager 相关的资料](#voyager_references)

## [2.voyager 相关的功能记录](#some_functions)

## [3.voyager 的扩展](#extend_voyager)

## [4.voyager 自动化](#auto_make_bread)

## [5.voyager 的部署](#voyager_deploy)

***
***
***

## 1.voyager 相关的资料<a name="voyager_references"/>
* [1.voyager 开发文档](https://voyager.readme.io/docs)
* [2.GitHub 地址](https://github.com/the-control-group/voyager)
* [3.voyager 官网](https://laravelvoyager.com/)


***

## 2.voyager 相关的功能记录<a name="some_functions"/>

#### 2.1作为一个 APP 的版本管理后台.
* 1.进入管理台的 `settings` 页后，可以增加一个特别的功能,就是返回 APP 的版本信息,如:

<img src="/assets/imgs/laravel/ScreenShot_2018-08-02_22.56.43.png" width="50%" height="50%">
 
 	* 1.相关的 `Route` 的代码，这样就能通过浏览器访问，返回一个 json 格式的 APP 版本信息:

```
Route::any('app_version', function () {
    return response(setting('site.app_version_code'));
});
```

<img src="/assets/imgs/laravel/ScreenShot_2018-08-02_23.00.39.png" width="50%" height="50%">
* 2.APP 端根据 [下载安装APK(兼容Android7.0)](https://www.jianshu.com/p/577816c3ce93) 去配置下载功能

* 3.APP 放在 fir.im 中，根据 [教程 去获取下载链接](https://fir.im/docs/install)

* 4.用 `com.loopj.android:android-async-http` 去做 APP 端的网络请求库.

***

## 3.voyager 的扩展<a name="extend_voyager"/>
* 1.pvtl/voyager-frontend
	* 1.用 Laravel 做底层, voyager 做 后台 的前台系统.
	* [2.GitHub 源码](https://github.com/pvtl/voyager-frontend)


***

## 4.voyager 自动化<a name="auto_make_bread"/>

#### 1.生成 BREAD 的数据.
* 1.参考 voyager 的 post 的 `BREAD` 的生成的方式(seeds/PostsTableSeeder).
* 2.`About` 的 `BREAD` 的生成例子:

```
<?php

use Illuminate\Database\Seeder;
use TCG\Voyager\Models\DataRow;
use TCG\Voyager\Models\DataType;
use TCG\Voyager\Models\Menu;
use TCG\Voyager\Models\MenuItem;
use TCG\Voyager\Models\Permission;
use App\About;

class AboutsTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {

        //Data Type,数据类型
        $dataType = $this->dataType('slug', 'abouts');
        if (!$dataType->exists) {
            $dataType->fill([
                'name'                  => 'abouts',
                'display_name_singular' => '关于我们',
                'display_name_plural'   => '关于我们',
                'icon'                  => 'voyager-people',
                'model_name'            => 'App\\About',
                'policy_name'           => '',
                'controller'            => '',
                'generate_permissions'  => 1,
                'description'           => '关于我们 页的数据设置',
            ])->save();
        }

        //Data Rows,每个字段的数据格式.
        $postDataType = DataType::where('slug', 'abouts')->firstOrFail();
        $dataRow = $this->dataRow($postDataType, 'id');
        if (!$dataRow->exists) {
            $dataRow->fill([
                'type'         => 'number',
                'display_name' => '自增 ID',
                'required'     => 1,
                'browse'       => 0,
                'read'         => 0,
                'edit'         => 0,
                'add'          => 0,
                'delete'       => 0,
                'details'      => json_encode([
                    'description' =>'自增 ID 的值.',
                ]),
                'order'        => 1,
            ])->save();
        }
        $dataRow = $this->dataRow($postDataType, 'icon');
        if (!$dataRow->exists) {
            $dataRow->fill([
                'type'         => 'text',
                'display_name' => '图标,如(fa fa-address-book)',
                'required'     => 1,
                'browse'       => 1,
                'read'         => 1,
                'edit'         => 1,
                'add'          => 1,
                'delete'       => 1,
                'details'      => json_encode([
                    'description' =>'复制 https://www.w3schools.com/icons/fontawesome_icons_webapp.asp 中对应的 Description 的值到这里即可',
                ]),
                'order'        => 2,
            ])->save();
        }
        $dataRow = $this->dataRow($postDataType, 'title');
        if (!$dataRow->exists) {
            $dataRow->fill([
                'type'         => 'text',
                'display_name' => '标题',
                'required'     => 1,
                'browse'       => 1,
                'read'         => 1,
                'edit'         => 1,
                'add'          => 1,
                'delete'       => 1,
                'details'      => '',
                'order'        => 3,
            ])->save();
        }
        $dataRow = $this->dataRow($postDataType, 'ability');
        if (!$dataRow->exists) {
            $dataRow->fill([
                'type'         => 'number',
                'display_name' => '能力值(0-100)',
                'required'     => 1,
                'browse'       => 1,
                'read'         => 1,
                'edit'         => 1,
                'add'          => 1,
                'delete'       => 1,
                'details'      => json_encode([
                    'description' => '范围为: 0 - 100 .',
                ]),
                'order'        => 4,
            ])->save();
        }
        $dataRow = $this->dataRow($postDataType, 'show');
        if (!$dataRow->exists) {
            $dataRow->fill([
                'type'         => 'radio_btn',
                'display_name' => '是否显示',
                'required'     => 1,
                'browse'       => 1,
                'read'         => 1,
                'edit'         => 1,
                'add'          => 1,
                'delete'       => 1,
                'details'      => json_encode([
                    'default' => '1',
                    'options' => [
                                '0'=>'否',
                                '1' =>'是',
                    ],
                ]),
                'order'        => 5,
            ])->save();
        }
        $dataRow = $this->dataRow($postDataType, 'created_at');
        if (!$dataRow->exists) {
            $dataRow->fill([
                'type'         => 'timestamp',
                'display_name' => __('voyager::seeders.data_rows.created_at'),
                'required'     => 0,
                'browse'       => 1,
                'read'         => 1,
                'edit'         => 0,
                'add'          => 0,
                'delete'       => 0,
                'details'      => '',
                'order'        => 6,
            ])->save();
        }

        $dataRow = $this->dataRow($postDataType, 'updated_at');
        if (!$dataRow->exists) {
            $dataRow->fill([
                'type'         => 'timestamp',
                'display_name' => __('voyager::seeders.data_rows.updated_at'),
                'required'     => 0,
                'browse'       => 0,
                'read'         => 0,
                'edit'         => 0,
                'add'          => 0,
                'delete'       => 0,
                'details'      => '',
                'order'        => 7,
            ])->save();
        }


        //Menu Item,在菜单项中添加 关于我们的栏目.
        $menu = Menu::where('name', 'admin')->firstOrFail();
        $menuItem = MenuItem::firstOrNew([
            'menu_id' => $menu->id,
            'title'   => '关于我们',
            'url'     => '',
            'route'   => 'voyager.abouts.index',
        ]);
        if (!$menuItem->exists) {
            $menuItem->fill([
                'target'     => '_self',
                'icon_class' => 'voyager-people',
                'color'      => null,
                'parent_id'  => null,
                'order'      => 20,
            ])->save();
        }
        //Permissions
        Permission::generateFor('abouts');

        //新增测试的数据.
        factory(About::class,5)->create();
    }

    /**
     * [post description].
     *
     * @param [type] $slug [description]
     *
     * @return [type] [description]
     */
    protected function findPost($slug)
    {
        return About::firstOrNew(['slug' => $slug]);
    }

    /**
     * [dataRow description].
     *
     * @param [type] $type  [description]
     * @param [type] $field [description]
     *
     * @return [type] [description]
     */
    protected function dataRow($type, $field)
    {
        return DataRow::firstOrNew([
            'data_type_id' => $type->id,
            'field'        => $field,
        ]);
    }

    /**
     * [dataType description].
     *
     * @param [type] $field [description]
     * @param [type] $for   [description]
     *
     * @return [type] [description]
     */
    protected function dataType($field, $for)
    {
        return DataType::firstOrNew([$field => $for]);
    }

}

```


***

## 5.voyager 的部署<a name="voyager_deploy"/>
* 1.部署到 `homestead` 的步骤.
	* 1.运行 `php artisan migrate` ,去把数据库先添加了.
	* 2.运行 `php artisan voyager:install --with-dummy` ,安装必要的 voyager 的信息(如 add user,add BREAD).
	* 3.运行 `composer dump-autoload`,`php artisan db:seed --class=SettingsTableGLBSeeder` ,把 `Settings` 的模拟数据添加进去.











