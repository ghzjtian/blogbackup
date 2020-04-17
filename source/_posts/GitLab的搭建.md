---
title: GitLab 的搭建
tags: [Linux]
categories: [Linux]
date: 2017-12-19 17:46:19
---



# GitLab 服务器的搭建

* 记录下公司的 GitLab 服务器的搭建过程。

<!-- more -->

## [1.GitLab 参考](#gitlab_references)
## [2.GitLab 存储相关](#gitlab_store)
## [3.GitLab 配置](#gitlab_config)
## [4.GitLab 错误的解决](#gitlab_issues)

***

## 1.GitLab 参考<a name="gitlab_references"/>
* 参考1: https://about.gitlab.com/installation/#debian
* 参考2: https://github.com/gitlabhq
* 3: http://www.jianshu.com/p/b26affeffc18
* 4.快速安装 GitLab 并汉化:  http://www.jianshu.com/p/7a0d6917e009

***

## 2.GitLab 存储相关<a name="gitlab_store"/>

* 1.项目文件的位置
	* [参考1.Repository storage paths](https://docs.gitlab.com/ee/administration/repository_storage_paths.html)
* 2.项目的路径在 `/var/opt/gitlab/git-data` 中(项目->个人->个人的项目).

```
user@glb:/home/tian# cd /var/opt/gitlab/git-data
user@glb:/var/opt/gitlab/git-data# ls
repositories
user@glb:/var/opt/gitlab/git-data# cd repositories
user@glb:/var/opt/gitlab/git-data/repositories# ls
Kyrie  Tian_test  Tim  yonghui.ye
user@glb:/var/opt/gitlab/git-data/repositories# cd Tim 
user@glb:/var/opt/gitlab/git-data/repositories/Tim# ls
Android_GreenGuide.git	       GoogleMapDev.git
Android_GreenGuide.wiki.git    GoogleMapDev.wiki.git
Cramer.git		       GreenGuide_Translate.git
Cramer.wiki.git		       GreenGuide_Translate.wiki.git
EmployeePackage.git	       InternalProject.git
EmployeePackage.wiki.git       InternalProject.wiki.git
GLB_BLE_Protocol.git	       newProject.git
GLB_BLE_Protocol.wiki.git      newProject.wiki.git
GLB_BLE_TestApp2.git	       PrivateProject.git
GLB_BLE_TestApp2.wiki.git      PrivateProject.wiki.git
GLB_BLE_TEST_APP.git	       TestProject.git
GLB_BLE_TEST_APP_iOS.git       TestProject.wiki.git
GLB_BLE_TEST_APP_iOS.wiki.git  ZTR_iOS.git
GLB_BLE_TEST_APP.wiki.git      ZTR_iOS.wiki.git
user@glb:/var/opt/gitlab/git-data/repositories/Tim# 
```

* 3.但是在项目中保存的文件应该是经过编译了的，实际的项目的文件找不到.

```
user@glb:/var/opt/gitlab/git-data/repositories/Tim# cd Cramer.git
user@glb:/var/opt/gitlab/git-data/repositories/Tim/Cramer.git# ls
config	description  HEAD  hooks  info	objects  refs
user@glb:/var/opt/gitlab/git-data/repositories/Tim/Cramer.git# cd objects
user@glb:/var/opt/gitlab/git-data/repositories/Tim/Cramer.git/objects# ls
01  0c	17  24	34  3f	4d  52	76  80	86  9e	b1  c3	cc  e0	info
02  0d	1a  2c	39  45	4e  57	79  81	8a  a0	b9  c8	ce  e2	pack
08  0e	1b  2d	3b  48	4f  6c	7b  83	8c  a1	bb  c9	d0  e7
09  0f	1c  2e	3c  4b	50  72	7e  84	9b  a6	bd  ca	d1  ec
0b  13	1f  2f	3e  4c	51  75	7f  85	9d  aa	c2  cb	d4  fa
user@glb:/var/opt/gitlab/git-data/repositories/Tim/Cramer.git/objects# du -h --max-depth=1
53M	./pack
8.0K	./39
8.0K	./57
20K	./75
8.0K	./2c
8.0K	./3f
12K	./ca
8.0K	./0b
8.0K	./3c
16K	./01
16K	./79
8.0K	./34
8.0K	./d0
8.0K	./b9
8.0K	./7e
8.0K	./76
8.0K	./17
8.0K	./4f
8.0K	./ec
8.0K	./45
8.0K	./80
8.0K	./e7
8.0K	./fa
8.0K	./09
8.0K	./7f
8.0K	./a1
8.0K	./86
8.0K	./1f
8.0K	./4d
8.0K	./7b
8.0K	./9d
8.0K	./bd
8.0K	./c9
8.0K	./1b
8.0K	./a0
8.0K	./d1
16K	./e0
8.0K	./24
8.0K	./cc
8.0K	./1c
8.0K	./0e
16K	./0f
12K	./cb
8.0K	./13
8.0K	./52
8.0K	./08
8.0K	./4c
8.0K	./02
20K	./8c
12K	./9e
8.0K	./a6
8.0K	./4e
8.0K	./6c
8.0K	./aa
12K	./2f
8.0K	./81
8.0K	./3b
8.0K	./84
8.0K	./c2
8.0K	./info
8.0K	./51
8.0K	./bb
12K	./0c
20K	./0d
8.0K	./4b
8.0K	./8a
8.0K	./9b
12K	./83
12K	./b1
12K	./3e
8.0K	./c3
12K	./e2
8.0K	./2d
8.0K	./d4
8.0K	./ce
8.0K	./2e
8.0K	./1a
8.0K	./c8
8.0K	./85
8.0K	./50
12K	./48
8.0K	./72
54M	.
```

***

## 3.GitLab 的配置<a name="gitlab_config"/>

* 1.GitLab 安装完成后，出现的提示信息.

```
GitLab was unable to detect a valid hostname for your instance.
Please configure a URL for your GitLab instance by setting `external_url`
configuration in /etc/gitlab/gitlab.rb file.
Then, you can start your GitLab instance by running the following command:
  sudo gitlab-ctl reconfigure

For a comprehensive list of configuration options please see the Omnibus GitLab readme
https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md


```

***

## 4.GitLab 错误的解决<a name="gitlab_issues"/>
### 1.出现 `Whoops, GitLab is taking too much time to respond.`,`Whoops, something went wrong on our end.` 错误:
<img src="/assets/imgs/git/screen_shot_gitlab.png" width="50%" height="50%">
<img src="/assets/imgs/git/screen_shot_gitlab2.png" width="50%" height="50%">

### 1.1.解决方法记录:
* 1.看到 `var` 目录满了，可能是这个原因:

```
# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       8.2G  3.1G  4.7G  40% /
udev             10M     0   10M   0% /dev
tmpfs           1.6G   49M  1.6G   4% /run
tmpfs           3.9G     0  3.9G   0% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
/dev/sda5       2.7G  2.6G     0 100% /var
/dev/sda7       360M  5.3M  332M   2% /tmp
/dev/sda8       432G  698M  409G   1% /home

# du -h --max-depth=1 /var
16K	/var/lost+found
4.0K	/var/local
8.0K	/var/www
8.0K	/var/tmp
40K	/var/mail
6.0M	/var/backups
820K	/var/spool
187M	/var/lib
1.4G	/var/cache
899M	/var/opt
84M	/var/log
2.6G	/var
```
* 2.所以把 `var/cache`和 `var/opt` 目录移到 `home` 目录中,建立软链接.

```
$ mv /var/cache /home/
$ ln -s /home/cache /var/cache
$ mv /var/opt /home/
$ ln -s /home/opt /var/opt
```

* 3.配置 `/etc/gitlab/gitlab.rb` 文件,然后运行 `sudo gitlab-ctl reconfigure`,参考 [Unicorn settings](https://docs.gitlab.com/omnibus/settings/unicorn.html),[502-Whoops, GitLab is taking too much time to respond](https://gitlab.com/gitlab-org/gitlab-ce/issues/30095)
```
unicorn['worker_processes'] = 3
unicorn['worker_timeout'] = 60
```
* 4.重新启动服务器，问题就解决了.(重启 gitlab 也无效 !)



