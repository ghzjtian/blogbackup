---
title: Angular 项目的发布
tags: [Angular]
categories: [Angular]
date: 2019-08-09 18:46:19
---

> 主要记录下 Angular 项目的部署的方法

<!-- more -->

## [1.参考](#references)
## [2.发布方法](#how_to_public)
* 1.发布到 `Github Page` 上面
* 2.发送到 `nginx` 服务器
* 3.发布到 `firebase`.

## [3.本地代码 HTTPS 部署](#local_deploy)
## [4.错误记录](#error_info_note)


***
***
***

## 1.参考<a name="references"/>
* [Angular 官方的部署文档](https://v6.angular.live/guide/deployment)
* [Running Angular CLI over HTTPS with a Trusted Certificate](https://medium.com/@rubenvermeulen/running-angular-cli-over-https-with-a-trusted-certificate-4a0d5f92747a)
* [Running Angular CLI over HTTPS with a Trusted Certificate](https://medium.com/@rubenvermeulen/running-angular-cli-over-https-with-a-trusted-certificate-4a0d5f92747a)
* [Serve an Angular app on localhost via HTTPS](https://blog.fullstacktraining.com/serve-an-angular-app-on-localhost-via-https/)

## 2.发布方法<a name="how_to_public"/>

#### [1.发布到 `Github Page` 上面](https://v6.angular.live/guide/deployment#deploy-to-github-pages)
* 1.官方已经讲得很详细，我这里就不再描述了.
* 2.但是 `404.html` 页面一直没什么反应,即我输入错了地址，也不会导航到 404 page.

#### 2.发送到 nginx 服务器

##### 1.参考:
* 1.[Nginx 配置设置](https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/#front-controller-pattern-web-apps)
* 2.[Nginx 后备页面配置范例](https://v6.angular.live/guide/deployment#fallback-configuration-examples)

##### 2.配置 nginx 的配置文件 `nginx.conf` ,增加 `try_files $uri $uri/ /index.html;` 的配置.

```
http {
         ...
  server {
        ...
    location / {
            ...
            try_files $uri $uri/ /index.html;
        }
```

##### 3.把 `ng build` 后生成的包放到指定的服务器地址下，即可看到效果.


#### 3.发布到 `firebase`.
###### 1.参考:
* 1.[Deploying an Angular CLI App to Production with Firebase](https://scotch.io/tutorials/deploying-an-angular-cli-app-to-production-with-firebase)
* 2.[Firebase 托管使用入门](https://firebase.google.com/docs/hosting/quickstart?authuser=0)

###### 2.步骤:
* 1.安装 `firebase tools` : `npm install -g firebase-tools`
* 2.在 `https://console.firebase.google.com` 中登录
* 3.到 Angular 项目的根目录下执行命令 `firebase login` `firebase init`, 然后按以下配置进行:

```
Are you ready to proceed? Yes
Which Firebase CLI features? Hosting (In the future, use whatever you need! Press space to select.)
Select a default Firebase project? Angular Hosting Test (Choose whatever app you created in the earlier steps)
What do you want to use as your public directory? dist/your-project-name (This is important! Angular creates the dist folder.)
Configure as a single-page app? Yes
```

* 4.然后运行 `firebase deploy` 即可部署你的网站到 firebase.

## 3.本地代码 HTTPS 部署<a name="local_deploy"/>

> Mac 的步骤

#### 1.直接在命令行运行 `ng serve --ssl true` 即可, 但是这样浏览器会报 `Certificate is not trusted` 的警告.
#### 2.想要解决这个问题， 需要以下的步骤:
* 1.运行命令 `openssl req -newkey rsa:2048 -x509 -nodes -keyout server.key -new -out server.crt -sha256 -days 365` 生成证书文件 `server.crt` 和 `server.key`.
* 2.双击 `server.crt ` 文件,在弹出的框里选择 `Login`, 然后在 `Login` 中双击选择刚刚生成的 `localhost`
* 3.在 `Trust` 中选择 `Always Trust`.
* 4.把 `ssl.crt` 和 `server.key`文件放置到 项目跟目录的 ssl 文件夹下.
* 5.重启 浏览器,然后运行命令 `ng serve --ssl true --ssl-cert "./ssl/server.crt" --ssl-key "./ssl/server.key"` .
* 6.再打开 [https://localhost:4200](https://localhost:4200) 即可看到浏览器已经信任了这个证书.


## 4.错误记录<a name="error_info_note"/>
#### 1.Angular CLI version `(8.1.3)`
* 1.发现在 html 中调用 ts 文件的 private 方法， `ng serve`, `ng build` 都不会报错， 但是 `ng build` 就会报错，如 `ERROR in src/app/header/header.component.html(24,49): : Property 'getUserRoles' is private and only accessible within class 'HeaderComponent'`, 记录仅供以后避免.



