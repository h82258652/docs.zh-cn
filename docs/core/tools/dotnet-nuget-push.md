---
title: dotnet nuget push 命令
description: dotnet nuget push 命令可将包推送到服务器并发布。
author: karann-msft
ms.date: 02/14/2020
ms.openlocfilehash: 71bc14fa729945b3d1e1508d014287cc5355f8cc
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637554"
---
# <a name="dotnet-nuget-push"></a>dotnet nuget push

**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet nuget push` - 将包推送到服务器，并将其发布。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet nuget push [<ROOT>] [-d|--disable-buffering] [--force-english-output]
    [--interactive] [-k|--api-key <API_KEY>] [-n|--no-symbols]
    [--no-service-endpoint] [-s|--source <SOURCE>] [--skip-duplicate]
    [-sk|--symbol-api-key <API_KEY>] [-ss|--symbol-source <SOURCE>]
    [-t|--timeout <TIMEOUT>]

dotnet nuget push -h|--help
```

## <a name="description"></a>描述

`dotnet nuget push` 将包推送到服务器，并将其发布。 push 命令使用在系统的 NuGet 配置文件或配置文件链中找到的服务器和凭据详细信息。 有关配置文件的详细信息，请参阅 [Configuring NuGet Behavior](/nuget/consume-packages/configuring-nuget-behavior)（配置 NuGet 行为）。 通过加载 *%AppData%\NuGet\NuGet.config* (Windows) 或 *$HOME/.local/share* (Linux/macOS) 获得 NuGet 的默认配置，然后加载任意 *nuget.config* 或 *.nuget\nuget.config*，从驱动器的根目录开始，并在当前目录中结束。

命令推送现有包。 它不会创建包。 若要创建包，请使用 [`dotnet pack`](dotnet-pack.md)。

## <a name="arguments"></a>自变量

- **`ROOT`**

  指定要推送的包的文件路径。

## <a name="options"></a>选项

- **`-d|--disable-buffering`**

  当推送到 HTTP(S) 服务器以减少内存使用率时，禁用缓冲。

- **`--force-english-output`**

  使用固定的、基于英语的区域性强制运行应用程序。

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`--interactive`**

  对于身份验证等操作，允许命令阻止并要求手动操作。 自 .NET Core 2.2 SDK 起可用的选项。

- **`-k|--api-key <API_KEY>`**

  服务器的 API 密钥。

- **`-n|--no-symbols`**

  不推送符号（即使存在）。

- **`--no-service-endpoint`**

  不将“api/v2/package”追加至源 URL。 自 .NET Core 2.1 SDK 起可用的选项。

- **`-s|--source <SOURCE>`**

  指定服务器 URL。 NuGet 标识 UNC 或本地文件夹源，只在其中复制文件，而不会使用 HTTP 进行推送。
  > [!IMPORTANT]
  > 从 NuGet 3.4.2 开始，此参数为必需，除非 NuGet 配置文件指定了 `DefaultPushSource` 值。 有关详细信息，请参阅[配置 NuGet 行为](/nuget/consume-packages/configuring-nuget-behavior)。

- **`--skip-duplicate`**

  将多个包推送到 HTTP(S) 服务器时，将任何 409 冲突响应视为警告，以便可以继续推送。 自 .NET Core 3.1 SDK 起可用。

- **`-sk|--symbol-api-key <API_KEY>`**

  符号服务器的 API 密钥。

- **`-ss|--symbol-source <SOURCE>`**

  指定符号服务器 URL。

- **`-t|--timeout <TIMEOUT>`**

  指定推送到服务器的超时（秒）。 默认值为 300 秒（5 分钟）。 指定 0 时应用默认值。

## <a name="examples"></a>示例

- 将 foo.nupkg 推送到 NuGet 配置文件中指定的默认推送源（使用 API 密钥）：

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a
  ```

- 将 foo.nupkg  推送到官方 NuGet 服务器，以指定 API 密钥：

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://api.nuget.org/v3/index.json
  ```
  
  * 将 foo.nupkg 推送到自定义推送源 `https://customsource`（指定 API 密钥）  ：

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://customsource/
  ```

- 将 foo.nupkg 推送到 NuGet 配置文件中指定的默认推送源：

  ```dotnetcli
  dotnet nuget push foo.nupkg
  ```

- 将 foo.symbols.nupkg 推送到默认符号源  ：

  ```dotnetcli
  dotnet nuget push foo.symbols.nupkg
  ```

- 将 foo.nupkg 推送到 NuGet 配置文件中指定的默认推送源（超时 360 秒）：

  ```dotnetcli
  dotnet nuget push foo.nupkg --timeout 360
  ```

- 将当前目录中的所有 .nupkg 文件推送到 NuGet 配置文件中指定的默认推送源：

  ```dotnetcli
  dotnet nuget push "*.nupkg"
  ```

  > [!NOTE]
  > 如果此命令不起作用，则可能是较旧版本的 SDK（.NET Core 2.1 SDK 及更早版本）中的 bug 导致的。
  > 要解决此问题，请升级 SDK 版本或改为运行以下命令：`dotnet nuget push "**/*.nupkg"`
  
  > [!NOTE]
  > 用于执行文件组合的 bash 等 shell 需要用引号括起来。 有关详细信息，请参阅 [NuGet/Home#4393](https://github.com/NuGet/Home/issues/4393#issuecomment-667618120)。

- 将所有 .nupkg 文件推送到 NuGet 配置文件中指定的默认推送源，即使 HTTP(S) 服务器返回“409 Conflict”响应也是如此：

  ```dotnetcli
  dotnet nuget push "*.nupkg" --skip-duplicate
  ```

- 将当前目录中的所有 .nupkg 文件推送到本地源目录  ：

  ```dotnetcli
  dotnet nuget push "*.nupkg" -s c:\mydir
  ```

  此命令不会将包存储在分层文件夹结构中，因此建议优化性能。 有关详细信息，请参阅[本地源](/nuget/hosting-packages/local-feeds)。  
