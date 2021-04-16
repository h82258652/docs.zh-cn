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
# <a name="read-environmental-conditions-from-a-sensor"></a><span data-ttu-id="dc6c9-103">从传感器读取环境条件</span><span class="sxs-lookup"><span data-stu-id="dc6c9-103">Read environmental conditions from a sensor</span></span>

<span data-ttu-id="dc6c9-104">IoT 设备其中最常见的一种场景是检测环境条件。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-104">One of the most common scenarios for IoT devices is detection of environmental conditions.</span></span> <span data-ttu-id="dc6c9-105">有多种传感器可用于监视温度、湿度、气压等。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-105">A variety of sensors are available to monitor temperature, humidity, barometric pressure, and more.</span></span>

<span data-ttu-id="dc6c9-106">在本主题中，你将使用 .NET 从传感器读取环境条件。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-106">In this topic, you will use .NET to read environmental conditions from a sensor.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dc6c9-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="dc6c9-107">Prerequisites</span></span>

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- <span data-ttu-id="dc6c9-108">[BME280](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout) 湿度/气压/温度传感器分线板</span><span class="sxs-lookup"><span data-stu-id="dc6c9-108">[BME280](https://learn.adafruit.com/adafruit-bme280-humidity-barometric-pressure-temperature-sensor-breakout) humidity/barometric pressure/temperature sensor breakout</span></span>
- <span data-ttu-id="dc6c9-109">跳线</span><span class="sxs-lookup"><span data-stu-id="dc6c9-109">Jumper wires</span></span>
- <span data-ttu-id="dc6c9-110">线路板（可选）</span><span class="sxs-lookup"><span data-stu-id="dc6c9-110">Breadboard (optional)</span></span>
- <span data-ttu-id="dc6c9-111">Raspberry Pi GPIO 分线板（可选）</span><span class="sxs-lookup"><span data-stu-id="dc6c9-111">Raspberry Pi GPIO breakout board (optional)</span></span>
- [!INCLUDE [tutorial-prereq-dotnet](../includes/tutorial-prereq-dotnet.md)]

> [!IMPORTANT]
> <span data-ttu-id="dc6c9-112">BME280 分线板的制造商有很多。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-112">There are many manufacturers of BME280 breakouts.</span></span> <span data-ttu-id="dc6c9-113">大多数设计都类似，并且制造商不应对功能进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-113">Most designs are similar, and the manufacturer shouldn't make any difference to the functionality.</span></span> <span data-ttu-id="dc6c9-114">本教程尝试考虑差异。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-114">This tutorial attempts to account for variations.</span></span> <span data-ttu-id="dc6c9-115">确保你的 BME280 分线板包括一个内置集成电路 (I2C) 接口。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-115">Ensure your BME280 breakout includes an Inter-Integrated Circuit (I2C) interface.</span></span>

[!INCLUDE [prepare-pi-i2c](../includes/prepare-pi-i2c.md)]

## <a name="prepare-the-hardware"></a><span data-ttu-id="dc6c9-116">准备硬件</span><span class="sxs-lookup"><span data-stu-id="dc6c9-116">Prepare the hardware</span></span>

<span data-ttu-id="dc6c9-117">使用硬件组件构建电路，如下图所示：</span><span class="sxs-lookup"><span data-stu-id="dc6c9-117">Use the hardware components to build the circuit as depicted in the following diagram:</span></span>

:::image type="content" source="../media/rpi-bmp280_i2c-thumb.png" alt-text="该 Fritzing 关系图显示了从 Raspberry Pi 到 BME280 分线板的连接" lightbox="../media/rpi-bmp280_i2c.png":::

<span data-ttu-id="dc6c9-119">以下是从 Raspberry Pi 到 BME280 分线板的连接：</span><span class="sxs-lookup"><span data-stu-id="dc6c9-119">The following are the connections from the Raspberry Pi to the BME280 breakout:</span></span>

- <span data-ttu-id="dc6c9-120">3.3V 到 VIN 或 3V3（显示为红色）</span><span class="sxs-lookup"><span data-stu-id="dc6c9-120">3.3V to VIN *OR* 3V3 (shown in red)</span></span>
- <span data-ttu-id="dc6c9-121">地面到 GND（黑色）</span><span class="sxs-lookup"><span data-stu-id="dc6c9-121">Ground to GND (black)</span></span>
- <span data-ttu-id="dc6c9-122">SDA (GPIO 2) 到 SDI 或 SDA（蓝色）</span><span class="sxs-lookup"><span data-stu-id="dc6c9-122">SDA (GPIO 2) to SDI *OR* SDA (blue)</span></span>
- <span data-ttu-id="dc6c9-123">SCL (GPIO 3) 到 SCK 或 SCL（橙色）</span><span class="sxs-lookup"><span data-stu-id="dc6c9-123">SCL (GPIO 3) to SCK *OR* SCL (orange)</span></span>

[!INCLUDE [tutorial-rpi-gpio](../includes/tutorial-rpi-gpio.md)]

[!INCLUDE [gpio-breakout](../includes/gpio-breakout.md)]

## <a name="create-the-app"></a><span data-ttu-id="dc6c9-124">创建应用</span><span class="sxs-lookup"><span data-stu-id="dc6c9-124">Create the app</span></span>

<span data-ttu-id="dc6c9-125">在首选开发环境中完成以下步骤：</span><span class="sxs-lookup"><span data-stu-id="dc6c9-125">Complete the following steps in your preferred development environment:</span></span>

1. <span data-ttu-id="dc6c9-126">使用 [.NET CLI](../../core/tools/dotnet-new.md) 或 [Visual Studio](../../core/tutorials/with-visual-studio.md) 创建新 .Net 控制台应用。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-126">Create a new .NET Console App using either the [.NET CLI](../../core/tools/dotnet-new.md) or [Visual Studio](../../core/tutorials/with-visual-studio.md).</span></span> <span data-ttu-id="dc6c9-127">将其命名为 SensorTutorial。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-127">Name it *SensorTutorial*.</span></span>

    ```dotnetcli
    dotnet new console -o SensorTutorial
    ```

1. [!INCLUDE [tutorial-add-packages](../includes/tutorial-add-packages.md)]
1. <span data-ttu-id="dc6c9-128">将 Program.cs 的内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="dc6c9-128">Replace the contents of *Program.cs* with the following code:</span></span>

    :::code language="csharp" source="~/iot-samples/tutorials/SensorTutorial/Program.cs" :::

    <span data-ttu-id="dc6c9-129">在上述代码中：</span><span class="sxs-lookup"><span data-stu-id="dc6c9-129">In the preceding code:</span></span>

    - <span data-ttu-id="dc6c9-130">`i2cSettings` 设置为 `I2cConnectionSettings` 的新实例。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-130">`i2cSettings` is set to a new instance of `I2cConnectionSettings`.</span></span> <span data-ttu-id="dc6c9-131">构造函数将 `busId` 参数设置为 1，将 `deviceAddress` 参数设置为 `Bme280.DefaultI2cAddress`。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-131">The constructor sets the `busId` parameter to 1 and the `deviceAddress` parameter to `Bme280.DefaultI2cAddress`.</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="dc6c9-132">一些 BME280 分线板制造商使用辅助地址值。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-132">Some BME280 breakout manufacturers use the secondary address value.</span></span> <span data-ttu-id="dc6c9-133">对于这些设备，请使用 `Bme280.SecondaryI2cAddress`。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-133">For those devices, use `Bme280.SecondaryI2cAddress`.</span></span>

    - <span data-ttu-id="dc6c9-134">一个 [using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)通过调用 `I2cDevice.Create` 并传入 `i2cSettings` 来创建 `I2cDevice` 的实例。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-134">A [using declaration](../../csharp/whats-new/csharp-8.md#using-declarations) creates an instance of `I2cDevice` by calling `I2cDevice.Create` and passing in `i2cSettings`.</span></span> <span data-ttu-id="dc6c9-135">这个 `I2cDevice` 表示 I2C 总线。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-135">This `I2cDevice` represents the I2C bus.</span></span> <span data-ttu-id="dc6c9-136">`using` 声明可确保对象已处置，硬件资源已正确释放。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-136">The `using` declaration ensures the object is disposed and hardware resources are released properly.</span></span>
    - <span data-ttu-id="dc6c9-137">另一个 `using` 声明创建 `Bme280` 的实例以表示传感器。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-137">Another `using` declaration creates an instance of `Bme280` to represent the sensor.</span></span> <span data-ttu-id="dc6c9-138">`I2cDevice` 在构造函数中传递。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-138">The `I2cDevice` is passed in the constructor.</span></span>
    - <span data-ttu-id="dc6c9-139">通过调用 `GetMeasurementDuration` 检索芯片使用其当前（默认）设置进行度量所需的时间。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-139">The time required for the chip to take measurements with the chip's current (default) settings is retrieved by calling `GetMeasurementDuration`.</span></span>
    - <span data-ttu-id="dc6c9-140">`while` 循环无限期运行。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-140">A `while` loop runs indefinitely.</span></span> <span data-ttu-id="dc6c9-141">每次迭代：</span><span class="sxs-lookup"><span data-stu-id="dc6c9-141">Each iteration:</span></span>
        1. <span data-ttu-id="dc6c9-142">清除控制台。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-142">Clears the console.</span></span>
        1. <span data-ttu-id="dc6c9-143">将电源模式设置为 `Bmx280PowerMode.Forced`。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-143">Sets the power mode to `Bmx280PowerMode.Forced`.</span></span> <span data-ttu-id="dc6c9-144">这会强制芯片执行一次度量，存储结果，然后进入睡眠状态。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-144">This forces the chip to perform one measurement, store the results, and then sleep.</span></span>
        1. <span data-ttu-id="dc6c9-145">读取温度、压力、湿度和海拔高度的值。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-145">Reads the values for temperature, pressure, humidity, and altitude.</span></span>

            > [!NOTE]
            > <span data-ttu-id="dc6c9-146">海拔高度由设备绑定进行计算。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-146">Altitude is calculated by the device binding.</span></span> <span data-ttu-id="dc6c9-147">`TryReadAltitude` 的重载使用平均海平面级压力来生成估算值。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-147">This overload of `TryReadAltitude` uses mean sea level pressure to generate an estimate.</span></span>

        1. <span data-ttu-id="dc6c9-148">将当前环境条件写入控制台。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-148">Writes the current environmental conditions to the console.</span></span>
        1. <span data-ttu-id="dc6c9-149">休眠 1000 毫秒。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-149">Sleeps 1000 ms.</span></span>

1. [!INCLUDE [tutorial-build](../includes/tutorial-build.md)]
1. [!INCLUDE [tutorial-deploy](../includes/tutorial-deploy.md)]
1. <span data-ttu-id="dc6c9-150">通过切换到部署目录并运行可执行文件，在 Raspberry Pi 上运行该应用。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-150">Run the app on the Raspberry Pi by switching to the deployment directory and running the executable.</span></span>

    ```bash
    ./SensorTutorial
    ```

    <span data-ttu-id="dc6c9-151">在控制台中查看传感器输出。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-151">Observe the sensor output in the console.</span></span>

1. <span data-ttu-id="dc6c9-152">按 <kbd>Ctrl+C</kbd> 终止程序。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-152">Terminate the program by pressing <kbd>Ctrl+C</kbd>.</span></span>

<span data-ttu-id="dc6c9-153">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="dc6c9-153">Congratulations!</span></span> <span data-ttu-id="dc6c9-154">你已使用 I2C 从温度/湿度/气压传感器读取值！</span><span class="sxs-lookup"><span data-stu-id="dc6c9-154">You've used I2C to read values from a temperature/humidity/barometric pressure sensor!</span></span>

## <a name="get-the-source-code"></a><span data-ttu-id="dc6c9-155">获取源代码</span><span class="sxs-lookup"><span data-stu-id="dc6c9-155">Get the source code</span></span>

<span data-ttu-id="dc6c9-156">[GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/SensorTutorial) 上提供此教程的源。</span><span class="sxs-lookup"><span data-stu-id="dc6c9-156">The source for this tutorial is [available on GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/tutorials/SensorTutorial).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc6c9-157">后续步骤</span><span class="sxs-lookup"><span data-stu-id="dc6c9-157">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dc6c9-158">了解如何在 LCD 上显示文本</span><span class="sxs-lookup"><span data-stu-id="dc6c9-158">Learn how to display text on an LCD</span></span>](../tutorials/lcd-display.md)
