---
title: 在 Linux 发行版上安装 .NET
description: 了解哪些 Linux 发行版支持在 Linux 上安装 .NET。
author: adegeo
ms.author: adegeo
ms.date: 01/06/2021
ms.openlocfilehash: 2b79bccf747d37c368b61eb17d3472d75bd665b3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873713"
---
# <a name="install-net-on-linux"></a>在 Linux 上安装 .NET

> [!div class="op_single_selector"]
>
> - [在 Windows 上安装](windows.md)
> - [在 macOS 上安装](macos.md)
> - [在 Linux 上安装](linux.md)

.NET 在不同的 Linux 发行版上可用。 大多数 Linux 平台和发行版每年都有一个主要版本，并提供用于安装 .NET 的包管理器。 本文介绍当前支持的版本以及使用的包管理器。

本文其余部分详细介绍了 .NET 支持的每个主要 Linux 发行版。 所有 .NET 版本在 [.NET Core 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 Linux 发行版达到生命周期之前仍受支持。

为了实现最佳兼容性，请选择长期支持版本 (LTS)。

## <a name="unsupported-releases"></a>不支持的版本

以下 .NET 版本 ❌ 不再受到支持。 这些版本的下载仍保持发布状态：

- 3.0
- 2.2
- 2.0

以下部分未详细介绍这些不受支持的版本，如果你尝试安装它们，则实际数据可能有所不同。

## <a name="manual-installation"></a>手动安装

如果你不想在 Linux 上使用包管理器来安装 .NET，可以通过下列方法之一来安装 .NET：

- [Snap 包](linux-snap.md)
- [使用 install-dotnet.sh 脚本安装](linux-scripted-manual.md#scripted-install)
- [手动提取二进制文件](linux-scripted-manual.md#manual-install)

请务必查看相应的发行页，以详细了解任何可能会在手动安装时缺失的必需依赖项。

## <a name="alpine"></a>Alpine

下表列出了当前支持的 .NET 版本以及支持它们的 Alpine 版本。 这些版本在 [.NET 到达支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 [Alpine 的版本到达有效期](https://wiki.alpinelinux.org/wiki/Alpine_Linux:Releases)之前仍受支持。

- ✔️ 指示 Alpine 或 .NET 版本仍受支持。
- ❌ 指示 Alpine 或 .NET 版本在该 Alpine 发行版本上不受支持。
- 当 Alpine 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| Alpine                      | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|-----------------------------|---------------|---------------|----------------|
| ✔️ [3.13](linux-alpine.md)  | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [3.12](linux-alpine.md)  | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [3.11](linux-alpine.md)  | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [3.10](linux-alpine.md)  | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ [3.9](linux-alpine.md)   | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ [3.8](linux-alpine.md)   | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |

有关详细信息，请参阅[在 Alpine 上安装 .NET](linux-alpine.md)。

## <a name="centos"></a>CentOS

CentOS 7 使用 Yum 作为包管理器，CentOS 8 使用 DNF。

下表列出了 CentOS 7 和 CentOS 8 上当前受支持的 .NET 版本。 这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 CentOS 版本不再受支持之前仍受支持。

| CentOS                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|--------------------------|---------------|---------------|----------------|
| ✔️ [8](linux-centos.md#centos-8-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [7](linux-centos.md#centos-7-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |

有关详细信息，请参阅[在 CentOS 上安装 .NET](linux-centos.md)。

## <a name="debian"></a>Debian

Debian 将 APT（高级包工具）用作包管理器。

下表列出了当前支持的 .NET 版本以及支持它们的 Debian 版本。 这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 [Debian 的版本达到生命周期](https://wiki.debian.org/DebianReleases)之前仍受支持。

- ✔️ 指示 Debian 或 .NET 版本仍受支持。
- ❌ 指示 Debian 或 .NET 版本在该 Debian 版本上不受支持。
- 当 Debian 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| Debian                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|--------------------------|---------------|---------------|----------------|
| ✔️ [10](linux-debian.md#debian-10-)     | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [9](linux-debian.md#debian-9-)       | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ❌ [8](linux-debian.md#debian-8-)       | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |

有关详细信息，请参阅[在 Debian 上安装 .NET](linux-debian.md)。

## <a name="fedora"></a>Fedora

Fedora 将 DNF 用作其包管理器。

下表列出了当前支持的 .NET 版本以及支持它们的 Fedora 版本。 这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 [Fedora 版本达到生命周期](https://fedoraproject.org/wiki/End_of_life)之前仍受支持。

- ✔️ 指示 Fedora 或 .NET 版本仍受支持。
- ❌ 指示 Fedora 或 .NET 版本在该 Fedora 版本上不受支持。
- 当 Fedora 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| Fedora                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|--------------------------|---------------|---------------|----------------|
| ✔️ [33](linux-fedora.md#install-net-50) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [32](linux-fedora.md#install-net-50) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ❌ [31](linux-fedora.md#install-on-older-distributions) | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ [30](linux-fedora.md#install-on-older-distributions) | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ [29](linux-fedora.md#install-on-older-distributions) | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ [28](linux-fedora.md#install-on-older-distributions) | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |
| ❌ [27](linux-fedora.md#install-on-older-distributions) | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |

有关详细信息，请参阅[在 Fedora 上安装 .NET](linux-fedora.md)。

## <a name="opensuse"></a>openSUSE

openSUSE 将 zypper 用作包管理器。

下表列出了 openSUSE 15 上当前受支持的 .NET 版本。 这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 openSUSE 版本不再受支持之前仍受支持。

| openSUSE                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|----------------------------|---------------|---------------|----------------|
| ✔️ [15](linux-opensuse.md#opensuse-15-)     | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |

有关详细信息，请参阅[在 openSUSE 上安装 .NET](linux-opensuse.md)。

## <a name="red-hat"></a>Red Hat

Red Hat Enterprise Linux (RHEL) 将 yum (RHEL 7) 和 DNF (RHEL 8) 用作包管理器。

下表列出了 RHEL 7 和 RHEL 8 上当前受支持的 .NET 版本。 这些版本在 [.NET 达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 RHEL 版本不再受到支持之前仍受支持。

- ✔️ 指示 RHEL 或 .NET 版本仍受支持。
- ❌ 指示 RHEL 或 .NET 版本在该 RHEL 版本上不受支持。
- 当 RHEL 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| RHEL                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|--------------------------|---------------|---------------|----------------|
| ✔️ [8](linux-rhel.md#rhel-8-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [7](linux-rhel.md#rhel-7--net-50) | ✔️ 2.1        | ✔️ [3.1](linux-rhel.md#rhel-7--net-core-31)        | ✔️ [5.0](linux-rhel.md#rhel-7--net-50) |

有关详细信息，请参阅[在 RHEL 上安装 .NET](linux-rhel.md)。

## <a name="sles"></a>SLES

SLES 将 zypper 用作包管理器。

下表列出了 SLES 12 SP2 和 SLES 15 上当前受支持的 .NET 版本。 这些版本在 [.NET 达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 SLES 版本不再受到支持之前仍受支持。

- ✔️ 指示 SLES 或 .NET 版本仍受支持。
- ❌ 指示 SLES 或 .NET 版本在该 SLES 版本上不受支持。
- 当 SLES 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| SLES                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|------------------------|---------------|---------------|----------------|
| ✔️ [15](linux-sles.md#sles-15-)     | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [12 SP2](linux-sles.md#sles-12-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |

有关详细信息，请参阅[在 SLES 上安装 .NET](linux-sles.md)。

## <a name="ubuntu"></a>Ubuntu

Ubuntu 将 APT（高级包工具）用作包管理器。

下表表示 Ubuntu 和 .NET 的支持状态。

- ✔️ 指示 Ubuntu 或 .NET 版本仍受支持。
- ❌ 指示 Ubuntu 或 .NET 版本在该 Ubuntu 版本上不受支持。
- 当 Ubuntu 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| Ubuntu                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|--------------------------|---------------|---------------|----------------|
| ✔️ [20.10](linux-ubuntu.md#2010-)       | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ✔️ [20.04 (LTS)](linux-ubuntu.md#2004-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ❌ [19.10](linux-ubuntu.md#1910-)       | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ❌ [19.04](linux-ubuntu.md#1904-)       | ✔️ 2.1        | ✔️ 3.1        | ❌ 5.0 |
| ❌ [18.10](linux-ubuntu.md#1810-)       | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |
| ✔️ [18.04 (LTS)](linux-ubuntu.md#1804-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |
| ❌ [17.10](linux-ubuntu.md#1710-)       | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |
| ❌ [17.04](linux-ubuntu.md#1704-)       | ✔️ 2.1        | ❌ 3.1        | ❌ 5.0 |
| ❌ [16.10](linux-ubuntu.md#1610-)       | ❌ 2.1        | ❌ 3.1        | ❌ 5.0 |
| ✔️ [16.04 (LTS)](linux-ubuntu.md#1604-) | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |

有关详细信息，请参阅[在 Ubuntu 上安装 .NET](linux-ubuntu.md)。

## <a name="next-steps"></a>后续步骤

- [如何检查是否已安装 .NET](how-to-detect-installed-versions.md?pivots=os-linux)。
- [教程：使用 Visual Studio Code 创建一个新应用](../tutorials/with-visual-studio-code.md)。
- [教程：使 .NET 应用容器化](../docker/build-container.md)。
