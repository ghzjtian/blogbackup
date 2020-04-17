---
title: Mac 下 Terminal 设置代理
tags: [Mac]
categories: [Mac]
date: 2019-07-22 17:46:19
---


> 对 Mac 电脑下的 Terminal 设置代理

<!-- more -->

## 1.参考
* 1.[在Mac终端下配置Proxy](https://miao1007.github.io/%E5%9C%A8mac%E7%BB%88%E7%AB%AF%E4%B8%8B%E9%85%8D%E7%BD%AEproxy/)

## 2.步骤:
* 1.用 SS 来配置 Proxy
* 2.用终端打开 `~/. bash_profile` 文件： `open -a xcode ~/. bash_profile`
* 3.新增以下内容:

```
#proxy
LANTERN='127.0.0.1:8787'
# 需要手动开启
SS_1086='127.0.0.1:1086'
SS='127.0.0.1:1087'
SS_1089='127.0.0.1:1089'
COW='127.0.0.1:7777'
POLIPO='127.0.0.1:8123'
defp=$SS

# No Proxy
function noproxy
{
unset http_proxy HTTP_PROXY https_proxy HTTPS_PROXY all_proxy ALL_PROXY ftp_proxy FTP_PROXY dns_proxy DNS_PROXY JAVA_OPTS GRADLE_OPTS MAVEN_OPTS
echo "clear proxy done"
}

function setproxy
{
if [ $# -eq 0 ]
then
inArg=$defp
else
inArg=$1
fi
HOST=$(echo $inArg |cut -d: -f1)
PORT=$(echo $inArg |cut -d: -f2)
http_proxy=http://$HOST:$PORT
HTTP_PROXY=$http_proxy
all_proxy=$http_proxy
ALL_PROXY=$http_proxy
ftp_proxy=$http_proxy
FTP_PROXY=$http_proxy
dns_proxy=$http_proxy
DNS_PROXY=$http_proxy
https_proxy=$http_proxy
HTTPS_PROXY=$https_proxy
JAVA_OPTS="-Dhttp.proxyHost=$HOST -Dhttp.proxyPort=$PORT -Dhttps.proxyHost=$HOST -Dhttps.proxyPort=$PORT"
GRADLE_OPTS="-Dgradle.user.home=$HOME/.gradle"
MAVEN_OPTS=$JAVA_OPTS
no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com,.coding.net,.ruby-china.org"
echo "current proxy is ${http_proxy}"
export no_proxy http_proxy HTTP_PROXY https_proxy HTTPS_PROXY all_proxy ALL_PROXY ftp_proxy FTP_PROXY dns_proxy DNS_PROXY JAVA_OPTS GRADLE_OPTS MAVEN_OPTS
}
# 打开终端就自动启动代理
setproxy

```

* 4.重启终端,会看到以下的输出 `current proxy is http://127.0.0.1:1087`:

![](http://pic.pgyjz.cn/blog/Mac/MacSetProxy/Snip20190722_1.png)

* 5.其他的一些控制命令:

```
#当不需要时，可以执行noproxy以取消
$ noproxy 
clear proxy done

#当然可以在某些情况下手动修改地址
$ setproxy $depf
current proxy is http://127.0.0.1:8123
$ setproxy $LANTERN
current proxy is http://127.0.0.1:8787
$ setproxy 127.0.0.1:443
current proxy is http://127.0.0.1:443

```

* 6.查看效果:

```
MacPro:~ glb_gz$ curl https://ip.cn
{"ip": "66.62.41.113", "country": "美国", "city": ""}
MacPro:~ glb_gz$ 
```
