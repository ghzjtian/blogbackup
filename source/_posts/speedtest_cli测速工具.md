---
title: speedtest-cli测速工具
tags: [Linux]
categories: [speedtest]
date: 2018-09-04 17:46:19
---


# speedtest-cli测速工具

##### 服务器网络测试工具.

>安装: [https://www.howtoforge.com/tutorial/check-internet-speed-with-speedtest-cli-on-ubuntu/](https://www.howtoforge.com/tutorial/check-internet-speed-with-speedtest-cli-on-ubuntu/)

<!-- more -->

### [1.用法](#usage)
### [2.效果:](#result)
### [3.安装](#install)
### [4.外网速度测试,到长沙(6132)](#out_net)
### [5.安装了 bbr 后](#after_install_bbr)

***
***
***




### 1.用法<a name="usage"/>
* 1.http://www.ttlsa.com/linux/use-speedtest-cli-test-internet-speed/
* 2.https://fiveyellowmice.com/posts/2016/12/bbr-congestion-algorithm-dark-science.html

***

### 2.效果:<a name="result"/>

```
# speedtest-cli
Retrieving speedtest.net configuration...
Testing from China Telecom Guangdong (183.6.115.218)...
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by CSL (Tai Po) [120.14 km]: 62.55 ms
Testing download speed................................................................................
Download: 9.51 Mbit/s
Testing upload speed................................................................................................
Upload: 6.56 Mbit/s

# speedtest-cli --bytes
Retrieving speedtest.net configuration...
Testing from China Telecom Guangdong (183.6.115.218)...
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by CSL (Tai Po) [120.14 km]: 16.276 ms
Testing download speed................................................................................
Download: 1.19 Mbyte/s
Testing upload speed................................................................................................
Upload: 0.83 Mbyte/s

# speedtest-cli --simple
Ping: 16.259 ms
Download: 8.11 Mbit/s
Upload: 6.60 Mbit/s

//Share
# speedtest-cli --share
Retrieving speedtest.net configuration...
Testing from China Telecom Guangdong (183.6.115.218)...
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by MTel (Macau) [106.14 km]: 26.26 ms
Testing download speed................................................................................
Download: 9.62 Mbit/s
Testing upload speed................................................................................................
Upload: 6.60 Mbit/s
Share results: http://www.speedtest.net/result/6775155371.png

```




***
### 3.安装<a name="install"/>
* 1.这次用的是第二种的方法去安装 speedtest-cli

```
# cd /tmp
# wget https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py
--2017-11-08 09:05:52--  https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.52.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.52.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 47441 (46K) [text/plain]
Saving to: `speedtest.py'

100%[=========================================================================>] 47,441      --.-K/s   in 0.002s  

2017-11-08 09:05:53 (18.4 MB/s) - `speedtest.py' saved [47441/47441]

# ls
master.zip  speedtest-cli-master  speedtest.py
# chmod 755 speedtest.py
# mv speedtest.py /usr/local/bin/speedtest-cli
# speedtest-cli
Retrieving speedtest.net configuration...
Testing from Choopa, LLC (104.238.115.218)...
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by Comcast (Seattle, WA) [1.51 km]: 1.355 ms
Testing download speed................................................................................
Download: 4493.23 Mbit/s
Testing upload speed................................................................................................
Upload: 1697.58 Mbit/s

```

***

### 4.外网速度测试,到长沙(6132)<a name="out_net"/>

```
# speedtest-cli --bytes --server=6132 --share
Retrieving speedtest.net configuration...
Testing from Choopa, LLC (104.238.115.218)...
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by ChinaTelecom.Hunan (Changsha) [9937.81 km]: 333.483 ms
Testing download speed................................................................................
Download: 2.67 Mbyte/s
Testing upload speed................................................................................................
Upload: 0.60 Mbyte/s
Share results: http://www.speedtest.net/result/6775344464.png
# 
```
![](/assets/imgs/net/6775344464.png)

***
### 5.安装了 bbr 后<a name="after_install_bbr"/>
>20171108,18:00


```
# speedtest-cli --bytes --server=6132 --share
Retrieving speedtest.net configuration...
Testing from Choopa, LLC (104.238.115.218)...
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by ChinaTelecom.Hunan (Changsha) [9937.81 km]: 408.372 ms
Testing download speed................................................................................
Download: 1.90 Mbyte/s
Testing upload speed................................................................................................
Upload: 0.71 Mbyte/s
Share results: http://www.speedtest.net/result/6775434818.png
# 
```
![](/assets/imgs/net/6775434818.png)

>20171108,22:00

```
# speedtest-cli --bytes --server=6132 --share
Retrieving speedtest.net configuration...
Testing from Choopa, LLC (104.238.115.218)...
Retrieving speedtest.net server list...
Selecting best server based on ping...
Hosted by ChinaTelecom.Hunan (Changsha) [9937.81 km]: 506.287 ms
Testing download speed................................................................................
Download: 1.53 Mbyte/s
Testing upload speed................................................................................................
Upload: 1.40 Mbyte/s
Share results: http://www.speedtest.net/result/6775983072.png
```
![](/assets/imgs/net/6775983072.png)






