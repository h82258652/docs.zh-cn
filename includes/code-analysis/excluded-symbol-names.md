---
ms.openlocfilehash: 83644b9205d650da68c910095dd1d22a14940c44
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "97366847"
---
### <a name="exclude-specific-symbols"></a>排除特定符号

可以从分析中排除特定符号，如类型和方法。 例如，若要指定规则不应针对名为 `MyType` 的类型中的任何代码运行，请将以下键值对添加到项目中的 .editorconfig 文件：

```ini
dotnet_code_quality.CAXXXX.excluded_symbol_names = MyType
```

选项值中允许的符号名称格式（用 `|` 分隔）：

- 仅符号名称（包括具有相应名称的所有符号，不考虑包含的类型或命名空间）。
- 完全限定的名称，使用符号的[文档 ID 格式](../../docs/csharp/programming-guide/xmldoc/processing-the-xml-file.md#id-strings)。 每个符号名称都需要带有一个符号类型前缀，例如表示方法的 `M:`、表示类型的 `T:`，以及表示命名空间的 `N:`。
- `.ctor` 表示构造函数，`.cctor` 表示静态构造函数。

示例：

| 选项值 | 总结 |
| --- | --- |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = MyType` | 匹配名为 `MyType` 的所有符号。 |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = MyType1|MyType2` | 匹配名为 `MyType1` 或 `MyType2` 的所有符号。 |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = M:NS.MyType.MyMethod(ParamType)` | 匹配带有指定的完全限定签名的特定方法 `MyMethod`。 |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = M:NS1.MyType1.MyMethod1(ParamType)|M:NS2.MyType2.MyMethod2(ParamType)` | 匹配带有各自的完全限定签名的特定方法 `MyMethod1` 和 `MyMethod2`。 |
