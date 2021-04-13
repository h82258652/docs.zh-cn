---
title: dotnet restore 命令
description: 了解如何使用 dotnet-restore 命令还原依赖项和特定于项目的工具。
ms.date: 02/27/2020
ms.openlocfilehash: 14bcf65bb78e6d1d96604c8a10a3ba94fab80db8
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874831"
---
# <a name="dotnet-restore"></a>dotnet restore

本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet restore` - 恢复项目的依赖项和工具。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet restore [<ROOT>] [--configfile <FILE>] [--disable-parallel]
    [-f|--force] [--force-evaluate] [--ignore-failed-sources]
    [--interactive] [--lock-file-path <LOCK_FILE_PATH>] [--locked-mode]
    [--no-cache] [--no-dependencies] [--packages <PACKAGES_DIRECTORY>]
    [-r|--runtime <RUNTIME_IDENTIFIER>] [-s|--source <SOURCE>]
    [--use-lock-file] [-v|--verbosity <LEVEL>]

dotnet restore -h|--help
```

## <a name="description"></a>描述

`dotnet restore` 命令使用 NuGet 还原依赖项以及在 project 文件中指定的特定于项目的工具。  在大多数情况下，不需要显式使用 `dotnet restore` 命令，因为在运行以下命令时，将会在必要时隐式运行 NuGet 还原：

- [`dotnet new`](dotnet-new.md)
- [`dotnet build`](dotnet-build.md)
- [`dotnet build-server`](dotnet-build-server.md)
- [`dotnet run`](dotnet-run.md)
- [`dotnet test`](dotnet-test.md)
- [`dotnet publish`](dotnet-publish.md)
- [`dotnet pack`](dotnet-pack.md)

有时，通过这些命令运行隐式 NuGet 还原可能不方便。 例如，某些自动化系统（如生成系统）需要显式调用 `dotnet restore`，以控制还原发生的时间，以便可以控制网络使用量。 为了防止运行隐式 NuGet 还原，可以通过上述任意命令使用 `--no-restore` 标记禁用隐式还原。

### <a name="specify-feeds"></a>指定源

为了还原依赖项，NuGet 需要包所在的源。 通常通过“nuGet.config”配置文件提供源。 安装 .NET SDK 时提供一个默认的配置文件。 若要指定其他源，请执行以下任一项操作：

- 在项目目录中创建自己的 nuget.config 文件。 有关详细信息，请参阅本文后面介绍的[常见 NuGet 配置](/nuget/consume-packages/configuring-nuget-behavior)和 [nuget.config 差异](#nugetconfig-differences)。
- 使用诸如 [`dotnet nuget add source`](dotnet-nuget-add-source.md) 等 `dotnet nuget` 命令。

可以使用 `-s` 选项替代 nuget.config 源。

有关如何使用经过身份验证的源的信息，请参阅[使用经过身份验证的源中的包](/nuget/consume-packages/consuming-packages-authenticated-feeds)。

### <a name="global-packages-folder"></a>全局包文件夹

对于依赖项，可以使用 `--packages` 参数指定还原操作期间放置还原包的位置。 如未指定，将使用默认的 NuGet 包缓存，可在所有操作系统上的用户主目录中的 `.nuget/packages` 目录找到它。 例如 Linux 上的 /home/user1 或 Windows 上的 C:\Users\user1 。

### <a name="project-specific-tooling"></a>特定于项目的工具

对于特定于项目的工具，`dotnet restore` 首先还原打包工具所在的包，然后继续还原 project 文件中指定的工具依赖项。

### <a name="nugetconfig-differences"></a>nuget.config 差异

`dotnet restore` 命令的行为会受 Nuget.Config 文件（如果有）中某些设置的影响。 例如，在 NuGet.Config 中设置 `globalPackagesFolder` 会将还原的 NuGet 包置于指定的文件夹中。 这是在 `dotnet restore` 命令中指定 `--packages` 选项的替代方法。 有关详细信息，请参阅 [nuget.config 参考](/nuget/schema/nuget-config-file)。

有三个 `dotnet restore` 可忽略的特定设置：

- [bindingRedirects](/nuget/schema/nuget-config-file#bindingredirects-section)

  绑定重定向不适用于 `<PackageReference>` 元素，并且 .NET 仅支持 NuGet 包的 `<PackageReference>` 元素。

- [解决方案](/nuget/schema/nuget-config-file#solution-section)

  此设置特定于 Visual Studio，不适用于 .NET。 .NET 不使用 `packages.config` 文件，而是使用 NuGet 包的 `<PackageReference>` 元素。

- [trustedSigners](/nuget/schema/nuget-config-file#trustedsigners-section)

  .NET 5.0.100 SDK 中添加了对跨平台包签名验证的支持。

## <a name="arguments"></a>自变量

- **`ROOT`**

  要还原的项目文件的可选路径。

## <a name="options"></a>选项

- **`--configfile <FILE>`**

  供还原操作使用的 NuGet 配置文件 (nuget.config)。

- **`--disable-parallel`**

  禁用并行还原多个项目。

- **`--force`**

  强制解析所有依赖项，即使上次还原已成功，也不例外。 指定此标记等同于删除 project.assets.json 文件。

- **`--force-evaluate`**

  即使锁定文件已存在，也会强制还原以重新评估所有依赖项。

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`--ignore-failed-sources`**

  如果存在符合版本要求的包，则源失败时警告。

- **`--interactive`**

  允许命令停止并等待用户输入或操作（例如，完成身份验证）。 从 .NET Core 2.1.400 开始。

- **`--lock-file-path <LOCK_FILE_PATH>`**

  写入项目锁定文件的输出位置。 默认情况下，此位置为 PROJECT_ROOT \packages.lock.json。

- **`--locked-mode`**

  不允许更新项目锁定文件。

- **`--no-cache`**

  指定不缓存 HTTP 请求。

- **`--no-dependencies`**

  当使用项目到项目 (P2P) 引用还原项目时，还原根项目，不还原引用。

- **`--packages <PACKAGES_DIRECTORY>`**

  指定还原包的目录。

- **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  指定程序包还原的运行时。 这用于还原 *.csproj* 文件中的 `<RuntimeIdentifiers>` 标记中未显式列出的运行时的程序包。 有关运行时标识符 (RID) 的列表，请参阅 [RID 目录](../rid-catalog.md)。 通过多次指定此选项提供多个 RID。

- **`-s|--source <SOURCE>`**

  指定要在还原操作期间使用的 NuGet 包源的 URI。 此设置会替代 nuget.config 文件中指定的所有源。 多次指定此选项可以提供多个源。

- **`--use-lock-file`**

  允许生成项目锁定文件并与还原一起使用。

- **`-v|--verbosity <LEVEL>`**

  设置命令的详细级别。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值是 `minimal`。

## <a name="examples"></a>示例

- 还原当前目录中项目的依赖项和工具：

  ```dotnetcli
  dotnet restore
  ```

- 还原在给定路径中找到的 `app1` 项目的依赖项和工具：

  ```dotnetcli
  dotnet restore ./projects/app1/app1.csproj
  ```

- 通过将提供的文件路径用作源，在当前目录中还原项目的依赖项和工具：

  ```dotnetcli
  dotnet restore -s c:\packages\mypackages
  ```

- 通过将提供的两个文件路径用作源，在当前目录中还原项目的依赖项和工具：

  ```dotnetcli
  dotnet restore -s c:\packages\mypackages -s c:\packages\myotherpackages
  ```

- 还原当前目录中项目的依赖项和工具，并显示详细的输出：

  ```dotnetcli
  dotnet restore --verbosity detailed
  ```
