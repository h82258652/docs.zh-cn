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
# <a name="develop-apps-for-iot-devices-with-the-net-iot-libraries"></a><span data-ttu-id="1a6c6-103">使用 .NET IoT 库开发适用于 IoT 设备的应用</span><span class="sxs-lookup"><span data-stu-id="1a6c6-103">Develop apps for IoT devices with the .NET IoT Libraries</span></span>

<span data-ttu-id="1a6c6-104">.NET 可在多种平台和体系结构上运行。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-104">.NET runs on a variety of platforms and architectures.</span></span> <span data-ttu-id="1a6c6-105">它支持 Raspberry Pi 和 Hummingboard 等常见物联网 (IoT) 插件板。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-105">Common Internet of things (IoT) boards, such as Raspberry Pi and Hummingboard, are supported.</span></span> <span data-ttu-id="1a6c6-106">IoT 应用通常与专用的硬件（例如传感器、模数转换器和 LCD 设备）交互。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-106">IoT apps typically interact with specialized hardware, such as sensors, analog-to-digital converters, and LCD devices.</span></span> <span data-ttu-id="1a6c6-107">.NET IoT 库支持这些场景。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-107">The .NET IoT Libraries enable these scenarios.</span></span>

## <a name="video-overview"></a><span data-ttu-id="1a6c6-108">视频概述</span><span class="sxs-lookup"><span data-stu-id="1a6c6-108">Video overview</span></span>

