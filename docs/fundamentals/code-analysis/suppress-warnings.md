---
title: 抑制代码分析警告
description: 了解抑制 .NET 代码分析冲突的不同方法。
ms.date: 01/28/2021
ms-topic: how-to
dev_langs:
- CSharp
- VB
helpviewer_keywords:
- code analysis, suppress warnings
- suppress code analysis warnings
ms.openlocfilehash: a2c85a032c8754be2a1c5c6c8cee06754b3231f2
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740548"
---
# <a name="how-to-suppress-code-analysis-warnings"></a>如何禁止显示代码分析警告

本文介绍了在开发 .NET 应用时抑制代码分析警告的不同方法。

> [!TIP]
> 如果使用 Visual Studio 作为开发环境，灯泡菜单可提供一些选项来生成用于抑制警告的代码。 有关详细信息，请参阅[抑制冲突](/visualstudio/code-quality/use-roslyn-analyzers?#suppress-violations)。

## <a name="disable-the-rule"></a>禁用规则

禁用导致警告的代码分析规则后，将对整个文件或项目禁用规则（具体取决于使用的[配置文件](configuration-files.md)的作用域）。 若要禁用规则，请在配置文件中将其严重性设置为 `none`。

```ini
[*.{cs,vb}]
dotnet_diagnostic.<rule-ID>.severity = none
```

有关规则严重性的详细信息，请参阅[配置规则严重性](~/docs/fundamentals/code-analysis/configuration-options.md#severity-level)。

## <a name="use-a-preprocessor-directive"></a>使用预处理器指令

使用 [#pragma 警告 (C#)](../../csharp/language-reference/preprocessor-directives.md#pragma-warning) 或[禁用 (Visual Basic) ](../../visual-basic/language-reference/directives/disable-enable.md) 指令来仅抑制特定代码行的警告。

```csharp
    try { ... }
    catch (Exception e)
    {
#pragma warning disable CA2200 // Rethrow to preserve stack details
        throw e;
#pragma warning restore CA2200 // Rethrow to preserve stack details
    }
```

```vb
    Try
        ...
    Catch e As Exception
#Disable Warning CA2200 ' Rethrow to preserve stack details
        Throw e
#Enable Warning CA2200 ' Rethrow to preserve stack details
    End Try
```

## <a name="use-the-suppressmessageattribute"></a>使用 SuppressMessageAttribute

可以使用 <xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute> 在源文件中或项目的全局抑制文件（GlobalSuppressions.cs 或 GlobalSuppressions.vb）中抑制警告 。 此特性提供了一种仅在项目或文件的特定部分抑制警告的方法。

<xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute> 特性的两个必需的位置参数 是：规则的类别和规则 ID 。 下面的代码片段传递这些参数的 `"Usage"` 和 `"CA2200:Rethrow to preserve stack details"`。

```csharp
[System.Diagnostics.CodeAnalysis.SuppressMessage("Usage", "CA2200:Rethrow to preserve stack details", Justification = "Not production code.")]
private static void IngorableCharacters()
{
    try
    {
        ...
    }
    catch (Exception e)
    {
        throw e;
    }
}
```

如果将该特性添加到全局抑制文件中，则会将抑制的[作用域](xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute.Scope)设置到所需的级别，例如 `"member"`。 使用 <xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute.Target> 属性指定应抑制其警告的 API。

```csharp
[assembly: SuppressMessage("Usage", "CA2200:Rethrow to preserve stack details", Justification = "Not production code.", Scope = "member", Target = "~M:MyApp.Program.IngorableCharacters")]
```

若要对未映射到显式提供的用户源的编译器生成代码抑制警告，必须将抑制特性放置在全局抑制文件中。 例如，下面的代码将抑制针对编译器发出的构造函数的冲突：

```csharp
[module: SuppressMessage("Microsoft.Design", "CA1055:AbstractTypesDoNotHavePublicConstructors", Scope="member", Target="MyTools.Type..ctor()")]
```

## <a name="see-also"></a>另请参阅

- [抑制冲突 (Visual Studio)](/visualstudio/code-quality/use-roslyn-analyzers?#suppress-violations)
