---
title: Shadowsocks web-gui 的搭建及使用
tags: [ss]
categories: [ss]
date: 2018-09-04 17:46:19
---



> 关于 Shadowsocks web-gui 的搭建及使用

<!-- more -->



# ss-manager 

> 通过网页管理 SS 的工具

## 1.参考
* 1.[shadowsocks-manager](https://github.com/shadowsocks/shadowsocks-manager)
* 2.[安装方式](https://shadowsocks.github.io/shadowsocks-manager/#/install?id=%e6%99%ae%e9%80%9a%e6%96%b9%e5%bc%8f)
* 3.[安装SHADOWSCOCKS-MANAGER](https://frank2019.me/articles/2019/09/04/1567580175121.html)


## 2.安装步骤
### 1.安装 `Node.js `
* 1.[NodeSource Node.js Binary Distributions](https://github.com/nodesource/distributions)
* 2.Node 的版本一定要为 `12.*` !!!.

```
# Using Debian, as root
curl -sL https://deb.nodesource.com/setup_12.x | bash -
apt-get install -y nodejs

$:/etc/shadowsocks-libev# node -v
v12.19.0
$:/etc/shadowsocks-libev# npm -v
6.14.8

```

### 2.安装 `sqlite`
* 1.[How to Install SQLite3 On Debian](https://www.spinup.com/how-to-install-sqlite-on-debian-10/)

```
# apt-get update

# apt install sqlite3

# sqlite3 --version
3.16.2 2017-01-06 16:32:41

```

### 3.安装 `Redis`
##### 1.[How To Install and Secure Redis on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-redis-on-ubuntu-18-04)

##### 2.安装
```
$ sudo apt update
$ sudo apt install redis-server
```

##### 3.修改配置

* 1.修改 1


```


$ sudo nano /etc/redis/redis.conf

# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd

```

* 2.修改 2
	* 1.Binding to localhost
	* 2.Configuring a Redis Password



##### 4.重启 Redis

```

$ sudo systemctl restart redis.service

```

### 3.安装 `ss-manager`

```
#安装 ssmgr
npm i -g shadowsocks-manager

#若出现权限相关的错误提示，则需要尝试：
sudo npm i -g shadowsocks-manager --unsafe-perm

#安装完成后，使用ssmgr命令来运行程序
```

### 4.安装 `pm2`

> PM2 is a daemon process manager that will help you manage and keep your application online 24/7

* 1.[pm2](https://pm2.keymetrics.io/)

### 5.配置 `ss-manager`

#### 1.节点

* 1.节点配置文件: `ss1.json`

```
{
  "type": "s",
  "shadowsocks": {
    "address": "127.0.0.1:6001"
  },
  "manager": {
    "address": "127.0.0.1:6002",
    "password": "xxx"
  },
  "db": "db.sqlite"
}
```

* 2.节点运行

```
# 运行 节点

$ pm2 --name ss01 -f start ssmgr -x -- -c /root/.ssmgr/ss1.json -r  
# pm2 delete ss01

$ pm2 startup #将pm2写入系统service开机启动  
# pm2 unstartup

$ pm2 save #保存pm2当前守护进程的列表
# pm2 unsave
```

#### 2.服务器

* 3.`web-gui.json` 服务器配置文件(把 `xxxx` 换成自己的参数)


```
{
    "type": "m",
    "manager": {
      "address": "x,x,x,x:6002",
      "password": "xxx"
    },
    "plugins": {
      "flowSaver": {
        "use": true
      },
      "user": {
        "use": true
      },
      "account": {
        "use": true
      },
      "email": {
        "use": true,
        "type": "smtp",
        "username": "12345678xxx@qq.com",
        "password": "xxxxx",
        "host": "smtp.qq.com",
        "port": "587",
        "secure": false
      },
      "webgui": {
        "use": true,
        "host": "0.0.0.0",
        "port": "8x",
        "admin_username": "xxx@qq.com",
        "admin_password": "xxx",
        "skin": "myskin",
        "language": "en-US",
        "site": "http://-1,-1,-1,-1"
      }
    },
    "db": "webgui.sqlite",
    "redis": {
      "host": "localhost",
      "port": xxx,
      "password": "xxxxxxx",
      "db": 0
    }
  }
```

* 2.运行服务器

```
# 运行服务器
# ssmgr -c /root/.ssmgr/web-gui.json

pm2 --name ss-web-gui -f start ssmgr -x -- -c /root/.ssmgr/web-gui.json

```


### 6.Web gui使用方法

#### 1.创建用户.

#### 2.创建账号

> 账号的意思就是创建一个可使用的端口号，以供用户使用.

* 1.创建时 指定用户.

