---
title: Selenium 自动化测试(javascript)
tags: [auto-test]
categories: [auto-test]
date: 2020-07-11 17:46:19
---

> Selenium 环境的搭建及简单的例子(Javascript)

<!-- more -->


# 1.参考

* 1.[Selenium Javascript](http://www.testclass.net/selenium_javascript/init)
* 2.[JavaScript（Node.js）+ Selenium自动化测试](https://www.cnblogs.com/fnng/p/5854875.html)
* 3.[Setting up your own test automation environment](https://developer.mozilla.org/zh-CN/docs/Learn/Tools_and_testing/Cross_browser_testing/Your_own_automation_environment)
* 4.[Selenium Tests with Mocha and Chai in JavaScript (against BrowserStack)](https://nehalist.io/selenium-tests-with-mocha-and-chai-in-javascript/)
* 5.[Upcoming Webinar: Cross Browser Test Management Using LambdaTest and PractiTest ](https://www.lambdatest.com/blog/mocha-javascript-tutorial-with-examples-for-selenium-testing/)

# 2.搭建步骤
## 1.安装 Node
## 2.安装 selenium webdriver

```
// 工程目录下运行如下命令
npm install selenium-webdriver
```

## 3.下载相应的驱动，使WebDriver能控制你需要测试的浏览器
### 1.下载最新版本的 GeckoDriver ( Firefox) 和 ChromeDriver 驱动.
* 1.注意 `GeckoDriver v0.26.0` 在 Mac OS 10.15 (Catalina)中有认证的问题，详细可看 [macOS notarization](https://firefox-source-docs.mozilla.org/testing/geckodriver/Notarization.html)

### 2.将它们解压到一个简单的目录下，如用户根目录。

### 3.把 chromedriver 和 geckodriver 驱动的目录添加到你的系统 PATH 变量。这应该是从你的硬盘根目录开始的一个绝对路径。举个例子，如果我们使用的是一个 Mac OS X 机器, 用户名为 bob, 我们把驱动放到了用户的根目录下，那这个路径就是 /Users/bob。



# 3.测试
### 1.使用 [mocha](https://mochajs.org/#installation) 框架
### 2.使用 `npm init -y` 创建 `package.json` 文件.
### 3.输入 `npm install mocha` 下载 mocha
### 4.输入 `npm install chai` 下载 [chai](https://www.chaijs.com/) 
### 3.在项目下新建 `test` 目录, 然后写入 mocha 测试案例代码.
### 4.命令行输入 `npm test` 即可看到写的测试案例会开始测试.
### 5.测试代码采用 BDD 风格.

# 4.测试报告输出

* 1.[Mochawesome](https://www.npmjs.com/package/mochawesome)


# 5.发现问题


* 1.发现 selenium 的相关文档在 `javascript` 中不充分，在网络中很难找到相关的解决方案， 如 [`select`](https://www.selenium.dev/docs/site/en/support_packages/working_with_select_elements/) element 的选择与值的改变都没有找到相关的资料.
* 2.`KATALON RECORDER(CHROME EXTENSIONS)` 对 javascript 的支持度也不好，所生成的代码不能使用.

