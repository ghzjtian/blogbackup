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
* 3.[Crontab Operation not permitted](https://apple.stackexchange.com/questions/378553/crontab-operation-not-permitted)

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
#!/bin/bash

# start_date_time="Xamarin============= Cleaning path(~/Library/Caches/Xamarin/mtbs) task start.(`date "+%Y-%m-%d %H:%M:%S"`)";
# echo $start_date_time >> ./log.txt
# ls -lh ~/Library/Caches/Xamarin/mtbs >> ./log.txt

rm -rf ~/Library/Caches/Xamarin/mtbs/builds
# ls -lh ~/Library/Caches/Xamarin/mtbs >> ./log.txt
finish_date_time="Xamarin============= Cleaning path(~/Library/Caches/Xamarin/mtbs) task finish.(`date "+%Y-%m-%d %H:%M:%S"`)";
echo $finish_date_time >> ./log.txt



# app_task_start_message="APP============= Cleaning path(~/Desktop/MowerGG-Installation-packages) task start.(`date "+%Y-%m-%d %H:%M:%S"`)";
# echo $app_task_start_message >> ./log.txt
# [How to Delete Files Older than 30 days in Linux](https://tecadmin.net/delete-files-older-x-days/)
find ~/Desktop/MowerGG-Installation-packages -name "*.zip" -type df -mtime +1 -exec rm -Rf {} \;
# [Bash Script OSX not deleting files it finds](https://stackoverflow.com/questions/9689124/bash-script-osx-not-deleting-files-it-finds)
app_task_finish_message="APP============= Cleaning path(~/Desktop/MowerGG-Installation-packages) task finish.(`date "+%Y-%m-%d %H:%M:%S"`)";
echo $app_task_finish_message >> ./log.txt



# a3s_frontend_task_start_message="a3s-frontend============= Cleaning path(~/Desktop/A3S-Frontend-Installation-packages) task start.(`date "+%Y-%m-%d %H:%M:%S"`)";
# echo $a3s_frontend_task_start_message >> ./log.txt
find /Users/globle/Desktop/A3S-Frontend-Installation-packages -name "*.zip" -type df -mtime +1 -exec rm -Rf {} \;
a3s_frontend_task_finish_message="a3s-frontend============= Cleaning path(~/Desktop/A3S-Frontend-Installation-packages) task finish.(`date "+%Y-%m-%d %H:%M:%S"`)";
echo $a3s_frontend_task_finish_message >> ./log.txt

echo "-" >> ./log.txt

```

* 3.设置 `auto-delete-task.sh` 文件的权限为 `755`

```
chmod 755 ./auto-delete-task.sh
```

* 4.删除指定日期的文件, 
	* 1.[delete all files in Downloads whose dates added are greater than 1 year](https://apple.stackexchange.com/a/236001)
	* 2.[Auto delete files older than 7 days](https://askubuntu.com/questions/789602/auto-delete-files-older-than-7-days)

```
// 删除 test 目录中超过 60 天的目录.
find /Users/USER/Documents/windows_Workplace/test -type d -ctime +60 -exec mv {} ~/.Trash \;
```

#### 2.添加一个定时任务

> 添加了定时任务后， `terminal` 每次操作都会有 mail 提示，这个有点麻烦.

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

#### 3.修改 cron 的权限

###### 1.经过以上的步骤，你会发现手动执行脚本可以删除指定的文件，但是用自动化的 cron 执行脚本就不会有效果，查看 `/private/var/mail/user` 文件，可以看到 ls 与 find 命令都没有权限

```
 ls: : Operation not permitted
```

###### 2.为 cron 添加 `Full Disk Access` 的权限
* 1.在 Terminal 中执行以下命令，查看 cron 的路径

```
 % which cron
/usr/sbin/cron
``` 

* 2.在 `System preferences.app > Security & Privacy > Privacy > Full Disk Access` 中，添加 cron 即可.

