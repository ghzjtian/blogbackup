---
title: Mac 创建定时任务
tags: [Mac]
categories: [Mac]
date: 2020-06-11 17:46:19
---


> 在 Mac 上用 crontab 创建简单的定时任务

<!-- more -->

## 1.参考
* 1.[Linux之crontab定时任务](https://www.jianshu.com/p/838db0269fd0)
* 2.[Scheduling Jobs With Crontab on macOS](https://medium.com/better-programming/https-medium-com-ratik96-scheduling-jobs-with-crontab-on-macos-add5a8b26c30)

## 2.创建一个每天自动删除文件的任务

> 每天凌晨 1 点自动删除指定目录下的文件夹

#### 1.创建定时任务脚本


* 1.在用户的主目录下创建 `auto-task-scripts` 文件夹， 

```
cd ~
mkdir auto-task-scripts
```

* 2.然后进入 `auto-task-scripts` , 新增 `auto-delete-task.sh` 及 `log.txt` 文件

```
cd auto-task-scripts

touch auto-delete-task.sh

touch log.txt


```

* 3.写入以下代码到 `auto-delete-task.sh` 中 

```
start_date_time="============= Cleaning task start.(`date "+%Y-%m-%d %H:%M:%S"`)";
echo $start_date_time >> ./log.txt
ls -lh ~/Desktop/auto-delete-test >> ./log.txt

# rm -rf need-delete-folder
rm -rf ~/Desktop/auto-delete-test/test-delete
rm -rf ~/Library/Caches/Xamarin/mtbs

ls -lh ~/Desktop/auto-delete-test >> ./log.txt
finish_date_time="============= Cleaning task finish.(`date "+%Y-%m-%d %H:%M:%S"`)";
echo $finish_date_time >> ./log.txt
echo " "
```

* 3.设置 `auto-delete-task.sh` 文件的权限为 `755`

```
chmod 755 ./auto-delete-task.sh
```

#### 2.添加一个定时任务

```
// 查看定时任务
$ crontab -l

// 编辑定时任务
$ crontab -e

// 每分钟都去执行 auto-delete-task.sh 脚本.
* * * * * cd ~/auto-task-scripts && ./auto-delete-task.sh

// 每天 (凌晨 01:01) 执行 auto-task-scripts 内的 auto-delete-task.sh 脚本.
01 01 * * * cd ~/auto-task-scripts && ./auto-delete-task.sh

```  
