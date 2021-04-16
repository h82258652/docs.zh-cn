---
title: 快速入门 - 使用 .NET 驱动 Raspberry Pi Sense HAT
description: 使用 Sense HAT（Raspberry Pi 的附加板），在 5 分钟内开始使用 .NET IoT 库。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: quickstart
ms.prod: dotnet
ms.openlocfilehash: 28d6650187bbf7b9ce91516f4da4d09b114c904a
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "102259694"
---
# <a name="quickstart---use-net-to-drive-a-raspberry-pi-sense-hat"></a>快速入门 - 使用 .NET 驱动 Raspberry Pi Sense HAT

Raspberry Pi [Sense HAT](https://www.raspberrypi.org/products/sense-hat/)（附加在开发板上的硬件）是 Raspberry Pi 的附加板  。 Sense HAT 配有一个 8×8 RGB LED 矩阵、一个包含五个按钮的操纵杆，以及以下传感器：

- 陀螺仪
- 加速计
- 磁力计
- 温度
- 气压
- 湿度

此快速入门使用 .NET 从 Sense HAT 检索传感器值、响应操纵杆输入并驱动 LED 矩阵。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- Sense HAT

[!INCLUDE [prepare-pi-i2c](../includes/prepare-pi-i2c.md)]

## <a name="run-the-quickstart"></a>运行快速入门

向 Raspberry Pi 附加 Sense HAT。 在 Raspberry Pi 上的 Bash 提示符中（本地或远程），运行以下命令：

```bash
. <(wget -q -O - https://aka.ms/dotnet-iot-sensehat-quickstart)
```

该命令会下载并运行脚本。 脚本：

- 安装 .NET SDK。
- 克隆包含 Sense HAT 快速入门项目的 GitHub 存储库。
- 生成项目。
- 运行项目。

显示传感器数据时，观察控制台输出。 LED 矩阵在蓝色背景中显示黄色像素。 握住操纵杆向任意方向移动，黄色像素也会沿该方向移动。 单击操纵杆中间的按钮，背景会从蓝色切换为红色。

## <a name="get-the-source-code"></a>获取源代码

[可在 GitHub 上获取](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/quickstarts/SenseHat.Quickstart)此快速入门的源。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何使用常规用途输入/输出使 LED 闪烁](../tutorials/blink-led.md)
