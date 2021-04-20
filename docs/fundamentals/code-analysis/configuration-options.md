---
title: 配置代码分析规则
description: 了解如何在分析器配置文件中配置代码分析规则。
ms.date: 09/24/2020
no-loc:
- EditorConfig
ms.openlocfilehash: b9f934d13a510fa9e349d1cefc8131f22e9cd549
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740964"
---
# <a name="configuration-options-for-code-analysis"></a>代码分析的配置选项

代码分析规则具有多种配置选项。 这些选项是在[分析器配置文件](configuration-files.md)中使用 `<option key> = <option value>` 语法以键值对形式指定的。

最常见的配置选项是[规则的严重性](#severity-level)。 你可以为所有分析器规则（包括[代码质量规则](quality-rules/index.md)和[代码样式规则](style-rules/index.md)）配置严重性级别。 例如，若要启用某个规则作为警告，可以向 EditorConfig 文件添加以下键值对。

`dotnet_diagnostic.<rule ID>.severity = warning`

你还可以配置其他选项，来自定义规则行为：

- 代码质量规则具有用于配置行为的[其他选项](code-quality-rule-options.md)，例如规则适用的方法名称。
- 代码样式规则具有[自定义代码样式选项](code-style-rule-options.md)。
- 第三方分析器规则可以使用自定义键名和值格式定义各自的配置选项。

要在[分析器配置文件](configuration-files.md)中配置特定规则的严重性，请使用下面的语法：

```ini
dotnet_diagnostic.<rule ID>.severity = <severity>
```

## <a name="general-options"></a>常规选项

这些选项适用于整个代码分析。 不能只将它们应用于一个规则或一组规则。

### <a name="exclude-generated-code"></a>排除生成的代码

通过将 `generated_code = true | false` 条目添加到[配置文件](configuration-files.md)，可以配置额外的文件和文件夹以作为生成的代码来处理。 .NET 代码分析器警告对生成的代码文件不起作用，比如对于设计器生成的文件，用户无法通过编辑这些文件来修复任何违规行为。 在大多数情况下，代码分析器会跳过生成的代码文件，并且不会报告这些文件上的违规行为。

默认情况下，具有特定文件扩展名或自动生成的文件头的文件会被视为生成的代码文件。 例如，以 `.designer.cs` 或 `.generated.cs` 结尾的文件名被视为生成的代码。 使用这个配置选项可以指定更多命名模式。

例如，若要将名称以 `.MyGenerated.cs` 结尾的所有文件视为生成的代码，请添加以下条目：

```ini
[*.MyGenerated.cs]
generated_code = true
```

## <a name="rule-specific-options"></a>特定于规则的选项

特定于规则的选项可应用于一个规则、一组规则或所有规则。 特定于规则的选项包括：

- [规则严重性级别](#severity-level)
- [特定于代码质量规则的选项](code-quality-rule-options.md)

### <a name="severity-level"></a>严重性级别

下表显示了可为所有分析器规则（包括[代码质量](quality-rules/index.md)和[代码样式](style-rules/index.md)规则）配置的各种规则严重性。

| 严重性配置值 | 生成时行为 |
|-|-|
| `error` | 违规行为以生成错误形式出现，并会导致生成失败。|
| `warning` | 违规行为以生成警告形式出现，但不会导致生成失败（除非你已设置将警告视为错误的选项）。 |
| `suggestion` | 违规行为以生成消息形式出现，在 Visual Studio IDE 中以建议形式出现。 |
| `silent` | 违规行为对用户不可见。 |
| `none` | 完全禁止显示规则。 |
| `default` | 使用规则的默认严重性。 [Roslyn 分析器存储库](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)列出了每个 .NET 版本的默认严重性。 在该表中，“禁用”与 `none` 对应，“隐藏”与 `silent` 对应，“信息”与 `suggestion` 对应。 |

> [!TIP]
> 若要了解规则严重性在 Visual Studio 中的显示方式，请参阅[严重性级别](/visualstudio/ide/editorconfig-language-conventions#severity-levels)。

#### <a name="scope"></a>范围

若要为单个规则设置规则严重性，请使用以下语法。

```ini
dotnet_diagnostic.<rule ID>.severity = <severity value>
```

若要为某个[规则类别](categories.md)设置默认规则严重性，请使用以下语法。 各规则参考页（例如，[CA1000](quality-rules/ca1000.md)）中提供了每个规则的类别。

```ini
dotnet_analyzer_diagnostic.category-<rule category>.severity = <severity value>
```

若要为所有分析器规则设置默认规则严重性，请使用以下语法。

```ini
dotnet_analyzer_diagnostic.severity = <severity value>
```

> [!IMPORTANT]
> 当你使用一个条目为多个规则配置严重性级别时，无论是为一个规则类别还是为所有规则配置，严重性都只适用于[默认情况下启用](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)的规则 。 若要启用默认情况下已禁用的规则，必须执行以下任一操作：
>
> - 为每个规则添加一个显式 `dotnet_diagnostic.<rule ID>.severity = <severity>` 配置条目。
> - 通过将 [\<AnalysisMode>](../../core/project-sdk/msbuild-props.md#analysismode) 设置为 `AllEnabledByDefault` 来启用所有规则。

#### <a name="precedence"></a>优先级

如果你有多个严重性配置条目可应用于同一个规则 ID，将按以下顺序选择优先级：

- 基于 ID 的单个规则的条目优先于一个类别的条目。
- 一个类别的条目优先于所有分析器规则的条目。

请考虑以下示例，其中 [CA1822](/visualstudio/code-quality/ca1822) 属于“性能”类别：

```ini
[*.cs]
dotnet_diagnostic.CA1822.severity = error
dotnet_analyzer_diagnostic.category-performance.severity = warning
dotnet_analyzer_diagnostic.severity = suggestion
```

在前面的示例中，三个严重性条目都适用于 CA1822。 但是，按照指定的优先级规则，第一个基于规则 ID 的条目优先于后续条目。 在此示例中，CA1822 的有效严重性为 `error`。 “性能”类别内的所有其他规则的严重性为 `warning`。

若要了解如何确定文件间的优先级，请参阅[“配置文件”一文的“优先级”部分](configuration-files.md#precedence)。
