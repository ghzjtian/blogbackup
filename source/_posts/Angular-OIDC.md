---
title: Angular 集成 OIDC 认证库
tags: [Angular]
categories: [Angular]
date: 2019-08-18 17:46:19
---


> 记录 Angular 集成 OIDC 库的过程

<!-- more -->

## 1.参考:
* [1.oidc-client-js](https://github.com/IdentityModel/oidc-client-js)
* [2.视频讲解(P9Implicit Flow 实例)](https://www.bilibili.com/video/av42364337/?p=9)
* [3.Github 源码(Identity-Server-4-Tutorial-Demo-Code)](https://github.com/solenovex/Identity-Server-4-Tutorial-Demo-Code)
* 4.[其他 oidc 库: angular-oauth2-oidc](https://github.com/manfredsteyer/angular-oauth2-oidc)

## 2.步骤


* 1.安装 [`oidc-client.js`](https://github.com/IdentityModel/oidc-client-js) 库.

* 2.在 `environment` 添加相关 `oidc` 的配置项.

* 3.添加一个 `oidc` service, 用于 auth 相关的功能.

* 4.在显示页面添加 oidc 的登录认证功能,如: 

```
// component
 constructor(public oidc: OpenIdConnectService) {}
```

```
// 显示用户名
 <span *ngIf="oidc.userAvailable">
                       {{oidc.user.profile.name}}
                    </span>
// 显示登录和登出的按钮.
  <button mat-menu-item *ngIf="!oidc.userAvailable" (click)="oidc.triggerSignIn()">
                登录
            </button>
  <button mat-menu-item *ngIf="oidc.userAvailable" (click)="oidc.triggerSignOut()">
                登出
            </button>

```


## 3.发现的问题及解决方法

### 1.在 Angular 生产环境中, `oidc-client` 包会发生 `Uncaught (in promise): Error: No matching state found in storage` 错误.

* 1.[No matching state found in storage #277](https://github.com/IdentityModel/oidc-client-js/issues/277)
* 2.[1.5.1: Failed to parse id_token #583](https://github.com/IdentityModel/oidc-client-js/issues/583)


#### 1.解决方法:

* 1.方法一:

> 修改 angular 项目中 angular.json 文件的 `"optimization": true,` 为 `"optimization": false,` 即可,但 `ng build --prod` 出来的包会从原来的 3M  变为 16M.

```
{
	"projects":{
		"architect":{
			"configurations":{
			// 这里改为 false
			"optimization": false,
			}
		}
	}
}
```

* 2.方法二：

> 把包降级到 `1.5.0` 版本:

```
npm uninstall oidc-client
npm install oidc-client@1.5.0 --save

```

这样使用 `ng build --prod` 生成的包大小还是 3M ，可以解决这个问题.


