---
title: 从模拟到数字转换器读取值
description: 了解如何使用模数转换器读取可变电压值。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: tutorial
ms.prod: dotnet
ms.openlocfilehash: 509616e3423fbb83b74bfbb8ecec1a7df49f0a20
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "102259730"
---
<!--markdownlint-disable DOCSMD011 -->
# <a name="read-values-from-an-analog-to-digital-converter"></a>从模拟到数字转换器读取值

模数转换器 (ADC) 设备可读取模拟输入电压值并将其转换为数字值。 ADC 用于从热敏电阻和电位差计等设备读取值，这些设备会根据某些条件更改电阻。

在本主题中，由于使用电位差计来调整输入电压，因此将使用 .NET 从 ADC 读取值。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- [MCP3008](https://www.microchip.com/wwwproducts/MCP3008) 模数转换器
- 三脚电位差计
- 试验板
- 跳线
- Raspberry Pi GPIO 分线板（可选/推荐）
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

[!INCLUDE [prepare-pi-spi](../includes/prepare-pi-spi.md)]

## <a name="prepare-the-hardware"></a>准备硬件

使用硬件组件构建下图所示的线路：

:::image type="content" source="../media/rpi-trimpot_spi-thumb.png" alt-text="显示具有 MCP3008 ADC 和电位差计的线路的 Fritzing 图" lightbox="../media/rpi-trimpot_spi.png":::

MCP3008 使用串行外围设备接口 (SPI) 进行通信。 下列是从 MCP3008 到 Raspberry Pi 和电位差计的连接：

- V<sub>DD</sub> 连接到 3.3V（红色所示）
- V<sub>REF</sub> 连接到 3.3V（红色）
- AGND 连接到地面（黑色）
- CLK 连接到 SCLK（橙色）
- D<sub>OUT</sub> 连接到 MISO（橙色）
- D<sub>IN</sub> 连接到 MOSI（橙色）
- CS/SHDN 连接到 CE0（绿色）
- DGND 连接到地面（黑色）
- CH0 连接到电位差计上的可变（中间）引脚（黄色）

为电位差计上的外部引脚提供 3.3V 的电源并接地。 顺序并不重要。

根据需要，请参阅以下引脚分配关系图：

| MCP3008  | Raspberry Pi GPIO |
|----------|-------------------|
| :::image type="content" source="../media/mcp3008-diagram-thumb.png" alt-text="显示 MCP3008 的引脚分配关系图" lightbox="../media/mcp3008-diagram.png"::: | :::image type="content" source="../media/gpio-pinout-diagram-thumb.png" alt-text="显示 Raspberry Pi GPIO 标头引脚分配的关系图。图片由 Raspberry Pi 基金会提供。" lightbox="../media/gpio-pinout-diagram.png":::<br />[图片由 Raspberry Pi 基金会提供](https://www.raspberrypi.org/documentation/usage/gpio/)。
 |

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a>创建应用

在首选开发环境中完成以下步骤：

1. 使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md) 创建一个新 .Net 控制台应用。 将其命名为 AdcTutorial。

    ```dotnetcli
    dotnet new console -o AdcTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. 将 Program.cs 的内容替换为以下代码：

    :::code language="csharp" source="~/iot-samples/tutorials/AdcTutorial/Program.cs" :::

    在上述代码中：

    - `hardwareSpiSettings` 设置为 `SpiConnectionSettings` 的新实例。 构造函数将 `busId` 参数设置为 0，将 `chipSelectLine` 参数设置为 0。
    - [using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)通过调用 `SpiDevice.Create` 和传入 `hardwareSpiSettings` 来创建 `SpiDevice` 的实例。 `SpiDevice` 表示 SPI 总线。 `using` 声明可确保释放对象并正确释放硬件资源。
    - 其他 `using` 声明创建 `Mcp3008` 的实例，并将 `SpiDevice` 传递到构造函数中。
    - `while` 循环无限期运行。 每次迭代：
        1. 通过调用 `mcp.Read(0)` 来读取 ADC 上的 CH0 值。
        1. 将值除以 10.24。 MCP3008 是 10 位 ADC，表示它会返回范围为 0-1023 的 1024 个可能的值。 将值除以 10.24 表示百分比形式的值。
        1. 将小数值四舍五入到最接近的整数。
        1. 将值以百分比形式写入控制台。
        1. 休眠 500 毫秒。

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. 通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。

    ```bash
    ./AdcTutorial
    ```

    旋转电位差计刻度时观察输出。 这是因为电位差计会改变供给 ADC 上 CH0 的电压。 ADC 将 CH0 上的输入电压与提供给 V<sub>REF</sub> 的参考电压进行比较，从而生成一个值。

1. 按 <kbd>Ctrl+C</kbd> 终止程序。

祝贺你！ 你使用 SPI 从模数转换器读取了值。

## <a name="get-the-source-code"></a>获取源代码

[GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/AdcTutorial) 上提供此教程的源。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何使用常规用途输入/输出使 LED 闪烁](../tutorials/blink-led.md)
