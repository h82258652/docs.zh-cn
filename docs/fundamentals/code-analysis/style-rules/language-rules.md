---
title: 代码样式语言规则
description: 了解使用 C# 和 Visual Basic 语言构造的不同代码样式规则。
ms.date: 09/25/2020
ms.topic: reference
author: gewarren
ms.author: gewarren
dev_langs:
- CSharp
- VB
helpviewer_keywords:
- language code style rules [EditorConfig]
- language rules
- EditorConfig language conventions
ms.openlocfilehash: 2aa2261534363f1da6a2109f092e08d210ebd915
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "98957970"
---
# <a name="language-rules"></a><span data-ttu-id="f2879-103">语言规则</span><span class="sxs-lookup"><span data-stu-id="f2879-103">Language rules</span></span>

<span data-ttu-id="f2879-104">代码样式语言规则会影响 .NET 编程语言的各种构造（如修饰符和括号）的使用方式。</span><span class="sxs-lookup"><span data-stu-id="f2879-104">Code style language rules affect how various constructs of .NET programming languages, for example, modifiers and parentheses, are used.</span></span> <span data-ttu-id="f2879-105">规则分为以下几类：</span><span class="sxs-lookup"><span data-stu-id="f2879-105">The rules fall into the following categories:</span></span>

- <span data-ttu-id="f2879-106">[.NET 样式规则](#net-style-rules)：适用于 C# 和 Visual Basic 的规则。</span><span class="sxs-lookup"><span data-stu-id="f2879-106">[.NET style rules](#net-style-rules): Rules that apply to both C# and Visual Basic.</span></span> <span data-ttu-id="f2879-107">这些规则的 EditorConfig 选项名称以 `dotnet_style_` 前缀开头。</span><span class="sxs-lookup"><span data-stu-id="f2879-107">The EditorConfig option names for these rules start with `dotnet_style_` prefix.</span></span>
- <span data-ttu-id="f2879-108">[C# 样式规则](#c-style-rules)：仅适用于 C# 语言的规则。</span><span class="sxs-lookup"><span data-stu-id="f2879-108">[C# style rules](#c-style-rules): Rules that are specific to C# language only.</span></span> <span data-ttu-id="f2879-109">这些规则的 EditorConfig 选项名称以 `csharp_style_` 前缀开头。</span><span class="sxs-lookup"><span data-stu-id="f2879-109">The EditorConfig option names for these rules start with `csharp_style_` prefix.</span></span>
- <span data-ttu-id="f2879-110">[Visual Basic 样式规则](#visual-basic-style-rules)：仅适用于 Visual Bsic 语言的规则。</span><span class="sxs-lookup"><span data-stu-id="f2879-110">[Visual Basic style rules](#visual-basic-style-rules): Rules that are specific to Visual Bsic language only.</span></span> <span data-ttu-id="f2879-111">这些规则的 EditorConfig 选项名称以 `visual_basic_style_` 前缀开头。</span><span class="sxs-lookup"><span data-stu-id="f2879-111">The EditorConfig option names for these rules start with `visual_basic_style_` prefix.</span></span>

## <a name="option-format"></a><span data-ttu-id="f2879-112">选项格式</span><span class="sxs-lookup"><span data-stu-id="f2879-112">Option format</span></span>

<span data-ttu-id="f2879-113">可以在 EditorConfig 文件中指定语言规则的选项，格式如下：</span><span class="sxs-lookup"><span data-stu-id="f2879-113">Options for language rules can be specified in an EditorConfig file with the following format:</span></span>

<span data-ttu-id="f2879-114">`option_name = value`（Visual Studio 2019 版本 16.9 预览版 2 及更高版本）</span><span class="sxs-lookup"><span data-stu-id="f2879-114">`option_name = value` (Visual Studio 2019 version 16.9 Preview 2 and later)</span></span>

<span data-ttu-id="f2879-115">或</span><span class="sxs-lookup"><span data-stu-id="f2879-115">or</span></span>

`option_name = value:severity`

- <span data-ttu-id="f2879-116">**值**</span><span class="sxs-lookup"><span data-stu-id="f2879-116">**Value**</span></span>

  <span data-ttu-id="f2879-117">对于每个语言规则，可指定一个定义是否或何时以此样式为首选项的值。</span><span class="sxs-lookup"><span data-stu-id="f2879-117">For each language rule, you specify a value that defines if or when to prefer the style.</span></span> <span data-ttu-id="f2879-118">许多规则都接受 `true` 值（以此样式为首选项）或 `false` 值（不以此样式为首选项）。</span><span class="sxs-lookup"><span data-stu-id="f2879-118">Many rules accept a value of `true` (prefer this style) or `false` (do not prefer this style).</span></span> <span data-ttu-id="f2879-119">其他规则接受 `when_on_single_line` 或 `never` 等值。</span><span class="sxs-lookup"><span data-stu-id="f2879-119">Other rules accept values such as `when_on_single_line` or `never`.</span></span>

- <span data-ttu-id="f2879-120">**严重性**（在 Visual Studio 2019 版本 16.9 预览版 2 及更高版本中可选）</span><span class="sxs-lookup"><span data-stu-id="f2879-120">**Severity** (optional in Visual Studio 2019 version 16.9 Preview 2 and later versions)</span></span>

  <span data-ttu-id="f2879-121">此规则的第二部分指定规则的[严重性级别](../configuration-options.md#severity-level)。</span><span class="sxs-lookup"><span data-stu-id="f2879-121">The second part of the rule specifies the [severity level](../configuration-options.md#severity-level) for the rule.</span></span> <span data-ttu-id="f2879-122">以这种方式指定时，只有 Visual Studio 之类的开发 IDE 会遵循严重性设置。</span><span class="sxs-lookup"><span data-stu-id="f2879-122">When specified in this way, the severity setting is only respected inside development IDEs, such as Visual Studio.</span></span> <span data-ttu-id="f2879-123">在生成过程中不会遵循这一点。</span><span class="sxs-lookup"><span data-stu-id="f2879-123">It is *not* respected during build.</span></span>

  <span data-ttu-id="f2879-124">若要在生成时强制实施代码样式规则，请改为使用分析器的基于规则 ID 的严重性配置语法来设置严重性。</span><span class="sxs-lookup"><span data-stu-id="f2879-124">To enforce code style rules at build time, set the severity by using the rule ID-based severity configuration syntax for analyzers instead.</span></span> <span data-ttu-id="f2879-125">语法采用形式 `dotnet_diagnostic.<rule ID>.severity = <severity>`（例如，`dotnet_diagnostic.IDE0040.severity = silent`）。</span><span class="sxs-lookup"><span data-stu-id="f2879-125">The syntax takes the form `dotnet_diagnostic.<rule ID>.severity = <severity>`, for example, `dotnet_diagnostic.IDE0040.severity = silent`.</span></span> <span data-ttu-id="f2879-126">有关详细信息，请参阅[严重性级别](../configuration-options.md#severity-level)。</span><span class="sxs-lookup"><span data-stu-id="f2879-126">For more information, see [severity level](../configuration-options.md#severity-level).</span></span>

> [!TIP]
>
> <span data-ttu-id="f2879-127">自 Visual Studio 2019 版本 16.3 起，在样式冲突发生之后，可以在[快速操作](/visualstudio/ide/quick-actions)灯泡菜单中配置代码样式规则。</span><span class="sxs-lookup"><span data-stu-id="f2879-127">Starting in Visual Studio 2019 version 16.3, you can configure code style rules from the [Quick Actions](/visualstudio/ide/quick-actions) light bulb menu after a style violation occurs.</span></span> <span data-ttu-id="f2879-128">有关详细信息，请参阅[在 Visual Studio 中自动配置代码样式](/visualstudio/ide/editorconfig-language-conventions#automatically-configure-code-styles-in-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="f2879-128">For more information, see [Automatically configure code styles in Visual Studio](/visualstudio/ide/editorconfig-language-conventions#automatically-configure-code-styles-in-visual-studio).</span></span>

## <a name="net-style-rules"></a><span data-ttu-id="f2879-129">.NET 样式规则</span><span class="sxs-lookup"><span data-stu-id="f2879-129">.NET style rules</span></span>

<span data-ttu-id="f2879-130">本节中的样式规则均适用于 C# 和 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="f2879-130">The style rules in this section are applicable to both C# and Visual Basic.</span></span>

- [<span data-ttu-id="f2879-131">“this.”和“Me.”限定符</span><span class="sxs-lookup"><span data-stu-id="f2879-131">'this.' and 'Me.' qualifiers</span></span>](ide0003-ide0009.md)
  - [<span data-ttu-id="f2879-132">dotnet_style_qualification_for_field</span><span class="sxs-lookup"><span data-stu-id="f2879-132">dotnet_style_qualification_for_field</span></span>](ide0003-ide0009.md#dotnet_style_qualification_for_field)
  - [<span data-ttu-id="f2879-133">dotnet_style_qualification_for_property</span><span class="sxs-lookup"><span data-stu-id="f2879-133">dotnet_style_qualification_for_property</span></span>](ide0003-ide0009.md#dotnet_style_qualification_for_property)
  - [<span data-ttu-id="f2879-134">dotnet_style_qualification_for_method</span><span class="sxs-lookup"><span data-stu-id="f2879-134">dotnet_style_qualification_for_method</span></span>](ide0003-ide0009.md#dotnet_style_qualification_for_method)
  - [<span data-ttu-id="f2879-135">dotnet_style_qualification_for_event</span><span class="sxs-lookup"><span data-stu-id="f2879-135">dotnet_style_qualification_for_event</span></span>](ide0003-ide0009.md#dotnet_style_qualification_for_event)
- [<span data-ttu-id="f2879-136">语言关键字，而非类型引用的框架类型名称</span><span class="sxs-lookup"><span data-stu-id="f2879-136">Language keywords instead of framework type names for type references</span></span>](ide0049.md)
  - [<span data-ttu-id="f2879-137">dotnet_style_predefined_type_for_locals_parameters_members</span><span class="sxs-lookup"><span data-stu-id="f2879-137">dotnet_style_predefined_type_for_locals_parameters_members</span></span>](ide0049.md#dotnet_style_predefined_type_for_locals_parameters_members)
  - [<span data-ttu-id="f2879-138">dotnet_style_predefined_type_for_member_access</span><span class="sxs-lookup"><span data-stu-id="f2879-138">dotnet_style_predefined_type_for_member_access</span></span>](ide0049.md#dotnet_style_predefined_type_for_member_access)
- [<span data-ttu-id="f2879-139">修饰符首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-139">Modifier preferences</span></span>](modifier-preferences.md#net-modifier-preferences)
  - [<span data-ttu-id="f2879-140">dotnet_style_require_accessibility_modifiers</span><span class="sxs-lookup"><span data-stu-id="f2879-140">dotnet_style_require_accessibility_modifiers</span></span>](ide0040.md#dotnet_style_require_accessibility_modifiers)
  - [<span data-ttu-id="f2879-141">csharp_preferred_modifier_order</span><span class="sxs-lookup"><span data-stu-id="f2879-141">csharp_preferred_modifier_order</span></span>](ide0036.md#csharp_preferred_modifier_order)
  - [<span data-ttu-id="f2879-142">visual_basic_preferred_modifier_order</span><span class="sxs-lookup"><span data-stu-id="f2879-142">visual_basic_preferred_modifier_order</span></span>](ide0036.md#visual_basic_preferred_modifier_order)
  - [<span data-ttu-id="f2879-143">dotnet_style_readonly_field</span><span class="sxs-lookup"><span data-stu-id="f2879-143">dotnet_style_readonly_field</span></span>](ide0044.md#dotnet_style_readonly_field)
- [<span data-ttu-id="f2879-144">括号首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-144">Parentheses preferences</span></span>](ide0047-ide0048.md)
  - [<span data-ttu-id="f2879-145">dotnet_style_parentheses_in_arithmetic_binary_operators</span><span class="sxs-lookup"><span data-stu-id="f2879-145">dotnet_style_parentheses_in_arithmetic_binary_operators</span></span>](ide0047-ide0048.md#dotnet_style_parentheses_in_arithmetic_binary_operators)
  - [<span data-ttu-id="f2879-146">dotnet_style_parentheses_in_relational_binary_operators</span><span class="sxs-lookup"><span data-stu-id="f2879-146">dotnet_style_parentheses_in_relational_binary_operators</span></span>](ide0047-ide0048.md#dotnet_style_parentheses_in_relational_binary_operators)
  - [<span data-ttu-id="f2879-147">dotnet_style_parentheses_in_other_binary_operators</span><span class="sxs-lookup"><span data-stu-id="f2879-147">dotnet_style_parentheses_in_other_binary_operators</span></span>](ide0047-ide0048.md#dotnet_style_parentheses_in_other_binary_operators)
  - [<span data-ttu-id="f2879-148">dotnet_style_parentheses_in_other_operators</span><span class="sxs-lookup"><span data-stu-id="f2879-148">dotnet_style_parentheses_in_other_operators</span></span>](ide0047-ide0048.md#dotnet_style_parentheses_in_other_operators)
- [<span data-ttu-id="f2879-149">表达式级首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-149">Expression-level preferences</span></span>](expression-level-preferences.md#net-expression-level-preferences)
  - [<span data-ttu-id="f2879-150">dotnet_style_object_initializer</span><span class="sxs-lookup"><span data-stu-id="f2879-150">dotnet_style_object_initializer</span></span>](ide0017.md#dotnet_style_object_initializer)
  - [<span data-ttu-id="f2879-151">dotnet_style_collection_initializer</span><span class="sxs-lookup"><span data-stu-id="f2879-151">dotnet_style_collection_initializer</span></span>](ide0028.md#dotnet_style_collection_initializer)
  - [<span data-ttu-id="f2879-152">dotnet_style_explicit_tuple_names</span><span class="sxs-lookup"><span data-stu-id="f2879-152">dotnet_style_explicit_tuple_names</span></span>](ide0033.md#dotnet_style_explicit_tuple_names)
  - [<span data-ttu-id="f2879-153">dotnet_style_prefer_inferred_tuple_names</span><span class="sxs-lookup"><span data-stu-id="f2879-153">dotnet_style_prefer_inferred_tuple_names</span></span>](ide0037.md#dotnet_style_prefer_inferred_tuple_names)
  - [<span data-ttu-id="f2879-154">dotnet_style_prefer_inferred_anonymous_type_member_names</span><span class="sxs-lookup"><span data-stu-id="f2879-154">dotnet_style_prefer_inferred_anonymous_type_member_names</span></span>](ide0037.md#dotnet_style_prefer_inferred_anonymous_type_member_names)
  - [<span data-ttu-id="f2879-155">dotnet_style_prefer_auto_properties</span><span class="sxs-lookup"><span data-stu-id="f2879-155">dotnet_style_prefer_auto_properties</span></span>](ide0032.md#dotnet_style_prefer_auto_properties)
  - [<span data-ttu-id="f2879-156">dotnet_style_prefer_conditional_expression_over_assignment</span><span class="sxs-lookup"><span data-stu-id="f2879-156">dotnet_style_prefer_conditional_expression_over_assignment</span></span>](ide0045.md#dotnet_style_prefer_conditional_expression_over_assignment)
  - [<span data-ttu-id="f2879-157">dotnet_style_prefer_conditional_expression_over_return</span><span class="sxs-lookup"><span data-stu-id="f2879-157">dotnet_style_prefer_conditional_expression_over_return</span></span>](ide0046.md#dotnet_style_prefer_conditional_expression_over_return)
  - [<span data-ttu-id="f2879-158">dotnet_style_prefer_compound_assignment</span><span class="sxs-lookup"><span data-stu-id="f2879-158">dotnet_style_prefer_compound_assignment</span></span>](ide0054-ide0074.md#dotnet_style_prefer_compound_assignment)
  - [<span data-ttu-id="f2879-159">dotnet_style_prefer_simplified_interpolation</span><span class="sxs-lookup"><span data-stu-id="f2879-159">dotnet_style_prefer_simplified_interpolation</span></span>](ide0071.md#dotnet_style_prefer_simplified_interpolation)
  - [<span data-ttu-id="f2879-160">dotnet_style_prefer_simplified_boolean_expressions</span><span class="sxs-lookup"><span data-stu-id="f2879-160">dotnet_style_prefer_simplified_boolean_expressions</span></span>](ide0075.md#dotnet_style_prefer_simplified_boolean_expressions)
  - <span data-ttu-id="f2879-161">[向 switch 语句添加缺失的事例](ide0010.md) - 此规则没有代码样式选项。</span><span class="sxs-lookup"><span data-stu-id="f2879-161">[Add missing cases to switch statement](ide0010.md) - This rule has no code style option.</span></span>
  - <span data-ttu-id="f2879-162">[将匿名类型转换为元组](ide0050.md) - 此规则没有代码样式选项。</span><span class="sxs-lookup"><span data-stu-id="f2879-162">[Convert anonymous type to tuple](ide0050.md) - This rule has no code style option.</span></span>
  - <span data-ttu-id="f2879-163">[使用“System.HashCode.Combine”](ide0070.md)- 此规则没有代码样式选项。</span><span class="sxs-lookup"><span data-stu-id="f2879-163">[Use 'System.HashCode.Combine'](ide0070.md) - This rule has no code style option.</span></span>
  - <span data-ttu-id="f2879-164">[将“typeof”转换为“nameof”](ide0082.md)- 此规则没有代码样式选项。</span><span class="sxs-lookup"><span data-stu-id="f2879-164">[Convert 'typeof' to 'nameof'](ide0082.md) - This rule has no code style option.</span></span>
- [<span data-ttu-id="f2879-165">Null 检查首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-165">Null-checking preferences</span></span>](null-checking-preferences.md#net-null-checking-preferences)
  - [<span data-ttu-id="f2879-166">dotnet_style_coalesce_expression</span><span class="sxs-lookup"><span data-stu-id="f2879-166">dotnet_style_coalesce_expression</span></span>](ide0029-ide0030.md#dotnet_style_coalesce_expression)
  - [<span data-ttu-id="f2879-167">dotnet_style_null_propagation</span><span class="sxs-lookup"><span data-stu-id="f2879-167">dotnet_style_null_propagation</span></span>](ide0031.md#dotnet_style_null_propagation)
  - [<span data-ttu-id="f2879-168">dotnet_style_prefer_is_null_check_over_reference_equality_method</span><span class="sxs-lookup"><span data-stu-id="f2879-168">dotnet_style_prefer_is_null_check_over_reference_equality_method</span></span>](ide0041.md#dotnet_style_prefer_is_null_check_over_reference_equality_method)
- [<span data-ttu-id="f2879-169">文件头首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-169">File header preferences</span></span>](ide0073.md)
  - [<span data-ttu-id="f2879-170">file_header_template</span><span class="sxs-lookup"><span data-stu-id="f2879-170">file_header_template</span></span>](ide0073.md#file_header_template)

## <a name="c-style-rules"></a><span data-ttu-id="f2879-171">C# 样式规则</span><span class="sxs-lookup"><span data-stu-id="f2879-171">C# style rules</span></span>

<span data-ttu-id="f2879-172">本节中的样式规则仅适用于 C# 语言。</span><span class="sxs-lookup"><span data-stu-id="f2879-172">The style rules in this section are applicable to C# language only.</span></span>

- [<span data-ttu-id="f2879-173">“var”首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-173">'var' preferences</span></span>](ide0007-ide0008.md)
  - [<span data-ttu-id="f2879-174">csharp_style_var_for_built_in_types</span><span class="sxs-lookup"><span data-stu-id="f2879-174">csharp_style_var_for_built_in_types</span></span>](ide0007-ide0008.md#csharp_style_var_for_built_in_types)
  - [<span data-ttu-id="f2879-175">csharp_style_var_when_type_is_apparent</span><span class="sxs-lookup"><span data-stu-id="f2879-175">csharp_style_var_when_type_is_apparent</span></span>](ide0007-ide0008.md#csharp_style_var_when_type_is_apparent)
  - [<span data-ttu-id="f2879-176">csharp_style_var_elsewhere</span><span class="sxs-lookup"><span data-stu-id="f2879-176">csharp_style_var_elsewhere</span></span>](ide0007-ide0008.md#csharp_style_var_elsewhere)
- [<span data-ttu-id="f2879-177">Expression-Bodied 成员</span><span class="sxs-lookup"><span data-stu-id="f2879-177">Expression-bodied members</span></span>](expression-bodied-members.md)
  - [<span data-ttu-id="f2879-178">csharp_style_expression_bodied_methods</span><span class="sxs-lookup"><span data-stu-id="f2879-178">csharp_style_expression_bodied_methods</span></span>](ide0022.md#csharp_style_expression_bodied_methods)
  - [<span data-ttu-id="f2879-179">csharp_style_expression_bodied_constructors</span><span class="sxs-lookup"><span data-stu-id="f2879-179">csharp_style_expression_bodied_constructors</span></span>](ide0021.md#csharp_style_expression_bodied_constructors)
  - [<span data-ttu-id="f2879-180">csharp_style_expression_bodied_operators</span><span class="sxs-lookup"><span data-stu-id="f2879-180">csharp_style_expression_bodied_operators</span></span>](ide0023-ide0024.md#csharp_style_expression_bodied_operators)
  - [<span data-ttu-id="f2879-181">csharp_style_expression_bodied_properties</span><span class="sxs-lookup"><span data-stu-id="f2879-181">csharp_style_expression_bodied_properties</span></span>](ide0025.md#csharp_style_expression_bodied_properties)
  - [<span data-ttu-id="f2879-182">csharp_style_expression_bodied_indexers</span><span class="sxs-lookup"><span data-stu-id="f2879-182">csharp_style_expression_bodied_indexers</span></span>](ide0026.md#csharp_style_expression_bodied_indexers)
  - [<span data-ttu-id="f2879-183">csharp_style_expression_bodied_accessors</span><span class="sxs-lookup"><span data-stu-id="f2879-183">csharp_style_expression_bodied_accessors</span></span>](ide0027.md#csharp_style_expression_bodied_accessors)
  - [<span data-ttu-id="f2879-184">csharp_style_expression_bodied_lambdas</span><span class="sxs-lookup"><span data-stu-id="f2879-184">csharp_style_expression_bodied_lambdas</span></span>](ide0053.md#csharp_style_expression_bodied_lambdas)
  - [<span data-ttu-id="f2879-185">csharp_style_expression_bodied_local_functions</span><span class="sxs-lookup"><span data-stu-id="f2879-185">csharp_style_expression_bodied_local_functions</span></span>](ide0061.md#csharp_style_expression_bodied_local_functions)
- [<span data-ttu-id="f2879-186">模式匹配首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-186">Pattern matching preferences</span></span>](pattern-matching-preferences.md)
  - [<span data-ttu-id="f2879-187">csharp_style_pattern_matching_over_is_with_cast_check</span><span class="sxs-lookup"><span data-stu-id="f2879-187">csharp_style_pattern_matching_over_is_with_cast_check</span></span>](ide0020-ide0038.md#csharp_style_pattern_matching_over_is_with_cast_check)
  - [<span data-ttu-id="f2879-188">csharp_style_pattern_matching_over_as_with_null_check</span><span class="sxs-lookup"><span data-stu-id="f2879-188">csharp_style_pattern_matching_over_as_with_null_check</span></span>](ide0019.md#csharp_style_pattern_matching_over_as_with_null_check)
  - [<span data-ttu-id="f2879-189">csharp_style_prefer_switch_expression</span><span class="sxs-lookup"><span data-stu-id="f2879-189">csharp_style_prefer_switch_expression</span></span>](ide0066.md#csharp_style_prefer_switch_expression)
  - [<span data-ttu-id="f2879-190">csharp_style_prefer_pattern_matching</span><span class="sxs-lookup"><span data-stu-id="f2879-190">csharp_style_prefer_pattern_matching</span></span>](ide0078.md#csharp_style_prefer_pattern_matching)
  - [<span data-ttu-id="f2879-191">csharp_style_prefer_not_pattern</span><span class="sxs-lookup"><span data-stu-id="f2879-191">csharp_style_prefer_not_pattern</span></span>](ide0083.md#csharp_style_prefer_not_pattern)
- [<span data-ttu-id="f2879-192">表达式级首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-192">Expression-level preferences</span></span>](expression-level-preferences.md#c-expression-level-preferences)
  - [<span data-ttu-id="f2879-193">csharp_style_inlined_variable_declaration</span><span class="sxs-lookup"><span data-stu-id="f2879-193">csharp_style_inlined_variable_declaration</span></span>](ide0018.md#csharp_style_inlined_variable_declaration)
  - [<span data-ttu-id="f2879-194">csharp_prefer_simple_default_expression</span><span class="sxs-lookup"><span data-stu-id="f2879-194">csharp_prefer_simple_default_expression</span></span>](ide0034.md#csharp_prefer_simple_default_expression)
  - [<span data-ttu-id="f2879-195">csharp_style_pattern_local_over_anonymous_function</span><span class="sxs-lookup"><span data-stu-id="f2879-195">csharp_style_pattern_local_over_anonymous_function</span></span>](ide0039.md#csharp_style_pattern_local_over_anonymous_function)
  - [<span data-ttu-id="f2879-196">csharp_style_deconstructed_variable_declaration</span><span class="sxs-lookup"><span data-stu-id="f2879-196">csharp_style_deconstructed_variable_declaration</span></span>](ide0042.md#csharp_style_deconstructed_variable_declaration)
  - [<span data-ttu-id="f2879-197">csharp_style_prefer_index_operator</span><span class="sxs-lookup"><span data-stu-id="f2879-197">csharp_style_prefer_index_operator</span></span>](ide0056.md#csharp_style_prefer_index_operator)
  - [<span data-ttu-id="f2879-198">csharp_style_prefer_range_operator</span><span class="sxs-lookup"><span data-stu-id="f2879-198">csharp_style_prefer_range_operator</span></span>](ide0057.md#csharp_style_prefer_range_operator)
  - [<span data-ttu-id="f2879-199">csharp_style_implicit_object_creation_when_type_is_apparent</span><span class="sxs-lookup"><span data-stu-id="f2879-199">csharp_style_implicit_object_creation_when_type_is_apparent</span></span>](ide0090.md#csharp_style_implicit_object_creation_when_type_is_apparent)
  - <span data-ttu-id="f2879-200">[向 switch 表达式添加缺失的事例](ide0072.md) - 此规则没有代码样式选项。</span><span class="sxs-lookup"><span data-stu-id="f2879-200">[Add missing cases to switch expression](ide0072.md) - This rule has no code style option.</span></span>
- [<span data-ttu-id="f2879-201">“NULL”检查首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-201">"Null" checking preferences</span></span>](null-checking-preferences.md#c-null-checking-preferences)
  - [<span data-ttu-id="f2879-202">csharp_style_throw_expression</span><span class="sxs-lookup"><span data-stu-id="f2879-202">csharp_style_throw_expression</span></span>](ide0016.md#csharp_style_throw_expression)
  - [<span data-ttu-id="f2879-203">csharp_style_conditional_delegate_call</span><span class="sxs-lookup"><span data-stu-id="f2879-203">csharp_style_conditional_delegate_call</span></span>](ide1005.md#csharp_style_conditional_delegate_call)
- [<span data-ttu-id="f2879-204">代码块首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-204">Code block preferences</span></span>](code-block-preferences.md)
  - [<span data-ttu-id="f2879-205">csharp_prefer_braces</span><span class="sxs-lookup"><span data-stu-id="f2879-205">csharp_prefer_braces</span></span>](ide0011.md#csharp_prefer_braces)
  - [<span data-ttu-id="f2879-206">csharp_prefer_simple_using_statement</span><span class="sxs-lookup"><span data-stu-id="f2879-206">csharp_prefer_simple_using_statement</span></span>](ide0063.md#csharp_prefer_simple_using_statement)
- [<span data-ttu-id="f2879-207">“using”指令首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-207">'using' directive preferences</span></span>](ide0065.md)
  - [<span data-ttu-id="f2879-208">csharp_using_directive_placement</span><span class="sxs-lookup"><span data-stu-id="f2879-208">csharp_using_directive_placement</span></span>](ide0065.md#csharp_using_directive_placement)
- [<span data-ttu-id="f2879-209">修饰符首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-209">Modifier preferences</span></span>](modifier-preferences.md#c-modifier-preferences)
  - [<span data-ttu-id="f2879-210">csharp_prefer_static_local_function</span><span class="sxs-lookup"><span data-stu-id="f2879-210">csharp_prefer_static_local_function</span></span>](ide0062.md#csharp_prefer_static_local_function)
  - <span data-ttu-id="f2879-211">[使结构字段可写](ide0064.md) - 此规则没有代码样式选项。</span><span class="sxs-lookup"><span data-stu-id="f2879-211">[Make struct fields writable](ide0064.md) - This rule has no code style option.</span></span>

## <a name="visual-basic-style-rules"></a><span data-ttu-id="f2879-212">Visual Basic 样式规则</span><span class="sxs-lookup"><span data-stu-id="f2879-212">Visual Basic style rules</span></span>

<span data-ttu-id="f2879-213">本节中的样式规则仅适用于 Visual Basic 语言。</span><span class="sxs-lookup"><span data-stu-id="f2879-213">The style rules in this section are applicable to Visual Basic language only.</span></span>

- [<span data-ttu-id="f2879-214">模式匹配首选项</span><span class="sxs-lookup"><span data-stu-id="f2879-214">Pattern matching preferences</span></span>](pattern-matching-preferences.md)
  - [<span data-ttu-id="f2879-215">visual_basic_style_prefer_isnot_expression</span><span class="sxs-lookup"><span data-stu-id="f2879-215">visual_basic_style_prefer_isnot_expression</span></span>](ide0084.md#visual_basic_style_prefer_isnot_expression)

## <a name="see-also"></a><span data-ttu-id="f2879-216">请参阅</span><span class="sxs-lookup"><span data-stu-id="f2879-216">See also</span></span>

- [<span data-ttu-id="f2879-217">不必要的代码规则</span><span class="sxs-lookup"><span data-stu-id="f2879-217">Unnecessary code rules</span></span>](unnecessary-code-rules.md)
- [<span data-ttu-id="f2879-218">格式设置规则</span><span class="sxs-lookup"><span data-stu-id="f2879-218">Formatting rules</span></span>](formatting-rules.md)
- [<span data-ttu-id="f2879-219">命名规则</span><span class="sxs-lookup"><span data-stu-id="f2879-219">Naming rules</span></span>](naming-rules.md)
- [<span data-ttu-id="f2879-220">.NET 代码样式规则参考</span><span class="sxs-lookup"><span data-stu-id="f2879-220">.NET code style rules reference</span></span>](index.md)
