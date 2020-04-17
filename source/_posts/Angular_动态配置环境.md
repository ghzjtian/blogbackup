---
title: Angular 动态配置环境
tags: [Angular]
categories: [Angular]
date: 2019-08-24 17:46:19
---


> 免 rebuild 配置环境的过程.

<!-- more -->

## 1.参考
* 1.[How to use environment variables to configure your Angular application without a rebuild](https://www.jvandemo.com/how-to-use-environment-variables-to-configure-your-angular-application-without-a-rebuild/)
	* 1.[Github: 2
13 4 jvandemo/angular-environment-variables-demo](https://github.com/jvandemo/angular-environment-variables-demo)
* 2.[Angular 2 / 4 change env variable after build](https://stackoverflow.com/questions/46077038/angular-2-4-change-env-variable-after-build)


## 2.步骤
* 1.新建一个环境 `javascript` 文件,在 `src/assets` 目录下新建一个 `env.js` 文件，写入以下内容:

```
(function (window) {
    window.__env = window.__env || {};

    // API url
    window.__env.apiUrl = 'http://dev.your-api.com';

    // Whether or not to enable debug mode
    // Setting this to false will disable console output
    window.__env.enableDebug = false;

}(this));


```

* 2.把新建的文件导入到项目中,向 `src/index.html` 的 `<head></head>` 中导入新建的 `env.js` 文件，效果如下所示:

```
<html ng-app="app">

  <head>
    <!-- Load environment variables -->
    <script src="assets/env.js"></script>
  </head>

  <body>
    ...
    <!-- Angular code is loaded here -->
  </body>  

</html>  
```

* 3.让 `env.js` 文件在 `ng build` 或 `ng serve` 中原样输出，需要在 `angular.json` 文件的 `assets` 下添加如下的代码,如 :

```
{
  "projects": {
    "app-name": {
      "architect": {
        "build": {
          "options": {
            "assets": [
              "src/favicon.ico",
              "src/assets",
              "src/assets/env.js"
            ]
          }
          "configurations": {
            "production": {
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.prod.ts"
                }
              ],
              // ...
            }
          }
        }
      }
    }
  }
}
```

* 4.在 `environment.ts` 中导入我们动态生成的值，如:

```
const key = '__env';
export const environment = {
    canDebug: window[key].enableDebug || false,
    }
```

* 5.然后即可在组件中使用，如:

```
// Component
export class AppComponent implements OnInit {
	devVersion = environment.appVersion + ',' + environment.canDebug;
}
```
```
// templateUrl
<span class="version" >{{devVersion}}</span>
```
* 6.然后动态改变 `env.js` 的值，网站的配置也会改变

  * 1.这是 `ng serve` 的效果，`ng build` 后放到 `nginx` 的效果也一样的,而且不用重启 nginx 都可以看到环境的变化.
![](http://pic.pgyjz.cn/blog/Angular/env-change/angular-env-change.gif)




