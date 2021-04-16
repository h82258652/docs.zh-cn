---
ms.openlocfilehash: 4125df1d64fe7f3f2eb1eb4a821ed46c8270c95f
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "97531897"
---
### <a name="exclude-specific-types-and-their-derived-types"></a><span data-ttu-id="fdf7c-101">排除特定类型及其派生类型</span><span class="sxs-lookup"><span data-stu-id="fdf7c-101">Exclude specific types and their derived types</span></span>

<span data-ttu-id="fdf7c-102">可以从分析中排除特定类型及其派生类型。</span><span class="sxs-lookup"><span data-stu-id="fdf7c-102">You can exclude specific types and their derived types from analysis.</span></span> <span data-ttu-id="fdf7c-103">例如，若要指定规则不应针对名为 `MyType` 的类型及其派生类型中的任何代码运行，请将以下键值对添加到项目中的 .editorconfig 文件：</span><span class="sxs-lookup"><span data-stu-id="fdf7c-103">For example, to specify that the rule should not run on any methods within types named `MyType` and their derived types, add the following key-value pair to an *.editorconfig* file in your project:</span></span>

```ini
dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = MyType
```

<span data-ttu-id="fdf7c-104">选项值中允许的符号名称格式（用 `|` 分隔）：</span><span class="sxs-lookup"><span data-stu-id="fdf7c-104">Allowed symbol name formats in the option value (separated by `|`):</span></span>

- <span data-ttu-id="fdf7c-105">仅类型名称（包括具有相应名称的所有类型，不考虑包含的类型或命名空间）。</span><span class="sxs-lookup"><span data-stu-id="fdf7c-105">Type name only (includes all types with the name, regardless of the containing type or namespace).</span></span>
- <span data-ttu-id="fdf7c-106">完全限定的名称，使用符号的[文档 ID 格式](../../docs/csharp/programming-guide/xmldoc/processing-the-xml-file.md#id-strings)，前缀为 `T:`（可选）。</span><span class="sxs-lookup"><span data-stu-id="fdf7c-106">Fully qualified names in the symbol's [documentation ID format](../../docs/csharp/programming-guide/xmldoc/processing-the-xml-file.md#id-strings), with an optional `T:` prefix.</span></span>

<span data-ttu-id="fdf7c-107">示例：</span><span class="sxs-lookup"><span data-stu-id="fdf7c-107">Examples:</span></span>

| <span data-ttu-id="fdf7c-108">选项值</span><span class="sxs-lookup"><span data-stu-id="fdf7c-108">Option Value</span></span> | <span data-ttu-id="fdf7c-109">总结</span><span class="sxs-lookup"><span data-stu-id="fdf7c-109">Summary</span></span> |
| --- | --- |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = MyType` | <span data-ttu-id="fdf7c-110">匹配名为 `MyType` 的所有类型及其所有派生类型。</span><span class="sxs-lookup"><span data-stu-id="fdf7c-110">Matches all types named `MyType` and all of their derived types.</span></span> |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = MyType1|MyType2` | <span data-ttu-id="fdf7c-111">匹配名为 `MyType1` 或 `MyType2` 的所有类型及其所有派生类型。</span><span class="sxs-lookup"><span data-stu-id="fdf7c-111">Matches all types named either `MyType1` or `MyType2` and all of their derived types.</span></span> |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = M:NS.MyType` | <span data-ttu-id="fdf7c-112">匹配带有给定的完全限定名称的特定类型 `MyType` 及其所有派生类型。</span><span class="sxs-lookup"><span data-stu-id="fdf7c-112">Matches specific type `MyType` with given fully qualified name and all of its derived types.</span></span> |
|`dotnet_code_quality.CAXXXX.excluded_type_names_with_derived_types = M:NS1.MyType1|M:NS2.MyType2` | <span data-ttu-id="fdf7c-113">匹配带有各自的完全限定名称的特定类型 `MyType1` 和 `MyType2` 及其所有派生类型。</span><span class="sxs-lookup"><span data-stu-id="fdf7c-113">Matches specific types `MyType1` and `MyType2` with the respective fully qualified names, and all of their derived types.</span></span> |
