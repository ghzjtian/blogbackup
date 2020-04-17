---
title: Homebrew 的源配置
tags: [Mac]
categories: [Mac]
date: 2019-12-22 17:46:19
---

> 记录 Mac 下设置 Homebrew 源的过程.

<!-- more -->



## 1.参考
* 1.[(Homebrew有比较快的源（mirror）吗？ - 剑微寒的回答 - 知乎](https://www.zhihu.com/question/31360766/answer/75845239)
* 2.[Mac HomeBrew国内镜像安装方法](https://juejin.im/post/5c738bacf265da2deb6aaf97)
* 3.[替换及重置Homebrew默认源](https://lug.ustc.edu.cn/wiki/mirrors/help/brew.git)


## 2.替换源过程
* 1.替换 brew.git

```
// 清理 代理
$:homebrew-core glb_gz$ noproxy
clear proxy done
cd "$(brew --repo)"
$:Homebrew glb_gz$ pwd
/usr/local/Homebrew
// 替换前
$:Homebrew glb_gz$ git remote get-url origin
https://github.com/Homebrew/brew
// 替换
$:Homebrew glb_gz$ git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
// 替换后
$:Homebrew glb_gz$ git remote get-url origin
https://mirrors.ustc.edu.cn/brew.git
$:homebrew-core glb_gz$ brew update
Already up-to-date.

```

* 2.替换 homebrew-core.git

```
// 清理 代理
$:homebrew-core glb_gz$ noproxy
clear proxy done
$:Homebrew glb_gz$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
$:homebrew-core glb_gz$ pwd
/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core
// 替换前
$:homebrew-core glb_gz$ git remote get-url origin 
https://github.com/Homebrew/homebrew-core
//替换后
$:homebrew-core glb_gz$ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
$:homebrew-core glb_gz$ git remote get-url origin 
https://mirrors.ustc.edu.cn/homebrew-core.git
$:homebrew-core glb_gz$ brew update
Already up-to-date.


```