---
title: Shadowsocks 的搭建
tags: [ss]
categories: [ss]
date: 2018-09-04 17:46:19
---



# Shadowsocks 的搭建
> 关于 Shadowsocks 的搭建计划.

<!-- more -->

## [1.shadowsocks-libev 的安装](#ss_install)
## [2.shadowsocks-libev 的多用户搭建](#ss_multi_user)
## [3.相关的控制的命令](#related_command)
## [4.相关的软件](#related_software)
## [5.相关端口被屏蔽](#related_port_shield)
## [6.客户端设置的分享](#share_settings)
## [7.Mac下共享ShadowsocksX-NG 给手机](#share_to_phone)


***
***
***

## 1.Shadowsocks-libev 的安装<a name="ss_install"/>
* [1.官方的 Github 安装介绍](https://github.com/shadowsocks/shadowsocks-libev)

***

## 2.shadowsocks-libev 的多用户搭建<a name="ss_multi_user"/>

### 1.用 SS-MANAGER 创建多用户
#### 1.编辑 `/etc/shadowsocks-libev/config.json` 文件

> 如这里添加了两个端口(101, 102)和连接密码.

```
{
  "server":["::0","0.0.0.0"],
  "port_password": {
        "101": "psw101",
        "102": "psw102"
  },
  "timeout":60,
  "method":"encry-methods",
  "fast_open":true
}
```

#### 2.开启 ss-manager 服务

##### 1.手动开启 
```
ss-manager --manager-address /var/run/shadowsocks-manager.sock -c /etc/shadowsocks-libev/config.json
```

##### 2.自动开启
* 1.参考: [Create systemd Services](https://wiki.debian.org/systemd/Services)
* 2.创建系统服务, 新增文件 `/etc/systemd/system/ss-manager.service`

```
[Unit]
Description=Shadowsocks Manager Server

[Service]
User=root
Group=root
Type=simple
Restart=always
ExecStart=/usr/bin/ss-manager --manager-address /var/run/shadowsocks-manager.sock -c /etc/shadowsocks-libev/config.json


[Install]
WantedBy=multi-user.target

```

* 2.设置开机启动

```
systemctl daemon-reload
systemctl enable ss-manager.service

systemctl start ss-manager.service

# systemctl stop ss-manager.service
# systemctl status ss-manager.service
```

***

### 2.用 SS-Server 创建多用户

#### [1.多用户的搭建](https://github.com/shadowsocks/shadowsocks-libev/issues/5)
#### 2.教程:
	* 1.到 `/etc/shadowsocks-libev` 中复制一个 `config.json` 文件，并修改为想要的设置
	* 2.然后用 `ss-server` 配置,配置完就重启,`ss-server -c config1.json -f pid1`
	* 3.如果有多个用户,就继续下面的配置(记得要有 configX.json 文件)
		* 1.`ss-server -c config2.json -f pid2`
		* 2.`ss-server -c config3.json -f pid3`


#### 3.Config.json 文件的配置 DEMO:

```
user@Company:/etc# cat /etc/shadowsocks-libev/config.json
{
    "server":"x.x.x.x",
    "server_port":101,
    "local_port":1080,
    "password":"111111",
    "timeout":60,
    "method":"aes-256-cfb"
}
```

***




***

## 3.相关的控制的命令<a name="related_command"/>

* 1.查看端口使用情况: `netstat －antu` or `netstat -tlnp`.
* 2.启动、停止、重启: `service shadowsocks-libev start/stop/restart`
	* 1.注意，如果是多用户的话，上面的命令只会对 `config.json` 有用.(同样原理适用于主机重启后 !! 所以主机重启后务必检查相关的端口配置有没有开启 !!!)
		* 1.停止: 
			* 1.用 `netstat -tlnp` 或 `ps -aux | less` 查看了程序的 `PID` 号后，就用 `kill -9 xxx(pid)` 去杀死指定 PID 的程序.
		* 2.开始: `ss-server -c config3.json -f pid3`


***

## 4.相关的软件<a name="related_software"/>
*  1.TCP BBR 拥堵控制算法 的开启:
	* [1.BBR 阻塞算法，真是黑科技](https://fiveyellowmice.com/posts/2016/12/bbr-congestion-algorithm-dark-science.html)
	* [2.Debian / Ubuntu 更新内核并开启 TCP BBR 拥塞控制算法](https://sb.sb/blog/debian-ubuntu-tcp-bbr/)
* [2.speedtest-cli测速工具](https://www.howtoforge.com/tutorial/check-internet-speed-with-speedtest-cli-on-ubuntu/)

* 3.搭建过程:
	* 1.内核查看的命令 `cat /proc/version`
	* 2.过程(摘自[1.BBR 阻塞算法，真是黑科技](https://fiveyellowmice.com/posts/2016/12/bbr-congestion-algorithm-dark-science.html)):
	
```
加载内核模块。
先运行 sudo modprobe tcp_bbr 看一看，没问题的话，就创建一个 /etc/modules-load.d/80-bbr.conf ，里面写上 tcp_bbr 七个字，就会每次开机自动加载 tcp_bbr 模块了。
让内核使用 BBR 为阻塞控制算法。
cat /proc/sys/net/ipv4/tcp_available_congestion_control 看看里面有没有 bbr 三个字。
没问题的话， sudo sysctl net.ipv4.tcp_congestion_control=bbr 来启用 BBR 。
除非你想每次开机都运行一遍 sysctl ，记得创建一个 /etc/sysctl.d/80-bbr.conf ，写上 net.ipv4.tcp_congestion_control = bbr 就可以了。
```

***

## 5.相关端口被屏蔽<a name="related_port_shield"/>

* 1.发现（20190304）一个端口 `9005` 不能连接成功，经过测试，发现是被国内屏蔽了.
	* 1.测试过程:
		* 1.本地的 Mac 用 `nc -v 62.42.41.223 9005` 不通过.
		* 2.外国的 Linux Debian 用 `nc -v 62.42.41.223 9005` 通过.



***

## 6.客户端设置的分享<a name="share_settings"/>
* 1.遇到的 Bug:
	* 1.经过实战发现,如果是用 ShadowsocksX-NG(for mac)相关的二维码分享,旧版本的 Shadowsocks(Android 或 Windows ) 都不会被识别，所以要分享 二维码的图片，还是要用旧版本的 Windows 客户端去做分享.



***

## 7.Mac下共享ShadowsocksX-NG 给手机<a name="share_to_phone"/>

* 1.参考: [7.Mac下共享ShadowsocksX-NG 给手机](https://www.jibing57.com/2019/03/24/share-ShadowsocksX-NG-to-iOS/)


##### 1.打开小飞机的“偏好设置”，切换到HTTP一栏，其中的HTTP代理监听地址默认是127.0.0.1，将其改成0.0.0.0。保留默认的1087或改为自己想要设置的端口，并勾选“开启HTTP代理服务器”。
##### 2.Phone连接到和Mac相同的局域网，然后在“无线局域网”中设置HTTP代理。
* 1.地址填写 电脑的地址
* 2.端口填写 1087.
* 3.用浏览器访问外网，测试一下效果.