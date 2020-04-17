---
title: Mac修改截图的默认文件名
tags: [Mac]
categories: [Mac]
date: 2018-06-01 17:46:19
---



### Mac修改截图的默认文件名

* 1.转载自: [Mac OS X EI Capitan 修改截图的默认文件名](http://www.mycode.net.cn/platform/linux-unix/1398.html)

<!-- more -->

#### 1.方法

* 1.方法就是在重启系统，在启动过程中，按住 ⌘R，在进入恢复界面后，选择上面菜单的 实用工具->终端，打开终端后输入 csrutil disable 关闭掉系统的 SIP 保护机制，然后重启再次进入系统，此时就可以修改系统文件了。接下来按如下步骤修改你需要的效果。

```
// 打开要修改的文件目录
cd /System/Library/CoreServices/SystemUIServer.app/Contents/Resources/zh_CN.lproj
// 转换文件为 xml 格式
sudo plutil -convert xml1 ScreenCapture.strings
// 用 vi 修改转换后的文件
sudo vi ScreenCapture.strings
```

修改为下面的样子:
![](http://p3v7okj0k.bkt.clouddn.com/wp-content/uploads/2016/01/2016-01-02_21.06.20.png)

* 2.按上面的修改保存后，再转换文件为纯二进制的，并重启 SystemUIServer：

```
sudo plutil -convert binary1 ScreenCapture.strings
killall SystemUIServer
```

* 3.如上修改完成后，我截图后文件保存的格式就是 2016-01-02_21.06.20.png 了，根据你自己的需求，你可以修改为任意其他的格式。最后别忘记，再次进入恢复模式，将 SIP 重新开起来。命令是 csrutil enable。

