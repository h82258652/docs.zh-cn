---
title: 代码质量规则配置选项
description: 了解如何为代码质量规则指定更多配置选项。
ms.date: 09/24/2020
no-loc:
- EditorConfig
ms.openlocfilehash: 982dc3bba7042083a22123b0ba4af31c1aeba889
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740990"
---
# <a name="code-quality-rule-configuration-options"></a>代码质量规则配置选项

除了[配置严重性](configuration-options.md#severity-level)外，代码质量规则还有其他配置选项。 例如，可以将每个代码质量分析器配置为仅应用于代码库的特定部分。 通过向指定规则严重性和常规编辑器首选项的同一个 [EditorConfig](https://editorconfig.org) 文件添加键值对，可指定这些选项。

## <a name="option-scopes"></a>选项作用域

每个优化选项都可以针对所有规则、某个规则类别（例如“安全性”或“设计”）或某个特定规则进行配置。

### <a name="all-rules"></a>┮Τ砏玥

若要为所有规则配置选项，请使用下面的语法：

|语法|示例|
|-|-|
| dotnet_code_quality.OptionName = OptionValue | `dotnet_code_quality.api_surface = public` |

### <a name="category-of-rules"></a>规则类别

若要为某个[规则类别](categories.md)配置选项，请使用下面的语法：

|语法|示例|
|-|-|
| dotnet_code_quality.RuleCategory.OptionName = OptionValue | `dotnet_code_quality.Naming.api_surface = public` |

### <a name="specific-rule"></a>特定规则

若要为某个特定规则配置选项，请使用下面的语法：

|语法|示例|
|-|-|
| dotnet_code_quality.RuleId.OptionName = OptionValue | `dotnet_code_quality.CA1040.api_surface = public` |

## <a name="options"></a>选项

本节列出了一部分可用选项。 若要查看可用选项的完整列表，请参阅[分析器配置](https://github.com/dotnet/roslyn-analyzers/blob/main/docs/Analyzer%20Configuration.md)。

### <a name="api_surface"></a>api_surface

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 要分析 API 图面的哪个部分 | `public`<br/>`internal` 或 `friend`<br/>`private`<br/>`all`<br/><br/>用逗号 (,) 分隔多个值 | `public` | [CA1000](quality-rules/ca1000.md) [CA1003](quality-rules/ca1003.md) [CA1008](quality-rules/ca1008.md) [CA1010](quality-rules/ca1010.md)<br/>[CA1012](quality-rules/ca1012.md) [CA1024](quality-rules/ca1024.md) [CA1027](quality-rules/ca1027.md) [CA1028](quality-rules/ca1028.md)<br/>[CA1030](quality-rules/ca1030.md) [CA1036](quality-rules/ca1036.md) [CA1040](quality-rules/ca1040.md) [CA1041](quality-rules/ca1041.md)<br/>[CA1043](quality-rules/ca1043.md) [CA1044](quality-rules/ca1044.md) [CA1051](quality-rules/ca1051.md) [CA1052](quality-rules/ca1052.md)<br/>[CA1054](quality-rules/ca1054.md) [CA1055](quality-rules/ca1055.md) [CA1056](quality-rules/ca1056.md) [CA1058](quality-rules/ca1058.md)<br/>[CA1063](quality-rules/ca1063.md) [CA1708](quality-rules/ca1708.md) [CA1710](quality-rules/ca1710.md) [CA1711](quality-rules/ca1711.md)<br/>[CA1714](quality-rules/ca1714.md) [CA1715](quality-rules/ca1715.md) [CA1716](quality-rules/ca1716.md) [CA1717](quality-rules/ca1717.md)<br/>[CA1720](quality-rules/ca1720.md) [CA1721](quality-rules/ca1721.md) [CA1725](quality-rules/ca1725.md) [CA1801](quality-rules/ca1801.md)<br/>[CA1802](quality-rules/ca1802.md) [CA1815](quality-rules/ca1815.md) [CA1819](quality-rules/ca1819.md) [CA2217](quality-rules/ca2217.md)<br/>[CA2225](quality-rules/ca2225.md) [CA2226](quality-rules/ca2226.md) [CA2231](quality-rules/ca2231.md) [CA2234](quality-rules/ca2234.md)<br/>|

### <a name="exclude_async_void_methods"></a>exclude_async_void_methods

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 是否忽略不返回值的异步方法 | `true`<br/>`false` | `false` | [CA2007](quality-rules/ca2007.md) |

> [!NOTE]
> 早期版本中将此选项命名为 `skip_async_void_methods`。

### <a name="exclude_single_letter_type_parameters"></a>exclude_single_letter_type_parameters

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 是否从规则中排除单字符的[类型参数](../../csharp/programming-guide/generics/generic-type-parameters.md)，例如，`Collection<S>` 中的 `S` | `true`<br/>`false` | `false` | [CA1715](quality-rules/ca1715.md) |

> [!NOTE]
> 早期版本中将此选项命名为 `allow_single_letter_type_parameters`。

### <a name="output_kind"></a>output_kind

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 指定应分析项目中生成此程序集类型的代码 | <xref:Microsoft.CodeAnalysis.OutputKind> 枚举的一个或多个字段<br/><br/>用逗号 (,) 分隔多个值 | 所有输出种类 | [CA2007](quality-rules/ca2007.md) |

### <a name="required_modifiers"></a>required_modifiers

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 指定应分析的 API 所需的修饰符 | 以下允许的修饰符表中的一个或多个值<br/><br/>用逗号 (,) 分隔多个值 | 取决于每个规则 | [CA1802](quality-rules/ca1802.md) |

| 允许的修饰符 | 总结 |
| --- | --- |
| `none` | 无修饰符要求 |
| `static` 或 `Shared` | 必须声明为 `static`（在 Visual Basic 中为 `Shared`） |
| `const` | 必须声明为 `const` |
| `readonly` | 必须声明为 `readonly` |
| `abstract` | 必须声明为 `abstract` |
| `virtual` | 必须声明为 `virtual` |
| `override` | 必须声明为 `override` |
| `sealed` | 必须声明为 `sealed` |
| `extern` | 必须声明为 `extern` |
| `async` | 必须声明为 `async` |

### <a name="exclude_extension_method_this_parameter"></a>exclude_extension_method_this_parameter

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 是否跳过对扩展方法的 `this` 参数的分析 | `true`<br/>`false` | `false` | [CA1062](quality-rules/ca1062.md) |

### <a name="null_check_validation_methods"></a>null_check_validation_methods

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| null 检查验证方法的名称，这些方法用于确定传递给方法的参数不是 null | 允许的方法名称格式（以 `|` 分隔）：<br/> - 仅方法名称（包括具有相应名称的所有方法，不考虑包含的类型或命名空间）<br/> - 完全限定的名称，使用符号的[文档 ID 格式](/dotnet/csharp/language-reference/language-specification/documentation-comments#id-string-format)，前缀为 `M:`（可选） | 无 | [CA1062](quality-rules/ca1062.md) |

### <a name="additional_string_formatting_methods"></a>additional_string_formatting_methods

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 其他字符串格式设置方法的名称 | 允许的方法名称格式（以 `|` 分隔）：<br/> - 仅方法名称（包括具有相应名称的所有方法，不考虑包含的类型或命名空间）<br/> - 完全限定的名称，使用符号的[文档 ID 格式](/dotnet/csharp/language-reference/language-specification/documentation-comments#id-string-format)，前缀为 `M:`（可选） | 无 | [CA2241](quality-rules/ca2241.md) |

### <a name="excluded_type_names_with_derived_types"></a>excluded_type_names_with_derived_types

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 类型的名称，用于将类型及其所有派生类型从分析范围内排除 | 允许的符号名称格式（以 `|` 分隔）：<br/> - 仅类型名称（包括具有相应名称的所有类型，不考虑包含的类型或命名空间）<br/> - 完全限定的名称，使用符号的[文档 ID 格式](/dotnet/csharp/language-reference/language-specification/documentation-comments#id-string-format)，前缀为 `T:`（可选） | 无 | [CA1303](quality-rules/ca1303.md) |

### <a name="excluded_symbol_names"></a>excluded_symbol_names

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 从分析范围排除的符号的名称 | 允许的符号名称格式（以 `|` 分隔）：<br/> - 仅符号名称（包括具有相应名称的所有符号，不考虑包含的类型或命名空间）<br/> - 完全限定的名称，使用符号的[文档 ID 格式](/dotnet/csharp/language-reference/language-specification/documentation-comments#id-string-format) 每个符号名称都需要带有一个符号类型前缀，例如表示方法的 `M:` 前缀、表示类型的 `T:` 前缀，以及表示命名空间的 `N:` 前缀。<br/> - `.ctor` 表示构造函数，`.cctor` 表示静态构造函数 | 无 | [CA1062](quality-rules/ca1062.md) [CA1303](quality-rules/ca1303.md) [CA2000](quality-rules/ca2000.md) [CA2100](quality-rules/ca2100.md) [CA2301](quality-rules/ca2301.md) [CA2302](quality-rules/ca2302.md)<br/>[CA2311](quality-rules/ca2311.md) [CA2312](quality-rules/ca2312.md) [CA2321](quality-rules/ca2321.md) [CA2322](quality-rules/ca2322.md) [CA2327](quality-rules/ca2327.md) [CA2328](quality-rules/ca2328.md)<br/>[CA2329](quality-rules/ca2329.md) [CA2330](quality-rules/ca2330.md) [CA3001](quality-rules/ca3001.md) [CA3002](quality-rules/ca3002.md) [CA3003](quality-rules/ca3003.md) [CA3004](quality-rules/ca3004.md)<br/>[CA3005](quality-rules/ca3005.md) [CA3006](quality-rules/ca3006.md) [CA3007](quality-rules/ca3007.md) [CA3008](quality-rules/ca3008.md) [CA3009](quality-rules/ca3009.md) [CA3010](quality-rules/ca3010.md)<br/>[CA3011](quality-rules/ca3011.md) [CA3012](quality-rules/ca3012.md) [CA5361](quality-rules/ca5361.md) CA5376 CA5377 [CA5378](quality-rules/ca5378.md)<br/>[CA5380](quality-rules/ca5380.md) [CA5381](quality-rules/ca5381.md) CA5382 CA5383 CA5384 CA5387<br/>CA5388 [CA5389](quality-rules/ca5389.md) CA5390 |

### <a name="disallowed_symbol_names"></a>disallowed_symbol_names

| 描述 | 允许的值 | 默认值 | 可配置的规则 |
| - | - | - | - |
| 不允许出现在分析上下文中的符号的名称 | 允许的符号名称格式（以 `|` 分隔）：<br/> - 仅符号名称（包括具有相应名称的所有符号，不考虑包含的类型或命名空间）<br/> - 完全限定的名称，使用符号的[文档 ID 格式](/dotnet/csharp/language-reference/language-specification/documentation-comments#id-string-format) 每个符号名称都需要带有一个符号类型前缀，例如表示方法的 `M:` 前缀、表示类型的 `T:` 前缀，以及表示命名空间的 `N:` 前缀。<br/> - `.ctor` 表示构造函数，`.cctor` 表示静态构造函数 | 无 | [CA1031](quality-rules/ca1031.md) |
