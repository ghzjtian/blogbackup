---
title: Swagger学习
tags: [Linux]
categories: [Linux]
date: 2018-07-11 22:46:19
---



# Swagger 学习

* 1.了解到 Swagger 是一个强大的 API 生成工具，现在正准备学习.

<!-- more -->

* [2.Swagger Editor GitHub 地址](https://github.com/swagger-api/swagger-editor/tree/master)
* [3.Swagger Hub](https://app.swaggerhub.com/home)
* [4.Swagger UI GitHub 地址](https://github.com/swagger-api/swagger-ui)
* [5.Swagger 资料文档](https://swagger.io/docs/)
* [6.Swagger Generator](http://generator.swagger.io/)




### [1.参考链接](#references)
### [2.使用教程](#use_manual)
### [3.错误](#issues)

***
***
***

### 1.参考链接:<a name="references"/>
* [1.如何编写基于OpenAPI规范的API文档](https://legacy.gitbook.com/book/huangwenchao/swagger/details)
* [2.瑞骋项目的api](http://47.93.19.146:8080/ruicheng-app/)
* [3.Swagger使用笔记](https://blog.just4fun.site/swagger-note.html)
* [4.Swagger-PHP 部署](https://www.jianshu.com/p/5f190a0b40cb)
* [5.laravel中部署 Swagger ui](https://segmentfault.com/a/1190000014059501)

***

### 2.使用教程<a name="use_manual"/>
##### 1.简单的使用教程
>因为复制的教程现在还没学到(如 在服务器中部署 Swagger,让 API 随着代码的改变而自动改变, laravel 部署 Swagger 等等)，所以先介绍简单的应用.

* 1.[下载 Editor 的代码](https://github.com/swagger-api/swagger-editor/tree/master)(也可以不下载，有个 [在线的编辑器](https://app.swaggerhub.com/home) 可以生成 API JSON 文档数据)
* 2.编写完 API 后，就保存 JSON 文件到本地.
<img src="/assets/imgs/api/Snip20180712_3.png" >

* 3.[下载 Swagger UI](https://github.com/swagger-api/swagger-ui) 的代码到本地,把 上一步保存的 swagger.json 文件复制到 dist 目录中, 然后复制 dist 目录到服务器.
 <img src="/assets/imgs/api/Snip20180712_4.png" >

* 4.在浏览器中访问 `dist/index.html` 即可看到刚刚编写的 API 的文档资料.(我这里把 dist 的名字改为了 api_doc)
<img src="/assets/imgs/api/Snip20180712_6.png" >
<img src="/assets/imgs/api/Snip20180712_5.png" >


***

### 3.错误<a name="issues"/>
* 1.在本地的 Editor 编写 API 中，发现执行 Excute 后会返回 `	TypeError: Failed to fetch` .
* 2.本地的 `Swagger-UI` 的 `dist/index.html` 打不开 `swagger.json`,一直显示 `Failed to load API definition`

* 3.把 API 放到服务器去运行，结果还是一样,显示 `Failed to load API definition`

* 4.还不知道怎么解决以上的问题!!

***
***
***


