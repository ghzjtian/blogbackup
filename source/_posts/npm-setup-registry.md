---
title: NodePackageManager(NPM) 的 registry 设置
tags: [node]
categories: [node]
date: 2019-09-05 17:46:19
---

> 配置 npm 的源

<!-- more -->

## [1.参考](#references)
## [2.过程](#steps)
## [3.错误处理](#error_process)

***
***
***

## 1.参考<a name="references"/>
* 1.[淘宝 NPM 镜像](https://npm.taobao.org)
* 2.[npm 淘宝镜像配置](https://gist.github.com/52cik/c1de8926e20971f415dd)
* 3.[配置cnpm](http://blog.pgyjz.cn/2019/09/05/npm%E8%AE%BE%E7%BD%AE/)

## 2.过程<a name="steps"/>

* 1.切换源(官方源 <-> 淘宝源)

```
# npm 官方源
npm set registry https://registry.npmjs.org/

# 淘宝源
# 注册模块镜像
npm set registry https://r.npm.taobao.org
# 注册模块镜像
npm set disturl https://npm.taobao.org/dist

# 清空缓存
npm cache clean --force


```

* 2.查看源

```
$ npm config list
; cli configs
metrics-registry = "https://r.npm.taobao.org/"
scope = ""
user-agent = "npm/6.9.0 node/v12.6.0 darwin x64"

; userconfig /Users/glb_gz/.npmrc
disturl = "https://npm.taobao.org/dist"
registry = "https://r.npm.taobao.org/"

; builtin config undefined
prefix = "/usr/local"

; node bin location = /usr/local/Cellar/node/12.6.0/bin/node
; cwd = /Users/glb_gz/Documents/Angular_workplace/Github/ng-zorro-antd
; HOME = /Users/glb_gz
; "npm config ls -l" to show all defaults.
```

## 3.错误处理<a name="error_process"/>

#### 1.Angular 运行命令` ng test `, [出现` jasminerequire not defined `](解决 ng test jasminerequire not defined问题)
* 1.删除 `node_modules` 文件夹，然后运行以下命令 `npm install --registry=https://registry.npm.taobao.org`;


#### 2.使用淘宝镜像的后遗症及解决方法: [npm ERR! Unexpected end of JSON input while parsing near '](https://github.com/vuejs-templates/webpack/issues/990#issuecomment-343736884)

