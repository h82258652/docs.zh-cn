---
title: 代码分析规则的配置文件
description: 了解用于配置代码分析规则的不同配置文件。
ms.date: 09/24/2020
ms.topic: conceptual
no-loc:
- EditorConfig
ms.openlocfilehash: b98fdd48f2373bd23fcd3273834860a60c682969
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "99216377"
---
# <a name="configuration-files-for-code-analysis-rules"></a>代码分析规则的配置文件

代码分析规则具有多种[配置选项](configuration-options.md)。 可以在下列任一分析器配置文件中将这些选项指定为键值对：

- [EditorConfig](#editorconfig) 文件：基于文件或基于文件夹的配置选项。
- [全局 AnalyzerConfig](#global-analyzerconfig) 文件：项目级别配置选项。 当某些项目文件位于项目文件夹外时，它非常有用。

## EditorConfig

[EditorConfig](/visualstudio/ide/create-portable-custom-editor-options) 文件用于提供适用于特定资源文件或文件夹的选项。 选项位于节标头下，用于标识适用的文件和文件夹。 为要配置的每个规则添加一个条目，并将其放置在相应的文件扩展名节下，例如 `[*.cs]`。

```ini
[*.cs]
<option_name> = <option_value>
```

在上面的示例中，`[*.cs]` 是一个 editorconfig 节标头，用于选择当前文件夹（包括子文件夹）中带有 `.cs` 文件扩展名的所有 C# 文件。 接下来 `<option_name> = <option_value>` 这一条目是一个分析器选项，将应用于所有 C# 文件。

可将文件放在相应的目录中，将 EditorConfig 文件约定应用于文件夹、项目或整个存储库。 可在生成时执行分析时以及在 Visual Studio 中编辑代码时应用这些选项。

如果有一个现有的 .editorconfig 文件可用于编辑器设置（如缩进大小或是否剪裁尾随空格），可将代码分析配置选项放在同一文件中。

> [!TIP]
> Visual Studio 提供 .editorconfig 项模板，通过该模板可轻松地将其中一个文件添加到项目中。 有关详细信息，请参阅[将 EditorConfig 文件添加到项目](/visualstudio/ide/create-portable-custom-editor-options#add-an-editorconfig-file-to-a-project)。

### <a name="example"></a>示例

下面是一个示例 EditorConfig 文件，用于配置选项和规则严重性：

```ini
# Remove the line below if you want to inherit .editorconfig settings from higher directories
root = true

# C# files
[*.cs]

#### Core EditorConfig Options ####

# Indentation and spacing
indent_size = 4
indent_style = space
tab_width = 4

#### .NET Coding Conventions ####

# this. and Me. preferences
dotnet_style_qualification_for_method = true

#### Diagnostic configuration ####

# CA1000: Do not declare static members on generic types
dotnet_diagnostic.CA1000.severity = warning
```

## <a name="global-analyzerconfig"></a>全局 AnalyzerConfig

从 .NET 5 SDK（在 Visual Studio 2019 版本 16.8 和更高版本中受支持）开始，还可配置包含全局 AnalyzerConfig 文件的分析器选项。 这些文件用于提供适用于项目中所有源文件的选项，不考虑其文件名和文件路径。

与 [EditorConfig](#editorconfig) 文件不同，全局配置文件不能用于为 IDE 配置编辑器样式设置，如缩进大小或是否剪裁尾随空格。 而是专用于指定项目级别分析器配置选项。

### <a name="format"></a>格式

EditorConfig 文件必须包含节标头（如 `[*.cs]`），以标识适用的文件和文件夹，但全局 AnalyzerConfig 文件没有节标头。 相反，它们需要 `is_global = true` 格式的顶级条目，以便与常规 EditorConfig 文件区分开来。 这表示文件中的所有选项都适用于整个项目。 例如：

```ini
is_global = true
<option_name> = <option_value>
```

### <a name="naming"></a>命名

EditorConfig 文件必须命名为 `.editorconfig`，而全局配置文件不需要有特定的名称或文件扩展名。 但是，如果将这些文件命名为 `.globalconfig`，它们将隐式应用于当前文件夹（包括子文件夹）中的所有 C# 和 Visual Basic 项目。 否则，必须将 `GlobalAnalyzerConfigFiles` 项显式添加到 MSBuild 项目文件中：

```xml
<ItemGroup>
  <GlobalAnalyzerConfigFiles Include="<path_to_global_analyzer_config>" />
</ItemGroup>
```

> [!NOTE]
> 即使文件命名为 `.globalconfig`，也需要顶级条目 `is_global = true`。

### <a name="example"></a>示例

下面是一个示例全局 AnalyzerConfig 文件，用于在项目级别配置选项和规则严重性：

```ini
# Top level entry required to mark this as a global AnalyzerConfig file
is_global = true

# NOTE: No section headers for configuration entries

#### .NET Coding Conventions ####

# this. and Me. preferences
dotnet_style_qualification_for_method = true:warning

#### Diagnostic configuration ####

# CA1000: Do not declare static members on generic types
dotnet_diagnostic.CA1000.severity = warning
```

## <a name="precedence"></a>优先级

EditorConfig 文件和全局 AnalyzerConfig 文件都为每个选项指定键值对。 如果有多个条目具有相同键但值不同，则会发生冲突。

### <a name="general-options"></a>常规选项

选项之间发生冲突时，可使用以下优先级规则来解决冲突：

- 相同配置文件中出现冲突条目：先选在文件中靠后的条目。 这适用于在单个 EditorConfig 文件中和单个全局 AnalyzerConfig 文件中的冲突条目。

- 两个 EditorConfig 文件中出现冲突条目：先选 EditorConfig 文件位于文件系统更深层的条目（因此文件路径较长）。

- 两个全局 AnalyzerConfig 文件中出现冲突条目：报告编译器警告，忽略这两个条目。

- EditorConfig 文件和全局 AnalyzerConfig 文件中出现冲突条目：先选 EditorConfig 文件中的条目。

### <a name="severity-options"></a>严重性选项

上述[常规优先规则](#general-options)适用于配置文件中指定的所有选项。 [严重性配置](configuration-options.md#severity-level)选项适用于下列其他优先级规则：

- 在命令行上作为编译器选项（`/nowarn` 或 `/warnaserror`）指定的严重性选项始终会重写 EditorConfig 和全局 AnalyzerConfig 文件中指定的[严重性配置](configuration-options.md#severity-level)选项。

- 还可使用[规则集](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules)文件指定严重性选项。 但是，规则集文件已弃用，改用 EditorConfig 和全局 AnalyzerConfig 文件。 建议[将规则集文件转换为等效的 EditorConfig 文件](/visualstudio/code-quality/use-roslyn-analyzers#convert-an-existing-ruleset-file-to-editorconfig-file)。 规则集文件和 EditorConfig 或全局 AnalyzerConfig 文件中的严重性冲突条目的优先级未定义。

- 有关具有不同键的相关严重性选项的优先级规则的信息（例如，为单个规则和为规则所属的类别指定不同的严重性），请参阅[代码分析的配置选项](configuration-options.md#precedence)。

## <a name="see-also"></a>另请参阅

- [EditorConfig 与全局 AnalyzerConfig 设计问题](https://github.com/dotnet/roslyn/issues/47707)
