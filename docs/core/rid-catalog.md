---
title: .NET 运行时标识符 (RID) 目录
description: 了解运行时标识符 (RID) 及如何在 .NET 中使用 RID。
ms.date: 01/28/2021
ms.openlocfilehash: 87fbad990fce5431769fa1631be7e079c2eb27a5
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106496665"
---
# <a name="net-rid-catalog"></a>.NET RID 目录

RID 是运行时标识符的缩写。 RID 值用于标识应用程序运行所在的目标平台。
.NET 包使用它们来表示 NuGet 包中特定于平台的资产。 以下值是 RID 的示例：`linux-x64`、`ubuntu.14.04-x64`、`win7-x64` 或 `osx.10.12-x64`。
对于具有本机依赖项的包，RID 将指定在其中可以还原包的平台。

可以在项目文件的 `<RuntimeIdentifier>` 元素中设置一个 RID。 可以将多个 RID 定义为项目文件的 `<RuntimeIdentifiers>` 元素中的列表（以分号分隔）。 也可使用以下 [.NET CLI 命令](./tools/index.md) 通过 `--runtime` 选项使用它们：

- [dotnet build](./tools/dotnet-build.md)
- [dotnet clean](./tools/dotnet-clean.md)
- [dotnet pack](./tools/dotnet-pack.md)
- [dotnet publish](./tools/dotnet-publish.md)
- [dotnet restore](./tools/dotnet-restore.md)
- [dotnet run](./tools/dotnet-run.md)
- [dotnet store](./tools/dotnet-store.md)

表示具体操作系统的 RID 通常遵循以下模式：`[os].[version]-[architecture]-[additional qualifiers]`，其中：

- `[os]` 是操作系统/平台系统名字对象。 例如 `ubuntu`。

- `[version]` 是操作系统版本，使用的格式是以点 (`.`) 分隔的版本号。 例如 `15.10`。

  - 版本不应为营销版本，因为它们通常代表该操作系统的多个离散版本，且具有不同的平台 API 外围应用。

- `[architecture]` 是处理器体系结构。 例如：`x86`、`x64`、`arm` 或 `arm64`。

- `[additional qualifiers]` 进一步区分了不同的平台。 例如：`aot`。

## <a name="rid-graph"></a>RID 图表

RID 图表或运行时回退图表是互相兼容的 RID 列表。 [Microsoft.NETCore.Platforms](https://www.nuget.org/packages/Microsoft.NETCore.Platforms/) 包中定义了 RID。 可以在 `dotnet/runtime` 存储库的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) 文件中查看支持的 RID 列表和 RID 图表。 在此文件中，可以看到除基 RID 以外的所有 RID 均包含 `"#import"` 语句。 这些语句指示的是兼容的 RID。

NuGet 还原包时，它将尝试找到指定运行时的完全匹配项。
如果未找到完全匹配项，NuGet 将返回此图表，直至它根据 RID 图表找到最相近的兼容系统。

以下示例是 `osx.10.12-x64` RID 的实际条目：

```json
"osx.10.12-x64": {
    "#import": [ "osx.10.12", "osx.10.11-x64" ]
}
```

上述 RID 指定 `osx.10.12-x64` 导入 `osx.10.11-x64`。 因此，当 NuGet 还原包时，它将尝试找到包中的 `osx.10.12-x64` 的完全匹配项。 例如，如果 NuGet 无法找到特定的运行时，可以还原指定 `osx.10.11-x64` 运行时的包。

以下示例演示了 runtime.json 文件中定义的另一个略大的 RID 图表：

```
    win7-x64    win7-x86
       |   \   /    |
       |   win7     |
       |     |      |
    win-x64  |  win-x86
          \  |  /
            win
             |
            any
```

所有 RID 最终都会映射回根 `any` RID。

使用 RID 时，必须牢记以下几个注意事项：

- 请勿尝试分析 RID 来检索组件部分。
- 请勿以编程方式生成 RID。
- 使用已为平台定义的 RID。
- RID 必须具有特定性，因此请勿通过实际的 RID 值假定任何情况。

## <a name="using-rids"></a>使用 RID

若要使用 RID，必须知道有哪些 RID。 新值将定期添加到该平台。
若要获取最新的完整版，请参阅 `dotnet/runtime` 存储库中的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) 文件。

