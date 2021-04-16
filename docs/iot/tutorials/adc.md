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
# <a name="read-values-from-an-analog-to-digital-converter"></a><span data-ttu-id="e51f9-103">从模拟到数字转换器读取值</span><span class="sxs-lookup"><span data-stu-id="e51f9-103">Read values from an analog-to-digital converter</span></span>

<span data-ttu-id="e51f9-104">模数转换器 (ADC) 设备可读取模拟输入电压值并将其转换为数字值。</span><span class="sxs-lookup"><span data-stu-id="e51f9-104">An analog-to-digital converter (ADC) is a device that can read an analog input voltage value and convert it into a digital value.</span></span> <span data-ttu-id="e51f9-105">ADC 用于从热敏电阻和电位差计等设备读取值，这些设备会根据某些条件更改电阻。</span><span class="sxs-lookup"><span data-stu-id="e51f9-105">ADCs are used for reading values from thermistors, potentiometers, and other devices that change resistance based on certain conditions.</span></span>

<span data-ttu-id="e51f9-106">在本主题中，由于使用电位差计来调整输入电压，因此将使用 .NET 从 ADC 读取值。</span><span class="sxs-lookup"><span data-stu-id="e51f9-106">In this topic, you will use .NET to read values from an ADC as you modulate the input voltage with a potentiometer.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e51f9-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="e51f9-107">Prerequisites</span></span>

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- <span data-ttu-id="e51f9-108">[MCP3008](https://www.microchip.com/wwwproducts/MCP3008) 模数转换器</span><span class="sxs-lookup"><span data-stu-id="e51f9-108">[MCP3008](https://www.microchip.com/wwwproducts/MCP3008) analog-to-digital converter</span></span>
- <span data-ttu-id="e51f9-109">三脚电位差计</span><span class="sxs-lookup"><span data-stu-id="e51f9-109">Three-pin potentiometer</span></span>
- <span data-ttu-id="e51f9-110">试验板</span><span class="sxs-lookup"><span data-stu-id="e51f9-110">Breadboard</span></span>
- <span data-ttu-id="e51f9-111">跳线</span><span class="sxs-lookup"><span data-stu-id="e51f9-111">Jumper wires</span></span>
- <span data-ttu-id="e51f9-112">Raspberry Pi GPIO 分线板（可选/推荐）</span><span class="sxs-lookup"><span data-stu-id="e51f9-112">Raspberry Pi GPIO breakout board (optional/recommended)</span></span>
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

[!INCLUDE [prepare-pi-spi](../includes/prepare-pi-spi.md)]

## <a name="prepare-the-hardware"></a><span data-ttu-id="e51f9-113">准备硬件</span><span class="sxs-lookup"><span data-stu-id="e51f9-113">Prepare the hardware</span></span>

<span data-ttu-id="e51f9-114">使用硬件组件构建下图所示的线路：</span><span class="sxs-lookup"><span data-stu-id="e51f9-114">Use the hardware components to build the circuit as depicted in the following diagram:</span></span>

:::image type="content" source="../media/rpi-trimpot_spi-thumb.png" alt-text="显示具有 MCP3008 ADC 和电位差计的线路的 Fritzing 图" lightbox="../media/rpi-trimpot_spi.png":::

<span data-ttu-id="e51f9-116">MCP3008 使用串行外围设备接口 (SPI) 进行通信。</span><span class="sxs-lookup"><span data-stu-id="e51f9-116">The MCP3008 uses Serial Peripheral Interface (SPI) to communicate.</span></span> <span data-ttu-id="e51f9-117">下列是从 MCP3008 到 Raspberry Pi 和电位差计的连接：</span><span class="sxs-lookup"><span data-stu-id="e51f9-117">The following are the connections from the MCP3008 to the Raspberry Pi and potentiometer:</span></span>

- <span data-ttu-id="e51f9-118">V<sub>DD</sub> 连接到 3.3V（红色所示）</span><span class="sxs-lookup"><span data-stu-id="e51f9-118">V<sub>DD</sub> to 3.3V (shown in red)</span></span>
- <span data-ttu-id="e51f9-119">V<sub>REF</sub> 连接到 3.3V（红色）</span><span class="sxs-lookup"><span data-stu-id="e51f9-119">V<sub>REF</sub> to 3.3V (red)</span></span>
- <span data-ttu-id="e51f9-120">AGND 连接到地面（黑色）</span><span class="sxs-lookup"><span data-stu-id="e51f9-120">AGND to ground (black)</span></span>
- <span data-ttu-id="e51f9-121">CLK 连接到 SCLK（橙色）</span><span class="sxs-lookup"><span data-stu-id="e51f9-121">CLK to SCLK (orange)</span></span>
- <span data-ttu-id="e51f9-122">D<sub>OUT</sub> 连接到 MISO（橙色）</span><span class="sxs-lookup"><span data-stu-id="e51f9-122">D<sub>OUT</sub> to MISO (orange)</span></span>
- <span data-ttu-id="e51f9-123">D<sub>IN</sub> 连接到 MOSI（橙色）</span><span class="sxs-lookup"><span data-stu-id="e51f9-123">D<sub>IN</sub> to MOSI (orange)</span></span>
- <span data-ttu-id="e51f9-124">CS/SHDN 连接到 CE0（绿色）</span><span class="sxs-lookup"><span data-stu-id="e51f9-124">CS/SHDN to CE0 (green)</span></span>
- <span data-ttu-id="e51f9-125">DGND 连接到地面（黑色）</span><span class="sxs-lookup"><span data-stu-id="e51f9-125">DGND to ground (black)</span></span>
- <span data-ttu-id="e51f9-126">CH0 连接到电位差计上的可变（中间）引脚（黄色）</span><span class="sxs-lookup"><span data-stu-id="e51f9-126">CH0 to variable (middle) pin on potentiometer (yellow)</span></span>

<span data-ttu-id="e51f9-127">为电位差计上的外部引脚提供 3.3V 的电源并接地。</span><span class="sxs-lookup"><span data-stu-id="e51f9-127">Supply 3.3V and ground to the outer pins on the potentiometer.</span></span> <span data-ttu-id="e51f9-128">顺序并不重要。</span><span class="sxs-lookup"><span data-stu-id="e51f9-128">Order is unimportant.</span></span>

<span data-ttu-id="e51f9-129">根据需要，请参阅以下引脚分配关系图：</span><span class="sxs-lookup"><span data-stu-id="e51f9-129">Refer to the following pinout diagrams as needed:</span></span>

| <span data-ttu-id="e51f9-130">MCP3008</span><span class="sxs-lookup"><span data-stu-id="e51f9-130">MCP3008</span></span>  | <span data-ttu-id="e51f9-131">Raspberry Pi GPIO</span><span class="sxs-lookup"><span data-stu-id="e51f9-131">Raspberry Pi GPIO</span></span> |
|----------|-------------------|
| :::image type="content" source="../media/mcp3008-diagram-thumb.png" alt-text="显示 MCP3008 的引脚分配关系图" lightbox="../media/mcp3008-diagram.png"::: | :::image type="content" source="../media/gpio-pinout-diagram-thumb.png" alt-text="显示 Raspberry Pi GPIO 标头引脚分配的关系图。图片由 Raspberry Pi 基金会提供。" lightbox="../media/gpio-pinout-diagram.png":::<br /><span data-ttu-id="e51f9-134">[图片由 Raspberry Pi 基金会提供](https://www.raspberrypi.org/documentation/usage/gpio/)。</span><span class="sxs-lookup"><span data-stu-id="e51f9-134">[Image courtesy Raspberry Pi Foundation](https://www.raspberrypi.org/documentation/usage/gpio/).</span></span>
 |

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a><span data-ttu-id="e51f9-135">创建应用</span><span class="sxs-lookup"><span data-stu-id="e51f9-135">Create the app</span></span>

<span data-ttu-id="e51f9-136">在首选开发环境中完成以下步骤：</span><span class="sxs-lookup"><span data-stu-id="e51f9-136">Complete the following steps in your preferred development environment:</span></span>

1. <span data-ttu-id="e51f9-137">使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md) 创建一个新 .Net 控制台应用。</span><span class="sxs-lookup"><span data-stu-id="e51f9-137">Create a new .NET Console App using either the [.NET CLI](../../core/tools/dotnet-new.md) or [Visual Studio](../../core/tutorials/with-visual-studio.md).</span></span> <span data-ttu-id="e51f9-138">将其命名为 AdcTutorial。</span><span class="sxs-lookup"><span data-stu-id="e51f9-138">Name it *AdcTutorial*.</span></span>

    ```dotnetcli
    dotnet new console -o AdcTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. <span data-ttu-id="e51f9-139">将 Program.cs 的内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="e51f9-139">Replace the contents of *Program.cs* with the following code:</span></span>

    :::code language="csharp" source="~/iot-samples/tutorials/AdcTutorial/Program.cs" :::

    <span data-ttu-id="e51f9-140">在上述代码中：</span><span class="sxs-lookup"><span data-stu-id="e51f9-140">In the preceding code:</span></span>

    - <span data-ttu-id="e51f9-141">`hardwareSpiSettings` 设置为 `SpiConnectionSettings` 的新实例。</span><span class="sxs-lookup"><span data-stu-id="e51f9-141">`hardwareSpiSettings` is set to a new instance of `SpiConnectionSettings`.</span></span> <span data-ttu-id="e51f9-142">构造函数将 `busId` 参数设置为 0，将 `chipSelectLine` 参数设置为 0。</span><span class="sxs-lookup"><span data-stu-id="e51f9-142">The constructor sets the `busId` parameter to 0 and the `chipSelectLine` parameter to 0.</span></span>
    - <span data-ttu-id="e51f9-143">[using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)通过调用 `SpiDevice.Create` 和传入 `hardwareSpiSettings` 来创建 `SpiDevice` 的实例。</span><span class="sxs-lookup"><span data-stu-id="e51f9-143">A [using declaration](../../csharp/whats-new/csharp-8.md#using-declarations) creates an instance of `SpiDevice` by calling `SpiDevice.Create` and passing in `hardwareSpiSettings`.</span></span> <span data-ttu-id="e51f9-144">`SpiDevice` 表示 SPI 总线。</span><span class="sxs-lookup"><span data-stu-id="e51f9-144">This `SpiDevice` represents the SPI bus.</span></span> <span data-ttu-id="e51f9-145">`using` 声明可确保释放对象并正确释放硬件资源。</span><span class="sxs-lookup"><span data-stu-id="e51f9-145">The `using` declaration ensures the object is disposed and hardware resources are released properly.</span></span>
    - <span data-ttu-id="e51f9-146">其他 `using` 声明创建 `Mcp3008` 的实例，并将 `SpiDevice` 传递到构造函数中。</span><span class="sxs-lookup"><span data-stu-id="e51f9-146">Another `using` declaration creates an instance of `Mcp3008` and passes the `SpiDevice` into the constructor.</span></span>
    - <span data-ttu-id="e51f9-147">`while` 循环无限期运行。</span><span class="sxs-lookup"><span data-stu-id="e51f9-147">A `while` loop runs indefinitely.</span></span> <span data-ttu-id="e51f9-148">每次迭代：</span><span class="sxs-lookup"><span data-stu-id="e51f9-148">Each iteration:</span></span>
        1. <span data-ttu-id="e51f9-149">通过调用 `mcp.Read(0)` 来读取 ADC 上的 CH0 值。</span><span class="sxs-lookup"><span data-stu-id="e51f9-149">Reads the value of CH0 on the ADC by calling `mcp.Read(0)`.</span></span>
        1. <span data-ttu-id="e51f9-150">将值除以 10.24。</span><span class="sxs-lookup"><span data-stu-id="e51f9-150">Divides the value by 10.24.</span></span> <span data-ttu-id="e51f9-151">MCP3008 是 10 位 ADC，表示它会返回范围为 0-1023 的 1024 个可能的值。</span><span class="sxs-lookup"><span data-stu-id="e51f9-151">The MCP3008 is a 10-bit ADC, which means it returns 1024 possible values ranging 0-1023.</span></span> <span data-ttu-id="e51f9-152">将值除以 10.24 表示百分比形式的值。</span><span class="sxs-lookup"><span data-stu-id="e51f9-152">Dividing the value by 10.24 represents the value as a percentage.</span></span>
        1. <span data-ttu-id="e51f9-153">将小数值四舍五入到最接近的整数。</span><span class="sxs-lookup"><span data-stu-id="e51f9-153">Rounds the value to the nearest integer.</span></span>
        1. <span data-ttu-id="e51f9-154">将值以百分比形式写入控制台。</span><span class="sxs-lookup"><span data-stu-id="e51f9-154">Writes the value to the console formatted as a percentage.</span></span>
        1. <span data-ttu-id="e51f9-155">休眠 500 毫秒。</span><span class="sxs-lookup"><span data-stu-id="e51f9-155">Sleeps 500 ms.</span></span>

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. <span data-ttu-id="e51f9-156">通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。</span><span class="sxs-lookup"><span data-stu-id="e51f9-156">Run the app on the Raspberry Pi by switching to the deployment directory and running the executable.</span></span>

    ```bash
    ./AdcTutorial
    ```

    <span data-ttu-id="e51f9-157">旋转电位差计刻度时观察输出。</span><span class="sxs-lookup"><span data-stu-id="e51f9-157">Observe the output as you rotate the potentiometer dial.</span></span> <span data-ttu-id="e51f9-158">这是因为电位差计会改变供给 ADC 上 CH0 的电压。</span><span class="sxs-lookup"><span data-stu-id="e51f9-158">This is due to the potentiometer varying the voltage supplied to CH0 on the ADC.</span></span> <span data-ttu-id="e51f9-159">ADC 将 CH0 上的输入电压与提供给 V<sub>REF</sub> 的参考电压进行比较，从而生成一个值。</span><span class="sxs-lookup"><span data-stu-id="e51f9-159">The ADC compares the input voltage on CH0 to the reference voltage supplied to V<sub>REF</sub> to generate a value.</span></span>

1. <span data-ttu-id="e51f9-160">按 <kbd>Ctrl+C</kbd> 终止程序。</span><span class="sxs-lookup"><span data-stu-id="e51f9-160">Terminate the program by pressing <kbd>Ctrl+C</kbd>.</span></span>

<span data-ttu-id="e51f9-161">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="e51f9-161">Congratulations!</span></span> <span data-ttu-id="e51f9-162">你使用 SPI 从模数转换器读取了值。</span><span class="sxs-lookup"><span data-stu-id="e51f9-162">You've used SPI to read values from an analog-to-digital converter.</span></span>

## <a name="get-the-source-code"></a><span data-ttu-id="e51f9-163">获取源代码</span><span class="sxs-lookup"><span data-stu-id="e51f9-163">Get the source code</span></span>

<span data-ttu-id="e51f9-164">[GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/AdcTutorial) 上提供此教程的源。</span><span class="sxs-lookup"><span data-stu-id="e51f9-164">The source for this tutorial is [available on GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/AdcTutorial).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e51f9-165">后续步骤</span><span class="sxs-lookup"><span data-stu-id="e51f9-165">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e51f9-166">了解如何使用常规用途输入/输出使 LED 闪烁</span><span class="sxs-lookup"><span data-stu-id="e51f9-166">Learn to use General Purpose Input/Output to blink an LED</span></span>](../tutorials/blink-led.md)
