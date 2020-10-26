---
title: Appium-Learn(javascript)
tags: [auto-test]
categories: [auto-test]
date: 2020-07-16 18:46:19
---


> APP UI 自动化测试(使用 javascript 语言)


<!-- more -->

# 1.参考
# 2.环境搭建
# 3.例子

***
***
***


# 1.参考

* 1.[11 Best Automation Tools For Testing Android Applications (Android App Testing Tools)](https://www.softwaretestinghelp.com/5-best-automation-tools-for-testing-android-applications/)
* 2.[Appium 移动应用自动化](http://appium.io/)
* 3.[Getting started](http://appium.io/docs/en/about-appium/getting-started/?lang=zh)
* 4.[Appium-Youtube](https://www.youtube.com/watch?v=i1tQ1pjEFWw)
* 5.[使用 node（wd）编写 Appium 测试用例](https://github.com/HuJiaoHJ/blog/issues/3)
* 6.[admc / wd](https://github.com/admc/wd)
* 7.[Appium Github](https://github.com/appium/appium)
* 8.[WD API](https://github.com/admc/wd/blob/master/doc/api.md)

# 2.环境搭建
#### 1.系统: Mac 10.15.5 Catalina
#### 2.设置 `zsh shell` 的 PATH(.zshrc 文件)

```
export ANDROID_HOME=/Users/glb_gz/Library/Android/sdk
export PATH=$ANDROID_HOME/platform-tools:$PATH
export PATH=$ANDROID_HOME/tools:$PATH
export PATH=$ANDROID_HOME/tools/bin:$PATH

export JAVA_HOME=$(/usr/libexec/java_home)
export PATH=$JAVA_HOME/bin:$PATH
```



# 3.例子
## 1.用命令行运行

> 打开 模拟器 的 Calculator 应用
> Javascript WebdriverIO

* 1.打开 `Android Emalator`. 打开 `Android Studio` -> `Tools` -> `AVD Manager`, 选择设备打开.
* 2.在 `Terminal` 中运行 `adb logcat > logfile.txt`(把 ADB 的日志记录到文件中，方便后面的查找) , 然后打开 Calculator APP， 然后在 logfile.txt 中查找 `for activity` 文本，看到 `Calculator ` 后即可查到包名及 Activity.  
* 2.也可以先打开 `Calculator` APP, 然后执行以下脚本，即可看到 `Calculator` APP 的信息(这种方法显示的 `MainActivity` 信息可能不正确)

```
$ adb shell dumpsys window windows | grep mFocusedApp

// Output
 mFocusedApp=AppWindowToken{78cea5a token=Token{93aba05 ActivityRecord{d02a17c u0 com.android.calculator2/.Calculator t10261}}}
```

* 3.在 `Terminal` 运行 `adb devices`, 查看 Android 模拟器的信息(注意 attached 的状态不能为 offline)(需要保持 `Android Studio` 的开启).

```
List of devices attached

emulator-5554  	device
```

* 4.新建 `index.js`, 填入以下配置信息， 然后先运行 `appium` 命令去启动 `appium`, 再打开一个 `Terminal`, 在 `index.js ` 文件目录下运行命令 `node index.js`

```
// javascript

const wdio = require("webdriverio");
const assert = require("assert");

const opts = {
  path: '/wd/hub',
  port: 4723,
  capabilities: {
    platformName: "Android", 
    deviceName: "emulator-5554",
    appPackage: "com.android.calculator2",
    appActivity: ".Calculator",
  }
};

async function main () {
  const client = await wdio.remote(opts);
  await client.deleteSession();
}

main();
```

## 2.用 `Appium Desktop` 运行.
#### 1.Installing Appium Desktop(1.17.1)
* 1.注意要在 `System preferences` -> `Security & Privacy` 中选择信任.

#### 2.Edit Configurations

```
ANDROID_HOME: /Users/glb_gz/Library/Android/sdk
# 注意 JAVA_HOME 不能像设置 PATH 那样设置为 `/usr/libexec/java_home` 
JAVA_HOME: /Library/Java/JavaVirtualMachines/jdk-11.0.1.jdk/Contents/Home
```

#### 3.JSON Representation 设置以下内容.

```
{
  "platformName": "Android",
  "deviceName": "emulator-5554",
  "appPackage": "com.android.calculator2",
  "appActivity": ".Calculator",
  "noReset": true
}
```


#### 4.