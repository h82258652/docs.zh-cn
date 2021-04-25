---
title: .NET SDK 遥测
description: 了解可收集使用情况信息以供分析的 .NET SDK 遥测功能、收集的数据，以及如何禁用遥测。
author: KathleenDollard
ms.date: 08/27/2019
ms.openlocfilehash: cb557973197277b86c5a85770539b3005e7d9e4a
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494215"
---
# <a name="net-sdk-telemetry"></a>.NET SDK 遥测

[.NET SDK](index.md) 包含遥测功能，可在 .NET CLI 崩溃时收集使用情况数据和异常信息。 .NET CLI 附带 .NET SDK，是一组用于生成、测试和发布 .NET 应用的谓词。 请务必让 .NET 团队了解到工具使用情况，以便我们对其做出改进。 有关故障的信息可帮助团队解决问题并修复 bug。

收集的数据根据 [Creative Commons Attribution 许可证](https://creativecommons.org/licenses/by/4.0/)以汇总形式发布。

## <a name="scope"></a>范围

`dotnet` 具有两个功能：运行应用程序和执行 CLI 命令。 按以下格式使用 `dotnet` 来启动应用程序时，不会收集遥测数据：

- `dotnet [path-to-app].dll`

使用任何 [.NET CLI 命令](index.md)时，都会收集遥测数据，如：

- `dotnet build`
- `dotnet pack`
- `dotnet run`

## <a name="how-to-opt-out"></a>如何选择退出

.NET SDK 遥测功能默认处于启用状态。 要选择退出遥测功能，请将 `DOTNET_CLI_TELEMETRY_OPTOUT` 环境变量设置为 `1` 或 `true`。

如果安装成功，.NET SDK 安装程序也会发送一个遥测条目。 若要选择退出，请在安装 .NET SDK 之前设置 `DOTNET_CLI_TELEMETRY_OPTOUT` 环境变量。

> [!IMPORTANT]
> 要在启动安装程序后选择退出，请执行以下操作：关闭安装程序，设置环境变量，然后使用该值集再次运行安装程序。

## <a name="disclosure"></a>公开

首次运行其中一个 [.NET CLI 命令](index.md)（如 `dotnet build`）时，.NET SDK 显示以下类似文本。 文本可能会因运行的 SDK 版本而略有不同。 此“首次运行”体验是 Microsoft 通知用户有关数据收集信息的方式。

```console
Telemetry
---------
The .NET tools collect usage data in order to help us improve your experience. The data is collected by Microsoft and shared with the community. You can opt-out of telemetry by setting the DOTNET_CLI_TELEMETRY_OPTOUT environment variable to '1' or 'true' using your favorite shell.

Read more about .NET CLI Tools telemetry: https://aka.ms/dotnet-cli-telemetry
```

若要禁用此消息和 .NET 欢迎消息，请将 `DOTNET_NOLOGO` 环境变量设置为 `true`。 请注意，此变量在遥测选择退出时不起作用。

## <a name="data-points"></a>数据点

遥测功能不收集用户名或电子邮件地址等个人数据。 也不会扫描代码，更不会提取项目级敏感数据，如名称、存储库或作者。 数据通过 [Azure Monitor](https://azure.microsoft.com/services/monitor/) 技术安全地发送到 Microsoft 服务器，提供对保留数据的受限访问权限，并在严格的安全控制下从安全的 [Azure 存储](https://azure.microsoft.com/services/storage/)系统发布。

保护你的隐私对我们很重要。 如果你怀疑遥测在收集敏感数据，或认为处理数据的方式不安全或不恰当，请在 [dotnet/sdk](https://github.com/dotnet/sdk/issues) 存储库中记录问题或发送电子邮件至 [dotnet@microsoft.com](mailto:dotnet@microsoft.com) 以供我们展开调查。

遥测功能收集以下数据：

| SDK 版本 | 数据 |
|--------------|------|
| 全部          | 调用时间戳。 |
| 全部          | 调用的命令（例如，“build”），从 2.1 开始进行哈希处理。 |
| 全部          | 用于确定地理位置的三个八进制数 IP 地址。 |
| 全部          | 操作系统和版本。 |
| 全部          | 运行 SDK 的运行时 ID (RID)。 |
| 全部          | .NET SDK 版本。 |
| 全部          | 遥测配置文件：一个可选值，仅在用户显式选择加入时可用，并在 Microsoft 内部使用。 |
| >=2.0        | 命令参数和选项：收集若干参数和选项（非任意字符串）。 请参阅[收集的选项](#collected-options)。 从 2.1.300 后进行哈希处理。 |
| >=2.0         | SDK 是否在容器中运行。 |
| >=2.0         | 目标框架（来自 `TargetFramework` 事件），从 2.1 开始进行哈希处理。 |
| >=2.0         | 经过哈希处理的媒体访问控制 (MAC) 地址 (SHA256)。 |
| >=2.0         | 经过哈希处理的当前工作目录。 |
| >=2.0         | 安装成功报告，包含进行了哈希处理的安装程序 exe 文件名。 |
| >=2.1.300     | 内核版本。 |
| >=2.1.300     | Libc 发行/版本。 |
| >=3.0.100     | 是否已重定向输出（true 或 false）。 |
| >=3.0.100     | CLI/SDK 故障时的异常类型及其堆栈跟踪（发送的堆栈跟踪中仅包含 CLI/SDK 代码）。 有关详细信息，请参阅[收集的 .NET CLI/SDK 故障异常遥测](#net-clisdk-crash-exception-telemetry-collected)。 |
| >=5.0.202     | 从进程开始到进入 CLI 程序的 main 方法为止的运行时间，可衡量主机和运行时的启动情况。 |
| >=5.0.202     | 在首次运行时将 .NET 工具添加到路径这一步的运行时间。 |
| >=5.0.202     | 首次运行时要显示首次使用时间通知的运行时间。 |
| >=5.0.202     | 首次运行时生成 ASP.NET 证书的运行时间。 |
| >=5.0.202     | 分析 CLI 输入的运行时间。 |

### <a name="collected-options"></a>收集的选项

某些命令发送其他数据。 小部分命令发送第一个参数：

| 命令               | 发送的第一个参数数据                |
|-----------------------|-----------------------------------------|
| `dotnet help <arg>`   | 正在查询命令帮助。  |
| `dotnet new <arg>`    | 模板名称（进行哈希处理）。             |
| `dotnet add <arg>`    | 单词 `package` 或 `reference`。      |
| `dotnet remove <arg>` | 单词 `package` 或 `reference`。      |
| `dotnet list <arg>`   | 单词 `package` 或 `reference`。      |
| `dotnet sln <arg>`    | 单词 `add`、`list` 或 `remove`。    |
| `dotnet nuget <arg>`  | 单词 `delete`、`locals` 或 `push`。 |

一小部分命令发送所选项目（如果使用）及其值：

| 选项                  | 命令                                                                                       |
|-------------------------|------------------------------------------------------------------------------------------------|
| `--verbosity`           | 所有命令                                                                                   |
| `--language`            | `dotnet new`                                                                                   |
| `--configuration`       | `dotnet build`, `dotnet clean`, `dotnet publish`, `dotnet run`, `dotnet test`                  |
| `--framework`           | `dotnet build`, `dotnet clean`, `dotnet publish`, `dotnet run`, `dotnet test`, `dotnet vstest` |
| `--runtime`             | `dotnet build`,  `dotnet publish`                                                              |
| `--platform`            | `dotnet vstest`                                                                                |
| `--logger`              | `dotnet vstest`                                                                                |
| `--sdk-package-version` | `dotnet migrate`                                                                               |

除 `--verbosity` 和 `--sdk-package-version` 外，从 .NET Core 2.1.100 SDK 开始，所有其他值都会进行哈希处理。

## <a name="net-clisdk-crash-exception-telemetry-collected"></a>收集的 .NET CLI/SDK 故障异常遥测

如果 .NET CLI/SDK 崩溃，则会收集 CLI/SDK 代码的异常和堆栈跟踪名称。 收集此信息是为了评估问题并改善 .NET SDK 和 CLI 的质量。 本文提供了所收集数据的信息。 本文还提供了有关生成自己的 .NET SDK 版本的用户如何避免无意泄露个人或敏感信息的提示。

### <a name="types-of-collected-data"></a>收集的数据类型

.NET CLI 只收集有关 CLI/SDK 异常的信息，不收集应用程序中的异常信息。 收集的数据包含异常和堆栈跟踪的名称。 此堆栈跟踪为 CLI/SDK 代码。

下面的示例显示所收集的数据类型：

```console
System.IO.IOException
at System.ConsolePal.WindowsConsoleStream.Write(Byte[] buffer, Int32 offset, Int32 count)
at System.IO.StreamWriter.Flush(Boolean flushStream, Boolean flushEncoder)
at System.IO.StreamWriter.Write(Char[] buffer)
at System.IO.TextWriter.WriteLine()
at System.IO.TextWriter.SyncTextWriter.WriteLine()
at Microsoft.DotNet.Cli.Utils.Reporter.WriteLine()
at Microsoft.DotNet.Tools.Run.RunCommand.EnsureProjectIsBuilt()
at Microsoft.DotNet.Tools.Run.RunCommand.Execute()
at Microsoft.DotNet.Tools.Run.RunCommand.Run(String[] args)
at Microsoft.DotNet.Cli.Program.ProcessArgs(String[] args, ITelemetry telemetryClient)
at Microsoft.DotNet.Cli.Program.Main(String[] args)
```

### <a name="avoid-inadvertent-disclosure-of-information"></a>避免意外泄露信息

.NET 参与者以及运行自己生成的 .NET SDK 版本的任何其他人都应考虑其 SDK 源代码的路径。 如果在使用属于自定义调试生成或者使用自定义生成符号文件配置的 .NET SDK 时出现故障，则生成计算机的 SDK 源文件路径将作为堆栈跟踪的一部分收集，并且不会进行哈希处理。

因此，.NET SDK 的自定义生成不应位于路径名公开个人或敏感信息的目录中。

## <a name="see-also"></a>请参阅

- [.NET CLI 遥测数据](https://dotnet.microsoft.com/platform/telemetry)
- [遥测参考源（dotnet/sdk 存储库）](https://github.com/dotnet/sdk/tree/main/src/Cli/dotnet/Telemetry)
