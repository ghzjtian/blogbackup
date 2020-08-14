---
title: Node 学习
tags: [Node]
categories: [Node]
date: 2020-04-18 17:46:19
---


> 初涉 Node

<!-- more -->

## 1.参考
* 1.[Run a node.js app as a background service](https://stackoverflow.com/questions/4018154/how-do-i-run-a-node-js-app-as-a-background-service/25998406#25998406)
* 2.[Run node.js service with systemd](https://www.axllent.org/docs/view/nodejs-service-with-systemd/)


## 2.学习例子

* 1.简单例子

> 使用方法: 1.用 terminal 定位到 `index.js` 文件所在的路径，然后执行命令 `node index.js` 即可新建一个服务器, 最后用浏览器访问 `http://localhost:8888` 即可看到效果.

```
// index.js
let http = require('http')

http.createServer((require,res) => {
                  
                  
                  console.log('success!');
                  
                  res.writeHead(200, { "Content-Type":"text/plain;charset=utf-8" });
                  
                  res.end(JSON.stringify({
                                         result: 'OK',
                                         message: '成功/Success!'
                                         }));
                  
                  }).listen(8888)


```