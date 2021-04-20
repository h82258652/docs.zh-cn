---
title: 使用 .NET IoT 库开发适用于 IoT 设备的应用
description: 了解如何使用 .NET 生成适用于 IoT 设备和场景的应用程序。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: overview
ms.prod: dotnet
ms.openlocfilehash: 4d8dffb28b28c999b3d1bf77a65265280a8ae7a1
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874779"
---
# <a name="develop-apps-for-iot-devices-with-the-net-iot-libraries"></a>使用 .NET IoT 库开发适用于 IoT 设备的应用

.NET 可在多种平台和体系结构上运行。 它支持 Raspberry Pi 和 Hummingboard 等常见物联网 (IoT) 插件板。 IoT 应用通常与专用的硬件（例如传感器、模数转换器和 LCD 设备）交互。 .NET IoT 库支持这些场景。

## <a name="video-overview"></a>视频概述

<!--markdownlint-disable MD034 -->
> [!VIDEO https://channel9.msdn.com/Series/IoT-101/Intro-to-IOT-with-NET-Core-1-of-9/player]

## <a name="libraries"></a>库

.NET IoT 库由两个 NuGet 包组成：

- [System.Device.Gpio](https://www.nuget.org/packages/System.Device.Gpio/)
- [Iot.Device.Bindings](https://www.nuget.org/packages/Iot.Device.Bindings/)

### <a name="systemdevicegpio"></a>System.Device.Gpio

`System.Device.Gpio` 支持多种协议，用于与低级别硬件引脚进行交互以控制设备。 其中包括:

- 常规用途 I/O (GPIO)
- 内置集成电路 (I2C)
- 串行外围接口 (SPI)
- 脉宽调制 (PWM)
- 串行端口

### <a name="iotdevicebindings"></a>Iot.Device.Bindings

`Iot.Device.Bindings` 包：

* 包含[设备绑定](https://github.com/dotnet/iot/blob/main/src/devices/README.md)，通过包装 System.Device.Gpio 简化应用开发。
* 受社区支持，并不断增加其他绑定。

常用的设备绑定包括：

- [CharacterLcd - LCD 字符显示器](https://github.com/dotnet/iot/tree/main/src/devices/CharacterLcd)
- [SN74HC595 - 8 位移位寄存器](https://github.com/dotnet/iot/tree/main/src/devices/Sn74hc595)
- [BrickPi3](https://github.com/dotnet/iot/tree/main/src/devices/BrickPi3)
- [Max7219 - LED 矩阵驱动器](https://github.com/dotnet/iot/tree/main/src/devices/Max7219)
- [RGBLedMatrix - RGB LED 点阵屏](https://github.com/dotnet/iot/tree/main/src/devices/RGBLedMatrix)

## <a name="supported-operating-systems"></a>支持的操作系统

支持 ARM/ARM64 和 Windows 10 IoT Core 的多数 Linux 版本都支持 `System.Device.Gpio`。

> [!TIP]
> 对于 Raspberry Pi，建议使用 [Raspberry PI OS](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)（以前称为 Raspbian）。

## <a name="supported-hardware-platforms"></a>支持的硬件平台

`System.Device.Gpio` 与大多数单插件板平台兼容。 建议使用 Raspberry Pi（2 和更高版本）以及 Hummingboard 平台。 已知的其他兼容平台有 BeagleBoard 和 ODROID。

支持通过 USB 转 SPI/I2C 桥接器使用 PC 平台。

> [!IMPORTANT]
> ARMv6 体系结构设备（包括 Raspberry Pi Zero 和 Raspberry Pi 2 之前的 Raspberry Pi 设备）不支持 .NET。

## <a name="resources"></a>资源

- [Github 上的 .NET IoT 库](https://github.com/dotnet/iot)
