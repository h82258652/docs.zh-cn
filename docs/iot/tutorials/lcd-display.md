---
title: 在 LCD 上显示文本
description: 了解如何使用 .NET IoT 库在液晶显示器上显示字符。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: tutorial
ms.prod: dotnet
ms.openlocfilehash: 005b40a7d9f46b84fcd90541248f5f4fd243e612
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "102255506"
---
<!--markdownlint-disable DOCSMD011 -->
# <a name="display-text-on-an-lcd"></a>在 LCD 上显示文本

LCD 字符显示器可用于显示信息，而无需外部监视器。 常见的 LCD 字符显示器可以直接连接到 GPIO 引脚，但这种方法最多需要使用 10 个 GPIO 引脚。 对于需要连接到设备组合的场景，将如此多的 GPIO 标头分配给字符显示器通常不切实际。

许多制造商会销售带有集成 GPIO 扩展器的 20x4 LCD 字符显示器。 字符显示器将直接连接到 GPIO 扩展器，然后通过内置集成电路 (I2C) 串行协议连接到 Raspberry Pi。

在本主题中，你将使用 .NET 通过 I2C GPIO 扩展器在 LCD 字符显示器上显示文本。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- [带有 I2C 接口的 20x4 LCD 字符显示器](https://www.bing.com/images/search?q=20x4+lcd+display+with+i2c)
- 跳线
- 线路板（可选/推荐）
- Raspberry Pi GPIO 分线板（可选/推荐）
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

> [!NOTE]
> LCD 字符显示器的制造商有很多。 大多数设计都相同，并且制造商不应对功能进行任何更改。 作为参考，本教程按照 [SunFounder LCD2004](https://www.sunfounder.com/lcd2004-module.html) 开发。

[!INCLUDE [prepare-pi-i2c](../includes/prepare-pi-i2c.md)]

## <a name="prepare-the-hardware"></a>准备硬件

使用跳线将 I2C GPIO 扩展器上的四个引脚连接到 Raspberry Pi，如下所示：

- GND 到地面
- VCC 到 5V
- SDA 到 SDA (GPIO 2)
- SCL 到 SCL (GPIO 3)

请根据需要参阅下图：

| I2C 接口（显示器背面） | Raspberry Pi GPIO |
|---------------------------------|-------------------|
| :::image type="content" source="../media/character-display-i2c-thumb.png" alt-text="显示 I2C GPIO 扩展器的字符显示器背面的图像。" lightbox="../media/character-display-i2c.png"::: | :::image type="content" source="../media/gpio-pinout-diagram-thumb.png" alt-text="显示 Raspberry Pi GPIO 标头引脚分配的关系图。图片由 Raspberry Pi 基金会提供。" lightbox="../media/gpio-pinout-diagram.png":::<br />[图片由 Raspberry Pi 基金会提供](https://www.raspberrypi.org/documentation/usage/gpio/)。
 |

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a>创建应用

在首选开发环境中完成以下步骤：

1. 使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md) 创建新 .Net 控制台应用。 将其命名为 LcdTutorial。

    ```dotnetcli
    dotnet new console -o LcdTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. 将 Program.cs 的内容替换为以下代码：

    :::code language="csharp" source="~/iot-samples/tutorials/LcdTutorial/Program.cs" :::

    在上述代码中：

    - 一个 [using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)通过调用 `I2cDevice.Create` 并传入带有 `busId` 和 `deviceAddress` 参数的新 `I2cConnectionSettings` 来创建 `I2cDevice` 的实例。 这个 `I2cDevice` 表示 I2C 总线。 `using` 声明可确保对象已处置，硬件资源已正确释放。

        > [!WARNING]
        > GPIO 扩展器的设备地址取决于制造商使用的芯片。 配备 PCF8574 芯片的 GPIO 扩展器使用地址 `0x27`，而使用 PCF8574A 芯片的扩展器使用地址 `0x3F`。 请查阅你的 LCD 文档。

    - 另一个 `using` 声明创建 `Pcf8574` 的实例，并将 `I2cDevice` 传递到构造函数中。 此实例表示 GPIO 扩展器。
    - 另一个 `using` 声明创建 `Lcd2004` 的实例以表示显示器。 多个参数已传递给构造函数，该构造函数描述用于与 GPIO 扩展器进行通信的设置。 GPIO 扩展器作为 `controller` 参数传递。
    - `while` 循环无限期运行。 每次迭代：
        1. 清除显示器。
        1. 将光标位置设置为当前行的第一个位置。
        1. 将当前时间写入显示器上当前光标位置。
        1. 循环访问当前行计数器。
        1. 休眠 1000 毫秒。

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. 通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。

    ```bash
    ./LcdTutorial
    ```

    观察 LCD 字符显示器，当前时间应显示在每一行上。

    > [!TIP]
    > 如果显示器已亮起，但看不到任何文本，请尝试调整显示器背面的对比度拨盘。

1. 按 <kbd>Ctrl+C</kbd> 终止程序。

祝贺你！ 你已使用 I2C 和 GPIO 扩展器在 LCD 上显示了文本！

## <a name="get-the-source-code"></a>获取源代码

[GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/LcdTutorial) 上提供此教程的源。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何使用常规用途输入/输出使 LED 闪烁](../tutorials/blink-led.md)
