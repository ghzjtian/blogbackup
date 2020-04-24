---
title: 标题
tags: [标签]
categories: [种类]
date: 2019-08-11 17:46:19
---




<!-- more -->

## package.json 版本符号

[* 1.package.json中^符号和~符号前缀的区别](https://www.cnblogs.com/chenyablog/p/7989939.html)
[* 2.dependencies§](https://docs.npmjs.com/files/package.json#dependencies)



```
版本号 x.y.z ：
 
z ：表示一些小的bugfix, 更改z的号，
 
y ：表示一些大的版本更改，比如一些API的变化
 
x ：表示一些设计的变动及模块的重构之类的，会升级x版本号
 
在package.json里面dependencies依赖包的版本号前面的符号有两种，一种是~，一种是^。
 
~的意思是匹配最近的小版本 比如~1.0.2将会匹配所有的1.0.x版本，但不匹配1.1.0
 
^的意思是最近的一个大版本 比如1.0.2 将会匹配 所有 1.x.x, 但不包括2.x.x
```