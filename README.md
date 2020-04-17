
# 个人随笔博客

## [1.`bootstrap` 文件的内容](#content_of_bootstrap)
## [2.重新部署的过程](#redeploy_step)


***
***
***


## 1.`bootstrap` 文件的内容<a name="content_of_bootstrap"/>
#### 1. `bootstrap` 文件的介绍
* 里面集成了常用的博客提交操作,如 `生成`，`部署`，`备份到 Git` 的操作，减少每次提交都要手动打命令的麻烦. 

#### 2. `bootstrap` 内容:
```
#!/bin/bash
CMD_PATH=`dirname $0`
cd $CMD_PATH
echo 'Begin---------------------------'
echo '提交到 blog'
echo 'hexo clean:'
hexo clean
echo 'hexo generate:'
hexo generate
echo 'hexo deploy:'
hexo deploy
echo '备份到 Github'
echo 'git add . :'
git add .


echo -n "Enter your commit Text(default is 'Common Update.'):  "
read aComment

# if 判断 https://billie66.github.io/TLCL/book/chap28.html
if [ -z "$aComment" ]; then
#赋值的操作 https://www.jianshu.com/p/24a5230460fd
aComment="Common Update."
fi

echo "git commit -m $aComment"
git commit -m "$aComment"

#echo 'git commit -m "Common Update." :'
#git commit -m "Common Update."


echo 'git push:'
git push
echo 'Finish--------------------------'
exit 0

```

***

## 2.重新部署的过程<a name="redeploy_step"/>
###  1.因为有时候需要在新的电脑里 `clone` 这个项目继续编写文章，现在在这里记录下过程:
* 1.参考:
	* 1.[记录网站诞生过程-使用hexo+github pages](https://www.jianshu.com/p/973e718e3096)
	* 2.[GitHub 中的 Hexo 主题:hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia)

* 2.过程
	* 0.安装好必要的软件工具,如: `homebrew`,`git`,`nodejs`,`hexo` 等等.
		* 1.还要 `npm install hexo-deployer-git --save` 安装 git .
		* 2.还要 `npm install hexo-server --save` 安装 `server`.
		* 3.最后 `npm install`.
	* 1.`clone` 下整个项目
	* 2.`clone` 下 `hexo-theme-yilia` 主题到上一步的 `themes` 目录下，注意 `hexo-theme-yilia` 主题目录的名字要跟 `_config.yml` 文件中 `theme: hexo-theme-yilia ` 的一致.
		* 1.一定要做这一步!!否则会遇到 `WARN  No layout: index.html` 错误及浏览网页时，只有一片空白的情况.
			* 1.[hexo本地测试运行重启后页面空白,提示 : WARN No layout: index.html?](https://www.zhihu.com/question/38781463)
	* 3.然后双击 `bootstrap` 即可自动上传及备份文章.


