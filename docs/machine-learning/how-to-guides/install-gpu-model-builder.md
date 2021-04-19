---
title: 如何在 Model Builder 中安装 GPU 支持
description: 了解如何在 Model Builder 中安装 GPU 支持
ms.date: 04/08/2021
author: luisquintanilla
ms.author: luquinta
ms.topic: how-to
ms.openlocfilehash: 81f84a17429fd03506bbce30f5646941e4e80b3b
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255423"
---
# <a name="how-to-install-gpu-support-in-model-builder"></a><span data-ttu-id="60c32-103">如何在 Model Builder 中安装 GPU 支持</span><span class="sxs-lookup"><span data-stu-id="60c32-103">How to install GPU support in Model Builder</span></span>

<span data-ttu-id="60c32-104">了解如何安装 GPU 驱动程序，以便将 GPU 与 Model Builder 一起使用。</span><span class="sxs-lookup"><span data-stu-id="60c32-104">Learn how to install the GPU drivers to use your GPU with Model Builder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60c32-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="60c32-105">Prerequisites</span></span>

- <span data-ttu-id="60c32-106">Model Builder Visual Studio 扩展。</span><span class="sxs-lookup"><span data-stu-id="60c32-106">Model Builder Visual Studio extension.</span></span> <span data-ttu-id="60c32-107">自版本 16.6.1 起，该扩展内置于 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="60c32-107">The extension is built into Visual Studio as of version 16.6.1.</span></span>
- <span data-ttu-id="60c32-108">[Model Builder Visual Studio GPU 支持扩展](https://marketplace.visualstudio.com/items?itemName=MLNET.ModelBuilderGPU)。</span><span class="sxs-lookup"><span data-stu-id="60c32-108">[Model Builder Visual Studio GPU support extension](https://marketplace.visualstudio.com/items?itemName=MLNET.ModelBuilderGPU).</span></span>
- <span data-ttu-id="60c32-109">至少一个 CUDA 兼容 GPU。</span><span class="sxs-lookup"><span data-stu-id="60c32-109">At least one CUDA compatible GPU.</span></span> <span data-ttu-id="60c32-110">有关兼容 GPU 的列表，请参阅 [NVIDIA 指南](https://developer.nvidia.com/cuda-gpus)。</span><span class="sxs-lookup"><span data-stu-id="60c32-110">For a list of compatible GPUs, see [NVIDIA's guide](https://developer.nvidia.com/cuda-gpus).</span></span>
- <span data-ttu-id="60c32-111">NVIDIA 开发人员帐户。</span><span class="sxs-lookup"><span data-stu-id="60c32-111">NVIDIA developer account.</span></span> <span data-ttu-id="60c32-112">如果没有帐户，请[创建一个免费帐户](https://developer.nvidia.com/developer-program)。</span><span class="sxs-lookup"><span data-stu-id="60c32-112">If you don't have one, [create a free account](https://developer.nvidia.com/developer-program).</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="60c32-113">安装依赖项</span><span class="sxs-lookup"><span data-stu-id="60c32-113">Install dependencies</span></span>

1. <span data-ttu-id="60c32-114">安装 [CUDA v10.1](https://developer.nvidia.com/cuda-10.1-download-archive-update2)。</span><span class="sxs-lookup"><span data-stu-id="60c32-114">Install [CUDA v10.1](https://developer.nvidia.com/cuda-10.1-download-archive-update2).</span></span> <span data-ttu-id="60c32-115">请确保安装 CUDA v10.1，而不是任何其他更新版本。</span><span class="sxs-lookup"><span data-stu-id="60c32-115">Make sure you install CUDA v10.1, not any other newer version.</span></span> <span data-ttu-id="60c32-116">不能安装多个版本的 CUDA。</span><span class="sxs-lookup"><span data-stu-id="60c32-116">You cannot have multiple versions of CUDA installed.</span></span>
1. <span data-ttu-id="60c32-117">安装 [cuDNN v7.6.4 for CUDA 10.1](https://developer.nvidia.com/rdp/cudnn-download)。</span><span class="sxs-lookup"><span data-stu-id="60c32-117">Install [cuDNN v7.6.4 for CUDA 10.1](https://developer.nvidia.com/rdp/cudnn-download).</span></span> <span data-ttu-id="60c32-118">不能安装多个版本的 cuDNN。</span><span class="sxs-lookup"><span data-stu-id="60c32-118">You cannot have multiple versions of cuDNN installed.</span></span> <span data-ttu-id="60c32-119">下载 cuDNN v7.6.4 zip 文件并将其解压缩后，将 `<CUDNN_zip_files_path>\cuda\bin\cudnn64_7.dll` 复制到 `<YOUR_DRIVE>\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\bin`。</span><span class="sxs-lookup"><span data-stu-id="60c32-119">After downloading cuDNN v7.6.4 zip file and unpacking it, copy `<CUDNN_zip_files_path>\cuda\bin\cudnn64_7.dll` to `<YOUR_DRIVE>\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\bin`.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="60c32-120">疑难解答</span><span class="sxs-lookup"><span data-stu-id="60c32-120">Troubleshooting</span></span>

<span data-ttu-id="60c32-121">**如何知道我拥有哪些 GPU？**</span><span class="sxs-lookup"><span data-stu-id="60c32-121">**How do I know what GPU I have?**</span></span>

<span data-ttu-id="60c32-122">按照提供的说明操作：</span><span class="sxs-lookup"><span data-stu-id="60c32-122">Follow instructions provided:</span></span>

1. <span data-ttu-id="60c32-123">右键单击桌面</span><span class="sxs-lookup"><span data-stu-id="60c32-123">Right-click on desktop</span></span>
1. <span data-ttu-id="60c32-124">如果在弹出窗口中看到“NVIDIA 控制面板”或“NVIDIA 显示”，则表示你有 NVIDIA GPU</span><span class="sxs-lookup"><span data-stu-id="60c32-124">If you see "NVIDIA Control Panel" or "NVIDIA Display" in the pop-up window, you have an NVIDIA GPU</span></span>
1. <span data-ttu-id="60c32-125">在弹出窗口中单击“NVIDIA 控制面板”或“NVIDIA 显示”</span><span class="sxs-lookup"><span data-stu-id="60c32-125">Click on "NVIDIA Control Panel" or "NVIDIA Display" in the pop-up window</span></span>
1. <span data-ttu-id="60c32-126">查看“图形卡信息”</span><span class="sxs-lookup"><span data-stu-id="60c32-126">Look at "Graphics Card Information"</span></span>
1. <span data-ttu-id="60c32-127">你将看到 NVIDIA GPU 的名称</span><span class="sxs-lookup"><span data-stu-id="60c32-127">You will see the name of your NVIDIA GPU</span></span>

<span data-ttu-id="60c32-128">**我看不到 NVIDIA 控制面板（或它无法打开），但我知道我有 NVIDIA GPU。**</span><span class="sxs-lookup"><span data-stu-id="60c32-128">**I don't see NVIDIA Control Panel (or it fails to open) but I know I have an NVIDIA GPU.**</span></span>

1. <span data-ttu-id="60c32-129">打开“设备管理器”</span><span class="sxs-lookup"><span data-stu-id="60c32-129">Open Device Manager</span></span>
1. <span data-ttu-id="60c32-130">查看“显示适配器”</span><span class="sxs-lookup"><span data-stu-id="60c32-130">Look at Display adapters</span></span>
1. <span data-ttu-id="60c32-131">为 GPU 安装适当的[驱动程序](https://www.nvidia.com/drivers) 。</span><span class="sxs-lookup"><span data-stu-id="60c32-131">Install appropriate [driver](https://www.nvidia.com/drivers) for your GPU.</span></span>
