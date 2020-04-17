---
title: Git 的学习
tags: [Git]
categories: [Git]
date: 2020-04-01 17:46:19
---

> 记录 Git 一些常用的命令.

<!-- more -->

[1.Stash your changes before switching branch with Git](http://www.codeblocq.com/2016/02/Stash-your-changes-before-switching-branch/)

#### 1.在 branch1 中做了修改，然后 stash the change , 然后修改文件 , 做完操作后再 stash 拿取刚刚的修改(几个 stash 之间的切换)(测试有conflict 与 没 conflict 的情况).
* 1.如果有 conflict, `git stash pop ` 无效, 并且报错

```
$ git stash pop
error: Your local changes to the following files would be overwritten by merge:
	testgitstash/file1.txt
Please commit your changes or stash them before you merge.
Aborting
```
* 2.需要先 add ,然后再 `git stash pop`, 这样才不报错，但是 文件中有 conflict 的信息

```
$ git status
On branch develop
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   .DS_Store
	modified:   testgitstash/file2.txt

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

	both modified:   testgitstash/file1.txt

```

查看 `testgitstash/file1.txt ` 文件的内容，会发现有 conflict 的文本.

```
test1
<<<<<<< Updated upstream
change2
=======
change 1.
>>>>>>> Stashed changes

```

#### 2.在 branch1 中做了修改，然后 stash the change , 然后切换到 branch2 , 做完操作后再回来这个 branch1, 拿取刚刚的修改.
#### 3.在 branch1 中做了修改，然后 stash the change, 然后从 remote/branch1 pull code, 测试有 conflict 与没 conflict 的情况.
#### 4.在 branch1 中做了修改，然后 stash the change, 然后从 branch2 merge code to branch, 测试有 conflict 与没 conflict 的情况.




