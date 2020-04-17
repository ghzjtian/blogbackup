---
title: Mac上的抓包工具分析
tags: [Mac]
categories: [Mac]
date: 2018-03-02 17:46:19
---


### [1.Charles](#charles)
##### 1.抓取 APP 的数据
##### 2.抓取 Mac 上的数据
##### 3.Charles 通过 Shadowsocks 去抓取被墙的网站的数据.

### [2. Wireshark](#wireshark)

<!-- more -->

***
***
***


### 1.Charles<a name="charles"/>
>可以抓取普通的 Get,post 的数据.

##### 1.抓取 APP 的数据

* 1.参考: [https://www.jianshu.com/p/235bc6c3ca77](https://www.jianshu.com/p/235bc6c3ca77)

* 2.代理:因为原理是通过代理去抓取数据，所以 APP 可以通过判断手机的网络是否启用了代理去禁止 APP 的启动.
	* [1.Android 判断设备 是否使用代理上网](https://www.jianshu.com/p/798702779d59)

##### 2.抓取 Mac 上的数据
> 1.注意不要开通其它的代理，否则会导致抓取不到数据
> 
> 2.参考: [Mac上使用Charles抓包](https://zhuanlan.zhihu.com/p/26182135)

* 1.开通了其它的代理设置时,抓取不到数据的情况截图:

![](/assets/imgs/web/ScreenShot_2018-10-08_11.35.56.png)

* 2.正常抓取的截图:

![](/assets/imgs/web/ScreenShot_2018-10-08_11.36.57.png)


##### 3.Charles 通过 ShadowsocksX-NG 去抓取被墙的网站的数据.
* 1.参考:
	* [Charles支持通过Shadowsocks代理抓包](https://sayue.me/2018/04/04/Charles%E6%94%AF%E6%8C%81%E9%80%9A%E8%BF%87shadowsocks%E4%BB%A3%E7%90%86%E6%8A%93%E5%8C%85/)
* 2.详细的步骤
	* 1.`ShadowsocksX-NG` 中配置的信息
		* 1.如我这里为 `Local Socks5 Listen Address:Port` 为 `127.0.0.1:1086`,`HTTP Proxy Listen Address:Port` 为 `127.0.0.1:1087`
![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-01-12 at 22.41.28.png)
![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-01-12 at 22.36.35.png)
![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-01-12 at 22.43.04.png)

	* 2.系统中 `Preferences/Network/Advanced/Proxies` 中 `Web Proxy(HTTP)`,`Secure Web Proxy(HTTPS)` `SOCKS Proxy` 都为 `127.0.0.1:1086`.
![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-01-12 at 22.36.54.png)

	* 3.修改 `Charles/Proxy` 中的 `SSL Proxying Setting`(监听 HTTPS) 和 `External Proxy Settings`(配置 Charles 通过 SS 代理,配置为 `127.0.0.1:1087`),分别为下图:
![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-01-12 at 22.50.45.png)
![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-01-12 at 22.50.57.png)
![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-01-12 at 22.51.10.png)
	
	* 4.手机访问 google 的效果
	![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-01-12 at 22.38.09.png)
	
	
***


### 2. Wireshark<a name="wireshark"/>
>参考1：[https://www.jianshu.com/p/62f00db7be68](https://www.jianshu.com/p/62f00db7be68)
>参考2：[https://www.bo56.com/mac%E5%AE%89%E8%A3%85wireshark/](https://www.bo56.com/mac%E5%AE%89%E8%A3%85wireshark/)
>





#### 2.1.用 wireshark 抓到的 TCP 包 的数据截图
>该程序为 Android 手机与 Mac 建立的 TCP 链接，所截取的图.
>ip.src == 10.100.1.251 or ip.dst == 10.100.1.251
>ip.src == 183.232.231.173 or ip.dst == 183.232.231.173
>ip.src == 14.215.177.39 or ip.dst == 14.215.177.39
>ip.src == 42.121.17.113 or ip.dst == 42.121.17.113
>ip.src == 120.26.196.41 or ip.dst == 120.26.196.41


>Server:[https://github.com/ghzjtian/TCP_Server](https://github.com/ghzjtian/TCP_Server)
>Android:[https://github.com/ghzjtian/TCP_Android](https://github.com/ghzjtian/TCP_Android)

![](/assets/imgs/web/Screenshot_20180302-180708.png)
![](/assets/imgs/web/ScreenShot2018-03-02_18.06.57.png)
![](/assets/imgs/web/ScreenShot2018-03-02_18.07.02.png)
![](/assets/imgs/web/ScreenShot2018-03-02_18.06.53.png)

#### 2.2截取其它 APP 的 TCP 链接数据.





