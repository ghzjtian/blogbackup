---
title: 电脑配置 Route
tags: [Mac]
categories: [Mac]
date: 2020-04-22 17:46:19
---

> 电脑手动 配置 Route 的方法

<!-- more -->

## Windows:
需要以管理员身份运行 cmd ，  然后输入以下命令:
route  add  10.0.0.0  mask  255.255.0.0    123.456.789.012


## Linux(还没验证):
sudo route add -net 10.0.0.0/16 gw 123.456.789.012

## Mac:
#### 1.查看 route list 命令:  `netstat -nr`

```
$ netstat -nr
Routing tables

Internet:
Destination        Gateway            Flags        Refs      Use   Netif Expire
default            10.0.1.1         UGSc          115        0     en0       
10/16              123.456.789.012     UGSc            0        0     en0       
10.100.1/24        link#8             UCS            11        0     en0      !
10.100.1.1/32      link#8             UCS             1        0     en0      !
```

#### 2.添加

* 1.添加 default

```
sudo route add  default   10.0.1.1
```

* 2.添加特殊的 route(一直设置不成功!!)

```
sudo route -n add -net 10.0.0.0/16 123.456.789.012
```


#### 3.删除

```
sudo route -n delete 10/16  123.456.789.012
```

#### 4.查看

> 查看到达指定 IP 地址路由的情况

* 1.Windows:

```
tracert 192.168.1.1
```

* 2.Mac

```
traceroute 192.168.1.1

```
