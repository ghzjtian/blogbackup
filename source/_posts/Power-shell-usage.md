---
title: Powershell 的用法
tags: [Mac]
categories: [Mac]
date: 2019-12-29 17:46:19
---


> 记录 Mac 上 Powershell 的安装及使用

<!-- more -->

## 1.参考
## 2.Mac 安装 Powershell
## 3.Powershell 使用


***
***
***

## 1.参考

* 1.[Installing PowerShell Core on macOS](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6)
* 2.[Multiple foreground colors in PowerShell in one command](https://stackoverflow.com/questions/2688547/multiple-foreground-colors-in-powershell-in-one-command)
* 3.[Run PowerShell 脚本](https://yanxyz.github.io/powershell/scripts/)


## 2.Mac 安装 Powershell

```
//  install PowerShell from brew.
brew cask install powershell
// 进入 Powershell
pwsh
// 退出 Powershell
exit
```

## 3.Powershell 使用

* 1.输出

```
Write-Host "Blue" -ForegroundColor Blue
```

* 2.退出

```
exit 0
```

* 3.方法

```
// 无参
function listAllExchange {
}
listAllExchange

// 一个参数
function customEchoRed($message) {
    Write-Host $message -ForegroundColor Red
}
customEchoRed("abc")

// 多个参数
function executeCommand($message, $rabbit_mq_command){
}
executeCommand "List all exchanges"  "rabbitmqadmin list exchanges"
```