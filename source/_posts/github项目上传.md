---
title: github项目上传
tags: [Git]
categories: [Git]
date: 2018-01-25 17:46:19
---



* 背景

>本地有项目，需要上传到 github 去保存.

<!-- more -->

***

# 详细步骤

### 1.在本地的 project 中创建一个 git 仓库.

```
git init
git remote add origin git@10.100.1.217:Tim/newProject.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

### 2.错误 及解决的方法

* 1.合并 pull 两个不同的项目

>fatal: refusing to merge unrelated histories

>[解决方法 :](http://blog.csdn.net/lindexi_gd/article/details/52554159)

```
git pull origin master ----allow-unrelated-histories
```

* 2.把一个 git 项目 1 复制到另外一个 git 项目 2 里面,

```
* 1.把 1 的 .git 文件删除掉
* 2.运行 `git rm --cached 1`
* 3.git add .
* 4.git commit -m "Add git1 to git2."

```

### 3.Git远程仓库的版本回退
>[参考1:](https://blog.csdn.net/ligang2585116/article/details/71094887) 

```
方式一：使用revert

git revert HEAD
git push origin master
1
2
方式二：使用reset

git reset --hard HEAD^
git push origin master -f
1
2
二者区别：

revert是放弃指定提交的修改，但是会生成一次新的提交，需要填写提交注释，以前的历史记录都在；
reset是指将HEAD指针指到指定提交，历史记录中不会出现放弃的提交记录。

```

***

* 模板方案:
![](/assets/imgs/github/ScreenShot2018-01-25_13.10.13.png)