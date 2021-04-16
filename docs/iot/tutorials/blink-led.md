---
title: 闪烁 LED
description: 了解如何使用 .NET IoT 库让 LED 闪烁。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: tutorial
ms.prod: dotnet
ms.openlocfilehash: 0d5db19faac0293b9982731f26dfd85d6ce07b3a
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "102259007"
---
# <a name="blink-an-led"></a>闪烁 LED

可以单独控制常规用途 I/O (GPIO) 引脚。 这对控制 LED、中继和其他有状态设备很有用。 在本主题中，你将使用 .NET 和 Raspberry Pi 的 GPIO 引脚使 LED 通电，并重复闪烁。

## <a name="prerequisites"></a>先决条件

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- 5 毫米 LED
- 330 Ω 电阻器
- 试验板
- 跳线
- Raspberry Pi GPIO 分线板（可选/推荐）
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

[!INCLUDE [ensure-ssh](../includes/ensure-ssh.md)]

## <a name="prepare-the-hardware"></a>准备硬件

使用硬件组件构建电路，如下图所示：

:::image type="content" source="../media/rpi-led_bb-thumb.png" alt-text="Fritzing 图显示具有 LED 和电阻器的线路" lightbox="../media/rpi-led_bb.png":::

上图描述了下列连接：

- GPIO 18 到 LED 阳极（较长、正极导线）
- LED 阴极（较短、负极导线）到 330 Ω 电阻器（任一端）
- 330 Ω 电阻器（另一端）到地面

[!INCLUDE [tutorial-rpi-gpio](../includes/tutorial-rpi-gpio.md)]

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a>创建应用

在首选开发环境中完成以下步骤：

1. 使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md) 创建新 .Net 控制台应用。 将其命名为 BlinkTutorial。

    ```dotnetcli
    dotnet new console -o BlinkTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. 将 Program.cs 的内容替换为以下代码：

    :::code language="csharp" source="~/iot-samples/tutorials/BlinkTutorial/Program.cs" :::

    在上述代码中：

    - [using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)创建 `GpioController` 实例。 `using` 声明可确保对象已处置，硬件资源已正确释放。
    - GPIO 引脚 18 已为输出打开
    - `while` 循环无限期运行。 每次迭代：
        1. 向 GPIO 引脚 18 写入值。 如果 `ledOn` 为 true，则迭代写入 `PinValue.High`（开）。 否则，写入 `PinValue.Low`。
        1. 休眠 1000 毫秒。
        1. 切换 `ledOn` 的值。

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. 通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。

    ```bash
    ./BlinkTutorial
    ```

    LED 每秒闪烁一次。

1. 按 <kbd>Ctrl+C</kbd> 终止程序。

祝贺你！ 你已使用 GPIO 让 LED 闪烁。

## <a name="get-the-source-code"></a>获取源代码

[GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/BlinkTutorial) 上提供此教程的源。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [了解如何从传感器读取环境条件](../tutorials/temp-sensor.md)
