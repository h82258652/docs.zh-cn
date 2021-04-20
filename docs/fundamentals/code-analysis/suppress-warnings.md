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
# <a name="how-to-suppress-code-analysis-warnings"></a><span data-ttu-id="17772-103">如何禁止显示代码分析警告</span><span class="sxs-lookup"><span data-stu-id="17772-103">How to suppress code analysis warnings</span></span>

<span data-ttu-id="17772-104">本文介绍了在开发 .NET 应用时抑制代码分析警告的不同方法。</span><span class="sxs-lookup"><span data-stu-id="17772-104">This article covers the various ways you can suppress warnings from code analysis when you build your .NET app.</span></span>

> [!TIP]
> <span data-ttu-id="17772-105">如果使用 Visual Studio 作为开发环境，灯泡菜单可提供一些选项来生成用于抑制警告的代码。</span><span class="sxs-lookup"><span data-stu-id="17772-105">If you're using Visual Studio as your development environment, the *light bulb* menu provides options that generate the code to suppress warnings for you.</span></span> <span data-ttu-id="17772-106">有关详细信息，请参阅[抑制冲突](/visualstudio/code-quality/use-roslyn-analyzers?#suppress-violations)。</span><span class="sxs-lookup"><span data-stu-id="17772-106">For more information, see [Suppress violations](/visualstudio/code-quality/use-roslyn-analyzers?#suppress-violations).</span></span>

## <a name="disable-the-rule"></a><span data-ttu-id="17772-107">禁用规则</span><span class="sxs-lookup"><span data-stu-id="17772-107">Disable the rule</span></span>

<span data-ttu-id="17772-108">禁用导致警告的代码分析规则后，将对整个文件或项目禁用规则（具体取决于使用的[配置文件](configuration-files.md)的作用域）。</span><span class="sxs-lookup"><span data-stu-id="17772-108">By disabling the code analysis rule that's causing the warning, you disable the rule for your entire file or project (depending on the scope of the [configuration file](configuration-files.md) that you use).</span></span> <span data-ttu-id="17772-109">若要禁用规则，请在配置文件中将其严重性设置为 `none`。</span><span class="sxs-lookup"><span data-stu-id="17772-109">To disable the rule, set its severity to `none` in the configuration file.</span></span>

```ini
[*.{cs,vb}]
dotnet_diagnostic.<rule-ID>.severity = none
```

<span data-ttu-id="17772-110">有关规则严重性的详细信息，请参阅[配置规则严重性](~/docs/fundamentals/code-analysis/configuration-options.md#severity-level)。</span><span class="sxs-lookup"><span data-stu-id="17772-110">For more information about rule severities, see [Configure rule severity](~/docs/fundamentals/code-analysis/configuration-options.md#severity-level).</span></span>

## <a name="use-a-preprocessor-directive"></a><span data-ttu-id="17772-111">使用预处理器指令</span><span class="sxs-lookup"><span data-stu-id="17772-111">Use a preprocessor directive</span></span>

<span data-ttu-id="17772-112">使用 [#pragma 警告 (C#)](../../csharp/language-reference/preprocessor-directives.md#pragma-warning) 或[禁用 (Visual Basic) ](../../visual-basic/language-reference/directives/disable-enable.md) 指令来仅抑制特定代码行的警告。</span><span class="sxs-lookup"><span data-stu-id="17772-112">Use a [#pragma warning (C#)](../../csharp/language-reference/preprocessor-directives.md#pragma-warning) or [Disable (Visual Basic)](../../visual-basic/language-reference/directives/disable-enable.md) directive to suppress the warning for only a specific line of code.</span></span>

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

## <a name="use-the-suppressmessageattribute"></a><span data-ttu-id="17772-113">使用 SuppressMessageAttribute</span><span class="sxs-lookup"><span data-stu-id="17772-113">Use the SuppressMessageAttribute</span></span>

<span data-ttu-id="17772-114">可以使用 <xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute> 在源文件中或项目的全局抑制文件（GlobalSuppressions.cs 或 GlobalSuppressions.vb）中抑制警告 。</span><span class="sxs-lookup"><span data-stu-id="17772-114">You can use a <xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute> to suppress a warning either in the source file or in a global suppressions file for the project (*GlobalSuppressions.cs* or *GlobalSuppressions.vb*).</span></span> <span data-ttu-id="17772-115">此特性提供了一种仅在项目或文件的特定部分抑制警告的方法。</span><span class="sxs-lookup"><span data-stu-id="17772-115">This attribute provides a way to suppress a warning in only certain parts of your project or file.</span></span>

<span data-ttu-id="17772-116"><xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute> 特性的两个必需的位置参数 是：规则的类别和规则 ID 。</span><span class="sxs-lookup"><span data-stu-id="17772-116">The two required, positional parameters for the <xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute> attribute are the *category* of the rule and the *rule ID*.</span></span> <span data-ttu-id="17772-117">下面的代码片段传递这些参数的 `"Usage"` 和 `"CA2200:Rethrow to preserve stack details"`。</span><span class="sxs-lookup"><span data-stu-id="17772-117">The following code snippet passes `"Usage"` and `"CA2200:Rethrow to preserve stack details"` for these parameters.</span></span>

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

<span data-ttu-id="17772-118">如果将该特性添加到全局抑制文件中，则会将抑制的[作用域](xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute.Scope)设置到所需的级别，例如 `"member"`。</span><span class="sxs-lookup"><span data-stu-id="17772-118">If you add the attribute to the global suppressions file, you [scope](xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute.Scope) the suppression to the desired level, for example `"member"`.</span></span> <span data-ttu-id="17772-119">使用 <xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute.Target> 属性指定应抑制其警告的 API。</span><span class="sxs-lookup"><span data-stu-id="17772-119">You specify the API where the warning should be suppressed using the <xref:System.Diagnostics.CodeAnalysis.SuppressMessageAttribute.Target> property.</span></span>

```csharp
[assembly: SuppressMessage("Usage", "CA2200:Rethrow to preserve stack details", Justification = "Not production code.", Scope = "member", Target = "~M:MyApp.Program.IngorableCharacters")]
```

<span data-ttu-id="17772-120">若要对未映射到显式提供的用户源的编译器生成代码抑制警告，必须将抑制特性放置在全局抑制文件中。</span><span class="sxs-lookup"><span data-stu-id="17772-120">To suppress warnings for compiler-generated code that doesn't map to explicitly provided user source, you must put the suppression attribute in a global suppressions file.</span></span> <span data-ttu-id="17772-121">例如，下面的代码将抑制针对编译器发出的构造函数的冲突：</span><span class="sxs-lookup"><span data-stu-id="17772-121">For example, the following code suppresses a violation against a compiler-emitted constructor:</span></span>

```csharp
[module: SuppressMessage("Microsoft.Design", "CA1055:AbstractTypesDoNotHavePublicConstructors", Scope="member", Target="MyTools.Type..ctor()")]
```

## <a name="see-also"></a><span data-ttu-id="17772-122">另请参阅</span><span class="sxs-lookup"><span data-stu-id="17772-122">See also</span></span>

- [<span data-ttu-id="17772-123">抑制冲突 (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="17772-123">Suppress violations (Visual Studio)</span></span>](/visualstudio/code-quality/use-roslyn-analyzers?#suppress-violations)
