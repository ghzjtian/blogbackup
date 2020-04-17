---
title: Xamarin开发相关
tags: [Xamarin]
categories: [Xamarin]
date: 2018-12-19 17:46:19
---

> 记录 Xamarin 开发相关的知识和过程记录

<!-- more -->

## [1.获取权限相关](#permission_related)
## [2.错误的记录](#Error_ifo)
## [3.Xamarin 的生命周期](#xamarin_lifecycle)
## [4.Xamarin iOS 开发相关](#xamarin_iOS)
## [5.Xamarin 所用到的第三方包](#third_library)
## [6.Xamarin 相关功能点的开发](#function_develop)
* 1.多语言及本地化的支持
* 2.DependencyService (如果 Xamarin.Forms 不支持一些功能，就需要调用原生的实现方法去获取一些数据)
* 3.加载项目中文件的内容

## [7.VS 的使用相关](#usage_of_vs)
## [8.VS 环境配置项的添加](#environment_config)

***
***
***


## 1.获取权限相关<a name="permission_related"/>
* 1.优秀的第三方框架:[jamesmontemagno/PermissionsPlugin](https://github.com/jamesmontemagno/PermissionsPlugin)


***

## 2.错误的记录<a name="Error_ifo"/>

请到文章 [Xamarin出错记录](http://blog.pgyjz.cn/2019/03/14/Xamarin%E5%87%BA%E9%94%99%E8%AE%B0%E5%BD%95/#more) 处查看.

***

## 3.Xamarin 的生命周期<a name="xamarin_lifecycle"/>

### 1.在 `Xamarin.Forms` 中，APP 运行的生命周期是这样的:
#### 1.Android
* 1.Android:在Android 代码的 `Project ` 中,如果有类继承了 `Android.App.Application` ,就会从该类的 `OnCreate` 开始进入 APP:

```
    [Application(Debuggable = IS_DEBUG, AllowBackup = true, AllowClearUserData = true)]
    public class MyApplication : Application {

        public const Boolean IS_DEBUG =
#if DEBUG
         true;
#else
         false;
#endif

        protected MyApplication(IntPtr javaReference, JniHandleOwnership transfer)
       : base(javaReference, transfer)
        {
        }

        public override void OnCreate()
        {
            base.OnCreate();

            System.Diagnostics.Debug.WriteLine("------->Android Application OnCreate");
        }

    }
```

* 2.然后跳转到继承 `global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity` 的类中运行:

```
    [Activity(Label = "BLE_APP", Icon = "@mipmap/launcher_icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            System.Diagnostics.Debug.WriteLine("------->Android MainActivity OnCreate");

            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(savedInstanceState);
            global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
            //这里就会跳到 Xamarin.Forms 中的类
            LoadApplication(new App());
        }
    }
```

* 3.然后就按照上一步的 `OnCreate ` 去跳到 `Xamarin.Forms` 中指定共有的类去 

#### 2.iOS

* 1.在一个继承自 `global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate` 的类中开始

```
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        //
        // This method is invoked when the application has loaded and is ready to run. In this 
        // method you should instantiate the window, load the UI into it and then make the window
        // visible.
        //
        // You have 17 seconds to return from this method, or iOS will terminate your application.
        //
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {

            System.Diagnostics.Debug.WriteLine("----->iOS AppDelegate App()");

            global::Xamarin.Forms.Forms.Init();
            //这里就会跳到 Xamarin.Forms 中的类
            LoadApplication(new App());



            return base.FinishedLaunching(app, options);
        }
    }
```

* 2.然后就按照上一步的 `FinishedLaunching ` 去跳到 `Xamarin.Forms` 中指定共有的类去 

***

## 4.Xamarin iOS 开发相关<a name="xamarin_iOS"/>
#### 1.问题点
* 1.在调试 iOS 设备时，经常一 install 后 `Visual Studio ` 就与 `iPhone/iPad` 断开连接，看不到相关的 log 信息.
	* 1.找不到解决的方法，暂时只能通过 `Run/Start Debugging` 去查看 log.

#### 2.更新 Prism 中的 Item View
* 1.ListView 中 Item 的 Model 继承 `INotifyPropertyChanged`,并实现其 `event`.
* 2.把 `ListView` 中的 `ItemSource` Item 更新。
* 3.用 `event` 通知更新 View,如更新 `Binding` `ConnStateStr ` 参数的 View 的值: ` PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ConnStateStr)));`

#### 3.Mac 版的 Visual Studio 打开 resx 文件方式
* 1.在做多语言时， Mac 版的 Visual Studio 没有专门的编辑器打开并快速地编辑 resx 文件(Windows 版有).
* 2.每次改动都需要手工复制粘贴 `<data></data>` 中的信息.

![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190408_2.png)

* 3.在网上看到有方法可以添加个编辑器，但是没有验证:
	* 1.[How to edit a .resx file on Mac OS ?](https://medium.com/@jdassonval/how-to-edit-a-resx-file-on-mac-os-c4dac927145c)
	* 2.[GitHub:Visual-Studio-for-Mac-ResxEditor](https://github.com/jzeferino/Visual-Studio-for-Mac-ResxEditor)

***

## 5.Xamarin 所用到的第三方包<a name="third_library"/>

* [1.权限动态获取相关](https://github.com/jamesmontemagno/PermissionsPlugin)
* 2.[二维码 ZXing.Net.Mobile](https://github.com/Redth/ZXing.Net.Mobile)
* 3.[Json 解析 Newtonsoft.Json](https://www.newtonsoft.com/json)
	* 1.Json 与对象间的相互转换.
* 4.[弹出对话框 Acr.UserDialogs](https://github.com/aritchie/userdialogs)
* 6.[圆角图片 Xam.Plugins.Forms.ImageCircle](https://github.com/jamesmontemagno/ImageCirclePlugin) 
* 7.[功能强大的第三方框架 Prism](https://github.com/PrismLibrary/Prism)
	* 1.[Prism.Plugin.Popups](https://github.com/dansiegel/Prism.Plugin.Popups)
	* 2.Documents
		* 1.[Prism-Documentation1](https://github.com/PrismLibrary/Prism-Documentation/blob/master/docs/commanding.md)
		* 2.[Prism_Documentation2](http://prismlibrary.github.io/docs/index.html)
	* 3.功能点
		* 1.快速导航
		* 2.数据快速绑定
		* 3.信息传递
		

***

## 6.Xamarin 相关功能点的开发<a name="function_develop"/>

#### 1.多语言及本地化的支持
* 1.参考:
	* 1.[Xamarin.Forms 本地化](https://docs.microsoft.com/zh-cn/xamarin/xamarin-forms/app-fundamentals/localization/)
	* 2.[How To Localise Your Xamarin.Forms Apps](https://www.mfractor.com/blogs/news/localising-your-xamarin-forms-apps)
* 2.步骤:
	* 1.先用 DepencyService 获取到手机的语言，然后设置 APP 的语言跟手机的一样.
	* 2.图片的本地化
		* 2.1 在 Android 和 iOS 处放置好相关的本地化图片
		![](http://pic.pgyjz.cn/blog/Xamarin/screenshot 2019-02-21 at 11.20.45.png)
		![](http://pic.pgyjz.cn/blog/Xamarin/screenshot 2019-02-21 at 11.20.30.png)
		* 2.2 在需要调用的地方:

```
var flag = new Image();
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                case Device.Android:
                    flag.Source = ImageSource.FromFile("flag.png");
                    break;
                case Device.UWP:
                    flag.Source = ImageSource.FromFile("Assets/Images/flag.png");
                    break;
            }

```

* 3.文字的本地化
	* 1.在 Xamarin.Forms 中添加 resx 文件(先填写缩写的名字然后再改名即可).
	* 2.添加本地化翻译(添加后记得 `clean project`)
	* 3.通过 `AppResources.ResourceName` 使用资源.



#### 2.DependencyService (如果 Xamarin.Forms 不支持一些功能，就需要调用原生的实现方法去获取一些数据)
* 1.参考:
	* 1.[Xamarin.Forms DependencyService](https://docs.microsoft.com/zh-cn/xamarin/xamarin-forms/app-fundamentals/dependency-service/)

* 2.相关的实现(获取 APP 版本信息)
	
<script src="https://gist.github.com/ghzjtian/60fdf98f0d23d52eb3461d080c5f6f61.js"></script>

#### 3.加载项目中文件的内容

<script src="https://gist.github.com/ghzjtian/ee42f6034222d41fd719003c325321da.js"></script>


## 7.VS 的使用相关<a name="usage_of_vs"/>

* 1.在工程中添加一个新的 `Project` 时，添加的步骤为(`Solution`->`Add`->`Add New Project`->`Library` -> `Class Library` 即可以看到新建的 Project ): 

![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190313_1.png)
![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190313_2.png)
![](http://pic.pgyjz.cn/blog/Xamarin/Snip20190313_3.png)



***


## 8.VS 环境配置项的添加<a name="environment_config"/>

> 配置项的添加有很多好处，首先就是 1.提高开发的效率 2.增加不同环境或切换不同的环境时，可以快速准确地修改到目的配置 `Define Symbols`.
> 


### 1.参考:
* 1.[Demystifying Build Configurations](https://devblogs.microsoft.com/xamarin/demystifying-build-configurations/)
	* 1.下面的步骤参考这边文章的.

### 2.步骤:
* 1.双击 `Solution Options`, 然后 `Build -> Configurations`(not Run > Configurations).
* 2.我们要新增一个 GF 的 Debug 配置 和一个 Release 配置，所以分别 Copy 里面已有的 `Debug` 和 `Release` ,并且命名为 `GF-Debug` 和 `GF-Release`

![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_2.png)
![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_3.png)

* 3.在 左上角选择 Android -> GF-Debug ,然后 Build ,但是出现了 `Info.plist not found` 的情况(解决方法为下面的 4,5,6 ).
![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_5.png)

* 4.添加 `GF-Debug` 和 `GF-Release` 的 Platform ,步骤为在 solution 中.

![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_6.png)
![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_7.png)
![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_8.png)

* 5.同理在 `iOS Project` 中重复步骤 4,效果如下
![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_10.png)
![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_11.png)

* 6.回到 `Solution` 下的 `Build-> Configurations -> Configurationn Mappings`,然后选择 iOS 的项目， Configuration 和 Platform 选择我们刚刚创建的，操作如下:

![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_12.png)

* 7.再次编译 Android 的 GF-Debug，就可以通过了.

* 8.但是编译 iOS 的 GF-Release 还是会出现 `Info.plist not found` 的问题，所以解决如下:
	* 1.打开 `MowerGG.iOS.csproj` 文件
	* 2.把所有 `Include=Info.plist condition ...` 改为 `<None Include="Info.plist" />`

![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_14.png)

* 9.如果编译 iOS 时只选择了 `GF-Release` 会出现 `warning MT1043` ,导致无法编译.
	* 1.解决方法为在左上角选择 `GF-Release|iPhone` 配置文件.

	![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_16.png)

	* 2.在操作了上一步后，编译可以通过了，但是发现 `GF-Release|iPhone` 不能部署到手机，错误如下:
	
	```
	warning MT1043: Failed to launch the application using the instruments service. Will try launching the app using gdb service.
Launching 'com.greenguide.app' on the device 'iPhoneGlb6'
Could not find the application 'com.greenguide.app' on the device 'iPhoneGlb6'.
	```

	* 3.在已经可以正常运行的配置上 Copy 一个，然后再修改(如 Copy 一个 Release configuration . )	



### 3.发现的问题点
* 1.如果 `iOS Project` 中没有相关的 Configuration , 就会显示 `Invalid configuration mapping`.

![](http://pic.pgyjz.cn/blog/Xamarin/DevelopRelated/Snip20190705_1.png)


* 2.Android Debug 出现 ` Error MSB6006: "java" exited with code 2. (MSB6006) ` 错误.
	* 1.[解决方法1](https://forums.xamarin.com/discussion/97803/getting-error-java-exe-exited-with-code-2):在 `Build->Android Build->General` 中选择 `Enable Multi-Dex`.









