---
title: 电脑与 手机 通过 USB 进行数据传输
tags: [iOS]
categories: [iOS]
date: 2019-11-27 17:46:19
---


> 电脑 与 手机 通过 USB 进行数据传输

<!-- more -->

## [1.参考](#references)
## [2.过程(电脑Windows/手机iOS)](#steps)
## [3.过程(手机 iOS Xamarin语言 版本)](#steps_iOS_xamarin)

***
***
***

## 1.参考<a name="references"/>
* 1.[iOS and MAC(经过验证可以用)](https://github.com/ghzjtian/peertalk)
	* 1.[kirankunigiri/peertalk-simple (Mac <-> iOS 传输其它的数据](https://github.com/kirankunigiri/peertalk-simple)
* 2.[Windows/iOS(Xamarin) 客户端(Communicating with your iOS app over USB (C# and/or Xamarin))](http://thecodewash.blogspot.com/2017/05/communicating-with-your-ios-app-over.html)
* 3.[使用usbmuxd服务，通过USB连接与PC端、Mac端实现通信，Peertalk的使用](https://www.jianshu.com/p/eba133891ec6)
* 4.[探讨使用USB线缆与iOS App通信的小笔记](https://www.jianshu.com/p/087fd3ccdd9f)
* 5.[iOS 与 Windows 的连接](http://docs.quamotion.mobi/docs/)
* 6.[libimobiledevice(libimobiledevice.1.2.1-r857-win-x64.zip) 的使用视频](https://www.youtube.com/watch?v=34OZp1rrxxg)
* 7.[Windows libimobiledevice-win32/imobiledevice-net 用法示例程序](https://github.com/libimobiledevice-win32/imobiledevice-net/tree/master/iMobileDevice-net.Demo)
* 8.[iMobileDevice API Documentation](https://libimobiledevice-win32.github.io/imobiledevice-net/api/iMobileDevice.html)
* 9.[iOS App连接外设的几种方式](https://www.jianshu.com/p/08da95add4da)


***

## 2.过程(电脑Windows 版本)<a name="steps"/>
#### 1.Windows 安装并启动 `usbmuxd service` 服务
* 1.方法1.安装 iTunes 即会自动带有
* 2.方法2. [直接下载 iTunes.exe](https://apple-mobile-device-support.updatestar.com/), 然后解压，得到 `AppleMobileDeviceSupport64.msi` 文件，双击安装即会安装 `usbmuxd service`.

#### 2.Windows 项目 [导入 `imobiledevice-net` nuget 库](https://www.nuget.org/packages/imobiledevice-net/)

#### 3.数据交互
* 1.监听变化

```
NativeLibraries.Load();
var _iDeviceApi = LibiMobileDevice.Instance.iDevice;
var ret = _iDeviceApi.idevice_event_subscribe(EventCallback(), new IntPtr());

private iDeviceEventCallBack EventCallback()
{
    return (ref iDeviceEvent devEvent, IntPtr data) =>
    {
        Debug.WriteLine("EventCallback() iDeviceEvent");
        switch (devEvent.@event)
        {
            case iDeviceEventType.DeviceAdd:
                Connect(devEvent.udidString);
                break;
            case iDeviceEventType.DeviceRemove:
                break;
            default:
                return;
        }
    };
}

```

* 2.连接

```
private void Connect(string newUdid)
{
    _iDeviceApi.idevice_new(out iDeviceHandle deviceHandle, newUdid).ThrowOnError();
    var error =_iDeviceApi.idevice_connect(deviceHandle, 5050, out iDeviceConnectionHandle connection);
    if (error != iDeviceError.Success) return;
    ReceiveDataFromDevice(connection);
}
```

* 3.接收数据

```
private void ReceiveDataFromDevice(iDeviceConnectionHandle connection)
{
    Task.Run(() =>
    {
        while (true)
        {
            uint receivedBytes = 0;
            _iDeviceApi.idevice_connection_receive(connection, _inboxBuffer, (uint)_inboxBuffer.Length,
                ref receivedBytes);
            if (receivedBytes <= 0) continue;
            // Do something with your received bytes
        }
    });
}
```

* 4.发送数据

```
_iDeviceApi.idevice_connection_send(connection, bytesPending, (uint)bytesPending.Length, ref sentBytes)
```

## 3.过程(手机 iOS Xamarin语言 版本)<a name="steps_iOS_xamarin"/>
#### 1.参考:
* 1.[Walkthrough: Binding an iOS Objective-C Library](https://docs.microsoft.com/en-us/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=macos)
* 2.[Create a Static Library in Xcode](https://www.jianshu.com/p/486e3b737707)
* 3.[iOS 静态库Framework、static Library(.a )、Bundle的浅析与制作](https://www.jianshu.com/p/8e3e6175c5f5)
* 4.[Possible to use Xamarin with iOS - OSX communication over USB? Works in native...](https://forums.xamarin.com/discussion/58933/possible-to-use-xamarin-with-ios-osx-communication-over-usb-works-in-native)


#### 2.有两种方法
##### 1.用 Peertalk 的 Object-C 库，生成 .a 静态库, 然后再用 Xamarin 的 `Creating Bindings with Objective Sharpie`.
* 1.把 Peertalk Object-C 代码生成 .a 库
* 2.建立 `Bindings with Objective Sharpie` Project, 导入相关的 `.h` 文件 和 `.a` 库.
* 3.在 Xamarin 项目中调用 `.a` 库的接口，完成 USB 数据交流的功能.

##### 2.直接了解 Peertalk 的 USB 通讯原理，然后用 Xamarin 实现.

```
 var socket = new Socket(AddressFamily.InterNetwork,
                       SocketType.Stream,
                       ProtocolType.Tcp);

                   socket.Bind(new IPEndPoint(IPAddress.Loopback, 5050));
                   socket.Listen(100);
                   socket.BeginAccept((ar) =>
                   {
                       var connectionAttempt = (Socket) ar.AsyncState;
                       var connectedSocket = connectionAttempt.EndAccept(ar);
                       // Do something with "connectedSocket"
                       
                       // Data send 
                       connectedSocket.Send(bytesSent, bytesSent.Length, 0);
                       // Data Received
                       
                        do
		                {
		                    bytes = connectedSocket.Receive(bytesReceived, bytesReceived.Length, 0);
		                    if (bytes <= 0  ) continue;
		                }
		                while (true);
                       
                       
              }, socket);
```


