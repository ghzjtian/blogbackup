---
title: iOS国际化
tags: [iOS]
categories: [iOS]
date: 2018-10-12 17:46:19
---


# 1.iOS 的国际化
> 参考:[iOS-应用名称和内容国际化](https://www.jianshu.com/p/f5e0d91a78ce)

<!-- more -->

## 1.iOS 的国际化有 名称国际化 ，内容国际化
## 2.内容国际化分为: .xib/storyboard 的国际化 ，.m 文件的国际化
* 1.下面详细说说 .m 文件的国际化,及其步骤:

### 3.m文件国际化的步骤
* 1.新建一个 繁体中文 的目录
<img src="/assets/imgs/iOS/Snip20181012_5.png">
* 2.新建后，可以在项目的目录中看到新建了一个 `zh-Hant.lproj` 文件.
<img src="/assets/imgs/iOS/Snip20181012_6.png">
* 3.如果是只要跟随系统语言的翻译，就可以直接用下面的代码去获取:
```
NSString *alertTitle = NSLocalizedString(@"BasicAlertTitle", @"Basic Alert Style");
```
* 4.如果系统的语言为 `en` ,但是我想 APP 显示 `zh-Hant` 中文繁体,可以这样设置:

```
    NSString *bundlePath = [[NSBundle mainBundle] pathForResource:@"zh-Hant" ofType:@"lproj"];
    
    NSString *languageStr = [[NSBundle bundleWithPath:bundlePath] localizedStringForKey:(key) value:nil table:@"Localizable"];
```


