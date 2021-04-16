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
# <a name="quickstart---use-net-to-drive-a-raspberry-pi-sense-hat"></a><span data-ttu-id="1fdc3-103">快速入门 - 使用 .NET 驱动 Raspberry Pi Sense HAT</span><span class="sxs-lookup"><span data-stu-id="1fdc3-103">Quickstart - Use .NET to drive a Raspberry Pi Sense HAT</span></span>

<span data-ttu-id="1fdc3-104">Raspberry Pi [Sense HAT](https://www.raspberrypi.org/products/sense-hat/)（附加在开发板上的硬件）是 Raspberry Pi 的附加板  。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-104">The Raspberry Pi [Sense HAT](https://www.raspberrypi.org/products/sense-hat/) (**H** ardware **A** ttached on **T** op) is an add-on board for Raspberry Pi.</span></span> <span data-ttu-id="1fdc3-105">Sense HAT 配有一个 8×8 RGB LED 矩阵、一个包含五个按钮的操纵杆，以及以下传感器：</span><span class="sxs-lookup"><span data-stu-id="1fdc3-105">The Sense HAT is equipped with an 8×8 RGB LED matrix, a five-button joystick, and includes the following sensors:</span></span>

- <span data-ttu-id="1fdc3-106">陀螺仪</span><span class="sxs-lookup"><span data-stu-id="1fdc3-106">Gyroscope</span></span>
- <span data-ttu-id="1fdc3-107">加速计</span><span class="sxs-lookup"><span data-stu-id="1fdc3-107">Accelerometer</span></span>
- <span data-ttu-id="1fdc3-108">磁力计</span><span class="sxs-lookup"><span data-stu-id="1fdc3-108">Magnetometer</span></span>
- <span data-ttu-id="1fdc3-109">温度</span><span class="sxs-lookup"><span data-stu-id="1fdc3-109">Temperature</span></span>
- <span data-ttu-id="1fdc3-110">气压</span><span class="sxs-lookup"><span data-stu-id="1fdc3-110">Barometric pressure</span></span>
- <span data-ttu-id="1fdc3-111">湿度</span><span class="sxs-lookup"><span data-stu-id="1fdc3-111">Humidity</span></span>

<span data-ttu-id="1fdc3-112">此快速入门使用 .NET 从 Sense HAT 检索传感器值、响应操纵杆输入并驱动 LED 矩阵。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-112">This quickstart uses .NET to retrieve sensor values from the Sense HAT, respond to joystick input, and drive the LED matrix.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1fdc3-113">先决条件</span><span class="sxs-lookup"><span data-stu-id="1fdc3-113">Prerequisites</span></span>

- [!INCLUDE [prereq-rpi](../includes/prereq-rpi.md)]
- <span data-ttu-id="1fdc3-114">Sense HAT</span><span class="sxs-lookup"><span data-stu-id="1fdc3-114">Sense HAT</span></span>

[!INCLUDE [prepare-pi-i2c](../includes/prepare-pi-i2c.md)]

## <a name="run-the-quickstart"></a><span data-ttu-id="1fdc3-115">运行快速入门</span><span class="sxs-lookup"><span data-stu-id="1fdc3-115">Run the quickstart</span></span>

<span data-ttu-id="1fdc3-116">向 Raspberry Pi 附加 Sense HAT。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-116">Attach the Sense HAT to your Raspberry Pi.</span></span> <span data-ttu-id="1fdc3-117">在 Raspberry Pi 上的 Bash 提示符中（本地或远程），运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="1fdc3-117">From a Bash prompt on the Raspberry Pi (local or remote), run the following command:</span></span>

```bash
. <(wget -q -O - https://aka.ms/dotnet-iot-sensehat-quickstart)
```

<span data-ttu-id="1fdc3-118">该命令会下载并运行脚本。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-118">The command downloads and runs a script.</span></span> <span data-ttu-id="1fdc3-119">脚本：</span><span class="sxs-lookup"><span data-stu-id="1fdc3-119">The script:</span></span>

- <span data-ttu-id="1fdc3-120">安装 .NET SDK。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-120">Installs the .NET SDK.</span></span>
- <span data-ttu-id="1fdc3-121">克隆包含 Sense HAT 快速入门项目的 GitHub 存储库。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-121">Clones a GitHub repository that includes the Sense HAT quickstart project.</span></span>
- <span data-ttu-id="1fdc3-122">生成项目。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-122">Builds the project.</span></span>
- <span data-ttu-id="1fdc3-123">运行项目。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-123">Runs the project.</span></span>

<span data-ttu-id="1fdc3-124">显示传感器数据时，观察控制台输出。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-124">Observe the console output as sensor data is displayed.</span></span> <span data-ttu-id="1fdc3-125">LED 矩阵在蓝色背景中显示黄色像素。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-125">The LED matrix displays a yellow pixel on a field of blue.</span></span> <span data-ttu-id="1fdc3-126">握住操纵杆向任意方向移动，黄色像素也会沿该方向移动。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-126">Holding the joystick in any direction moves the yellow pixel in that direction.</span></span> <span data-ttu-id="1fdc3-127">单击操纵杆中间的按钮，背景会从蓝色切换为红色。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-127">Clicking the center joystick button causes the background to switch from blue to red.</span></span>

## <a name="get-the-source-code"></a><span data-ttu-id="1fdc3-128">获取源代码</span><span class="sxs-lookup"><span data-stu-id="1fdc3-128">Get the source code</span></span>

<span data-ttu-id="1fdc3-129">[可在 GitHub 上获取](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/quickstarts/SenseHat.Quickstart)此快速入门的源。</span><span class="sxs-lookup"><span data-stu-id="1fdc3-129">The source for this quickstart is [available on GitHub](https://github.com/MicrosoftDocs/dotnet-iot-assets/tree/master/quickstarts/SenseHat.Quickstart).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1fdc3-130">后续步骤</span><span class="sxs-lookup"><span data-stu-id="1fdc3-130">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1fdc3-131">了解如何使用常规用途输入/输出使 LED 闪烁</span><span class="sxs-lookup"><span data-stu-id="1fdc3-131">Learn to use General Purpose Input/Output to blink an LED</span></span>](../tutorials/blink-led.md)
