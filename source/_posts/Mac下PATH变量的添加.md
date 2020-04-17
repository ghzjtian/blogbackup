---
title: Mac 下 PATH 变量的添加
tags: [Mac]
categories: [Mac]
date: 2018-06-1 17:46:19
---



### Mac 下 PATH 变量的添加

* 记录下在终端环境下，添加 其它程序的 命令到 terminal 中，实现在任何地方都可以立刻执行程序的命令.

* 参考1: [https://stackoverflow.com/questions/25373188/laravel-installation-how-to-place-the-composer-vendor-bin-directory-in-your](https://stackoverflow.com/questions/25373188/laravel-installation-how-to-place-the-composer-vendor-bin-directory-in-your)
* 参考2: [https://stackoverflow.com/questions/25373188/laravel-installation-how-to-place-the-composer-vendor-bin-directory-in-your](https://stackoverflow.com/questions/25373188/laravel-installation-how-to-place-the-composer-vendor-bin-directory-in-your)


<!-- more -->

#### 1.没添加环境变量前后对比:

* 1.添加前
```
tianzeng$ laravel
-bash: laravel: command not found
```
* 2.添加后:

```
$ laravel
Laravel Installer 2.0.1

Usage:
  command [options] [arguments]

Options:
  -h, --help            Display this help message
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  help  Displays help for a command
  list  Lists commands
  new   Create a new Laravel application.
tianzeng$ 

```

### 2.添加的方式
* 1.如添加 laravel:

```
echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bashrc
source ~/.bashrc
```
* 2.如果是以 ~/.bash_profile 去执行环境变量的话

```
echo 'export PATH="$PATH:$HOME/.composer/vendor/bin"' >> ~/.bash_profile
```

### 3.查看效果
>从新打开一个 terminal 即可.







