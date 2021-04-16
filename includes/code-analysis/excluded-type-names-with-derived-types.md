---
ms.openlocfilehash: 4125df1d64fe7f3f2eb1eb4a821ed46c8270c95f
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "97531897"
---
### <a name="exclude-specific-types-and-their-derived-types"></a>排除特定类型及其派生类型

可以从分析中排除特定类型及其派生类型。 例如，若要指定规则不应针对名为 `MyType` 的类型及其派生类型中的任何代码运行，请将以下键值对添加到项目中的 .editorconfig 文件：

```ini
dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = MyType
```

选项值中允许的符号名称格式（用 `|` 分隔）：

- 仅类型名称（包括具有相应名称的所有类型，不考虑包含的类型或命名空间）。
- 完全限定的名称，使用符号的[文档 ID 格式](../../docs/csharp/programming-guide/xmldoc/processing-the-xml-file.md#id-strings)，前缀为 `T:`（可选）。

示例：

| 选项值 | 总结 |
| --- | --- |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = MyType` | 匹配名为 `MyType` 的所有类型及其所有派生类型。 |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = MyType1|MyType2` | 匹配名为 `MyType1` 或 `MyType2` 的所有类型及其所有派生类型。 |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = M:NS.MyType` | 匹配带有给定的完全限定名称的特定类型 `MyType` 及其所有派生类型。 |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = M:NS1.MyType1|M:NS2.MyType2` | 匹配带有各自的完全限定名称的特定类型 `MyType1` 和 `MyType2` 及其所有派生类型。 |
