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
# <a name="language-rules"></a>语言规则

代码样式语言规则会影响 .NET 编程语言的各种构造（如修饰符和括号）的使用方式。 规则分为以下几类：

- [.NET 样式规则](#net-style-rules)：适用于 C# 和 Visual Basic 的规则。 这些规则的 EditorConfig 选项名称以 `dotnet_style_` 前缀开头。
- [C# 样式规则](#c-style-rules)：仅适用于 C# 语言的规则。 这些规则的 EditorConfig 选项名称以 `csharp_style_` 前缀开头。
- [Visual Basic 样式规则](#visual-basic-style-rules)：仅适用于 Visual Bsic 语言的规则。 这些规则的 EditorConfig 选项名称以 `visual_basic_style_` 前缀开头。

## <a name="option-format"></a>选项格式

可以在 EditorConfig 文件中指定语言规则的选项，格式如下：

`option_name = value`（Visual Studio 2019 版本 16.9 预览版 2 及更高版本）

或

`option_name = value:severity`

- **值**

  对于每个语言规则，可指定一个定义是否或何时以此样式为首选项的值。 许多规则都接受 `true` 值（以此样式为首选项）或 `false` 值（不以此样式为首选项）。 其他规则接受 `when_on_single_line` 或 `never` 等值。

- **严重性**（在 Visual Studio 2019 版本 16.9 预览版 2 及更高版本中可选）

  此规则的第二部分指定规则的[严重性级别](../configuration-options.md#severity-level)。 以这种方式指定时，只有 Visual Studio 之类的开发 IDE 会遵循严重性设置。 在生成过程中不会遵循这一点。

  若要在生成时强制实施代码样式规则，请改为使用分析器的基于规则 ID 的严重性配置语法来设置严重性。 语法采用形式 `dotnet_diagnostic.<rule ID>.severity = <severity>`（例如，`dotnet_diagnostic.IDE0040.severity = silent`）。 有关详细信息，请参阅[严重性级别](../configuration-options.md#severity-level)。

> [!TIP]
>
> 自 Visual Studio 2019 版本 16.3 起，在样式冲突发生之后，可以在[快速操作](/visualstudio/ide/quick-actions)灯泡菜单中配置代码样式规则。 有关详细信息，请参阅[在 Visual Studio 中自动配置代码样式](/visualstudio/ide/editorconfig-language-conventions#automatically-configure-code-styles-in-visual-studio)。

## <a name="net-style-rules"></a>.NET 样式规则

本节中的样式规则均适用于 C# 和 Visual Basic。

- [“this.”和“Me.”限定符](ide0003-ide0009.md)
  - [dotnet_style_qualification_for_field](ide0003-ide0009.md#dotnet_style_qualification_for_field)
  - [dotnet_style_qualification_for_property](ide0003-ide0009.md#dotnet_style_qualification_for_property)
  - [dotnet_style_qualification_for_method](ide0003-ide0009.md#dotnet_style_qualification_for_method)
  - [dotnet_style_qualification_for_event](ide0003-ide0009.md#dotnet_style_qualification_for_event)
- [语言关键字，而非类型引用的框架类型名称](ide0049.md)
  - [dotnet_style_predefined_type_for_locals_parameters_members](ide0049.md#dotnet_style_predefined_type_for_locals_parameters_members)
  - [dotnet_style_predefined_type_for_member_access](ide0049.md#dotnet_style_predefined_type_for_member_access)
- [修饰符首选项](modifier-preferences.md#net-modifier-preferences)
  - [dotnet_style_require_accessibility_modifiers](ide0040.md#dotnet_style_require_accessibility_modifiers)
  - [csharp_preferred_modifier_order](ide0036.md#csharp_preferred_modifier_order)
  - [visual_basic_preferred_modifier_order](ide0036.md#visual_basic_preferred_modifier_order)
  - [dotnet_style_readonly_field](ide0044.md#dotnet_style_readonly_field)
- [括号首选项](ide0047-ide0048.md)
  - [dotnet_style_parentheses_in_arithmetic_binary_operators](ide0047-ide0048.md#dotnet_style_parentheses_in_arithmetic_binary_operators)
  - [dotnet_style_parentheses_in_relational_binary_operators](ide0047-ide0048.md#dotnet_style_parentheses_in_relational_binary_operators)
  - [dotnet_style_parentheses_in_other_binary_operators](ide0047-ide0048.md#dotnet_style_parentheses_in_other_binary_operators)
  - [dotnet_style_parentheses_in_other_operators](ide0047-ide0048.md#dotnet_style_parentheses_in_other_operators)
- [表达式级首选项](expression-level-preferences.md#net-expression-level-preferences)
  - [dotnet_style_object_initializer](ide0017.md#dotnet_style_object_initializer)
  - [dotnet_style_collection_initializer](ide0028.md#dotnet_style_collection_initializer)
  - [dotnet_style_explicit_tuple_names](ide0033.md#dotnet_style_explicit_tuple_names)
  - [dotnet_style_prefer_inferred_tuple_names](ide0037.md#dotnet_style_prefer_inferred_tuple_names)
  - [dotnet_style_prefer_inferred_anonymous_type_member_names](ide0037.md#dotnet_style_prefer_inferred_anonymous_type_member_names)
  - [dotnet_style_prefer_auto_properties](ide0032.md#dotnet_style_prefer_auto_properties)
  - [dotnet_style_prefer_conditional_expression_over_assignment](ide0045.md#dotnet_style_prefer_conditional_expression_over_assignment)
  - [dotnet_style_prefer_conditional_expression_over_return](ide0046.md#dotnet_style_prefer_conditional_expression_over_return)
  - [dotnet_style_prefer_compound_assignment](ide0054-ide0074.md#dotnet_style_prefer_compound_assignment)
  - [dotnet_style_prefer_simplified_interpolation](ide0071.md#dotnet_style_prefer_simplified_interpolation)
  - [dotnet_style_prefer_simplified_boolean_expressions](ide0075.md#dotnet_style_prefer_simplified_boolean_expressions)
  - [向 switch 语句添加缺失的事例](ide0010.md) - 此规则没有代码样式选项。
  - [将匿名类型转换为元组](ide0050.md) - 此规则没有代码样式选项。
  - [使用“System.HashCode.Combine”](ide0070.md)- 此规则没有代码样式选项。
  - [将“typeof”转换为“nameof”](ide0082.md)- 此规则没有代码样式选项。
- [Null 检查首选项](null-checking-preferences.md#net-null-checking-preferences)
  - [dotnet_style_coalesce_expression](ide0029-ide0030.md#dotnet_style_coalesce_expression)
  - [dotnet_style_null_propagation](ide0031.md#dotnet_style_null_propagation)
  - [dotnet_style_prefer_is_null_check_over_reference_equality_method](ide0041.md#dotnet_style_prefer_is_null_check_over_reference_equality_method)
- [文件头首选项](ide0073.md)
  - [file_header_template](ide0073.md#file_header_template)

## <a name="c-style-rules"></a>C# 样式规则

本节中的样式规则仅适用于 C# 语言。

- [“var”首选项](ide0007-ide0008.md)
  - [csharp_style_var_for_built_in_types](ide0007-ide0008.md#csharp_style_var_for_built_in_types)
  - [csharp_style_var_when_type_is_apparent](ide0007-ide0008.md#csharp_style_var_when_type_is_apparent)
  - [csharp_style_var_elsewhere](ide0007-ide0008.md#csharp_style_var_elsewhere)
- [Expression-Bodied 成员](expression-bodied-members.md)
  - [csharp_style_expression_bodied_methods](ide0022.md#csharp_style_expression_bodied_methods)
  - [csharp_style_expression_bodied_constructors](ide0021.md#csharp_style_expression_bodied_constructors)
  - [csharp_style_expression_bodied_operators](ide0023-ide0024.md#csharp_style_expression_bodied_operators)
  - [csharp_style_expression_bodied_properties](ide0025.md#csharp_style_expression_bodied_properties)
  - [csharp_style_expression_bodied_indexers](ide0026.md#csharp_style_expression_bodied_indexers)
  - [csharp_style_expression_bodied_accessors](ide0027.md#csharp_style_expression_bodied_accessors)
  - [csharp_style_expression_bodied_lambdas](ide0053.md#csharp_style_expression_bodied_lambdas)
  - [csharp_style_expression_bodied_local_functions](ide0061.md#csharp_style_expression_bodied_local_functions)
- [模式匹配首选项](pattern-matching-preferences.md)
  - [csharp_style_pattern_matching_over_is_with_cast_check](ide0020-ide0038.md#csharp_style_pattern_matching_over_is_with_cast_check)
  - [csharp_style_pattern_matching_over_as_with_null_check](ide0019.md#csharp_style_pattern_matching_over_as_with_null_check)
  - [csharp_style_prefer_switch_expression](ide0066.md#csharp_style_prefer_switch_expression)
  - [csharp_style_prefer_pattern_matching](ide0078.md#csharp_style_prefer_pattern_matching)
  - [csharp_style_prefer_not_pattern](ide0083.md#csharp_style_prefer_not_pattern)
- [表达式级首选项](expression-level-preferences.md#c-expression-level-preferences)
  - [csharp_style_inlined_variable_declaration](ide0018.md#csharp_style_inlined_variable_declaration)
  - [csharp_prefer_simple_default_expression](ide0034.md#csharp_prefer_simple_default_expression)
  - [csharp_style_pattern_local_over_anonymous_function](ide0039.md#csharp_style_pattern_local_over_anonymous_function)
  - [csharp_style_deconstructed_variable_declaration](ide0042.md#csharp_style_deconstructed_variable_declaration)
  - [csharp_style_prefer_index_operator](ide0056.md#csharp_style_prefer_index_operator)
  - [csharp_style_prefer_range_operator](ide0057.md#csharp_style_prefer_range_operator)
  - [csharp_style_implicit_object_creation_when_type_is_apparent](ide0090.md#csharp_style_implicit_object_creation_when_type_is_apparent)
  - [向 switch 表达式添加缺失的事例](ide0072.md) - 此规则没有代码样式选项。
- [“NULL”检查首选项](null-checking-preferences.md#c-null-checking-preferences)
  - [csharp_style_throw_expression](ide0016.md#csharp_style_throw_expression)
  - [csharp_style_conditional_delegate_call](ide1005.md#csharp_style_conditional_delegate_call)
- [代码块首选项](code-block-preferences.md)
  - [csharp_prefer_braces](ide0011.md#csharp_prefer_braces)
  - [csharp_prefer_simple_using_statement](ide0063.md#csharp_prefer_simple_using_statement)
- [“using”指令首选项](ide0065.md)
  - [csharp_using_directive_placement](ide0065.md#csharp_using_directive_placement)
- [修饰符首选项](modifier-preferences.md#c-modifier-preferences)
  - [csharp_prefer_static_local_function](ide0062.md#csharp_prefer_static_local_function)
  - [使结构字段可写](ide0064.md) - 此规则没有代码样式选项。

## <a name="visual-basic-style-rules"></a>Visual Basic 样式规则

本节中的样式规则仅适用于 Visual Basic 语言。

- [模式匹配首选项](pattern-matching-preferences.md)
  - [visual_basic_style_prefer_isnot_expression](ide0084.md#visual_basic_style_prefer_isnot_expression)

## <a name="see-also"></a>请参阅

- [不必要的代码规则](unnecessary-code-rules.md)
- [格式设置规则](formatting-rules.md)
- [命名规则](naming-rules.md)
- [.NET 代码样式规则参考](index.md)
