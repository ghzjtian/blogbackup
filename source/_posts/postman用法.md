---
title: postman 用法
tags: [postman]
categories: [postman]
date: 2019-06-24 17:46:19
---


> 介绍自己用 PostMan(Version 7.2.2 (7.2.2)) 的心得

<!-- more -->

## [1.参考](#references)
## [2.变量的使用](#variable_usage)
## [3.代理的设置](#set_proxy_server)
## [4.Mock Server 的使用](#mock_usage)

***
***
***

## 1.参考<a name="references"/>
* 1.[postman 的基础使用篇(一)](https://segmentfault.com/a/1190000011991458)
* 2.[postman的代理使用篇(四)](https://segmentfault.com/a/1190000012024844)
* 3.[postman 变量使用篇(六)](https://segmentfault.com/a/1190000012077563)
* 4.[Postman Sandbox API reference](https://learning.getpostman.com/docs/postman/scripts/postman_sandbox_api_reference/)


## 2.变量的使用<a name="variable_usage"/>
#### 1.变量的增加和使用
> 点击 `setting 图标` -> `相应的环境` 即可查看或编辑对应变量.

![](http://pic.pgyjz.cn/blog/Mac/Snip20190624_2.png)
![](http://pic.pgyjz.cn/blog/Mac/Snip20190624_3.png)

#### 2.脚本动态改变变量.
> 1.PostMan 提供了丰富的脚本去实现运行时动态设置参数,详情可看 [Postman Sandbox API reference](https://learning.getpostman.com/docs/postman/scripts/postman_sandbox_api_reference/) .
> 
> 2.用的语言为 javascript, 同理也可以用 `Command + option + C` 打开控制台，即可看到 `console.log("");` 的输出.

![](http://pic.pgyjz.cn/blog/Mac/Snip20190624_5.png)
![](http://pic.pgyjz.cn/blog/Mac/Snip20190624_6.png)

***

## 3.代理的设置<a name="set_proxy_server"/>
* 1.可以让 `PostMan` 走 ss 的代理.
* 2.但是我只选 `User System Proxy` 的话，代理没效果，不知道为什么.
![](http://pic.pgyjz.cn/blog/Mac/screenshot 2019-06-24 at 18.19.57.png)

***

## 4.Mock Server 的使用<a name="mock_usage"/>

> 其实 PostMan 官方的这篇文章 [Setting up a mock server](https://learning.getpostman.com/docs/postman/mock_servers/setting_up_mock/) 还有 APP `PostMan` 都有详细的教程，所以我这里只是用中文的形式把我理解的写出来.

![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_2.png)

#### 1.在已有的 `Collection` 上增加一个 `Mock Server`.

![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_3.png)

* environment 不设置也是可以的.
![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_4.png)

* 点击 `Create mock server`,就会创建一个 Mock Server 了.

![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_5.png)

* 我们测试一下这个链接，可以运行，但是当然会返回错误信息，因为还没有配置返回的 `Sample API`.
![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_6.png)



#### 2.增加一个返回示例

![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_7.png)

![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_10.png)

* 可以看到我们这里有两个 Mock Server(教程之前已经提前创建好了一个)
![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_11.png)

* 打开 URL，可以看到返回了我们之前设置好的返回示例.

![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_13.png)

* 在 外部的浏览器去运行这个 URL , 可以看到也是返回一样的结果

![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_15.png)

* 但是因为我的账号是免费版，所以 Mock 的适用有一定的限额，一个月才 1000 次.

![](http://pic.pgyjz.cn/blog/postman/postmanUsage/Snip20190628_16.png)


