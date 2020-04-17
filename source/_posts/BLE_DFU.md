---
title: BLE DFU 相关
tags: [BLE]
categories: [BLE]
date: 2019-01-22 17:46:19
---

>记录 APP 开发 BLE DFU 相关功能的步骤.

<!-- more -->

## [1.参考链接](#references)
* 1.相关参考的链接.
* 2.相关参考的解释.
## [2.详细过程](#details)
* 1.DFU 的步骤
	* 0.Nordic BLE DFU 的架构设计.
	* 1.查询版本
	* 2.唤醒 BLE 设备的 DFU 模式(从普通模式转到 DFU 模式 )
	* 3.开始 DFU.


***
***
***

## 1.参考链接<a name="references"/>
* 1.相关参考的链接.
	* 1.[DFU Architecture(DFU 的整体架构)](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.sdk51.v10.0.0%2Fbledfu_architecture_bl.html)
	* 3.[How to generate the INIT file for DFU.pdf](https://github.com/NordicSemiconductor/Android-nRF-Connect/blob/master/init%20packet%20handling/How%20to%20generate%20the%20INIT%20file%20for%20DFU.pdf)
	* 4.[Android DFU Library](https://github.com/NordicSemiconductor/Android-DFU-Library)




* 2.`DFU Architecture `的解释.
	* 1.详细的命令解释在路径 `Software Development Kit > Previous versions of nRF5 SDK > nRF51 SDK v10.0.0 > Examples > DFU bootloader examples > BLE & HCI/UART Bootloader/DFU > Transport layers -> BLE DFU Service` 里
	* 2.概略的步骤解释在路径 `Software Development Kit > Previous versions of nRF5 SDK > nRF51 SDK v10.0.0 > Examples > DFU bootloader examples > BLE & HCI/UART Bootloader/DFU > Transport layers -> BLE DFU Profile` 里.

![](http://pic.pgyjz.cn/blog/BLE/Snip20190718_3.png)
![](http://pic.pgyjz.cn/blog/BLE/Snip20190718_4.png)

***

## 2.详细过程<a name="details"/>
> BLE 的板子是用 51822 系列的板子,传统模式的 DFU .

* 1.所用到的 DFU 相关的信息:

```
        public readonly Guid m_dfu_service = Guid.Parse("00001530-1212-EFDE-1523-785FEABCD123");
        public readonly Guid m_control_point_char = Guid.Parse("00001531-1212-EFDE-1523-785FEABCD123");
        	public readonly Guid m_descriptor_char = Guid.Parse("00002902-0000-1000-8000-00805F9B34FB");
        public readonly Guid m_package_char = Guid.Parse("00001532-1212-EFDE-1523-785FEABCD123");
        public readonly Guid m_version_char = Guid.Parse("00001534-1212-EFDE-1523-785FEABCD123");
        
```

### 1.DFU 的步骤

![](http://pic.pgyjz.cn/blog/Xamarin/ota_dfu_typical_ctrl_pt_procedure.svg)

#### 0.Nordic BLE DFU 的架构设计.
![](http://pic.pgyjz.cn/blog/Xamarin/bledfu_architecture.svg)


#### 1.查询版本
* 1.从 `00001534-1212-EFDE-1523-785FEABCD123` 中查询版本
	* 1.如果是 1 , Device 处于 普通模式，需要发命令让 Device 转到 DFU 模式(下面步骤 2).
	* 2.如果是 8, Device 处于 DFU 模式,开始进行 DFU 操作.(下面步骤 3)
	
#### 2.唤醒 BLE 设备的 DFU 模式(从普通模式转到 DFU 模式 )
* 1.打开 `Control Point` 的 `Notifications`,开始监听.
* 2.向 `Control Point` 的 `descriptor` 发送值`{0x01,0x00}`.
* 3.发送 `{0x01,0x04}` 到 `Control Point` 中.
	* 1.命令后面的一位 `0x04`指的是类型，因为我们这里是 `application`.(softdevice is 0x01,bootloader is 0x02,application is 0x04)
* 4.然后 Device 会主动断开连接，开始进入 DFU 模式，APP 需要进行重新连接，然后从步骤1(查询版本)开始.

#### 3.开始 DFU.
* 1.向 `Control Point` 的 `descriptor` 发送值`{0x01,0x00}`.
* 2.打开 `Control Point` 的 `Notifications`,开始监听.
* 3.发送 `{0x01,0x04}`(Start  DFU) 到 `Control Point` 中.
	* 1.命令后面的一位 `0x04`指的是类型，因为我们这里是 `application`.(softdevice is 0x01,bootloader is 0x02,application is 0x04)
* 4.发送 firmware 的大小 到 Device.
	* 1.发送 12 个Bytes 的数据到 `00001532 ` 中.
	* 2.这 12 个 Bytes 的数据分别是 `<Length of SoftDevice><Length of bootloader><Length of application>`,各占 4 个 Bytes.
	* 3.记得 4 个 Bytes 的数据是从低位到高位的，如 `softDevice size is 0,bootloader size is 0 ,application size is 38520` 则生成的 12 位的数据为 `(0x): 00-00-00-00-00-00-00-00-78-96-00-00`
	* 4.因为我测试 DFU 用的 firmware 用的是 `传统模式的 DFU` ,所以是个 `.zip`包，包里有 `.bin`,`.dat`,`.manifest.json` 文件.这里的 `size` 用的就是 `.bin` 文件的 size. 
	* [5.详情可以看这里的 `Image Size`](http://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.sdk5.v11.0.0%2Fbledfu_transport_bleservice.html&cp=4_0_10_4_3_1_4_1)
* 5.发送 `{0x02,0x00}`(Init DFU Parameters) 到 `Control Point` 中.
* 6.发送 (Init Data) 到 `00001532` 中.
	* 1.这里的 `Init Data` 为 zip 包中的 `.dat` 文件的内容，大小为 14 个 Byte.
* 7.发送 `{0x02,0x01}`(Init DFU Parameters) 到 `Control Point` 中.
* 8.发送 `{0x03}`(Receive Firmware Image) 到 `Control Point` 中.
* 9.APP 开始发送 firmware 文件
	* 1.开始发送 zip 文件下的 `.bin` 文件的内容到 Device(00001532) 中.
* 10.发送 `{0x04}`(Validate Firmware) 到 `Control Point` 中.
* 11.发送 `{0x05}`(Activate Image and Reset) 到 `Control Point` 中.
	* 1.让 Device 重启.

![](http://pic.pgyjz.cn/blog/Xamarin/exp_ota_dfu_transfer.svg)
