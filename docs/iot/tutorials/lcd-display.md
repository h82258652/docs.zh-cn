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
# <a name="display-text-on-an-lcd"></a><span data-ttu-id="28c0a-103">在 LCD 上显示文本</span><span class="sxs-lookup"><span data-stu-id="28c0a-103">Display text on an LCD</span></span>

<span data-ttu-id="28c0a-104">LCD 字符显示器可用于显示信息，而无需外部监视器。</span><span class="sxs-lookup"><span data-stu-id="28c0a-104">LCD character displays are useful for displaying information without the need for an external monitor.</span></span> <span data-ttu-id="28c0a-105">常见的 LCD 字符显示器可以直接连接到 GPIO 引脚，但这种方法最多需要使用 10 个 GPIO 引脚。</span><span class="sxs-lookup"><span data-stu-id="28c0a-105">Common LCD character displays can be connected directly to the GPIO pins, but such an approach requires the use of up to 10 GPIO pins.</span></span> <span data-ttu-id="28c0a-106">对于需要连接到设备组合的场景，将如此多的 GPIO 标头分配给字符显示器通常不切实际。</span><span class="sxs-lookup"><span data-stu-id="28c0a-106">For scenarios that require connecting to a combination of devices, devoting so much of the GPIO header to a character display is often impractical.</span></span>

<span data-ttu-id="28c0a-107">许多制造商会销售带有集成 GPIO 扩展器的 20x4 LCD 字符显示器。</span><span class="sxs-lookup"><span data-stu-id="28c0a-107">Many manufacturers sell 20x4 LCD character displays with an integrated GPIO expander.</span></span> <span data-ttu-id="28c0a-108">字符显示器将直接连接到 GPIO 扩展器，然后通过内置集成电路 (I2C) 串行协议连接到 Raspberry Pi。</span><span class="sxs-lookup"><span data-stu-id="28c0a-108">The character display connects directly to the GPIO expander, which then connects to the Raspberry Pi via the Inter-Integrated Circuit (I2C) serial protocol.</span></span>

