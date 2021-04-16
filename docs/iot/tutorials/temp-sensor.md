---
title: 从传感器读取环境条件
description: 了解如何使用 .NET IoT 库读取温度、气压和湿度。
author: camsoper
ms.author: casoper
ms.date: 11/14/2020
ms.topic: tutorial
ms.prod: dotnet
ms.openlocfilehash: bc5c2b9f0876603c152da859547c58b83826969c
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "102259832"
---
# <a name="read-environmental-conditions-from-a-sensor"></a>从传感器读取环境条件

IoT 设备其中最常见的一种场景是检测环境条件。 有多种传感器可用于监视温度、湿度、气压等。

在本主题中，你将使用 .NET 从传感器读取环境条件。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- [BME280](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout) 湿度/气压/温度传感器分线板
- 跳线
- 线路板（可选）
- Raspberry Pi GPIO 分线板（可选）
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

> [!IMPORTANT]
> BME280 分线板的制造商有很多。 大多数设计都类似，并且制造商不应对功能进行任何更改。 本教程尝试考虑差异。 确保你的 BME280 分线板包括一个内置集成电路 (I2C) 接口。

[!INCLUDE [prepare-pi-i2c](../includes/prepare-pi-i2c.md)]

## <a name="prepare-the-hardware"></a>准备硬件

使用硬件组件构建电路，如下图所示：

:::image type="content" source="../media/rpi-bmp280_i2c-thumb.png" alt-text="该 Fritzing 关系图显示了从 Raspberry Pi 到 BME280 分线板的连接" lightbox="../media/rpi-bmp280_i2c.png":::

以下是从 Raspberry Pi 到 BME280 分线板的连接：

- 3.3V 到 VIN 或 3V3（显示为红色）
- 地面到 GND（黑色）
- SDA (GPIO 2) 到 SDI 或 SDA（蓝色）
- SCL (GPIO 3) 到 SCK 或 SCL（橙色）

[!INCLUDE [tutorial-rpi-gpio](../includes/tutorial-rpi-gpio.md)]

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a>创建应用

在首选开发环境中完成以下步骤：

1. 使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md) 创建新 .Net 控制台应用。 将其命名为 SensorTutorial。

    ```dotnetcli
    dotnet new console -o SensorTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. 将 Program.cs 的内容替换为以下代码：

    :::code language="csharp" source="~/iot-samples/tutorials/SensorTutorial/Program.cs" :::

    在上述代码中：

    - `i2cSettings` 设置为 `I2cConnectionSettings` 的新实例。 构造函数将 `busId` 参数设置为 1，将 `deviceAddress` 参数设置为 `Bme280.DefaultI2cAddress`。

        > [!IMPORTANT]
        > 一些 BME280 分线板制造商使用辅助地址值。 对于这些设备，请使用 `Bme280.SecondaryI2cAddress`。

    - 一个 [using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)通过调用 `I2cDevice.Create` 并传入 `i2cSettings` 来创建 `I2cDevice` 的实例。 这个 `I2cDevice` 表示 I2C 总线。 `using` 声明可确保对象已处置，硬件资源已正确释放。
    - 另一个 `using` 声明创建 `Bme280` 的实例以表示传感器。 `I2cDevice` 在构造函数中传递。
    - 通过调用 `GetMeasurementDuration` 检索芯片使用其当前（默认）设置进行度量所需的时间。
    - `while` 循环无限期运行。 每次迭代：
        1. 清除控制台。
        1. 将电源模式设置为 `Bmx280PowerMode.Forced`。 这会强制芯片执行一次度量，存储结果，然后进入睡眠状态。
        1. 读取温度、压力、湿度和海拔高度的值。

            > [!NOTE]
            > 海拔高度由设备绑定进行计算。 `TryReadAltitude` 的重载使用平均海平面级压力来生成估算值。

        1. 将当前环境条件写入控制台。
        1. 休眠 1000 毫秒。

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. 通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。

    ```bash
    ./SensorTutorial
    ```

    在控制台中查看传感器输出。

1. 按 <kbd>Ctrl+C</kbd> 终止程序。

祝贺你！ 你已使用 I2C 从温度/湿度/气压传感器读取值！

## <a name="get-the-source-code"></a>获取源代码

[GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/SensorTutorial) 上提供此教程的源。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何在 LCD 上显示文本](../tutorials/lcd-display.md)
