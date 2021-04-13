---
title: 在 Alpine 上安装 .NET - .NET
description: 演示在 Alpine 上安装 .NET SDK 和 .NET 运行时的各种方式。
author: adegeo
ms.author: adegeo
ms.date: 01/06/2021
ms.openlocfilehash: 6cd36fa6329d3c1a5835d4c202ac0024ffa41892
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079500"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-alpine"></a>在 Alpine 上安装 .NET SDK 或 .NET 运行时

本文介绍如何在 Alpine 上安装 .NET。 如果 Alpine 版本不再受到支持，则该版本不再支持 .NET。 不过，可以按照这些说明在这些版本上运行 .NET，即使它不受支持。

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="install"></a>安装

安装程序不可用于 Alpine Linux。 必须使用以下方式之一安装 .NET：

- [Snap 包](linux-snap.md)
- [使用 install-dotnet.sh 脚本安装](linux-scripted-manual.md#scripted-install)
- [手动提取二进制文件](linux-scripted-manual.md#manual-install)

## <a name="supported-distributions"></a>支持的发行版

下表列出了当前支持的 .NET 版本以及支持它们的 Alpine 版本。 这些版本在 [.NET 到达支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 [Alpine 的版本到达有效期](https://wiki.alpinelinux.org/wiki/Alpine_Linux:Releases)之前仍受支持。

- ✔️ 指示 Alpine 或 .NET 版本仍受支持。
- ❌ 指示 Alpine 或 .NET 版本在该 Alpine 发行版本上不受支持。
- 当 Alpine 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| Alpine  | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|-------- |---------------|---------------|----------------|
| ✔️ 3.13 | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ 3.12 | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ 3.11 | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ 3.10 | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ 3.9  | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ 3.8  | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |

以下 .NET 版本不再受到支持。 这些版本的下载仍保持发布状态：

- 3.0
- 2.2
- 2.0

## <a name="dependencies"></a>依赖项

Alpine Linux 上的 .NET 要求安装以下依赖项：

- icu-libs
- krb5-libs
- libgcc
- libgdiplus（.NET 应用需要 System.Drawing.Common 程序集时）
- libintl
- libssl1.1（Alpine v3.9 或更高版本）
- libssl1.0（Alpine v3.8 或更低版本）
- libstdc++
- zlib

若要安装必需项，请运行以下命令：

```bash
apk add bash icu-libs krb5-libs libgcc libintl libssl1.1 libstdc++ zlib
```

若要安装 libgdiplus，可能需要指定一个存储库：

```bash
apk add libgdiplus --repository https://dl-3.alpinelinux.org/alpine/edge/testing/
```

## <a name="next-steps"></a>后续步骤

- [如何为 .NET CLI 启用 Tab 自动补全](../tools/enable-tab-autocomplete.md)
- [教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序](../tutorials/with-visual-studio-code.md)
