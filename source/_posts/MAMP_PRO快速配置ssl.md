---
title: MAMP PRO 快速配置 ssl
tags: [Mac]
categories: [Mac]
date: 2018-02-13 17:46:19
---



### MAMP PRO 快速配置 ssl
>参考: [http://documentation.mamp.info/en/MAMP-PRO-Mac/Settings/Hosts/SSL/](http://documentation.mamp.info/en/MAMP-PRO-Mac/Settings/Hosts/SSL/)

<!-- more -->

#### [1.详细的步骤](#steps)
#### [2.出现的错误](#errors)
***
***

#### 1.详细的步骤:<a name="steps"/>
* 1.增加一个 主机.
![](/assets/imgs/web/ScreenShot2018-02-13_11.07.02.png)

* 2.填写相关的信息.
![](/assets/imgs/web/ScreenShot2018-02-13_11.06.37.png)

* 3.开启 ssl.
![](/assets/imgs/web/ScreenShot2018-02-13_11.09.17.png)

* 4.验证(可以访问 ssl 的网站了)

![](/assets/imgs/web/ScreenShot2018-02-13_11.26.07.png)

![](/assets/imgs/web/ScreenShot2018-02-13_11.27.23.png)

***

#### 2.出现的错误<a name="errors"/>
>未解决.

* 1.在本地的 MBP 中，用浏览器访问 127.0.0.1 和 localhost 都可以正常地访问,访问 https://localhost 也很正常，但是访问 https://127.0.0.1 就会出现下面的错误:

```
Forbidden
You don't have permission to access / on this server.
```






