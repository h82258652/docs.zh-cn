---
title: 确定要迁移的项目序列
description: 大型应用程序通常不会同时迁移到新的平台，而是一系列较小的步骤。 了解如何规划将 ASP.NET MVC 应用程序迁移到 ASP.NET Core 的步骤。
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 34bcfbf4ea3941329419ba64253a2177247df7b8
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122906"
---
# <a name="identify-sequence-of-projects-to-migrate"></a>确定要迁移的项目序列

对于涉及多个前端应用的解决方案，最好逐个迁移应用。 例如，创建一个仅包含一个前端应用及其依赖项的解决方案，以便你可以轻松地确定所涉及的工作范围。 解决方案是轻型的，如果需要，可以在多个解决方案中包含项目。 迁移时，可使用解决方案作为组织工具。

确定要迁移的 ASP.NET 应用并使其相关项目 (理想情况下，在解决方案) 中，下一步是确定框架和 NuGet 依赖项。 识别所有依赖项后，最简单的迁移方法是 "自下而上的" 方法。 利用这种方法，首先迁移最低级别的依赖项。 然后，将迁移下一级别的依赖项，直到最终只有一个前端应用。 图3-1 显示了一组构成应用程序的项目示例。 低级别类库位于底部，而 ASP.NET MVC 项目位于顶部。

![项目依赖项](./media/Figure3-1.png)

**图 3-1。** 项目依赖项关系图。

选择一个特定的前端应用，即一个 ASP.NET MVC 5/Web API 2 项目。 确定其在解决方案中的依赖关系，并映射其依赖项，直至获得完整列表。 映射项目依赖项时，图3-1 中所示的关系图可能很有用。 Visual Studio 可以根据你使用的版本， [为你的解决方案生成依赖关系图](/visualstudio/modeling/create-layer-diagrams-from-your-code)。 [.Net 可移植性分析器](../../standard/analyzers/portability-analyzer.md) 还可以生成依赖关系图。

图3-2 显示了 ".Net 可 [移植性分析器" Visual Studio 扩展](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)的安装程序：

![安装 .NET 可移植性分析器扩展](./media/Figure3-2.png)

**图3-2。** .NET 可移植性分析器安装程序。

此扩展插件支持 Visual Studio 2017 及更高版本。 安装完成后，可在 "**分析** 可  >  **移植性分析器设置**" 菜单中进行配置，如图3-3 所示。

![配置 .NET 可移植性分析器扩展](./media/Figure3-3.png)

**图3-3。** 配置 .NET 可移植性分析器。

分析器生成每个程序集的详细报表。 报表：

* 描述每个项目与给定目标框架（如 .NET Core 3.1 或 .NET Standard 2.0）的兼容性。
* 帮助团队评估将特定项目移植到特定目标框架所需的工作量。

下一节将介绍此分析的详细信息。

映射出项目及其相互关系后，就可以确定迁移项目的顺序了，这是一项工作。 从没有依赖项的项目开始。 然后，将树的工作方式设置为依赖于这些项目的项目。

在图3-1 所示的示例中，你将从 *Utils* 项目开始，因为它不依赖于任何其他项目。 接下来， *Contoso. 数据* 仅依赖于 "Utils"。 然后迁移 "Businesslogic.dll" 库，最后迁移前端 ASP.NET "Web" 项目。 下面的 "自下而上" 方法非常适用于相对较小且分解良好的应用程序，这些应用程序的所有项目都已迁移后可作为一个单元进行迁移。 具有更多复杂性或更长时间迁移的更多代码的较大应用可能需要考虑更多增量策略。

## <a name="unit-tests"></a>单元测试

前面的关系图中缺少的是单元测试项目。 但愿有些测试至少涵盖了正在移植的库的某些现有行为。

如果你有单元测试，最好先转换这些项目。 你需要继续测试正在处理的项目中的更改。 请记住，移植到 .NET Core 对代码库进行了重大更改。

MSTest、xUnit 和 NUnit 都适用于 .NET Core。 如果当前没有适用于你的应用程序的测试，请考虑构建一些用于验证系统当前行为的特性测试。 优点是迁移完成后，可以确认应用的行为保持不变。

## <a name="considerations-for-migrating-many-apps"></a>迁移多个应用时的注意事项

某些组织将有许多不同的应用程序进行迁移，手动迁移每个应用可能需要 tenable 过多的资源。 在这些情况下，建议一定程度的自动化。 本章后面的步骤可自动执行。 结构更改（如项目文件差异和常见包的更新）可通过脚本来应用。 可以优化这些脚本，因为它们在多个项目上以迭代方式运行。 每次运行时，都会检查每个项目是否需要任何手动步骤。 如果可能，自动执行这些手动步骤。 使用此方法时，组织应在一段时间内迁移其应用时更快、更好地增长，并改进了自动化支持的每个步骤。

观看有关如何在此 [dotNetConf 演示文稿中通过 Lizzy Gallagher Of Mastercard](https://www.youtube.com/watch?v=C-2haqb60No)使用此方法的概述。 此演示中采用的五个阶段包括：

- 迁移第三方 NuGet 依赖项
- 迁移应用以使用新 *的 .csproj* 文件格式
- 将内部 NuGet 依赖项更新到 .NET Standard
- 将应用迁移到 ASP.NET Core (目标 .NET Framework) 
- 将所有应用更新为面向 .NET Core 3。1

当应用程序自动执行时，如果应用程序遵循一致的编码准则和项目组织，则会很有帮助。 自动化工作依赖于此一致性才能有效。 除了分析和迁移项目文件外，还可以自动迁移常见的代码模式。 一些代码模式示例包括如何声明控制器操作的方式，或它们如何返回结果。

例如，迁移脚本可以在与以下模式之一 *匹配的* 代码行中搜索匹配的文件：

```csharp
   return new HttpStatusCodeResult(200);
   // or
   return new HttpStatusCodeResult(HttpStatusCode.OK);
```

在 ASP.NET Core 中，前面的代码行都可以替换为：

```csharp
    return Ok();
```

## <a name="summary"></a>总结

将大型 .NET Framework 应用移植到 .NET Core 的最佳方法是：

1. 标识项目依赖项。
1. 分析每个项目的端口所需的内容。
1. 从下向上启动。

可以使用 ".NET 可移植性分析器" 来确定兼容现有库与目标平台之间的兼容性。 自动测试可帮助确保在应用程序移植时不会引入重大更改。 这些测试项目应位于第一个移植的项目中。

## <a name="references"></a>参考

- [从 .NET Framework 移植到 .NET Core](../../core/porting/index.md)
- [.NET 可移植性分析器](../../standard/analyzers/portability-analyzer.md)
- [第9频道： ".NET 可移植性分析器" (视频) ](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer)
- [2年，200应用：大规模的 .NET Core 迁移 (视频) ](https://www.youtube.com/watch?v=C-2haqb60No)

>[!div class="step-by-step"]
>[上一页](migrate-large-solutions.md)
>[下一页](understand-update-dependencies.md)
