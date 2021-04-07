---
title: 了解和更新依赖项
description: 若要将 .NET Framework 项目迁移到 .NET Core，则必须将其依赖项更新为使用 .NET Core。 本部分介绍可用于规划大型应用的迁移的工具和方法。
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 484691496d3691151fd3ca83ec776dbb31327c09
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122750"
---
# <a name="understand-and-update-dependencies"></a>了解和更新依赖项

确定应用程序的各个项目必须迁移到的顺序后，下一步是了解每个项目的依赖项，并在必要时对其进行更新。 对于平台依赖项，最佳启动方法是在相关程序集上运行 [.net 可移植性分析器](../../standard/analyzers/portability-analyzer.md) ，然后查看生成的详细结果。 配置工具以指定一个或多个目标平台，如 .NET Core 3.1 或 .NET Standard 2.0。 其中提供了针对每个目标平台的详细信息。 图3-4 显示了该工具的输出示例。

![.NET 可移植性分析器报表详细信息](./media/Figure3-4.png)

**图3-4。** .NET 可移植性分析器报表详细信息。

## <a name="update-class-library-dependencies"></a>更新类库依赖项

大型应用程序通常涉及多个项目，而 ASP.NET MVC web 项目之外的大多数项目都可能是类库。 类库在 .NET 平台之间通常是最容易的端口，尤其是与 ASP.NET 项目相比，它们在最难 (，通常需要重新创建) 。

团队可以考虑用于将类库迁移到 .NET Core 的 " [try-转换工具](https://github.com/dotnet/try-convert) " 或 " [.net 升级助手" 工具](https://aka.ms/dotnet-upgrade-assistant) 。 这些工具分析 .NET Framework 的项目文件，并尝试将其迁移到 .NET Core 项目文件格式，进行任何修改，使其能够在此过程中安全执行。 这些工具可能需要一些手动帮助才能使用 ASP.NET 项目，但通常可以帮助加速类库的迁移过程。

"Try-转换" 和 "升级助手" 工具部署为 .NET Core 命令行工具。 它们仅在 Windows 上运行，因为它们设计为适用于 .NET Framework 应用。 你可以通过从命令提示符运行以下命令来安装 try-convert：

```dotnetcli
dotnet tool install -g try-convert
```

成功安装该工具后，可以 `try-convert` 在类库的项目文件所在的文件夹中运行。

安装 try-convert) 后，在安装了以下命令 (安装 .NET 升级助手：

```dotnetcli
dotnet tool install -g upgrade-assistant
```

`upgrade-assistant <project>`在项目文件所在的文件夹中，通过命令运行该工具。

## <a name="update-nuget-package-dependencies"></a>更新 NuGet 包依赖关系

分析第三方 NuGet 包的使用情况，并确定其中是否有任何 .NET Standard (，或者是否支持它，但仅支持新版本) 。 [ `<PackageReference>` 使用 Visual Studio 的转换器工具更新 NuGet 包以使用语法](/nuget/consume-packages/migrate-packages-config-to-package-reference)可能会很有帮助，以便顶级依赖项可见。 接下来，检查这些包的当前版本或更高版本是否支持 .NET Core 或 .NET Standard。 此信息可在 [nuget.org] 上找到，也可在 Visual Studio 中针对每个包。

如果支持使用应用当前使用的包版本，很好！ 如果不是，请查看包的更新版本是否具有支持和研究升级所涉及的内容。 包中可能存在重大更改，尤其是在当前使用的版本和要升级到的版本之间，包的主版本发生更改。

在某些情况下，给定包的任何版本均不适用于 .NET Core。 在这种情况下，团队有几个选项。 它们可以继续，具体取决于 .NET Framework 版本，但这样做有限制。 应用只能在 Windows 上运行，并且团队可能需要在包的二进制文件上运行可移植性分析器，以查看是否有可能遇到任何问题。 当然，团队需要进行全面测试，因为如果使用的 .NET Framework 包引用的 Api 在 .NET Core 中不可用，则会发生运行时异常。 另一种方法是查找其他包，或者，如果所需的包是开放源，请将其升级到 .NET Standard 或 .NET Core 本身。

## <a name="migrate-aspnet-mvc-projects"></a>迁移 ASP.NET MVC 项目

`System.Web`.Net Core 中不存在命名空间和类型。 分析依赖项并使用类似的工具时 `try-convert` ，你会发现，它们不会为自动迁移 ASP.NET MVC 项目及其引用的任何代码提供很多建议 `System.Web` 。 对于这些项目，你将需要从新的 ASP.NET Core web 项目开始，并手动将文件迁移到此项目。

通常情况下，最好最大程度地减少应用的业务逻辑在其用户界面层中的程度。 还最好将控制器和视图保持为小。 遵循本指南的应用比 ASP.NET web 项目中具有大量逻辑的应用程序更易于移植。 如果你的应用正在考虑迁移，但尚未开始处理，请在维护它时记住这一点。 为了尽量减少 ASP.NET MVC 或 Web API 项目中的代码量，在将应用程序移植到应用程序时，可能会导致更少的工作量。

下一章进一步了如何从 ASP.NET MVC 和 Web API 项目迁移到 ASP.NET Core 项目的详细信息。 上一章介绍了应用之间的最大差异。 完成基本项目结构后，迁移单个控制器和视图通常是非常简单的，尤其是在它们主要侧重于 web 职责时。

## <a name="references"></a>参考

- [.NET 升级助手工具](https://aka.ms/dotnet-upgrade-assistant)
- [尝试转换工具](https://github.com/dotnet/try-convert)
- [apiport 工具](https://github.com/microsoft/dotnet-apiport)

>[!div class="step-by-step"]
>[上一页](identify-migration-sequence.md)
>[下一页](strategies-migrating-in-production.md)
