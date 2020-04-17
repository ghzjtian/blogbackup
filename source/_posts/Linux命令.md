---
title: Linux命令
tags: [Linux]
categories: [Linux]
date: 2018-11-02 17:46:19
---

## [1.Linux 相关命令](#linux_command)
### 1.1参考
### 1.2文件相关命令
### 1.3网络相关命令
#### 1.端口检测

<!-- more -->

***
***
***


## 1.Linux 相关命令<a name="linux_command"/>

### 1.1参考
* 1.[Linux中常用的命令都是哪些单词的缩写？](https://www.zhihu.com/question/49073893)

### 1.2文件相关命令
* 0.相关参考:
	* 1.参考1: [每天一个linux命令（33）：df 命令](http://www.cnblogs.com/peida/archive/2012/12/07/2806483.html)
	* 2.参考2: [Linux查看文件和文件夹大小](https://www.jianshu.com/p/1c22dcb17a2e)
* 1.列出文件系统的可用空间及使用情况(df -- Disk Free):`df -h`
	
```
# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       8.2G  3.1G  4.7G  40% /
udev             10M     0   10M   0% /dev
tmpfs           1.6G   49M  1.6G   4% /run
tmpfs           3.9G  4.0K  3.9G   1% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup
/dev/sda5       2.7G  291M  2.3G  12% /var
/dev/sda7       360M  5.3M  332M   2% /tmp
/dev/sda8       432G  3.0G  407G   1% /home
```

* 2.查看文件或文件夹的磁盘使用空间(du -- Disk Usage):`du -h --max-depth=1 your_dest_dir`

```

# du -h --max-depth=1 /var
16K	/var/lost+found
4.0K	/var/local
8.0K	/var/www
8.0K	/var/tmp
48K	/var/mail
6.0M	/var/backups
816K	/var/spool
187M	/var/lib
93M	/var/log
286M	/var
```

### 1.3网络相关命令
#### 1.端口检测
###### 0.相关参考
* 1.[linux 检测远程端口是否打开](http://www.cnblogs.com/onmyway20xx/p/3626433.html)
* 2.[检测远程主机上的某个端口是否开启](https://www.jianshu.com/p/0abf51f22e40)
* 3.[Mac安装telnet](https://www.jianshu.com/p/78c7f4ab19bd)
* 4.[mac os x 查看网络端口情况](https://my.oschina.net/foreverich/blog/402252)

###### 1.查看当前主机 tcp 开放了哪些端口
```
root@MyServer:~# netstat -anp tcp
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 62.42.41.223:9001       0.0.0.0:*               LISTEN      3816/ss-server      
tcp        0      0 62.42.41.223:9002       0.0.0.0:*               LISTEN      3887/ss-server      
tcp        0      0 62.42.41.223:9003       0.0.0.0:*               LISTEN      3893/ss-server      
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      18545/nginx: master 
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      4254/sshd           
tcp        0      0 62.42.41.223:22         218.92.1.131:56386      ESTABLISHED 4345/sshd: root [pr 
tcp        0      0 62.42.41.223:9002       218.19.136.78:27844     SYN_RECV    -                   
tcp        0      0 62.42.41.223:9003       218.19.136.78:27847     ESTABLISHED 3893/ss-server
```
###### 2.远程监测端口是否打开:

```

root@server3:~# nc -v 62.42.41.223 9002
Warning: forward host lookup failed for 62.42.41.223.vultr.com: No address associated with name
62.42.41.223.vultr.com [62.42.41.223] 9002 (?) open
^C
root@server3:~# nc -v 62.42.41.223 9005
Warning: forward host lookup failed for 62.42.41.223.vultr.com: No address associated with name
62.42.41.223.vultr.com [62.42.41.223] 9005 (?) : Connection refused

```

###### 3.苹果电脑自带软件

```
使用网络实用工具
网络实用工具是苹果自带的网络分析工具
10.8之前的位于 launchpad --> 其他--> 网络实用工具
10.9之后隐藏了该应用，但可以通过 spotlight 搜索 网络实用工具或者 最左上角的苹果标志 --> 关于本机 -->点按'系统报告' --> 标题栏的'窗口' --> 网络实用工具 --> 点按'端口扫描'
```
