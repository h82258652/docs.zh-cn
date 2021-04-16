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
# <a name="blink-an-led"></a><span data-ttu-id="bf6c5-103">闪烁 LED</span><span class="sxs-lookup"><span data-stu-id="bf6c5-103">Blink an LED</span></span>

<span data-ttu-id="bf6c5-104">可以单独控制常规用途 I/O (GPIO) 引脚。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-104">General-purpose I/O (GPIO) pins can be controlled individually.</span></span> <span data-ttu-id="bf6c5-105">这对控制 LED、中继和其他有状态设备很有用。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-105">This is useful for controlling LEDs, relays, and other stateful devices.</span></span> <span data-ttu-id="bf6c5-106">在本主题中，你将使用 .NET 和 Raspberry Pi 的 GPIO 引脚使 LED 通电，并重复闪烁。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-106">In this topic, you will use .NET and your Raspberry Pi's GPIO pins to power an LED and blink it repeatedly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf6c5-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="bf6c5-107">Prerequisites</span></span>

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- <span data-ttu-id="bf6c5-108">5 毫米 LED</span><span class="sxs-lookup"><span data-stu-id="bf6c5-108">5 mm LED</span></span>
- <span data-ttu-id="bf6c5-109">330 Ω 电阻器</span><span class="sxs-lookup"><span data-stu-id="bf6c5-109">330 Ω resistor</span></span>
- <span data-ttu-id="bf6c5-110">试验板</span><span class="sxs-lookup"><span data-stu-id="bf6c5-110">Breadboard</span></span>
- <span data-ttu-id="bf6c5-111">跳线</span><span class="sxs-lookup"><span data-stu-id="bf6c5-111">Jumper wires</span></span>
- <span data-ttu-id="bf6c5-112">Raspberry Pi GPIO 分线板（可选/推荐）</span><span class="sxs-lookup"><span data-stu-id="bf6c5-112">Raspberry Pi GPIO breakout board (optional/recommended)</span></span>
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

[!INCLUDE [ensure-ssh](../includes/ensure-ssh.md)]

## <a name="prepare-the-hardware"></a><span data-ttu-id="bf6c5-113">准备硬件</span><span class="sxs-lookup"><span data-stu-id="bf6c5-113">Prepare the hardware</span></span>

<span data-ttu-id="bf6c5-114">使用硬件组件构建电路，如下图所示：</span><span class="sxs-lookup"><span data-stu-id="bf6c5-114">Use the hardware components to build the circuit as depicted in the following diagram:</span></span>

:::image type="content" source="../media/rpi-led_bb-thumb.png" alt-text="Fritzing 图显示具有 LED 和电阻器的线路" lightbox="../media/rpi-led_bb.png":::

<span data-ttu-id="bf6c5-116">上图描述了下列连接：</span><span class="sxs-lookup"><span data-stu-id="bf6c5-116">The image above depicts the following connections:</span></span>

- <span data-ttu-id="bf6c5-117">GPIO 18 到 LED 阳极（较长、正极导线）</span><span class="sxs-lookup"><span data-stu-id="bf6c5-117">GPIO 18 to LED anode (longer, positive lead)</span></span>
- <span data-ttu-id="bf6c5-118">LED 阴极（较短、负极导线）到 330 Ω 电阻器（任一端）</span><span class="sxs-lookup"><span data-stu-id="bf6c5-118">LED cathode (shorter, negative lead) to 330 Ω resistor (either end)</span></span>
- <span data-ttu-id="bf6c5-119">330 Ω 电阻器（另一端）到地面</span><span class="sxs-lookup"><span data-stu-id="bf6c5-119">330 Ω resistor (other end) to ground</span></span>

[!INCLUDE [tutorial-rpi-gpio](../includes/tutorial-rpi-gpio.md)]

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a><span data-ttu-id="bf6c5-120">创建应用</span><span class="sxs-lookup"><span data-stu-id="bf6c5-120">Create the app</span></span>

<span data-ttu-id="bf6c5-121">在首选开发环境中完成以下步骤：</span><span class="sxs-lookup"><span data-stu-id="bf6c5-121">Complete the following steps in your preferred development environment:</span></span>

