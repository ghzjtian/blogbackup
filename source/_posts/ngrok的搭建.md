---
title: ngrok的搭建
tags: [Linux]
categories: [Linux]
date: 2018-09-04 17:46:19
---



# ngrok的搭建
> 内网穿透工具 ngrok 的搭建 。

<!-- more -->

### [1.参考链接](#references)
### [2.搭建步骤](#deploy_step)
### [3.安装 screen](#screen_install)

***
***
***


### 1.参考链接<a name="references"/>

* 1.官网：[https://ngrok.com/](https://ngrok.com/)
* 2.源码：[https://github.com/inconshreveable/ngrok](https://github.com/inconshreveable/ngrok)

***

### 2.搭建步骤<a name="deploy_step"/>

#### 1.使用 ngrok 的免费服务.
* [1.Setup & Installation](https://dashboard.ngrok.com/get-started)
* 2.但是这个免费版的不能 自定义 域名,如出现一下提示.

```
Mac Tian$ ./ngrok http -subdomain=tiantestdomain 80
Tunnel session failed: Only paid plans may bind custom subdomains.
Failed to bind the custom subdomain 'tiantestdomain' for the account 'JingTianZeng'.
This account is on the 'Free' plan.

Upgrade to a paid plan at: https://dashboard.ngrok.com/billing/plan

ERR_NGROK_313
```

#### 2.因为这个 ngrok 是个开源的项目，所以可以自己搭建一个内网穿透服务.
##### 1.参考：

* 1.[VPS自搭建Ngrok内网穿透服务()](https://www.jianshu.com/p/d35962b0dba4)
* 2.[How to run your own ngrokd server](https://github.com/inconshreveable/ngrok/blob/master/docs/SELFHOSTING.md)
* 3.[Run Ngrok on Your Own Server Using Self-Signed SSL Certificate (主要参考这个 !!!)](https://www.svenbit.com/2014/09/run-ngrok-on-your-own-server/)

##### 2.具体过程(位于公网的服务器端,有唯一的 Address,负责 DNS 的转换.):

* 1.按照 3 的教程,一步一步来就可以了 !!!
* 2.主要的代码有:

```
		user@JapanServer:~# ls
		go  go1.8.1.linux-amd64.tar.gz	ngrok  ngrok_server
		
		user@JapanServer:~# cd ngrok_server
		
		user@JapanServer:~/ngrok_server# ls
		ngrokd	server.crt  server.key
		
		# 开启 ngrokd
	./ngrokd -tlsKey=server.key -tlsCrt=server.crt -domain="ngrok.tianlovezhen.site" -httpAddr=":80" -httpsAddr=":443"
	
```
		
* 3.让 ngrokd 在后台运行(参考 3 的评论区.)
	* 1.因为按照 3 的教程, ngrokd 在 ssh 关闭后，也会自动关闭.所以需要让它在后台自动运行.
	* 2.运行命令 `./ngrokd -log=stdout -tlsKey=device.key -tlsCrt=device.crt -domain="something.tunnel.com" -httpAddr=":80" -httpsAddr=":443" > /dev/null &`
	* 3.如果需要关闭，就运行 `ps -ef | grep "ngrokd"` 或 `netstat -ntlp` 查找出 `ngrok` 的 PID ，然后运行 `kill -9 [PID]` 去关闭 `ngrokd`

* 4.让 ngrokd 在后台运行(用 screen 去实现)
	* 1.具体用法请参考 [ SSH远程会话管理工具 - screen使用教程](https://www.vpser.net/manage/screen.html)

##### 3.客户端(位于内网的服务器，负责网页的内容)
* 1.下载上一步 `make release-server release-client` 后生成的客户端程序 `ngrok`,在所运行命令路径的 `bin` 中.
	* 1.下载 `Linux:~ user$ scp user@1.2.3.4:/user/ngrok/bin/ngrok /Users/user/Desktop`
	* 2.生成一个配置文件如: `ngrok.cfg` ,在里面写上: 

```
server_addr: [NGROK_BASE_DOMAIN]:4443
trust_host_root_certs: false
```

* 3.运行命令开始内网穿透,如: `./ngrok -subdomain testing -config=ngrok.cfg 80`

* 4.后台运行(需要借助 screen 去实现)
	* 1.具体用法请参考 [ SSH远程会话管理工具 - screen使用教程](https://www.vpser.net/manage/screen.html)



***

### 3.安装 screen<a name="screen_install"/>
* 1.参考:
	* [1.SSH远程会话管理工具 - screen使用教程](https://www.vpser.net/manage/screen.html)
* 2.怎样在 linux 启动时开启 screen ,并自动打开 ngrokd 或 ngrok ???











