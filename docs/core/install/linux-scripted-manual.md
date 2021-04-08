---
title: 在 Linux 上手动安装 .NET - .NET
description: 演示如何在 Linux 上无包管理器的情况下安装 .NET SDK 和 .NET Runtime。 使用安装脚本或手动提取二进制文件。
author: adegeo
ms.author: adegeo
ms.date: 01/06/2021
ms.openlocfilehash: 9176f7477cf80c26ef0b5b7c438c47afcb423799
ms.sourcegitcommit: 44af69720863bd09bd7a4509bf1ec119466ba6e8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106231292"
---
# <a name="install-the-net-sdk-or-the-net-runtime-manually"></a>手动安装 .NET SDK 或 .NET Runtime

.NET 在 Linux 上受支持，本文就将介绍如何使用安装脚本或通过提取二进制文件在 Linux 上安装 .NET。 有关支持内置包管理器的发行版列表，请参阅[在 Linux 上安装 .NET](linux.md)。

还可通过 Snap 安装 .NET。 有关详细信息，请参阅[通过 Snap 安装 .NET SDK 或 .NET Runtime](linux-snap.md)。

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="net-releases"></a>.NET 版本

下表列出了 .NET（和 .NET Core）版本：

| ✔️ 受支持 | ❌ 不受支持 |
|-------------|---------------|
| 5.0         | 3.0           |
| 3.1 (LTS)   | 2.2           |
| 2.1 (LTS)   | 2.0           |
|             | 1.1           |
|             | 1.0           |

有关 .NET 版本的生命周期的详细信息，请参阅 [.NET Core 和 .NET 5 支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。

## <a name="dependencies"></a>依赖项

安装 .NET 时，例如[手动安装](#manual-install)时，可能不会安装特定依赖项。 下面的列表详细列出了 Microsoft 支持的 Linux 发行版以及可能需要安装的依赖项。 更多信息，请查看发行版页面：

- [Alpine](linux-alpine.md#dependencies)
- [Debian](linux-debian.md#dependencies)
- [CentOS](linux-centos.md#dependencies)
- [Fedora](linux-fedora.md#dependencies)
- [RHEL](linux-rhel.md#dependencies)
- [SLES](linux-sles.md#dependencies)
- [Ubuntu](linux-ubuntu.md#dependencies)

有关依赖项的一般信息，请参阅[独立式 Linux 应用](https://github.com/dotnet/core/blob/main/Documentation/self-contained-linux-apps.md)。

### <a name="rpm-dependencies"></a>RPM 依赖项

如果之前未列出发行版，并且该版本基于 RPM，则可能需要以下依赖项：

- krb5-libs
- libicu
- openssl-libs

如果目标运行时环境的 OpenSSL 版本为1.1 或更高版本，则需要安装 compat-openssl10。

### <a name="deb-dependencies"></a>DEB 依赖项

如果之前未列出发行版，并且该版本基于 debian，则可能需要以下依赖项：

- libc6
- libgcc1
- libgssapi-krb5-2
- libicu67
- libssl1.1
- libstdc++6
- zlib1g

### <a name="common-dependencies"></a>通用依赖项

对于使用 System.Drawing.Common 程序集的 .NET 应用，还需要以下依赖项：

- [libgdiplus（版本 6.0.1 或更高版本）](https://www.mono-project.com/docs/gui/libgdiplus/)

  > [!WARNING]
  > 可以通过将 Mono 存储库添加到系统来安装最新版 libgdiplus。 有关详细信息，请参阅 <https://www.mono-project.com/download/stable/>。

## <a name="scripted-install"></a>脚本安装

[dotnet-install 脚本](../tools/dotnet-install-script.md)用于 **SDK** 和 **运行时** 的自动化和非管理员安装。 可通过 <https://dot.net/v1/dotnet-install.sh> 下载脚本。

> ！[重要说明]需要 Bash 才能运行该脚本。

此脚本默认安装最新的 SDK [长期支持 (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) 版本，即 .NET Core 3.1。 若要安装当前版本（可能不是 (LTS) 版本），请使用 `-c Current` 参数。

```bash
./dotnet-install.sh -c Current
```

若要安装 .NET 运行时而非 SDK，请使用 `--runtime` 参数。

```bash
./dotnet-install.sh -c Current --runtime aspnetcore
```

可以通过更改 `-c` 参数以指示特定版本来安装特定版本。 以下命令将安装 .NET SDK 5.0。

```bash
./dotnet-install.sh -c 5.0
```

有关详细信息，请参阅 [dotnet-install 脚本参考](../tools/dotnet-install-script.md)。

## <a name="manual-install"></a>手动安装

<!-- Note, this content is copied in macos.md. Any fixes should be applied there too, though content may be different -->

除了使用包管理器，还可以下载并手动安装 SDK 和运行时。 手动安装通常作为持续集成测试的一部分执行，或在不支持的 Linux 发行版上执行。 对于开发人员或用户，使用包管理器会更好。

如果安装 .NET SDK，则无需安装相应的运行时。 首先，从以下站点之一下载 SDK 或运行时的二进制版本：

- ✔️ [.NET 5.0 下载](https://dotnet.microsoft.com/download/dotnet/5.0)
- ✔️ [.NET Core 3.1 下载](https://dotnet.microsoft.com/download/dotnet/3.1)
- ✔️ [.NET Core 2.1 下载](https://dotnet.microsoft.com/download/dotnet/2.1)
- [所有 .NET Core 下载项](https://dotnet.microsoft.com/download/dotnet)

接下来，提取已下载的文件并使用 `export` 命令设置 .NET 使用的变量，然后确保 .NET 在 PATH 中。

若要提取运行时并使 .NET CLI 命令可用于终端，请先下载 .NET 二进制版本。 然后，打开终端并从保存文件的目录运行以下命令。 根据下载内容，存档文件名称可能不同。

使用以下命令提取已下载的运行时或 SDK。 请记得将 `DOTNET_FILE` 值更改为你的文件名：

```bash
DOTNET_FILE=dotnet-sdk-5.0.102-linux-x64.tar.gz
export DOTNET_ROOT=$HOME/dotnet

mkdir -p "$DOTNET_ROOT" && tar zxf "$DOTNET_FILE" -C "$DOTNET_ROOT"

export PATH=$PATH:$DOTNET_ROOT
```

> [!TIP]
> 前面的 `export` 命令只会使 .NET CLI 命令对运行它的终端会话可用。
>
> 你可以编辑 shell 配置文件，永久地添加这些命令。 Linux 提供了许多不同的 shell，每个都有不同的配置文件。 例如：
>
> - **Bash Shell**：~/.bash_profile、~/.bashrc
> - **Korn Shell**：~/.kshrc 或 .profile
> - **Z Shell**：~/.zshrc 或 .zprofile
>
> 为 shell 编辑相应的源文件，并将 `:$HOME/dotnet` 添加到现有 `PATH` 语句的末尾。 如果不包含 `PATH` 语句，则使用 `export PATH=$PATH:$HOME/dotnet` 添加新行。
>
> 另外，将 `export DOTNET_ROOT=$HOME/dotnet` 添加至文件的末尾。

使用此方法可以将不同的版本安装到不同的位置，并明确选择应用程序要使用的对应版本。

## <a name="next-steps"></a>后续步骤

- [如何为 .NET CLI 启用 Tab 自动补全](../tools/enable-tab-autocomplete.md)
- [教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序](../tutorials/with-visual-studio-code.md)