可移植 RID 是添加到 RID 图表的值，未绑定到特定的版本或 OS 发行。 它们是首选选项，尤其是在处理多个 Linux 发行版时，因为大多数发行版 RID 都映射到可移植的 RID。

以下列表显示了一小部分用于每个 OS 的最常见 RID。

## <a name="windows-rids"></a>Windows RID

仅列出了公共值。 若要获取最新的完整版，请参阅 `dotnet/runtime` 存储库中的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) 文件。

- 可移植
  - `win-x64`
  - `win-x86`
  - `win-arm`
  - `win-arm64`
- Windows 7 / Windows Server 2008 R2
  - `win7-x64`
  - `win7-x86`
- Windows 8.1 / Windows Server 2012 R2
  - `win81-x64`
  - `win81-x86`
  - `win81-arm`
- Windows 10 / Windows Server 2016
  - `win10-x64`
  - `win10-x86`
  - `win10-arm`
  - `win10-arm64`

有关详细信息，请参阅 [.NET 依赖项和要求](./install/windows.md#dependencies)。

## <a name="linux-rids"></a>Linux RID

仅列出了公共值。 若要获取最新的完整版，请参阅 `dotnet/runtime` 存储库中的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) 文件。 运行以下未列出的发行版的设备可能适用于其中一个可移植 RID。 例如，可以将运行未列出的 Linux 发行版的 Raspberry Pi 设备定向为使用 `linux-arm`。

- 可移植
  - `linux-x64`（大多数桌面发行版，如 CentOS、Debian、Fedora、Ubuntu 及派生版本）
  - `linux-musl-x64`（使用 [musl](https://wiki.musl-libc.org/projects-using-musl.html) 的轻量级发行版，如 Alpine Linux）
  - `linux-arm`（在 ARM 上运行的 Linux 发行版本，如 Raspberry Pi Model 2 及更高版本上的 Raspbian）
  - `linux-arm64`（在 64 位 ARM 上运行的 Linux 发行版本，如 Raspberry Pi Model 3 及更高版本上的 Ubuntu 服务器 64 位）
- Red Hat Enterprise Linux
  - `rhel-x64`（被 `linux-x64` 取代，适用于 RHEL 6 以上版本）
  - `rhel.6-x64`
- Tizen
  - `tizen`
  - `tizen.4.0.0`
  - `tizen.5.0.0`

有关详细信息，请参阅 [.NET 依赖项和要求](./install/linux.md)。

## <a name="macos-rids"></a>macOS RID

macOS RID 使用较早的“OSX”品牌。 仅列出了公共值。 若要获取最新的完整版，请参阅 `dotnet/runtime` 存储库中的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/src/runtime.json) 文件。

- 可移植
  - `osx-x64`（最低 OS 版本为 macOS 10.12 Sierra）
- macOS 10.10  Yosemite
  - `osx.10.10-x64`
- macOS 10.11 El Capitan
  - `osx.10.11-x64`
- macOS 10.12 Sierra
  - `osx.10.12-x64`
- macOS 10.13 High Sierra
  - `osx.10.13-x64`
- macOS 10.14 Mojave
  - `osx.10.14-x64`
- macOS 10.15 Catalina
  - `osx.10.15-x64`
- macOS 11.01 Big Sur
  - `osx.11.0-x64`
  - `osx.11.0-arm64`

有关详细信息，请参阅 [.NET 依赖项和要求](./install/macos.md#dependencies)。

## <a name="see-also"></a>请参阅

- [运行时 ID](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/readme.md)
