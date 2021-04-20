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
# <a name="configuration-options-for-code-analysis"></a><span data-ttu-id="0f2e1-103">代码分析的配置选项</span><span class="sxs-lookup"><span data-stu-id="0f2e1-103">Configuration options for code analysis</span></span>

<span data-ttu-id="0f2e1-104">代码分析规则具有多种配置选项。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-104">Code analysis rules have various configuration options.</span></span> <span data-ttu-id="0f2e1-105">这些选项是在[分析器配置文件](configuration-files.md)中使用 `<option key> = <option value>` 语法以键值对形式指定的。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-105">These options are specified as key-value pairs in an [analyzer configuration file](configuration-files.md) using the syntax `<option key> = <option value>`.</span></span>

<span data-ttu-id="0f2e1-106">最常见的配置选项是[规则的严重性](#severity-level)。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-106">The most common option you'll configure is a [rule's severity](#severity-level).</span></span> <span data-ttu-id="0f2e1-107">你可以为所有分析器规则（包括[代码质量规则](quality-rules/index.md)和[代码样式规则](style-rules/index.md)）配置严重性级别。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-107">You can configure severity level for all analyzer rules, including [code quality rules](quality-rules/index.md) and [code style rules](style-rules/index.md).</span></span> <span data-ttu-id="0f2e1-108">例如，若要启用某个规则作为警告，可以向 EditorConfig 文件添加以下键值对。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-108">For example, to enable a rule as a warning, you can add the following key-value pair to an EditorConfig file.</span></span>

`dotnet_diagnostic.<rule ID>.severity = warning`

<span data-ttu-id="0f2e1-109">你还可以配置其他选项，来自定义规则行为：</span><span class="sxs-lookup"><span data-stu-id="0f2e1-109">You can also configure additional options to customize rule behavior:</span></span>

- <span data-ttu-id="0f2e1-110">代码质量规则具有用于配置行为的[其他选项](code-quality-rule-options.md)，例如规则适用的方法名称。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-110">Code quality rules have [additional options](code-quality-rule-options.md) to configure behavior, such as which method names a rule should apply to.</span></span>
- <span data-ttu-id="0f2e1-111">代码样式规则具有[自定义代码样式选项](code-style-rule-options.md)。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-111">Code style rules have [custom code style options](code-style-rule-options.md).</span></span>
- <span data-ttu-id="0f2e1-112">第三方分析器规则可以使用自定义键名和值格式定义各自的配置选项。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-112">Third party analyzer rules can define their own configuration options, with custom key names and value formats.</span></span>

<span data-ttu-id="0f2e1-113">要在[分析器配置文件](configuration-files.md)中配置特定规则的严重性，请使用下面的语法：</span><span class="sxs-lookup"><span data-stu-id="0f2e1-113">The syntax for configuring a specific rule's severity in an [analyzer configuration file](configuration-files.md) is as follows:</span></span>

```ini
dotnet_diagnostic.<rule ID>.severity = <severity>
```

## <a name="general-options"></a><span data-ttu-id="0f2e1-114">常规选项</span><span class="sxs-lookup"><span data-stu-id="0f2e1-114">General options</span></span>

<span data-ttu-id="0f2e1-115">这些选项适用于整个代码分析。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-115">These options apply to code analysis as a whole.</span></span> <span data-ttu-id="0f2e1-116">不能只将它们应用于一个规则或一组规则。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-116">They cannot be applied only to a single rule or set of rules.</span></span>

### <a name="exclude-generated-code"></a><span data-ttu-id="0f2e1-117">排除生成的代码</span><span class="sxs-lookup"><span data-stu-id="0f2e1-117">Exclude generated code</span></span>

<span data-ttu-id="0f2e1-118">通过将 `generated_code = true | false` 条目添加到[配置文件](configuration-files.md)，可以配置额外的文件和文件夹以作为生成的代码来处理。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-118">You can configure additional files and folders to be treated as generated code by adding a `generated_code = true | false` entry to your [configuration file](configuration-files.md).</span></span> <span data-ttu-id="0f2e1-119">.NET 代码分析器警告对生成的代码文件不起作用，比如对于设计器生成的文件，用户无法通过编辑这些文件来修复任何违规行为。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-119">.NET code analyzer warnings aren't useful on generated code files, such as designer-generated files, which users can't edit to fix any violations.</span></span> <span data-ttu-id="0f2e1-120">在大多数情况下，代码分析器会跳过生成的代码文件，并且不会报告这些文件上的违规行为。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-120">In most cases, code analyzers skip generated code files and don't report violations on these files.</span></span>

<span data-ttu-id="0f2e1-121">默认情况下，具有特定文件扩展名或自动生成的文件头的文件会被视为生成的代码文件。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-121">By default, files with certain file extensions or auto-generated file headers are treated as generated code files.</span></span> <span data-ttu-id="0f2e1-122">例如，以 `.designer.cs` 或 `.generated.cs` 结尾的文件名被视为生成的代码。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-122">For example, a file name ending with `.designer.cs` or `.generated.cs` is considered generated code.</span></span> <span data-ttu-id="0f2e1-123">使用这个配置选项可以指定更多命名模式。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-123">This configuration option lets you specify additional naming patterns.</span></span>

<span data-ttu-id="0f2e1-124">例如，若要将名称以 `.MyGenerated.cs` 结尾的所有文件视为生成的代码，请添加以下条目：</span><span class="sxs-lookup"><span data-stu-id="0f2e1-124">For example, to treat all files whose name ends with `.MyGenerated.cs` as generated code, add the following entry:</span></span>

```ini
[*.MyGenerated.cs]
generated_code = true
```

## <a name="rule-specific-options"></a><span data-ttu-id="0f2e1-125">特定于规则的选项</span><span class="sxs-lookup"><span data-stu-id="0f2e1-125">Rule-specific options</span></span>

<span data-ttu-id="0f2e1-126">特定于规则的选项可应用于一个规则、一组规则或所有规则。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-126">Rule-specific options can be applied to a single rule, a set of rules, or all rules.</span></span> <span data-ttu-id="0f2e1-127">特定于规则的选项包括：</span><span class="sxs-lookup"><span data-stu-id="0f2e1-127">The rule-specific options include:</span></span>

- [<span data-ttu-id="0f2e1-128">规则严重性级别</span><span class="sxs-lookup"><span data-stu-id="0f2e1-128">Rule severity level</span></span>](#severity-level)
- [<span data-ttu-id="0f2e1-129">特定于代码质量规则的选项</span><span class="sxs-lookup"><span data-stu-id="0f2e1-129">Options specific to *code-quality* rules</span></span>](code-quality-rule-options.md)

### <a name="severity-level"></a><span data-ttu-id="0f2e1-130">严重性级别</span><span class="sxs-lookup"><span data-stu-id="0f2e1-130">Severity level</span></span>

<span data-ttu-id="0f2e1-131">下表显示了可为所有分析器规则（包括[代码质量](quality-rules/index.md)和[代码样式](style-rules/index.md)规则）配置的各种规则严重性。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-131">The following table shows the different rule severities that you can configure for all analyzer rules, including [code quality](quality-rules/index.md) and [code style](style-rules/index.md) rules.</span></span>

| <span data-ttu-id="0f2e1-132">严重性配置值</span><span class="sxs-lookup"><span data-stu-id="0f2e1-132">Severity configuration value</span></span> | <span data-ttu-id="0f2e1-133">生成时行为</span><span class="sxs-lookup"><span data-stu-id="0f2e1-133">Build-time behavior</span></span> |
|-|-|
| `error` | <span data-ttu-id="0f2e1-134">违规行为以生成错误形式出现，并会导致生成失败。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-134">Violations appear as build *errors* and cause builds to fail.</span></span>|
| `warning` | <span data-ttu-id="0f2e1-135">违规行为以生成警告形式出现，但不会导致生成失败（除非你已设置将警告视为错误的选项）。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-135">Violations appear as build *warnings* but do not cause builds to fail (unless you have an option set to treat warnings as errors).</span></span> |
| `suggestion` | <span data-ttu-id="0f2e1-136">违规行为以生成消息形式出现，在 Visual Studio IDE 中以建议形式出现。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-136">Violations appear as build *messages* and as suggestions in the Visual Studio IDE.</span></span> |
| `silent` | <span data-ttu-id="0f2e1-137">违规行为对用户不可见。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-137">Violations aren't visible to the user.</span></span> |
| `none` | <span data-ttu-id="0f2e1-138">完全禁止显示规则。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-138">Rule is suppressed completely.</span></span> |
| `default` | <span data-ttu-id="0f2e1-139">使用规则的默认严重性。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-139">The default severity of the rule is used.</span></span> <span data-ttu-id="0f2e1-140">[Roslyn 分析器存储库](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)列出了每个 .NET 版本的默认严重性。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-140">The default severities for each .NET release are listed in the [roslyn-analyzers repo](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md).</span></span> <span data-ttu-id="0f2e1-141">在该表中，“禁用”与 `none` 对应，“隐藏”与 `silent` 对应，“信息”与 `suggestion` 对应。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-141">In that table, "Disabled" corresponds to `none`, "Hidden" corresponds to `silent`, and "Info" corresponds to `suggestion`.</span></span> |

> [!TIP]
> <span data-ttu-id="0f2e1-142">若要了解规则严重性在 Visual Studio 中的显示方式，请参阅[严重性级别](/visualstudio/ide/editorconfig-language-conventions#severity-levels)。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-142">For information about how rule severities surface in Visual Studio, see [Severity levels](/visualstudio/ide/editorconfig-language-conventions#severity-levels).</span></span>

#### <a name="scope"></a><span data-ttu-id="0f2e1-143">范围</span><span class="sxs-lookup"><span data-stu-id="0f2e1-143">Scope</span></span>

<span data-ttu-id="0f2e1-144">若要为单个规则设置规则严重性，请使用以下语法。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-144">To set the rule severity for a single rule, use the following syntax.</span></span>

```ini
dotnet_diagnostic.<rule ID>.severity = <severity value>
```

<span data-ttu-id="0f2e1-145">若要为某个[规则类别](categories.md)设置默认规则严重性，请使用以下语法。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-145">To set the default rule severity for a [category of rules](categories.md), use the following syntax.</span></span> <span data-ttu-id="0f2e1-146">各规则参考页（例如，[CA1000](quality-rules/ca1000.md)）中提供了每个规则的类别。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-146">The category for each rule is provided in the individual rule reference pages, for example, [CA1000](quality-rules/ca1000.md).</span></span>

```ini
dotnet_analyzer_diagnostic.category-<rule category>.severity = <severity value>
```

<span data-ttu-id="0f2e1-147">若要为所有分析器规则设置默认规则严重性，请使用以下语法。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-147">To set the default rule severity for all analyzer rules, use the following syntax.</span></span>

```ini
dotnet_analyzer_diagnostic.severity = <severity value>
```

> [!IMPORTANT]
> <span data-ttu-id="0f2e1-148">当你使用一个条目为多个规则配置严重性级别时，无论是为一个规则类别还是为所有规则配置，严重性都只适用于[默认情况下启用](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)的规则 。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-148">When you configure the severity level for multiple rules with a single entry, either for a *category* of rules or for *all* rules, the severity only applies to rules that are [enabled by default](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md).</span></span> <span data-ttu-id="0f2e1-149">若要启用默认情况下已禁用的规则，必须执行以下任一操作：</span><span class="sxs-lookup"><span data-stu-id="0f2e1-149">To enable rules that are disabled by default, you must either:</span></span>
>
> - <span data-ttu-id="0f2e1-150">为每个规则添加一个显式 `dotnet_diagnostic.<rule ID>.severity = <severity>` 配置条目。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-150">Add an explicit `dotnet_diagnostic.<rule ID>.severity = <severity>` configuration entry for each rule.</span></span>
> - <span data-ttu-id="0f2e1-151">通过将 [\<AnalysisMode>](../../core/project-sdk/msbuild-props.md#analysismode) 设置为 `AllEnabledByDefault` 来启用所有规则。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-151">Enable *all* rules by setting [\<AnalysisMode>](../../core/project-sdk/msbuild-props.md#analysismode) to `AllEnabledByDefault`.</span></span>

#### <a name="precedence"></a><span data-ttu-id="0f2e1-152">优先级</span><span class="sxs-lookup"><span data-stu-id="0f2e1-152">Precedence</span></span>

<span data-ttu-id="0f2e1-153">如果你有多个严重性配置条目可应用于同一个规则 ID，将按以下顺序选择优先级：</span><span class="sxs-lookup"><span data-stu-id="0f2e1-153">If you have multiple severity configuration entries that can be applied to the same rule ID, precedence is chosen in the following order:</span></span>

- <span data-ttu-id="0f2e1-154">基于 ID 的单个规则的条目优先于一个类别的条目。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-154">An entry for an individual rule by ID takes precedence over an entry for a category.</span></span>
- <span data-ttu-id="0f2e1-155">一个类别的条目优先于所有分析器规则的条目。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-155">An entry for a category takes precedence over an entry for all analyzer rules.</span></span>

<span data-ttu-id="0f2e1-156">请考虑以下示例，其中 [CA1822](/visualstudio/code-quality/ca1822) 属于“性能”类别：</span><span class="sxs-lookup"><span data-stu-id="0f2e1-156">Consider the following example, where [CA1822](/visualstudio/code-quality/ca1822) has the category "Performance":</span></span>

```ini
[*.cs]
dotnet_diagnostic.CA1822.severity = error
dotnet_analyzer_diagnostic.category-performance.severity = warning
dotnet_analyzer_diagnostic.severity = suggestion
```

<span data-ttu-id="0f2e1-157">在前面的示例中，三个严重性条目都适用于 CA1822。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-157">In the preceding example, all three severity entries are applicable to CA1822.</span></span> <span data-ttu-id="0f2e1-158">但是，按照指定的优先级规则，第一个基于规则 ID 的条目优先于后续条目。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-158">However, using the specified precedence rules, the first rule ID-based entry wins over the next entries.</span></span> <span data-ttu-id="0f2e1-159">在此示例中，CA1822 的有效严重性为 `error`。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-159">In this example, CA1822 will have an effective severity of `error`.</span></span> <span data-ttu-id="0f2e1-160">“性能”类别内的所有其他规则的严重性为 `warning`。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-160">All other rules within the "Performance" category will have a severity of `warning`.</span></span>

<span data-ttu-id="0f2e1-161">若要了解如何确定文件间的优先级，请参阅[“配置文件”一文的“优先级”部分](configuration-files.md#precedence)。</span><span class="sxs-lookup"><span data-stu-id="0f2e1-161">For information about how inter-file precedence is decided, see the [Precedence section of the Configuration files article](configuration-files.md#precedence).</span></span>
