---
title: Xamarin出错记录
tags: [Xamarin]
categories: [Xamarin]
date: 2019-03-14 17:46:19
---

> 记录下 Xamarin 在 `Visual Studio for Mac` 环境下的错误及解决方法

<!-- more -->

## 1.错误及解决方法的记录

***

## 1.错误及解决方法的记录

> 如果遇到奇怪的错误(第一次编译运行可以，没任何改动，第二次编译运行却不能运行)，可以先 卸载 了 APP,清理 VS 的缓存,然后再重新运行.
> 最好用 iOS 去运行测试.

### 1.Xamarin 项目编译时,出现 `The "Microsoft.Build.Tasks.Git.LocateRepository" task failed unexpectedly.System.BadImageFormatException: Method has no body` Error.
* 1.查到信息可能是 `dotnet 的版本问题`: [The "Microsoft.Build.Tasks.Git.LocateRepository" task failed unexpectedly.](https://github.com/dotnet/sourcelink/issues/107)
	* 1.查看 Mac 中的 dotnet 的版本: `glb_gz$ dotnet --list-sdks
2.1.302 [/usr/local/share/dotnet/sdk]`
	* 2.下载 `dotnet` 安装包: 
		* 1.[https://docs.microsoft.com/en-us/dotnet/core/tools/global-json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json)
		* 2.[https://dotnet.microsoft.com/download/thank-you/dotnet-sdk-2.2.101-macos-x64-installer](https://dotnet.microsoft.com/download/thank-you/dotnet-sdk-2.2.101-macos-x64-installer)
		* 3.然后重新打开项目并编译，但结果还是一样.
	* 3.下载 `Microsoft.Build.Tasks` 包:
		* 0.升级 VS  到 7.7,并且升级其他的一些工具,如: Mono,JDK,Xamarin.Android 等等.
		* 1.安装 PM 命令行工具到 VS 中: [https://github.com/mrward/monodevelop-nuget-extensions](https://github.com/mrward/monodevelop-nuget-extensions#installation)
		* 2.安装 `Microsoft.Build.Tasks` 到项目中: [https://www.nuget.org/packages/Microsoft.Build.Tasks.Git/](https://www.nuget.org/packages/Microsoft.Build.Tasks.Git/)
		* 3.在 `View/Pads/Package Console Extension` 中打开 `Package Console Extension`,然后安装 `Install-Package Microsoft.Build.Tasks.Git -Version 1.0.0-beta2-18618-05` ,但是没有成功，提示: `Command Install-Package not found`.  
		* 4.在 项目路径的终端中运行: `dotnet add package Microsoft.Build.Tasks.Git --version 1.0.0-beta2-18618-05` ,安装成功，但是还是报同样的错误.
	* 4.正式放弃治疗!!!


### 2.获取项目文件数据的时候，出现 `NULL exception error.`
* 1.用 `Assembly.GetExecutingAssembly().GetManifestResourceStream()`方法获取文件的数据时，一直返回 null.
* 2.解决方法:
	* 1.右击需要获取数据的文件(图片的获取也一样的道理)，然后选择 `Build Action` -> `EmbeddedResource` ,就可以了.

![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190118_1.png)

### 3.遇到错误 ` Error APT0000: Error retrieving parent for item: No resource found that matches the given name 'Theme.AppCompat.Light.DarkActionBar'. (APT0000) (BLE_APP2.Android)`
* 1.查到是 Packages 中 `Android.Support.v7.AppCompat` 版本太低，所导致,遂想更新到最新的版本.
* 2.无法更新，报错误 ` Package Xamarin.Android.Support.Design 27.0.2.1 is not compatible with monoandroid71 (MonoAndroid,Version=v7.1). Package Xamarin.Android.Support.Design 27.0.2.1 supports: monoandroid81 (MonoAndroid,Version=v8.1)`.
* 3.故想更新 `Mono.Android` 的版本，所以在 `右键 Project -> options -> Build -> General -> Compile using Android Version` 选择目标的版本即可.
* 4.然后更新 `Android.Support.v7.AppCompat` ，就可以正常地编译运行了.

### 4.遇到错误 `MTOUCH: Error MT2101: Can't resolve the reference ... , referenced from the method ... (MT2101) (BLE_List_Connect.iOS)`
* 1.把项目回退，然后再一个文件一个文件地添加,测试是否重现错误，.

### 5.遇到错误 在 `MainPageViewModel` 中取得 `App.xaml.cs` 中静态变量的值为空.
* 1.项目用 `Prism` 做基础的框架,然后从 `App.xaml.cs` 跳转到 `MainPageViewModel.cs` , `App.xaml.cs` 中有个 Static 变量，在 `MainPageViewModel.cs ` 的构造方法中获取这个变量的值，会一直显示为 `null`.
	* 1.在除了 构造方法 的其他地方获取这个 Static 变量的值就没问题.
	* 2.尝试用 值传递的方式，直接传送数据到第二个页面(也是不行!!!).

	```
	 protected override async void OnInitialized()
        {
            InitializeComponent();

            var navigationParams = new NavigationParameters();
            navigationParams.Add("adapter",Adapter);
            await NavigationService.NavigateAsync("NavigationPage/MainPage", navigationParams);
        }
	```

	* 3.在 `MainPageViewModel.cs / OnNavigatedTo` 中获取这个 Static 的值，也是不行!!!


### 6.`ble.net` 包在 GG 项目中一直连接不成功.
* 0.iOS 手机.
* 1.就算时回滚了项目也不行(以前是可以的).
* 2.但 `Ble_list_conn` 项目用 `ble.net` 包，同样的手机就很正常.
* 3.现在查到可能是 `Scan` 与 `Connect` 有冲突，所以现在尝试在 `connect` 的时候,把 `Scan` 停止掉.
* 4.证实是 `Scan` 与 `Connect` 有冲突.
	
### 7.出现 `Do you have a shared runtime build of your app with AndroidManifest.xml` 错误

* 1.[参考](https://forums.xamarin.com/discussion/128386/i-need-to-manually-uninstall-the-app-every-time-i-want-to-deploy-on-my-device)

```

Forwarding debugger port 8847
Detecting existing process
> am start -n "com.greenworkstools.mowergg/md5e58438a37f85ea0542e2cf77e1751acd.MainActivity"
> Starting: Intent { cmp=com.greenworkstools.mowergg/md5e58438a37f85ea0542e2cf77e1751acd.MainActivity }
...
com.greenworkstools.mowergg/files/.__override__, app_libdir: /data/app/com.greenworkstools.mowergg-6BHblXJgnD5enhv8O4gR5A==/lib/arm64 nor in previously printed locations.
[monodroid] Do you have a shared runtime build of your app with AndroidManifest.xml android:minSdkVersion < 10 while running on a 64-bit Android 5.0 target? This combination is not supported.
[monodroid] Please either set android:minSdkVersion >= 10 or use a build without the shared runtime (like default Release configuration).
```

* 2.解决方法: 
	* 1.把 `Options -> Android Build -> General -> UseShared Mono Runtime ` 的勾去掉.
	![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190328_1.png)
	
### 8.出现 `Version conflict detected for Xamarin.Android.Support.Compat` 错误
* 1.相应安装 `ble.net.android` 包，但提示 `Version conflict detected for Xamarin.Android.Support.Compat. Install/reference Xamarin.Android.Support.Compat 28.0.0.1 directly to project MowerGG.Android to resolve this issue.` 错误.
* 2.参考 [https://github.com/jguertl/SharePlugin/issues/91](https://github.com/jguertl/SharePlugin/issues/91) 说是可以手动解决，以下是我手动解决这个问题的方法:
	* 1.用文本编译器打开 `.Android.csproj` 文件.
	![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190402_2.png)
	* 2.找到所需要手动更改版本的包名，然后手动更改版本.
	![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190402_1.png)
	
	
### 9.出现 `not enough space`

* 1.详细的错误 log 为 `Xamarin Mono.AndroidTools.InstallFailedException: Unexpected install output: Error: android.os.ParcelableException: java.io.IOException: Requested internal only, but not enough space`。
	* 1.在 `Android Studio` 上安装其他的 APP 也安装不成功，提示 `Android Studio Application Failed uninstall the existing application` 信息.

* 2.解决方法:
	* 1.证实不关手机存储大小的问题，我手机有 500 M 的可存储空间，但还是安装不上.
	* 2.把手机中的旧的 APP(相同一个 APP) 卸载掉.
	* 3.把手机中有关 `Xamarin` 的 Library 卸载掉, 如在 `华为 Honor V8(KNT-UL10),EMUI 8.0.0,Android 8.0.0` 中的路径为: `设置 -> 应用和通知 -> 应用管理 -> Xamarin.Android API-XX Support` 卸载掉.
	![](http://pic.pgyjz.cn/blog/Xamarin/1301554961700_.pic.jpg)
	* 4.这样 `Android Studio ` 的 APP 可以运行，但是 `Visual Studio` APP 还是不能运行安装.
	* 5.参考下面的  `10.出现 TypeLoadException` ,然后手机让 内部的存储 至少有 1G 的可用空间，`Visual Studio` 才安装上了 APP.

### 10.出现 `TypeLoadException`
* 1.一打开 APP 就 Crash,完整的 Log 为: `this.InitializeComponent() System.TypeLoadException`:

![](http://pic.pgyjz.cn/blog/Xamarin/screenshot 2019-04-03 at 15.18.39.png)

* 2.参考: [Unhandled Exception: System.TypeLoadException: Could not resolve type with token 010000c1](https://forums.xamarin.com/discussion/127860/unhandled-exception-system-typeloadexception-could-not-resolve-type-with-token-010000c1)的做法，解决了问题

```
1. Clean all 
2. Close IDE
3. uninstall the APP from your phone.
4. Remove all bin and obj folders from all projects (shared and device specific)
5. Restart IDE, build and run
```