<!--markdownlint-disable MD034 -->
> [!VIDEO https://channel9.msdn.com/Series/IoT-101/Intro-to-IOT-with-NET-Core-1-of-9/player]

## <a name="libraries"></a><span data-ttu-id="1a6c6-109">库</span><span class="sxs-lookup"><span data-stu-id="1a6c6-109">Libraries</span></span>

<span data-ttu-id="1a6c6-110">.NET IoT 库由两个 NuGet 包组成：</span><span class="sxs-lookup"><span data-stu-id="1a6c6-110">The .NET IoT Libraries are composed of two NuGet packages:</span></span>

- [<span data-ttu-id="1a6c6-111">System.Device.Gpio</span><span class="sxs-lookup"><span data-stu-id="1a6c6-111">System.Device.Gpio</span></span>](https://www.nuget.org/packages/System.Device.Gpio/)
- [<span data-ttu-id="1a6c6-112">Iot.Device.Bindings</span><span class="sxs-lookup"><span data-stu-id="1a6c6-112">Iot.Device.Bindings</span></span>](https://www.nuget.org/packages/Iot.Device.Bindings/)

### <a name="systemdevicegpio"></a><span data-ttu-id="1a6c6-113">System.Device.Gpio</span><span class="sxs-lookup"><span data-stu-id="1a6c6-113">System.Device.Gpio</span></span>

<span data-ttu-id="1a6c6-114">`System.Device.Gpio` 支持多种协议，用于与低级别硬件引脚进行交互以控制设备。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-114">`System.Device.Gpio` supports a variety of protocols for interacting with low-level hardware pins to control devices.</span></span> <span data-ttu-id="1a6c6-115">其中包括:</span><span class="sxs-lookup"><span data-stu-id="1a6c6-115">These include:</span></span>

- <span data-ttu-id="1a6c6-116">常规用途 I/O (GPIO)</span><span class="sxs-lookup"><span data-stu-id="1a6c6-116">General-purpose I/O (GPIO)</span></span>
- <span data-ttu-id="1a6c6-117">内置集成电路 (I2C)</span><span class="sxs-lookup"><span data-stu-id="1a6c6-117">Inter-Integrated Circuit (I2C)</span></span>
- <span data-ttu-id="1a6c6-118">串行外围接口 (SPI)</span><span class="sxs-lookup"><span data-stu-id="1a6c6-118">Serial Peripheral Interface (SPI)</span></span>
- <span data-ttu-id="1a6c6-119">脉宽调制 (PWM)</span><span class="sxs-lookup"><span data-stu-id="1a6c6-119">Pulse Width Modulation (PWM)</span></span>
- <span data-ttu-id="1a6c6-120">串行端口</span><span class="sxs-lookup"><span data-stu-id="1a6c6-120">Serial port</span></span>

### <a name="iotdevicebindings"></a><span data-ttu-id="1a6c6-121">Iot.Device.Bindings</span><span class="sxs-lookup"><span data-stu-id="1a6c6-121">Iot.Device.Bindings</span></span>

<span data-ttu-id="1a6c6-122">`Iot.Device.Bindings` 包：</span><span class="sxs-lookup"><span data-stu-id="1a6c6-122">The `Iot.Device.Bindings` package:</span></span>

* <span data-ttu-id="1a6c6-123">包含[设备绑定](https://github.com/dotnet/iot/blob/main/src/devices/README.md)，通过包装 System.Device.Gpio 简化应用开发。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-123">Contains [device bindings](https://github.com/dotnet/iot/blob/main/src/devices/README.md) to streamline app development by wrapping System.Device.Gpio.</span></span>
* <span data-ttu-id="1a6c6-124">受社区支持，并不断增加其他绑定。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-124">Is community-supported, and additional bindings are added continually.</span></span>

<span data-ttu-id="1a6c6-125">常用的设备绑定包括：</span><span class="sxs-lookup"><span data-stu-id="1a6c6-125">Commonly used device bindings include:</span></span>

- [<span data-ttu-id="1a6c6-126">CharacterLcd - LCD 字符显示器</span><span class="sxs-lookup"><span data-stu-id="1a6c6-126">CharacterLcd - LCD character display</span></span>](https://github.com/dotnet/iot/tree/main/src/devices/CharacterLcd)
- [<span data-ttu-id="1a6c6-127">SN74HC595 - 8 位移位寄存器</span><span class="sxs-lookup"><span data-stu-id="1a6c6-127">SN74HC595 - 8-bit shift register</span></span>](https://github.com/dotnet/iot/tree/main/src/devices/Sn74hc595)
- [<span data-ttu-id="1a6c6-128">BrickPi3</span><span class="sxs-lookup"><span data-stu-id="1a6c6-128">BrickPi3</span></span>](https://github.com/dotnet/iot/tree/main/src/devices/BrickPi3)
- [<span data-ttu-id="1a6c6-129">Max7219 - LED 矩阵驱动器</span><span class="sxs-lookup"><span data-stu-id="1a6c6-129">Max7219 - LED Matrix driver</span></span>](https://github.com/dotnet/iot/tree/main/src/devices/Max7219)
- [<span data-ttu-id="1a6c6-130">RGBLedMatrix - RGB LED 点阵屏</span><span class="sxs-lookup"><span data-stu-id="1a6c6-130">RGBLedMatrix - RGB LED Matrix</span></span>](https://github.com/dotnet/iot/tree/main/src/devices/RGBLedMatrix)

## <a name="supported-operating-systems"></a><span data-ttu-id="1a6c6-131">支持的操作系统</span><span class="sxs-lookup"><span data-stu-id="1a6c6-131">Supported operating systems</span></span>

<span data-ttu-id="1a6c6-132">支持 ARM/ARM64 和 Windows 10 IoT Core 的多数 Linux 版本都支持 `System.Device.Gpio`。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-132">`System.Device.Gpio` is supported on most versions of Linux that support ARM/ARM64 and Windows 10 IoT Core.</span></span>

> [!TIP]
> <span data-ttu-id="1a6c6-133">对于 Raspberry Pi，建议使用 [Raspberry PI OS](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)（以前称为 Raspbian）。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-133">For Raspberry Pi, [Raspberry Pi OS](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)  (formerly Raspbian) is recommended.</span></span>

## <a name="supported-hardware-platforms"></a><span data-ttu-id="1a6c6-134">支持的硬件平台</span><span class="sxs-lookup"><span data-stu-id="1a6c6-134">Supported hardware platforms</span></span>

<span data-ttu-id="1a6c6-135">`System.Device.Gpio` 与大多数单插件板平台兼容。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-135">`System.Device.Gpio` is compatible with most single-board platforms.</span></span> <span data-ttu-id="1a6c6-136">建议使用 Raspberry Pi（2 和更高版本）以及 Hummingboard 平台。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-136">Recommended platforms are Raspberry Pi (2 and greater) and Hummingboard.</span></span> <span data-ttu-id="1a6c6-137">已知的其他兼容平台有 BeagleBoard 和 ODROID。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-137">Other platforms known to be compatible are BeagleBoard and ODROID.</span></span>

<span data-ttu-id="1a6c6-138">支持通过 USB 转 SPI/I2C 桥接器使用 PC 平台。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-138">PC platforms are supported via the use of a USB to SPI/I2C bridge.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1a6c6-139">ARMv6 体系结构设备（包括 Raspberry Pi Zero 和 Raspberry Pi 2 之前的 Raspberry Pi 设备）不支持 .NET。</span><span class="sxs-lookup"><span data-stu-id="1a6c6-139">.NET is not supported on ARMv6 architecture devices, including Raspberry Pi Zero and Raspberry Pi devices prior to Raspberry Pi 2.</span></span>

## <a name="resources"></a><span data-ttu-id="1a6c6-140">资源</span><span class="sxs-lookup"><span data-stu-id="1a6c6-140">Resources</span></span>

- [<span data-ttu-id="1a6c6-141">Github 上的 .NET IoT 库</span><span class="sxs-lookup"><span data-stu-id="1a6c6-141">.NET IoT Libraries on Github</span></span>](https://github.com/dotnet/iot)
