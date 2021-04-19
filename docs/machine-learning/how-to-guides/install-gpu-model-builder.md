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
# <a name="how-to-install-gpu-support-in-model-builder"></a>如何在 Model Builder 中安装 GPU 支持

了解如何安装 GPU 驱动程序，以便将 GPU 与 Model Builder 一起使用。

## <a name="prerequisites"></a>先决条件

- Model Builder Visual Studio 扩展。 自版本 16.6.1 起，该扩展内置于 Visual Studio 中。
- [Model Builder Visual Studio GPU 支持扩展](https://marketplace.visualstudio.com/items?itemName=MLNET.ModelBuilderGPU)。
- 至少一个 CUDA 兼容 GPU。 有关兼容 GPU 的列表，请参阅 [NVIDIA 指南](https://developer.nvidia.com/cuda-gpus)。
- NVIDIA 开发人员帐户。 如果没有帐户，请[创建一个免费帐户](https://developer.nvidia.com/developer-program)。

## <a name="install-dependencies"></a>安装依赖项

1. 安装 [CUDA v10.1](https://developer.nvidia.com/cuda-10.1-download-archive-update2)。 请确保安装 CUDA v10.1，而不是任何其他更新版本。 不能安装多个版本的 CUDA。
1. 安装 [cuDNN v7.6.4 for CUDA 10.1](https://developer.nvidia.com/rdp/cudnn-download)。 不能安装多个版本的 cuDNN。 下载 cuDNN v7.6.4 zip 文件并将其解压缩后，将 `<CUDNN_zip_files_path>\cuda\bin\cudnn64_7.dll` 复制到 `<YOUR_DRIVE>\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\bin`。

## <a name="troubleshooting"></a>疑难解答

**如何知道我拥有哪些 GPU？**

按照提供的说明操作：

1. 右键单击桌面
1. 如果在弹出窗口中看到“NVIDIA 控制面板”或“NVIDIA 显示”，则表示你有 NVIDIA GPU
1. 在弹出窗口中单击“NVIDIA 控制面板”或“NVIDIA 显示”
1. 查看“图形卡信息”
1. 你将看到 NVIDIA GPU 的名称

**我看不到 NVIDIA 控制面板（或它无法打开），但我知道我有 NVIDIA GPU。**

1. 打开“设备管理器”
1. 查看“显示适配器”
1. 为 GPU 安装适当的[驱动程序](https://www.nvidia.com/drivers) 。
