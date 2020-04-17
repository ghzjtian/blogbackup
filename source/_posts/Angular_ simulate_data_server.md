---
title: Angular 模拟数据服务器
tags: [Angular]
categories: [Angular]
date: 2019-11-24 17:46:19
---

> `angular-in-memory-web-api` 可以模拟后端返回的数据, 达到快速开发的目的.
 
 <!-- more -->

## 1.参考
## 2.过程


***
***
***

## 1.参考
* 1.[Angular.cn 模拟数据服务器](https://v6.angular.live/tutorial/toh-pt6#simulate-a-data-server)
* 2.[angular/in-memory-web-api](https://github.com/angular/in-memory-web-api#import-the-in-memory-web-api-module)
* 3.[例子](https://stackblitz.com/angular/lygvpvqpvbx?file=src%2Fapp%2Fhero.service.ts)


## 2.过程
* 1.安装

```
npm install angular-in-memory-web-api --save
# 生成 service 类
ng generate service InMemoryData
```

* 2.在 module 中配置

```
imports: [
  HttpClientModule,
  // 如果是 prod 环境，自动用真实的 Http 请求代替.
  environment.production ?
    [] : HttpClientInMemoryWebApiModule.forRoot(InMemHeroService)
  ...
]
```

* 3.可以多个 URL mock data.
* 4.URL 可以是自定义的,[Default parseRequestUrl](https://github.com/angular/in-memory-web-api#default-parserequesturl)

```
#在 module 中配置，如配置为
   HttpClientInMemoryWebApiModule.forRoot(
      InMemoryDataService, { dataEncapsulation: false, apiBase: 'a/b/c/'}
    )
    
# 则 URL 可定义为: 
private heroesUrl = '/api/value/get/heroes' ;  // URL to web api

```
