---
ms.openlocfilehash: 83644b9205d650da68c910095dd1d22a14940c44
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "97366847"
---
### <a name="exclude-specific-symbols"></a><span data-ttu-id="63c94-101">排除特定符号</span><span class="sxs-lookup"><span data-stu-id="63c94-101">Exclude specific symbols</span></span>

<span data-ttu-id="63c94-102">可以从分析中排除特定符号，如类型和方法。</span><span class="sxs-lookup"><span data-stu-id="63c94-102">You can exclude specific symbols, such as types and methods, from analysis.</span></span> <span data-ttu-id="63c94-103">例如，若要指定规则不应针对名为 `MyType` 的类型中的任何代码运行，请将以下键值对添加到项目中的 .editorconfig 文件：</span><span class="sxs-lookup"><span data-stu-id="63c94-103">For example, to specify that the rule should not run on any code within types named `MyType`, add the following key-value pair to an *.editorconfig* file in your project:</span></span>

```ini
dotnet_code_quality.CAXXXX.excluded_symbol_names = MyType
```

<span data-ttu-id="63c94-104">选项值中允许的符号名称格式（用 `|` 分隔）：</span><span class="sxs-lookup"><span data-stu-id="63c94-104">Allowed symbol name formats in the option value (separated by `|`):</span></span>

- <span data-ttu-id="63c94-105">仅符号名称（包括具有相应名称的所有符号，不考虑包含的类型或命名空间）。</span><span class="sxs-lookup"><span data-stu-id="63c94-105">Symbol name only (includes all symbols with the name, regardless of the containing type or namespace).</span></span>
- <span data-ttu-id="63c94-106">完全限定的名称，使用符号的[文档 ID 格式](../../docs/csharp/programming-guide/xmldoc/processing-the-xml-file.md#id-strings)。</span><span class="sxs-lookup"><span data-stu-id="63c94-106">Fully qualified names in the symbol's [documentation ID format](../../docs/csharp/programming-guide/xmldoc/processing-the-xml-file.md#id-strings).</span></span> <span data-ttu-id="63c94-107">每个符号名称都需要带有一个符号类型前缀，例如表示方法的 `M:`、表示类型的 `T:`，以及表示命名空间的 `N:`。</span><span class="sxs-lookup"><span data-stu-id="63c94-107">Each symbol name requires a symbol-kind prefix, such as `M:` for methods, `T:` for types, and `N:` for namespaces.</span></span>
- <span data-ttu-id="63c94-108">`.ctor` 表示构造函数，`.cctor` 表示静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="63c94-108">`.ctor` for constructors and `.cctor` for static constructors.</span></span>

<span data-ttu-id="63c94-109">示例：</span><span class="sxs-lookup"><span data-stu-id="63c94-109">Examples:</span></span>

| <span data-ttu-id="63c94-110">选项值</span><span class="sxs-lookup"><span data-stu-id="63c94-110">Option Value</span></span> | <span data-ttu-id="63c94-111">总结</span><span class="sxs-lookup"><span data-stu-id="63c94-111">Summary</span></span> |
| --- | --- |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = MyType` | <span data-ttu-id="63c94-112">匹配名为 `MyType` 的所有符号。</span><span class="sxs-lookup"><span data-stu-id="63c94-112">Matches all symbols named `MyType`.</span></span> |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = MyType1|MyType2` | <span data-ttu-id="63c94-113">匹配名为 `MyType1` 或 `MyType2` 的所有符号。</span><span class="sxs-lookup"><span data-stu-id="63c94-113">Matches all symbols named either `MyType1` or `MyType2`.</span></span> |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = M:NS.MyType.MyMethod(ParamType)` | <span data-ttu-id="63c94-114">匹配带有指定的完全限定签名的特定方法 `MyMethod`。</span><span class="sxs-lookup"><span data-stu-id="63c94-114">Matches specific method `MyMethod` with the specified fully qualified signature.</span></span> |
|`dotnet_code_quality.CAXXXX.excluded_symbol_names = M:NS1.MyType1.MyMethod1(ParamType)|M:NS2.MyType2.MyMethod2(ParamType)` | <span data-ttu-id="63c94-115">匹配带有各自的完全限定签名的特定方法 `MyMethod1` 和 `MyMethod2`。</span><span class="sxs-lookup"><span data-stu-id="63c94-115">Matches specific methods `MyMethod1` and `MyMethod2` with the respective fully qualified signatures.</span></span> |