<span data-ttu-id="28c0a-109">在本主题中，你将使用 .NET 通过 I2C GPIO 扩展器在 LCD 字符显示器上显示文本。</span><span class="sxs-lookup"><span data-stu-id="28c0a-109">In this topic, you will use .NET to display text on an LCD character display using an I2C GPIO expander.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28c0a-110">先决条件</span><span class="sxs-lookup"><span data-stu-id="28c0a-110">Prerequisites</span></span>

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- [<span data-ttu-id="28c0a-111">带有 I2C 接口的 20x4 LCD 字符显示器</span><span class="sxs-lookup"><span data-stu-id="28c0a-111">20x4 LCD Character Display with I2C interface</span></span>](https://www.bing.com/images/search?q=20x4+lcd+display+with+i2c)
- <span data-ttu-id="28c0a-112">跳线</span><span class="sxs-lookup"><span data-stu-id="28c0a-112">Jumper wires</span></span>
- <span data-ttu-id="28c0a-113">线路板（可选/推荐）</span><span class="sxs-lookup"><span data-stu-id="28c0a-113">Breadboard (optional/recommended)</span></span>
- <span data-ttu-id="28c0a-114">Raspberry Pi GPIO 分线板（可选/推荐）</span><span class="sxs-lookup"><span data-stu-id="28c0a-114">Raspberry Pi GPIO breakout board (optional/recommended)</span></span>
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

> [!NOTE]
> <span data-ttu-id="28c0a-115">LCD 字符显示器的制造商有很多。</span><span class="sxs-lookup"><span data-stu-id="28c0a-115">There are many manufacturers of LCD character displays.</span></span> <span data-ttu-id="28c0a-116">大多数设计都相同，并且制造商不应对功能进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="28c0a-116">Most designs are identical, and the manufacturer shouldn't make any difference to the functionality.</span></span> <span data-ttu-id="28c0a-117">作为参考，本教程按照 [SunFounder LCD2004](https://www.sunfounder.com/lcd2004-module.html) 开发。</span><span class="sxs-lookup"><span data-stu-id="28c0a-117">For reference, this tutorial was developed with the [SunFounder LCD2004](https://www.sunfounder.com/lcd2004-module.html).</span></span>

[!INCLUDE [prepare-pi-i2c](../includes/prepare-pi-i2c.md)]

## <a name="prepare-the-hardware"></a><span data-ttu-id="28c0a-118">准备硬件</span><span class="sxs-lookup"><span data-stu-id="28c0a-118">Prepare the hardware</span></span>

<span data-ttu-id="28c0a-119">使用跳线将 I2C GPIO 扩展器上的四个引脚连接到 Raspberry Pi，如下所示：</span><span class="sxs-lookup"><span data-stu-id="28c0a-119">Use jumper wires to connect the four pins on the I2C GPIO expander to the Raspberry Pi as follows:</span></span>

- <span data-ttu-id="28c0a-120">GND 到地面</span><span class="sxs-lookup"><span data-stu-id="28c0a-120">GND to ground</span></span>
- <span data-ttu-id="28c0a-121">VCC 到 5V</span><span class="sxs-lookup"><span data-stu-id="28c0a-121">VCC to 5V</span></span>
- <span data-ttu-id="28c0a-122">SDA 到 SDA (GPIO 2)</span><span class="sxs-lookup"><span data-stu-id="28c0a-122">SDA to SDA (GPIO 2)</span></span>
- <span data-ttu-id="28c0a-123">SCL 到 SCL (GPIO 3)</span><span class="sxs-lookup"><span data-stu-id="28c0a-123">SCL to SCL (GPIO 3)</span></span>

<span data-ttu-id="28c0a-124">请根据需要参阅下图：</span><span class="sxs-lookup"><span data-stu-id="28c0a-124">Refer to the following figures as needed:</span></span>

| <span data-ttu-id="28c0a-125">I2C 接口（显示器背面）</span><span class="sxs-lookup"><span data-stu-id="28c0a-125">I2C interface (back of display)</span></span> | <span data-ttu-id="28c0a-126">Raspberry Pi GPIO</span><span class="sxs-lookup"><span data-stu-id="28c0a-126">Raspberry Pi GPIO</span></span> |
|---------------------------------|-------------------|
| :::image type="content" source="../media/character-display-i2c-thumb.png" alt-text="显示 I2C GPIO 扩展器的字符显示器背面的图像。" lightbox="../media/character-display-i2c.png"::: | :::image type="content" source="../media/gpio-pinout-diagram-thumb.png" alt-text="显示 Raspberry Pi GPIO 标头引脚分配的关系图。图片由 Raspberry Pi 基金会提供。" lightbox="../media/gpio-pinout-diagram.png":::<br /><span data-ttu-id="28c0a-129">[图片由 Raspberry Pi 基金会提供](https://www.raspberrypi.org/documentation/usage/gpio/)。</span><span class="sxs-lookup"><span data-stu-id="28c0a-129">[Image courtesy Raspberry Pi Foundation](https://www.raspberrypi.org/documentation/usage/gpio/).</span></span>
 |

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a><span data-ttu-id="28c0a-130">创建应用</span><span class="sxs-lookup"><span data-stu-id="28c0a-130">Create the app</span></span>

<span data-ttu-id="28c0a-131">在首选开发环境中完成以下步骤：</span><span class="sxs-lookup"><span data-stu-id="28c0a-131">Complete the following steps in your preferred development environment:</span></span>

1. <span data-ttu-id="28c0a-132">使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md) 创建新 .Net 控制台应用。</span><span class="sxs-lookup"><span data-stu-id="28c0a-132">Create a new .NET Console App using either the [.NET CLI](../../core/tools/dotnet-new.md) or [Visual Studio](../../core/tutorials/with-visual-studio.md).</span></span> <span data-ttu-id="28c0a-133">将其命名为 LcdTutorial。</span><span class="sxs-lookup"><span data-stu-id="28c0a-133">Name it *LcdTutorial*.</span></span>

    ```dotnetcli
    dotnet new console -o LcdTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. <span data-ttu-id="28c0a-134">将 Program.cs 的内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="28c0a-134">Replace the contents of *Program.cs* with the following code:</span></span>

    :::code language="csharp" source="~/iot-samples/tutorials/LcdTutorial/Program.cs" :::

    <span data-ttu-id="28c0a-135">在上述代码中：</span><span class="sxs-lookup"><span data-stu-id="28c0a-135">In the preceding code:</span></span>

    - <span data-ttu-id="28c0a-136">一个 [using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)通过调用 `I2cDevice.Create` 并传入带有 `busId` 和 `deviceAddress` 参数的新 `I2cConnectionSettings` 来创建 `I2cDevice` 的实例。</span><span class="sxs-lookup"><span data-stu-id="28c0a-136">A [using declaration](../../csharp/whats-new/csharp-8.md#using-declarations) creates an instance of `I2cDevice` by calling `I2cDevice.Create` and passing in a new `I2cConnectionSettings` with the `busId` and `deviceAddress` parameters.</span></span> <span data-ttu-id="28c0a-137">这个 `I2cDevice` 表示 I2C 总线。</span><span class="sxs-lookup"><span data-stu-id="28c0a-137">This `I2cDevice` represents the I2C bus.</span></span> <span data-ttu-id="28c0a-138">`using` 声明可确保对象已处置，硬件资源已正确释放。</span><span class="sxs-lookup"><span data-stu-id="28c0a-138">The `using` declaration ensures the object is disposed and hardware resources are released properly.</span></span>

        > [!WARNING]
        > <span data-ttu-id="28c0a-139">GPIO 扩展器的设备地址取决于制造商使用的芯片。</span><span class="sxs-lookup"><span data-stu-id="28c0a-139">The device address for the GPIO expander depends on the chip used by the manufacturer.</span></span> <span data-ttu-id="28c0a-140">配备 PCF8574 芯片的 GPIO 扩展器使用地址 `0x27`，而使用 PCF8574A 芯片的扩展器使用地址 `0x3F`。</span><span class="sxs-lookup"><span data-stu-id="28c0a-140">GPIO expanders equipped with a PCF8574 use the address `0x27`, while those using PCF8574A chips use `0x3F`.</span></span> <span data-ttu-id="28c0a-141">请查阅你的 LCD 文档。</span><span class="sxs-lookup"><span data-stu-id="28c0a-141">Consult your LCD's documentation.</span></span>

    - <span data-ttu-id="28c0a-142">另一个 `using` 声明创建 `Pcf8574` 的实例，并将 `I2cDevice` 传递到构造函数中。</span><span class="sxs-lookup"><span data-stu-id="28c0a-142">Another `using` declaration creates an instance of `Pcf8574` and passes the `I2cDevice` into the constructor.</span></span> <span data-ttu-id="28c0a-143">此实例表示 GPIO 扩展器。</span><span class="sxs-lookup"><span data-stu-id="28c0a-143">This instance represents the GPIO expander.</span></span>
    - <span data-ttu-id="28c0a-144">另一个 `using` 声明创建 `Lcd2004` 的实例以表示显示器。</span><span class="sxs-lookup"><span data-stu-id="28c0a-144">Another `using` declaration creates an instance of `Lcd2004` to represent the display.</span></span> <span data-ttu-id="28c0a-145">多个参数已传递给构造函数，该构造函数描述用于与 GPIO 扩展器进行通信的设置。</span><span class="sxs-lookup"><span data-stu-id="28c0a-145">Several parameters are passed to the constructor describing the settings to use to communicate with the GPIO expander.</span></span> <span data-ttu-id="28c0a-146">GPIO 扩展器作为 `controller` 参数传递。</span><span class="sxs-lookup"><span data-stu-id="28c0a-146">The GPIO expander is passed as the `controller` parameter.</span></span>
    - <span data-ttu-id="28c0a-147">`while` 循环无限期运行。</span><span class="sxs-lookup"><span data-stu-id="28c0a-147">A `while` loop runs indefinitely.</span></span> <span data-ttu-id="28c0a-148">每次迭代：</span><span class="sxs-lookup"><span data-stu-id="28c0a-148">Each iteration:</span></span>
        1. <span data-ttu-id="28c0a-149">清除显示器。</span><span class="sxs-lookup"><span data-stu-id="28c0a-149">Clears the display.</span></span>
        1. <span data-ttu-id="28c0a-150">将光标位置设置为当前行的第一个位置。</span><span class="sxs-lookup"><span data-stu-id="28c0a-150">Sets the cursor position to the first position on the current line.</span></span>
        1. <span data-ttu-id="28c0a-151">将当前时间写入显示器上当前光标位置。</span><span class="sxs-lookup"><span data-stu-id="28c0a-151">Writes the current time to the display at the current cursor position.</span></span>
        1. <span data-ttu-id="28c0a-152">循环访问当前行计数器。</span><span class="sxs-lookup"><span data-stu-id="28c0a-152">Iterates the current line counter.</span></span>
        1. <span data-ttu-id="28c0a-153">休眠 1000 毫秒。</span><span class="sxs-lookup"><span data-stu-id="28c0a-153">Sleeps 1000 ms.</span></span>

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. <span data-ttu-id="28c0a-154">通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。</span><span class="sxs-lookup"><span data-stu-id="28c0a-154">Run the app on the Raspberry Pi by switching to the deployment directory and running the executable.</span></span>

    ```bash
    ./LcdTutorial
    ```

    <span data-ttu-id="28c0a-155">观察 LCD 字符显示器，当前时间应显示在每一行上。</span><span class="sxs-lookup"><span data-stu-id="28c0a-155">Observe the LCD character display as the current time displays on each line.</span></span>

    > [!TIP]
    > <span data-ttu-id="28c0a-156">如果显示器已亮起，但看不到任何文本，请尝试调整显示器背面的对比度拨盘。</span><span class="sxs-lookup"><span data-stu-id="28c0a-156">If the display is lit but you don't see any text, try adjusting the contrast dial on the back of the display.</span></span>

1. <span data-ttu-id="28c0a-157">按 <kbd>Ctrl+C</kbd> 终止程序。</span><span class="sxs-lookup"><span data-stu-id="28c0a-157">Terminate the program by pressing <kbd>Ctrl+C</kbd>.</span></span>

<span data-ttu-id="28c0a-158">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="28c0a-158">Congratulations!</span></span> <span data-ttu-id="28c0a-159">你已使用 I2C 和 GPIO 扩展器在 LCD 上显示了文本！</span><span class="sxs-lookup"><span data-stu-id="28c0a-159">You've displayed text on an LCD using a I2C and a GPIO expander!</span></span>

## <a name="get-the-source-code"></a><span data-ttu-id="28c0a-160">获取源代码</span><span class="sxs-lookup"><span data-stu-id="28c0a-160">Get the source code</span></span>

<span data-ttu-id="28c0a-161">[GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/LcdTutorial) 上提供此教程的源。</span><span class="sxs-lookup"><span data-stu-id="28c0a-161">The source for this tutorial is [available on GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/LcdTutorial).</span></span>

## <a name="next-steps"></a><span data-ttu-id="28c0a-162">后续步骤</span><span class="sxs-lookup"><span data-stu-id="28c0a-162">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="28c0a-163">了解如何使用常规用途输入/输出使 LED 闪烁</span><span class="sxs-lookup"><span data-stu-id="28c0a-163">Learn to use General Purpose Input/Output to blink an LED</span></span>](../tutorials/blink-led.md)
