---
title: Windows BLE 开发
tags: [BLE]
categories: [BLE]
date: 2019-12-02 17:46:19
---

> 记录 Windows 平台开发 BLE Application 的过程

<!-- more -->

## [1.Windows BLE 开发资料.](#windows_ble_references)
## [2.USB BLE 扩展](#win_ble_extend)
## [3.检查 PC 是否支持蓝牙](#win_ble_check_manual)
## [4.硬件要求](#win_hardware)
## [5.开发步骤](#win_ble_develop)
## [6.问题](#issues)


***
***
***

## 1.Windows BLE 开发资料.<a name="windows_ble_references"/>
			
* 1.[Windows-universal-samples/Samples/BluetoothLE/](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothLE)
* 4.[Windows 8.1 低功耗蓝牙开发](https://www.cnblogs.com/dearsj001/p/BLE4Windows.html)
* 5.[Bluetooth GATT Client](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/gatt-client)
* 6.[How to register for BLE notifcations from a WPF app running on Windows 10 Creators Update?](https://stackoverflow.com/questions/45946559/how-to-register-for-ble-notifcations-from-a-wpf-app-running-on-windows-10-creato)
* 7.[nRF Connect for Desktop](https://www.nordicsemi.com/Software-and-tools/Development-Tools/nRF-Connect-for-desktop/Download#infotabs)
* 8.[NordicSemiconductor/pc-nrfconnect-ble](https://github.com/NordicSemiconductor/pc-nrfconnect-ble)
* 9.BluetoothLEExplorer
	* 1.[Windows Download](https://www.microsoft.com/en-us/p/bluetooth-le-explorer/9n0ztkf1qd98#activetab=pivot:overviewtab)
	* 2.[Windows microsoft/BluetoothLEExplorer source code](https://github.com/Microsoft/BluetoothLEExplorer)

***

## 2.USB BLE <a name="win_ble_extend"/>

> 台式机没有蓝牙硬件，可以买个 USB BLE 芯片去扩展这个功能.

* 1.[ 奥睿科（ORICO）USB蓝牙适配器4.0接收器兼容4.1/4.2/5.0电脑手机耳机音频发射器 黑色-次日达](https://item.jd.com/47752032148.html)

***

## 3.检查 PC 是否支持蓝牙<a name="win_ble_check_manual"/>
* 1.[How to check if your PC supports Bluetooth 4.0](https://winaero.com/blog/how-to-check-if-your-pc-supports-bluetooth-4-0/)
* 2.[How to check Bluetooth Adapter version in Windows 10](https://www.thewindowsclub.com/how-to-check-bluetooth-version-in-windows-10)

***

## 4.硬件要求<a name="win_hardware"/>
* 1.Windows 10
	* 1.Windows 8.1 只支持 Client,为了扩展的需要，最好是用 Windows 10
* 2.电脑带有 BLE 的支持
	* 1.去 “设备管理器”->蓝牙，查看下面的列表，如果里面有“Microsoft Bluetooth LE 枚举器”, 就代表是支持的.
		* 1.如果没有看到 `Microsoft Bluetooth LE 枚举器 `, 去“设置”->“更改电脑设置”->“电脑和设备”->“蓝牙”中，把蓝牙打开. 如果也没有，就证明电脑的硬件不支持 BLE 或 需要去更新系统和蓝牙驱动.

***

## 5.开发步骤<a name="win_ble_develop"/>
#### 1.检查蓝牙的状态:
* 1.Universal Windows Platform (UWP) 程序
```
   Windows.Devices.Bluetooth.BluetoothAdapter adapter = await Windows.Devices.Bluetooth.BluetoothAdapter.GetDefaultAsync();

            if(adapter ==  null)
            {
                MessageDialog msg = new MessageDialog("Error getting access to Bluetooth adapter. Do you have a have bluetooth enabled?", "Error");
                await msg.ShowAsync();

                IsPeripheralRoleSupported = false;
                IsCentralRoleSupported = false;
            }
            else
            { 
                IsPeripheralRoleSupported = adapter.IsPeripheralRoleSupported;
                IsCentralRoleSupported = adapter.IsCentralRoleSupported;
            }
```

* 2.Windows Forms WPF 程序

```
  /**
         *  [How to check if bluetooth is enabled on a device](https://stackoverflow.com/a/48794678)
         * **/
        public bool GetBluetoothIsEnable()
        {
            SelectQuery sq = new SelectQuery("SELECT DeviceId FROM Win32_PnPEntity WHERE service='BthLEEnum'");
            ManagementObjectSearcher searcher = new ManagementObjectSearcher(sq);
            return searcher.Get().Count > 0;
        }

```

#### 2.开始扫描:

```
  if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
            {
                string[] requestedProperties =
                {
                    "System.Devices.GlyphIcon",
                    "System.Devices.Aep.Category",
                    "System.Devices.Aep.ContainerId",
                    "System.Devices.Aep.DeviceAddress",
                    "System.Devices.Aep.IsConnected",
                    "System.Devices.Aep.IsPaired",
                    "System.Devices.Aep.IsPresent",
                    "System.Devices.Aep.ProtocolId",
                    "System.Devices.Aep.Bluetooth.Le.IsConnectable",
                    "System.Devices.Aep.SignalStrength",
                    "System.Devices.Aep.Bluetooth.LastSeenTime",
                    "System.Devices.Aep.Bluetooth.Le.IsConnectable",
                };

                // BT_Code: Currently Bluetooth APIs don't provide a selector to get ALL devices that are both paired and non-paired.
                deviceWatcher = DeviceInformation.CreateWatcher(
                    BTLEDeviceWatcherAQSString,
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);
            }
            else
            {
                string[] requestedProperties =
                {
                    "System.Devices.GlyphIcon",
                    "System.Devices.Aep.Category",
                    "System.Devices.Aep.ContainerId",
                    "System.Devices.Aep.DeviceAddress",
                    "System.Devices.Aep.IsConnected",
                    "System.Devices.Aep.IsPaired",
                    "System.Devices.Aep.IsPresent",
                    "System.Devices.Aep.ProtocolId",
                    "System.Devices.Aep.Bluetooth.Le.IsConnectable",
                    "System.Devices.Aep.SignalStrength",
                };

                // BT_Code: Currently Bluetooth APIs don't provide a selector to get ALL devices that are both paired and non-paired.
                deviceWatcher = DeviceInformation.CreateWatcher(
                    BTLEDeviceWatcherAQSString,
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);
            }

            // Register event handlers before starting the watcher.
            deviceWatcher.Added += DeviceWatcher_Added;
            deviceWatcher.Updated += DeviceWatcher_Updated;
            deviceWatcher.Removed += DeviceWatcher_Removed;
            deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
            deviceWatcher.Stopped += DeviceWatcher_Stopped;

            deviceWatcher.Start();

```

```
   private void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation deviceInfo)
        {

            try
            {
                // Protect against race condition if the task runs after the app stopped the deviceWatcher.
                if (sender == deviceWatcher)
                {
                   
                }
            }
            catch (Exception ex)
            {
                Debug.WriteLine("DeviceWatcher_Added: " + ex.Message);
            }

        }

        private void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate deviceInfoUpdate)
        {
            if (sender == deviceWatcher)
            {
               //  Debug.WriteLine(String.Format("Updated {0}{1}", deviceInfoUpdate.Id, ""));
            }
        }

        private void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate deviceInfoUpdate)
        {
            if (sender == deviceWatcher)
            {
             //   Debug.WriteLine(String.Format("DeviceWatcher_Removed {0}{1}", deviceInfoUpdate.Id, ""));
            }
        }

        private void DeviceWatcher_EnumerationCompleted(DeviceWatcher sender, object e)
        {
            if (sender == deviceWatcher)
            {
                Debug.WriteLine(String.Format("DeviceWatcher_EnumerationCompleted"));
            }
        }

        private void DeviceWatcher_Stopped(DeviceWatcher sender, object e)
        {
            if (sender == deviceWatcher)
            {
                Debug.WriteLine(String.Format("DeviceWatcher_Stopped"));
            }
           
        }

```

#### 3.连接

#### 4.查看 Services
#### 5.查看 Characteristics and set notifications
#### 6.Write data to Specific Characteristics

***

## 6.问题<a name="issues"/>
* 1.在 `References` 里不见了 `Windows` 的 Tab
	* 1.解决方法:
	* [1.How to call Windows.Devices.Bluetooth in WPF app RRS feed](https://social.msdn.microsoft.com/Forums/vstudio/en-US/cbd15b39-f26b-4cc3-8ff1-edbb4beb52fe/how-to-call-windowsdevicesbluetooth-in-wpf-app?forum=wpf)
	* 2.在 `csproj` 处添加 `<TargetPlatformVersion>10.0</TargetPlatformVersion>`

* 2.出现 `未能使用“通用身份验证”连接到设备“127.0.0.1”` 问题, 详细如下:
	* 1.`
未能使用“通用身份验证”连接到设备“127.0.0.1”。请验证项目调试设置中指定了正确的远程身份验证模式。COMException - 由于目标计算机积极拒绝，无法连接。
`
	* 2.解决方法, 因为是在本地计算机运行，所以要选择 CPU 架构为 `X86` or `X64`

* 3.出现项目点击运行,老是跳过的问题
	* 1.解决方法:右键 `Visual Studio 2019`, 以管理员方式运行.