1. <span data-ttu-id="bf6c5-122">使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md) 创建新 .Net 控制台应用。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-122">Create a new .NET Console App using either the [.NET CLI](../../core/tools/dotnet-new.md) or [Visual Studio](../../core/tutorials/with-visual-studio.md).</span></span> <span data-ttu-id="bf6c5-123">将其命名为 BlinkTutorial。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-123">Name it *BlinkTutorial*.</span></span>

    ```dotnetcli
    dotnet new console -o BlinkTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. <span data-ttu-id="bf6c5-124">将 Program.cs 的内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="bf6c5-124">Replace the contents of *Program.cs* with the following code:</span></span>

    :::code language="csharp" source="~/iot-samples/tutorials/BlinkTutorial/Program.cs" :::

    <span data-ttu-id="bf6c5-125">在上述代码中：</span><span class="sxs-lookup"><span data-stu-id="bf6c5-125">In the preceding code:</span></span>

    - <span data-ttu-id="bf6c5-126">[using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)创建 `GpioController` 实例。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-126">A [using declaration](../../csharp/whats-new/csharp-8.md#using-declarations) creates an instance of `GpioController`.</span></span> <span data-ttu-id="bf6c5-127">`using` 声明可确保对象已处置，硬件资源已正确释放。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-127">The `using` declaration ensures the object is disposed and hardware resources are released properly.</span></span>
    - <span data-ttu-id="bf6c5-128">GPIO 引脚 18 已为输出打开</span><span class="sxs-lookup"><span data-stu-id="bf6c5-128">GPIO pin 18 is opened for output</span></span>
    - <span data-ttu-id="bf6c5-129">`while` 循环无限期运行。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-129">A `while` loop runs indefinitely.</span></span> <span data-ttu-id="bf6c5-130">每次迭代：</span><span class="sxs-lookup"><span data-stu-id="bf6c5-130">Each iteration:</span></span>
        1. <span data-ttu-id="bf6c5-131">向 GPIO 引脚 18 写入值。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-131">Writes a value to GPIO pin 18.</span></span> <span data-ttu-id="bf6c5-132">如果 `ledOn` 为 true，则迭代写入 `PinValue.High`（开）。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-132">If `ledOn` is true, it writes `PinValue.High` (on).</span></span> <span data-ttu-id="bf6c5-133">否则，写入 `PinValue.Low`。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-133">Otherwise, it writes `PinValue.Low`.</span></span>
        1. <span data-ttu-id="bf6c5-134">休眠 1000 毫秒。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-134">Sleeps 1000 ms.</span></span>
        1. <span data-ttu-id="bf6c5-135">切换 `ledOn` 的值。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-135">Toggles the value of `ledOn`.</span></span>

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. <span data-ttu-id="bf6c5-136">通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-136">Run the app on the Raspberry Pi by switching to the deployment directory and running the executable.</span></span>

    ```bash
    ./BlinkTutorial
    ```

    <span data-ttu-id="bf6c5-137">LED 每秒闪烁一次。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-137">The LED blinks off and on every second.</span></span>

1. <span data-ttu-id="bf6c5-138">按 <kbd>Ctrl+C</kbd> 终止程序。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-138">Terminate the program by pressing <kbd>Ctrl+C</kbd>.</span></span>

<span data-ttu-id="bf6c5-139">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="bf6c5-139">Congratulations!</span></span> <span data-ttu-id="bf6c5-140">你已使用 GPIO 让 LED 闪烁。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-140">You've used GPIO to blink an LED.</span></span>

## <a name="get-the-source-code"></a><span data-ttu-id="bf6c5-141">获取源代码</span><span class="sxs-lookup"><span data-stu-id="bf6c5-141">Get the source code</span></span>

<span data-ttu-id="bf6c5-142">[GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/BlinkTutorial) 上提供此教程的源。</span><span class="sxs-lookup"><span data-stu-id="bf6c5-142">The source for this tutorial is [available on GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/BlinkTutorial).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf6c5-143">后续步骤</span><span class="sxs-lookup"><span data-stu-id="bf6c5-143">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bf6c5-144">了解如何从传感器读取环境条件</span><span class="sxs-lookup"><span data-stu-id="bf6c5-144">Learn how to read environmental conditions from a sensor</span></span>](../tutorials/temp-sensor.md)
