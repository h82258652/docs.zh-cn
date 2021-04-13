---
description: 用于语言功能规则的 C# 编译器选项。 这些选项可控制编译器如何解释某些语言构造。
title: C# 编译器选项 - 语言功能规则
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- CheckForOverflowUnderflow compiler option [C#]
- AllowUnsafeBlocks compiler option [C#]
- DefineConstants compiler option [C#]
- LangVersion compiler option [C#]
- Nullable compiler option [C#]
ms.openlocfilehash: a7767d548e41e054006fb53cad47e8ab1f14b21b
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106496704"
---
# <a name="c-compiler-options-for-language-feature-rules"></a>用于语言功能规则的 C# 编译器选项

以下选项控制编译器如何解释语言功能。 新的 MSBuild 语法以粗体显示。 旧的 csc.exe 语法以 `code style` 显示。

- **CheckForOverflowUnderflow** / `-checked`：生成溢出检查。
- **AllowUnsafeBlocks** / `-unsafe`：允许“不安全”代码。
- **DefineConstants** / `-define`：定义条件编译符号。
- **LangVersion** / `-langversion`：指定语言版本，如 `default`（最新主版本）或 `latest`（最新版本，包括次要版本）。
- **Nullable** / `-nullable`：启用可为空上下文或可为空警告。

## <a name="checkforoverflowunderflow"></a>CheckForOverflowUnderflow

CheckForOverflowUnderflow 选项指定产生的值超出数据类型范围的整数算法语句是否将导致运行时异常。  

```xml
<CheckForOverflowUnderflow>true</CheckForOverflowUnderflow>
```

`checked` 或 `unchecked` 关键字范围内的整数算法语句不受 CheckForOverflowUnderflow 选项的影响。 如果不在 `checked` 或 `unchecked` 关键字范围内的整数算法语句产生的值超出数据类型范围，并且 CheckForOverflowUnderflow 为 `true`，则该语句将在运行时导致异常。 如果 CheckForOverflowUnderflow 为 `false`，则该语句在运行时不会导致异常。 此选项的默认值为“-checked-”`false`；溢出检查已禁用。

## <a name="allowunsafeblocks"></a>AllowUnsafeBlocks

AllowUnsafeBlocks 编译器选项允许使用 [unsafe](../keywords/unsafe.md) 关键字的代码进行编译。 此选项的默认值为 `false`，表示不允许不安全代码。

```xml
<AllowUnsafeBlocks>true</AllowUnsafeBlocks>
```

有关不安全代码的详细信息，请参阅[不安全代码和指针](../unsafe-code.md)。

## <a name="defineconstants"></a>DefineConstants

DefineConstants 选项将定义程序中所有源代码文件的符号。

```xml
<DefineConstants>name;name2</DefineConstants>
```

此选项指定要定义的一个或多个符号的名称。 DefineConstants 选项具有与 [#define](../preprocessor-directives.md#defining-symbols) 预处理器指令相同的效果，只不过编译器选项对项目中的所有文件都有效。 符号在源文件中保持已定义状态，直到源文件中的 [#undef](../preprocessor-directives.md#defining-symbols) 指令删除该定义。 当你使用 `-define` 选项时，一个文件中的 `#undef` 指令不影响项目中的其他源代码文件。 可以将由此选项创建的符号同 [#if](../preprocessor-directives.md#conditional-compilation)、[#else](../preprocessor-directives.md)、[#elif](../preprocessor-directives.md#conditional-compilation) 和 [#endif](../preprocessor-directives.md#conditional-compilation) 一起使用，对源文件进行条件编译。 C# 编译器本身不定义源代码中使用的符号或宏；所有符号定义必须都是用户定义的。

> [!NOTE]
> 同 C++ 等语言一样，C# `#define` 指令不允许为符号赋值。 例如，`#define` 不能用于创建宏或定义常数。 如果需要定义一个常数，请使用 `enum` 变量。 若要创建 C++ 风格的宏，请考虑泛型等替代项。 由于宏非常容易出错，所以 C# 不允许使用宏，但提供了更安全的替代项。

## <a name="langversion"></a>LangVersion

使编译器仅接受包含在所选 C# 语言规范中的语法。

```xml
<LangVersion>9.0</LangVersion>
```

以下为有效值：

[!INCLUDE [lang-versions-table](../includes/langversion-table.md)]

默认语言版本依赖于应用程序的目标框架以及所安装的 SDK 或 Visual Studio 的版本。 这些规则是在 [C# 语言版本控制](../configure-language-version.md#defaults)中定义的。

C# 应用程序引用的元数据不受 LangVersion 编译器选项约束。

每个版本的 C# 编译器都包含语言规范的扩展，因此 LangVersion 不提供早期版本编译器的同等功能。

此外，虽然 C# 版本更新通常与主要的 .NET Framework 版本一致，但新的语法和功能不一定绑定到该特定的 Framework 版本。 虽然新功能肯定需要与 C# 修订版一起发布的新编译器更新，但每项具体功能都有自己的最小 .NET API 或公共语言运行时要求，这些要求通过包括 NuGet 包或其他库允许功能在下层框架上运行。

无论使用哪一项 LangVersion 设置，请使用当前版本的公共语言运行时来创建 .exe 或 .dll。 友元程序集和 [ModuleAssemblyName](advanced.md#moduleassemblyname) 是一个例外，它们在 -langversion:ISO-1 下工作。

若要了解指定 C# 语言版本的其他方式，请参阅 [C# 语言版本控制](../configure-language-version.md)。

有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.LanguageVersion%2A>。

### <a name="c-language-specification"></a>C# 语言规范

| Version          | 链接                       | 描述                                                             |
|------------------|----------------------------|-------------------------------------------------------------------------|
| C# 7.0 和更高版本 |                            | 当前不可用                                                 |
| C# 6.0           | [链接][csharp-6]           | C# 语言规范版本 6 - 非官方草稿：.NET Foundation |
| C# 5.0           | [下载 PDF][csharp-5]   | 标准 ECMA-334 第 5 版                                           |
| C# 3.0           | [下载 DOC][csharp-3]   | C# 语言规范版本 3.0：Microsoft Corporation            |
| C# 2.0           | [下载 PDF][csharp-2]   | 标准 ECMA-334 第 4 版                                           |
| C# 1.2           | [下载 DOC][csharp-1.2] | C# 语言规范版本 1.2：Microsoft Corporation            |
| C# 1.0           | [下载 DOC][csharp-1]   | C# 语言规范版本 1.0：Microsoft Corporation            |

[csharp-6]: /dotnet/csharp/language-reference/language-specification/introduction
[csharp-5]: https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf
[csharp-3]: https://download.microsoft.com/download/3/8/8/388e7205-bc10-4226-b2a8-75351c669b09/CSharp%20Language%20Specification.doc
[csharp-2]: https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%204th%20edition%20June%202006.pdf
[csharp-1.2]: https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%202nd%20edition%20December%202002.pdf
[csharp-1]: https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%201st%20edition%20December%202001.pdf

### <a name="minimum-sdk-version-needed-to-support-all-language-features"></a>支持所有语言功能所需的最低 SDK 版本

下表列出了支持相应语言版本的 C# 编译器的 SDK 的最低版本：

| C# 版本 | 最低 SDK 版本                                                                  |
|------------|--------------------------------------------------------------------------------------|
| C# 8.0     | Microsoft Visual Studio/生成工具 2019，版本 16.3 或 .NET Core 3.0 SDK         |
| C# 7.3     | Microsoft Visual Studio/生成工具 2017，版本 15.7                               |
| C# 7.2     | Microsoft Visual Studio/生成工具 2017，版本 15.5                               |
| C# 7.1     | Microsoft Visual Studio/生成工具 2017，版本 15.3                               |
| C# 7.0     | Microsoft Visual Studio/生成工具 2017                                             |
| C# 6       | Microsoft Visual Studio/生成工具 2015                                             |
| C# 5       | Microsoft Visual Studio/生成工具 2012 或捆绑的 .NET Framework 4.5 编译器      |
| C# 4       | Microsoft Visual Studio/生成工具 2010 或捆绑的 .NET Framework 4.0 编译器      |
| C# 3       | Microsoft Visual Studio/生成工具 2008 或捆绑的 .NET Framework 3.5 编译器      |
| C# 2       | Microsoft Visual Studio/生成工具 2005 或捆绑的 .Net Framework 2.0 编译器      |
| C# 1.0/1.2 | Microsoft Visual Studio/生成工具 .NET 2002 或捆绑的 .NET Framework 1.0 编译器 |

## <a name="nullable"></a>Nullable

使用 Nullable 选项可指定可为空上下文。

```xml
<Nullable>enable</Nullable>
```

参数必须为以下项之一：`enable`、`disable`、`warnings` 或 `annotations`。 `enable` 参数启用可为空上下文。 指定 `disable` 将禁用可为空上下文。 如果提供 `warnings` 参数，将启用可为空警告上下文。 如果指定 `annotations` 参数，将启用可为空注释上下文。

流分析用于在可执行代码中推断变量的为空性。 推断出的变量的为空性与变量声明的为空性无关。 即使有条件地省略方法调用，也会对其进行分析。 例如，发布模式下的 <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>。

采用以下特性进行注释的方法调用也将影响流分析：

- 简单的前提条件：<xref:System.Diagnostics.CodeAnalysis.AllowNullAttribute> 和 <xref:System.Diagnostics.CodeAnalysis.DisallowNullAttribute>
- 简单的后置条件：<xref:System.Diagnostics.CodeAnalysis.MaybeNullAttribute> 和 <xref:System.Diagnostics.CodeAnalysis.NotNullAttribute>
- 有条件后置条件：<xref:System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute> 和 <xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute>
- <xref:System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute>（例如 <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> 的 `DoesNotReturnIf(false)`）和 <xref:System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute>
- <xref:System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute>
- 成员后置条件：<xref:System.Diagnostics.CodeAnalysis.MemberNotNullAttribute.%23ctor(System.String)> 和 <xref:System.Diagnostics.CodeAnalysis.MemberNotNullAttribute.%23ctor(System.String[])>

> [!IMPORTANT]
> 全局可为空上下文不适用于生成的代码文件。 无论此设置如何，都会针对标记为“已生成”的任何源文件禁用可为空上下文。 可采用四种方法将文件标记为“已生成”：
>
> 1. 在 .editorconfig 中，在应用于该文件的部分中指定 `generated_code = true`。
> 1. 将 `<auto-generated>` 或 `<auto-generated/>` 放在文件顶部的注释中。 它可以位于该注释中的任意行上，但注释块必须是该文件中的第一个元素。
> 1. 文件名以 TemporaryGeneratedFile_ 开头
> 1. 文件名用以 .designer.cs、.generated.cs、.g.cs 或 .g.i.cs 结尾   。
>
> 生成器可以选择使用 [`#nullable`](../preprocessor-directives.md#nullable-context) 预处理器指令。
