---
title: Blog搭建
tags: [blog]
categories: [blog]
date: 2018-09-10 17:46:19
---

> 记录个人博客的搭建过程(GitHub Pages + Hexo).

<!-- more -->

## 1.相关参考
* [1.Websites for you and your projects.](https://pages.github.com/)

## [2.Hexo搭建](#hexo_deploy)

## [3.yilia主题](#yilia_theme)

## [4.GitBook 遇到的相关的问题](#gitbook_issues)
* 1.问题一
* 2.问题二
* 3.问题三

## [5.自定义域名指向](#custom_domain)

***
***
***


## 2.Hexo搭建<a name="hexo_deploy"/>

#### 1.安装 Hexo

>[https://hexo.io/zh-cn/docs/](https://hexo.io/zh-cn/docs/)

```
npm install -g hexo-cli
```

#### 2.建站

>参考:[http://www.jianshu.com/p/834d7cc0668d](http://www.jianshu.com/p/834d7cc0668d)

* 1.初始化仓库

```
hexo init yourname
cd yourname
npm install
```

* 2.修改 _config.yml 文件.

<img src="/assets/imgs/blog/Snip20171111_8.png" width="100%" height="100%">

>记得要有缩进!: [https://hexo.io/zh-cn/docs/deployment.html](https://hexo.io/zh-cn/docs/deployment.html)
>
>记得添加空格:  [https://github.com/hexojs/hexo/issues/1154](https://github.com/hexojs/hexo/issues/1154)



#### 3.发布

>参考:[http://www.jianshu.com/p/834d7cc0668d](http://www.jianshu.com/p/834d7cc0668d)

```
hexo clean
hexo g
hexo d
```



#### 4.错误

* 1.发现错误 `Deployer not found: git`,就算是像 '教程1' 说的那样，把 git 改为 github ,再运行 `npm install hexo-deployer-git --save` 也没用.

>教程1: [https://github.com/hexojs/hexo/issues/1040](https://github.com/hexojs/hexo/issues/1040)

```
MBP:myBlog tianzeng$ hexo d
ERROR Deployer not found: github
```

* 2.解决，原来是在出现错误后，运行
* 1.Step 1

```
MBP:MyBlog2 tianzeng$ hexo deploy
ERROR Deployer not found: git
MBP:MyBlog2 tianzeng$ npm install hexo-deployer-git --save
```

* 2.Step 2

```
MBP:MyBlog2 tianzeng$ hexo d
INFO Deploying: git
INFO Setting up Git deployment...
```

这样就可以了.

<img src="/assets/imgs/blog/Snip20171111_9.png" width="100%" height="100%">



#### 5.脚本

>参考: [https://github.com/qinjx/30min_guides/blob/master/shell.md](https://github.com/qinjx/30min_guides/blob/master/shell.md)

* 在 Mac 上自动完成 blog 的上传和 Git 备份!

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

<img src="/assets/imgs/blog/Snip20171111_10.png" width="100%" height="100%">

***

#### 6.把 CNAME 添加到项目中

>参考:[http://webcache.googleusercontent.com/search?q=cache:Y4TSEfU_xUoJ:jeasonstudio.github.io/2016/05/26/Mac%25E4%25B8%258A%25E6%2590%25AD%25E5%25BB%25BA%25E5%259F%25BA%25E4%25BA%258EGitHub-Page%25E7%259A%2584Hexo%25E5%258D%259A%25E5%25AE%25A2/+&cd=2&hl=zh-CN&ct=clnk&gl=ph](http://webcache.googleusercontent.com/search?q=cache:Y4TSEfU_xUoJ:jeasonstudio.github.io/2016/05/26/Mac%25E4%25B8%258A%25E6%2590%25AD%25E5%25BB%25BA%25E5%259F%25BA%25E4%25BA%258EGitHub-Page%25E7%259A%2584Hexo%25E5%258D%259A%25E5%25AE%25A2/+&cd=2&hl=zh-CN&ct=clnk&gl=ph)

* 1.把 CNAME 文件放到 /blog/themes/landscape/source 目录下即可(其他想同步到 github 上的文件也是同理).



#### 7.文章开头title
>标题,标签，种类,日期.

```
---
title: Hello World
tags: [tags]
categories: [categories]
date: 2017-11-11 11:46:19
---
```

***

## 3.yilia主题<a name="yilia_theme"/>

>GitHub: [https://github.com/litten/hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia)


#### 1.git clone 主题.
#### 2.把主题放到 themes 下面.
#### 3.在 `_config.yml` 中把

```
#theme: landscape
theme: hexo-theme-yilia
```


#### 4.点击 '所有文章' 按钮，但什么也没显示
> 解决方法:需要设置缺失模块。

* 1、请确保node版本大于6.2
* 2、在博客根目录（注意不是yilia根目录）执行以下命令：

`
npm i hexo-generator-json-content --save
`

3、在根目录的 `_config.yml` 里添加配置：

```
jsonContent:
meta: false
pages: false
posts:
title: true
date: true
path: true
text: false
raw: false
content: false
slug: false
updated: false
comments: false
link: false
permalink: false
excerpt: false
categories: false
tags: true
```


#### 5.在需要截断的地方，添加以下代码 `<!-- more -->` ,这样在主页显示的时候，就会显示 `more` 前面的一段预览代码.


#### 6.`themes`中的`config.yml` 例子

```
# Header

menu:
  主页: /

# SubNav
subnav:
  github: "https://github.com/ghzjtian"
  #weibo: "#"
  #rss: "#"
  #zhihu: "#"
  qq: "2941249122"
  #weixin: "#"
  #jianshu: "#"
  #douban: "#"
  #segmentfault: "#"
  #bilibili: "#"
  #acfun: "#"
  mail: "mailto:wyzjtian@163.com"
  #facebook: "#"
  #google: "#"
  #twitter: "#"
  #linkedin: "#"

rss: /atom.xml

# 是否需要修改 root 路径
# 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，
# 请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。
root: /

# Content

# 文章太长，截断按钮文字
excerpt_link: more
# 文章卡片右下角常驻链接，不需要请设置为false
show_all_link: '展开全文'
# 数学公式
mathjax: false
# 是否在新窗口打开链接
open_in_new: false

# 打赏
# 打赏type设定：0-关闭打赏； 1-文章对应的md文件里有reward:true属性，才有打赏； 2-所有文章均有打赏
reward_type: 2
# 打赏wording
reward_wording: '谢谢你的奖赏'
# 支付宝二维码图片地址，跟你设置头像的方式一样。比如：/assets/img/alipay.jpg
alipay: /assets/imgs/alipay.png
# 微信二维码图片地址
weixin: /assets/imgs/wechat.png

# 目录
# 目录设定：0-不显示目录； 1-文章对应的md文件里有toc:true属性，才有目录； 2-所有文章均显示目录
toc: 1
# 根据自己的习惯来设置，如果你的目录标题习惯有标号，置为true即可隐藏hexo重复的序号；否则置为false
toc_hide_index: true
# 目录为空时的提示
toc_empty_wording: '目录，不存在的…'

# 是否有快速回到顶部的按钮
top: true

# Miscellaneous
baidu_analytics: ''
google_analytics: ''
favicon: /favicon.png

#你的头像url
avatar: /assets/imgs/gz.jpeg

#是否开启分享
share_jia: true

#评论：1、多说；2、网易云跟帖；3、畅言；4、Disqus；5、Gitment
#不需要使用某项，直接设置值为false，或注释掉
#具体请参考wiki：https://github.com/litten/hexo-theme-yilia/wiki/

#1、多说
duoshuo: false

#2、网易云跟帖
wangyiyun: false

#3、畅言
changyan_appid: false
changyan_conf: false

#4、Disqus 在hexo根目录的config里也有disqus_shortname字段，优先使用yilia的
disqus: false

#5、Gitment
gitment_owner: false      #你的 GitHub ID
gitment_repo: ''          #存储评论的 repo
gitment_oauth:
  client_id: ''           #client ID
  client_secret: ''       #client secret

# 样式定制 - 一般不需要修改，除非有很强的定制欲望…
style:
  # 头像上面的背景颜色
  header: '#4d4d4d'
  # 右滑板块背景
  slider: 'linear-gradient(200deg,#a0cfe4,#e8c37e)'

# slider的设置
slider:
  # 是否默认展开tags板块
  showTags: true

# 智能菜单
# 如不需要，将该对应项置为false
# 比如
#smart_menu:
#  friends: false
smart_menu:
  innerArchive: '所有文章'
  friends: false
  aboutme: '关于我'

friends:
  友情链接1: http://localhost:4000/
  友情链接2: http://localhost:4000/
  友情链接3: http://localhost:4000/
  友情链接4: http://localhost:4000/
  友情链接5: http://localhost:4000/
  友情链接6: http://localhost:4000/

aboutme: 毕业于华师，工作在广州<br><br>勤勤恳恳搬砖，筑起自己天地

```

***

## 4.GitBook 遇到的相关的问题<a name="gitbook_issues"/>

#### 1.问题一:

> 这两天想用GitBook写一点东西，突然发现导出的html的导航条不能点击(点击没反应)，如:

![Snip20170829_2.png](http://upload-images.jianshu.io/upload_images/2079545-69facfc3baec7fac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我的配置是这样的，
```
$ GitBook --version
CLI version: 2.3.2
GitBook version: 3.2.3
```

* 解决方法:
经过一番的google,发现是 `GitBook version` 的问题.[参考: https://segmentfault.com/q/1010000006051743](https://segmentfault.com/q/1010000006051743)

	* 1.build的时候指定版本2.6.7，就可以的了: `$ gitbook build --gitbook=2.6.7`

	* 2.也可以在 `book.json` 中指明要编译的GitBook的版本: `"gitbook": "2.6.7",`

***

#### 2.问题二:

> 在指明了GitBook的version后，编译 `$ gitbook build` 时又发现了另外的一个问题，出错: `"Cannot find module 'internal/fs'"`

* 解决方法
[https://github.com/npm/npm/issues/14232](https://github.com/npm/npm/issues/14232)
是node 的问题，把node 切换回旧的就行了,node 切回前:

```
$ node -v
v8.4.0
$ npm -v
5.3.0
```

切回后:

```
$ node -v
v6.11.2
$ npm -v
3.10.10
```

* node切换的步骤:

 * 1.下载 `n`: `$ npm install -g n`
 * 2.下载node 的LTS版本,这样就可以了:

```
$ sudo n lts

     install : node-v6.11.2
       mkdir : /usr/local/n/versions/node/6.11.2
       fetch : https://nodejs.org/dist/v6.11.2/node-v6.11.2-darwin-x64.tar.gz
######################################################################## 100.0%
   installed : v6.11.2
```

* 这样就可以用version 2.6.7进行愉快地导出html了:

```
$ gitbook build
(node:8140) fs: re-evaluating native module sources is not supported. If you are using the graceful-fs module, please update it to a more recent version.
(node:8140) fs: re-evaluating native module sources is not supported. If you are using the graceful-fs module, please update it to a more recent version.
info: loading book configuration....
warn: gitbook version specified in your book.json might be too strict for future patches, '2.x.x' is more adequate 
info: OK 
info: load plugin gitbook-plugin-tbfed-pagefooter ....OK 
info: load plugin gitbook-plugin-splitter ....OK 
info: load plugin gitbook-plugin-anchor-navigation-ex ....OK 
info: load plugin gitbook-plugin-highlight ....OK 
info: load plugin gitbook-plugin-search ....OK 
info: load plugin gitbook-plugin-sharing ....OK 
info: load plugin gitbook-plugin-fontsettings ....OK 
info: >> 7 plugins loaded 
info: start generation with website generator 
info: clean website generator
info: OK 
info: generation is finished 

Done, without error
```

#### 3.问题三
* 1.出现 `Cannot find module 'internal/util/types'` 错误
	* 1.怀疑是 Mac 升级了相关的 `node,npm` 等等引起的错误(gitbook v2.6.7 支持的 node 的版本太低,现在的 Mac 的相关软件的版本比较高)，将不做处理，怕引起相关软件的不兼容.
	* 2.目前 Mac 的软件的版本为:
	
```
MB-Pro-2:ge_li_bo_pei_xun tianzeng$ node -v
v10.3.0
MB-Pro-2:ge_li_bo_pei_xun tianzeng$ npm -v
6.1.0
```


***

## 5.自定义域名指向<a name="custom_domain"/>

#### 方法 1:
* 1.在 Github 的仓库 `Settings` 中设置

#### 方法 2:
* 1.在 本地仓库的 `相应的主题/source` 下添加 `CNAME` 文件，并在里面放上想要的 `domain` ,如:
<img src="/assets/imgs/blog/Snip20181108_3.png" width="80%" height="80%">





