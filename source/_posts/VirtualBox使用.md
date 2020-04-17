---
title: VirtualBox 软件的使用
tags: [虚拟机]
categories: [虚拟机]
date: 2019-06-4 17:46:19
---


> 介绍 Mac 中 VirtualBox 软件的使用

<!-- more -->
## [0.参考](#references)
## [1.VirtualBox 信息](#virtual_ifo)
## [2.新建一个虚拟机](#create_new_virtual_machine)
## [3.共享目录](#share_directory)
## [4.SSH 远程登录及 scp 文件传输](#ssh_login_scp_fileTransfer)


***
***
***

## 0.参考<a name="references"/>
* [参考1:How to SCP a file from Mac -> Ubuntu VirtualBox?](https://askubuntu.com/questions/48436/how-to-scp-a-file-from-mac-ubuntu-virtualbox)


## 1.VirtualBox 信息<a name="virtual_ifo"/>
* 1.[VirtualBox 官网](https://www.virtualbox.org/wiki/Downloads)

***

## 2.新建一个虚拟机<a name="create_new_virtual_machine"/>
> 以 Debian 为例.

#### 1.参考:[Installing Debian Linux in a VirtualBox Virtual Machine](http://www.brianlinkletter.com/installing-debian-linux-in-a-virtualbox-virtual-machine/)

#### 2.下载好一个 Debian 安装包
* 1.这里在 Debian 的 [官网](https://www.debian.org/distrib/) 下载.

#### 3.配置好安装包
* 1.在 Storage 中配置好安装包.

#### 4.然后点击开始
* 1.第一次会运行安装程序,按照提示步骤去一步一步安装就可以了.

![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-06-05 at 11.12.08.png)

* 2.安装完成.

![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-06-05 at 11.18.21.png)

***

## 3.共享目录<a name="share_directory"/>
#### 1.参考
* 1.[Permanently share a folder between host (Mac) and guest (Linux) OS using VirtualBox](https://ryansechrest.com/2012/10/permanently-share-a-folder-between-host-mac-and-guest-linux-os-using-virtualbox/#comment-350254)

#### 2.根据 `参考1` 去配置共享目录，但发现不行.



***

## 4.SSH 远程登录及 scp 文件传输<a name="ssh_login_scp_fileTransfer"/>
#### 1.配置端口号等等信息.
* 1.在主机运行的情况下，点击 `VirtualBox` 中的 `Settings` -> `Network` -> `Advanced` -> `Port Forwarding` , 然后如果没有 SSH 就自己新建一个：`Name "ssh", protocol TCP, Host port = 2281, Guest port = 22`.操作过程如下所示:

	![](http://pic.pgyjz.cn/blog/Mac/Snip20190613_1.png)
	![](http://pic.pgyjz.cn/blog/Mac/Snip20190613_2.png)
	![](http://pic.pgyjz.cn/blog/Mac/Snip20190613_3.png)

#### 2.然后从 Mac 登录 VirtualBox 中的虚拟机
* 1.Mac 中的 terminal 执行 `ssh userName@host -p remotePort`, 如我的就为: `ssh tian@127.0.0.1 -p 2281`,然后输入用户的密码即可登录成功.

```
MacBook-Pro:macUser$ ssh tian@127.0.0.1 -p 2281
tian@127.0.0.1's password: 
Linux debian 4.9.0-9-amd64 #1 SMP Debian 4.9.168-1+deb9u2 (2019-05-13) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Jun 13 02:23:16 2019
```

#### 3.用 SCP 实现文件传输.
* 1.上传(Mac 端 Terminal 的操作):
	* 1.如把 Mac 桌面的 `README.md` 文件上传到 `VirtualBox Linux 机器`: `scp -P portNumber MacFileAddress LinuxUser@host:LinuxFileLocation`
 
 ```
MacBook-Pro:macUser$ scp -P 2281 /Users/gz/Desktop/README.md tian@127.0.0.1:/tmp
tian@127.0.0.1's password: 
README.md                                                                                                                   100% 2594   857.3KB/s   00:00    

 ```

* 2.下载(Mac 端 Terminal 的操作):
	* 1.从 `VirtualBox Linux 机器` 下载文件到 Mac 中: `scp -P portNumber LinuxUser@host: LinuxFileAddress  MacFileAddress`

```
MacBook-Pro:macUser$ scp -P 2281 tian@127.0.0.1:/tmp/README.md /Users/gz/Downloads
tian@127.0.0.1's password: 
Permission denied, please try again.
tian@127.0.0.1's password: 
README.md                                                                                                                   100% 2594     1.1MB/s   00:00    

```
