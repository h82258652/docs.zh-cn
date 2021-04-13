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
# <a name="c-compiler-options-for-language-feature-rules"></a><span data-ttu-id="520d4-104">用于语言功能规则的 C# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="520d4-104">C# Compiler Options for language feature rules</span></span>

<span data-ttu-id="520d4-105">以下选项控制编译器如何解释语言功能。</span><span class="sxs-lookup"><span data-stu-id="520d4-105">The following options control how the compiler interprets language features.</span></span> <span data-ttu-id="520d4-106">新的 MSBuild 语法以粗体显示。</span><span class="sxs-lookup"><span data-stu-id="520d4-106">The new MSBuild syntax is shown in **Bold**.</span></span> <span data-ttu-id="520d4-107">旧的 csc.exe 语法以 `code style` 显示。</span><span class="sxs-lookup"><span data-stu-id="520d4-107">The older *csc.exe* syntax is shown in `code style`.</span></span>

- <span data-ttu-id="520d4-108">**CheckForOverflowUnderflow** / `-checked`：生成溢出检查。</span><span class="sxs-lookup"><span data-stu-id="520d4-108">**CheckForOverflowUnderflow** / `-checked`: Generate overflow checks.</span></span>
- <span data-ttu-id="520d4-109">**AllowUnsafeBlocks** / `-unsafe`：允许“不安全”代码。</span><span class="sxs-lookup"><span data-stu-id="520d4-109">**AllowUnsafeBlocks** / `-unsafe`: Allow 'unsafe' code.</span></span>
- <span data-ttu-id="520d4-110">**DefineConstants** / `-define`：定义条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="520d4-110">**DefineConstants** / `-define`: Define conditional compilation symbol(s).</span></span>
- <span data-ttu-id="520d4-111">**LangVersion** / `-langversion`：指定语言版本，如 `default`（最新主版本）或 `latest`（最新版本，包括次要版本）。</span><span class="sxs-lookup"><span data-stu-id="520d4-111">**LangVersion** / `-langversion`: Specify language version such as `default` (latest major version), or `latest` (latest version, including minor versions).</span></span>
- <span data-ttu-id="520d4-112">**Nullable** / `-nullable`：启用可为空上下文或可为空警告。</span><span class="sxs-lookup"><span data-stu-id="520d4-112">**Nullable** / `-nullable`: Enable nullable context, or nullable warnings.</span></span>

## <a name="checkforoverflowunderflow"></a><span data-ttu-id="520d4-113">CheckForOverflowUnderflow</span><span class="sxs-lookup"><span data-stu-id="520d4-113">CheckForOverflowUnderflow</span></span>

<span data-ttu-id="520d4-114">CheckForOverflowUnderflow 选项指定产生的值超出数据类型范围的整数算法语句是否将导致运行时异常。</span><span class="sxs-lookup"><span data-stu-id="520d4-114">The **CheckForOverflowUnderflow** option specifies whether an integer arithmetic statement that results in a value that is outside the range of the data type causes a run-time exception.</span></span>  

```xml
<CheckForOverflowUnderflow>true</CheckForOverflowUnderflow>
```

<span data-ttu-id="520d4-115">`checked` 或 `unchecked` 关键字范围内的整数算法语句不受 CheckForOverflowUnderflow 选项的影响。</span><span class="sxs-lookup"><span data-stu-id="520d4-115">An integer arithmetic statement that is in the scope of a `checked` or `unchecked` keyword isn't subject to the effect of the **CheckForOverflowUnderflow** option.</span></span> <span data-ttu-id="520d4-116">如果不在 `checked` 或 `unchecked` 关键字范围内的整数算法语句产生的值超出数据类型范围，并且 CheckForOverflowUnderflow 为 `true`，则该语句将在运行时导致异常。</span><span class="sxs-lookup"><span data-stu-id="520d4-116">If an integer arithmetic statement that isn't in the scope of a `checked` or `unchecked` keyword results in a value outside the range of the data type, and **CheckForOverflowUnderflow** is `true`, that statement causes an exception at run time.</span></span> <span data-ttu-id="520d4-117">如果 CheckForOverflowUnderflow 为 `false`，则该语句在运行时不会导致异常。</span><span class="sxs-lookup"><span data-stu-id="520d4-117">If **CheckForOverflowUnderflow** is `false`, that statement doesn't cause an exception at run time.</span></span> <span data-ttu-id="520d4-118">此选项的默认值为“-checked-”`false`；溢出检查已禁用。</span><span class="sxs-lookup"><span data-stu-id="520d4-118">The default value for this option is `false`; overflow checking is disabled.</span></span>

## <a name="allowunsafeblocks"></a><span data-ttu-id="520d4-119">AllowUnsafeBlocks</span><span class="sxs-lookup"><span data-stu-id="520d4-119">AllowUnsafeBlocks</span></span>

<span data-ttu-id="520d4-120">AllowUnsafeBlocks 编译器选项允许使用 [unsafe](../keywords/unsafe.md) 关键字的代码进行编译。</span><span class="sxs-lookup"><span data-stu-id="520d4-120">The **AllowUnsafeBlocks** compiler option allows code that uses the [unsafe](../keywords/unsafe.md) keyword to compile.</span></span> <span data-ttu-id="520d4-121">此选项的默认值为 `false`，表示不允许不安全代码。</span><span class="sxs-lookup"><span data-stu-id="520d4-121">The default value for this option is `false`, meaning unsafe code is not allowed.</span></span>

```xml
<AllowUnsafeBlocks>true</AllowUnsafeBlocks>
```

<span data-ttu-id="520d4-122">有关不安全代码的详细信息，请参阅[不安全代码和指针](../unsafe-code.md)。</span><span class="sxs-lookup"><span data-stu-id="520d4-122">For more information about unsafe code, see [Unsafe Code and Pointers](../unsafe-code.md).</span></span>

## <a name="defineconstants"></a><span data-ttu-id="520d4-123">DefineConstants</span><span class="sxs-lookup"><span data-stu-id="520d4-123">DefineConstants</span></span>

<span data-ttu-id="520d4-124">DefineConstants 选项将定义程序中所有源代码文件的符号。</span><span class="sxs-lookup"><span data-stu-id="520d4-124">The **DefineConstants** option defines symbols in all source code files of your program.</span></span>

```xml
<DefineConstants>name;name2</DefineConstants>
```

<span data-ttu-id="520d4-125">此选项指定要定义的一个或多个符号的名称。</span><span class="sxs-lookup"><span data-stu-id="520d4-125">This option specifies the names of one or more symbols that you want to define.</span></span> <span data-ttu-id="520d4-126">DefineConstants 选项具有与 [#define](../preprocessor-directives.md#defining-symbols) 预处理器指令相同的效果，只不过编译器选项对项目中的所有文件都有效。</span><span class="sxs-lookup"><span data-stu-id="520d4-126">The **DefineConstants** option has the same effect as the [#define](../preprocessor-directives.md#defining-symbols) preprocessor directive except that the compiler option is in effect for all files in the project.</span></span> <span data-ttu-id="520d4-127">符号在源文件中保持已定义状态，直到源文件中的 [#undef](../preprocessor-directives.md#defining-symbols) 指令删除该定义。</span><span class="sxs-lookup"><span data-stu-id="520d4-127">A symbol remains defined in a source file until an [#undef](../preprocessor-directives.md#defining-symbols) directive in the source file removes the definition.</span></span> <span data-ttu-id="520d4-128">当你使用 `-define` 选项时，一个文件中的 `#undef` 指令不影响项目中的其他源代码文件。</span><span class="sxs-lookup"><span data-stu-id="520d4-128">When you use the `-define` option, an `#undef` directive in one file has no effect on other source code files in the project.</span></span> <span data-ttu-id="520d4-129">可以将由此选项创建的符号同 [#if](../preprocessor-directives.md#conditional-compilation)、[#else](../preprocessor-directives.md)、[#elif](../preprocessor-directives.md#conditional-compilation) 和 [#endif](../preprocessor-directives.md#conditional-compilation) 一起使用，对源文件进行条件编译。</span><span class="sxs-lookup"><span data-stu-id="520d4-129">You can use symbols created by this option with [#if](../preprocessor-directives.md#conditional-compilation), [#else](../preprocessor-directives.md), [#elif](../preprocessor-directives.md#conditional-compilation), and [#endif](../preprocessor-directives.md#conditional-compilation) to compile source files conditionally.</span></span> <span data-ttu-id="520d4-130">C# 编译器本身不定义源代码中使用的符号或宏；所有符号定义必须都是用户定义的。</span><span class="sxs-lookup"><span data-stu-id="520d4-130">The C# compiler itself defines no symbols or macros that you can use in your source code; all symbol definitions must be user-defined.</span></span>

> [!NOTE]
> <span data-ttu-id="520d4-131">同 C++ 等语言一样，C# `#define` 指令不允许为符号赋值。</span><span class="sxs-lookup"><span data-stu-id="520d4-131">The C# `#define` directive does not allow a symbol to be given a value, as in languages such as C++.</span></span> <span data-ttu-id="520d4-132">例如，`#define` 不能用于创建宏或定义常数。</span><span class="sxs-lookup"><span data-stu-id="520d4-132">For example, `#define` cannot be used to create a macro or to define a constant.</span></span> <span data-ttu-id="520d4-133">如果需要定义一个常数，请使用 `enum` 变量。</span><span class="sxs-lookup"><span data-stu-id="520d4-133">If you need to define a constant, use an `enum` variable.</span></span> <span data-ttu-id="520d4-134">若要创建 C++ 风格的宏，请考虑泛型等替代项。</span><span class="sxs-lookup"><span data-stu-id="520d4-134">If you want to create a C++ style macro, consider alternatives such as generics.</span></span> <span data-ttu-id="520d4-135">由于宏非常容易出错，所以 C# 不允许使用宏，但提供了更安全的替代项。</span><span class="sxs-lookup"><span data-stu-id="520d4-135">Since macros are notoriously error-prone, C# disallows their use but provides safer alternatives.</span></span>

## <a name="langversion"></a><span data-ttu-id="520d4-136">LangVersion</span><span class="sxs-lookup"><span data-stu-id="520d4-136">LangVersion</span></span>

<span data-ttu-id="520d4-137">使编译器仅接受包含在所选 C# 语言规范中的语法。</span><span class="sxs-lookup"><span data-stu-id="520d4-137">Causes the compiler to accept only syntax that is included in the chosen C# language specification.</span></span>

```xml
<LangVersion>9.0</LangVersion>
```

<span data-ttu-id="520d4-138">以下为有效值：</span><span class="sxs-lookup"><span data-stu-id="520d4-138">The following values are valid:</span></span>

[!INCLUDE [lang-versions-table](../includes/langversion-table.md)]

<span data-ttu-id="520d4-139">默认语言版本依赖于应用程序的目标框架以及所安装的 SDK 或 Visual Studio 的版本。</span><span class="sxs-lookup"><span data-stu-id="520d4-139">The default language version depends on the target framework for your application and the version of the SDK or Visual Studio installed.</span></span> <span data-ttu-id="520d4-140">这些规则是在 [C# 语言版本控制](../configure-language-version.md#defaults)中定义的。</span><span class="sxs-lookup"><span data-stu-id="520d4-140">Those rules are defined in [C# language versioning](../configure-language-version.md#defaults).</span></span>

<span data-ttu-id="520d4-141">C# 应用程序引用的元数据不受 LangVersion 编译器选项约束。</span><span class="sxs-lookup"><span data-stu-id="520d4-141">Metadata referenced by your C# application isn't subject to the **LangVersion** compiler option.</span></span>

<span data-ttu-id="520d4-142">每个版本的 C# 编译器都包含语言规范的扩展，因此 LangVersion 不提供早期版本编译器的同等功能。</span><span class="sxs-lookup"><span data-stu-id="520d4-142">Because each version of the C# compiler contains extensions to the language specification, **LangVersion** doesn't give you the equivalent functionality of an earlier version of the compiler.</span></span>

<span data-ttu-id="520d4-143">此外，虽然 C# 版本更新通常与主要的 .NET Framework 版本一致，但新的语法和功能不一定绑定到该特定的 Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="520d4-143">Additionally, while C# version updates generally coincide with major .NET Framework releases, the new syntax and features aren't necessarily tied to that specific framework version.</span></span> <span data-ttu-id="520d4-144">虽然新功能肯定需要与 C# 修订版一起发布的新编译器更新，但每项具体功能都有自己的最小 .NET API 或公共语言运行时要求，这些要求通过包括 NuGet 包或其他库允许功能在下层框架上运行。</span><span class="sxs-lookup"><span data-stu-id="520d4-144">While the new features definitely require a new compiler update that is also released alongside the C# revision, each specific feature has its own minimum .NET API or common language runtime requirements that may allow it to run on downlevel frameworks by including NuGet packages or other libraries.</span></span>

<span data-ttu-id="520d4-145">无论使用哪一项 LangVersion 设置，请使用当前版本的公共语言运行时来创建 .exe 或 .dll。</span><span class="sxs-lookup"><span data-stu-id="520d4-145">Regardless of which **LangVersion** setting you use, use the current version of the common language runtime to create your .exe or .dll.</span></span> <span data-ttu-id="520d4-146">友元程序集和 [ModuleAssemblyName](advanced.md#moduleassemblyname) 是一个例外，它们在 -langversion:ISO-1 下工作。</span><span class="sxs-lookup"><span data-stu-id="520d4-146">One exception is friend assemblies and [**ModuleAssemblyName**](advanced.md#moduleassemblyname), which work under **-langversion:ISO-1**.</span></span>

<span data-ttu-id="520d4-147">若要了解指定 C# 语言版本的其他方式，请参阅 [C# 语言版本控制](../configure-language-version.md)。</span><span class="sxs-lookup"><span data-stu-id="520d4-147">For other ways to specify the C# language version, see [C# language versioning](../configure-language-version.md).</span></span>

<span data-ttu-id="520d4-148">有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.LanguageVersion%2A>。</span><span class="sxs-lookup"><span data-stu-id="520d4-148">For information about how to set this compiler option programmatically, see <xref:VSLangProj80.CSharpProjectConfigurationProperties3.LanguageVersion%2A>.</span></span>

### <a name="c-language-specification"></a><span data-ttu-id="520d4-149">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="520d4-149">C# language specification</span></span>

| <span data-ttu-id="520d4-150">Version</span><span class="sxs-lookup"><span data-stu-id="520d4-150">Version</span></span>          | <span data-ttu-id="520d4-151">链接</span><span class="sxs-lookup"><span data-stu-id="520d4-151">Link</span></span>                       | <span data-ttu-id="520d4-152">描述</span><span class="sxs-lookup"><span data-stu-id="520d4-152">Description</span></span>                                                             |
|------------------|----------------------------|-------------------------------------------------------------------------|
| <span data-ttu-id="520d4-153">C# 7.0 和更高版本</span><span class="sxs-lookup"><span data-stu-id="520d4-153">C# 7.0 and later</span></span> |                            | <span data-ttu-id="520d4-154">当前不可用</span><span class="sxs-lookup"><span data-stu-id="520d4-154">Not currently available</span></span>                                                 |
| <span data-ttu-id="520d4-155">C# 6.0</span><span class="sxs-lookup"><span data-stu-id="520d4-155">C# 6.0</span></span>           | <span data-ttu-id="520d4-156">[链接][csharp-6]</span><span class="sxs-lookup"><span data-stu-id="520d4-156">[Link][csharp-6]</span></span>           | <span data-ttu-id="520d4-157">C# 语言规范版本 6 - 非官方草稿：.NET Foundation</span><span class="sxs-lookup"><span data-stu-id="520d4-157">C# Language Specification Version 6 - Unofficial Draft: .NET Foundation</span></span> |
| <span data-ttu-id="520d4-158">C# 5.0</span><span class="sxs-lookup"><span data-stu-id="520d4-158">C# 5.0</span></span>           | <span data-ttu-id="520d4-159">[下载 PDF][csharp-5]</span><span class="sxs-lookup"><span data-stu-id="520d4-159">[Download PDF][csharp-5]</span></span>   | <span data-ttu-id="520d4-160">标准 ECMA-334 第 5 版</span><span class="sxs-lookup"><span data-stu-id="520d4-160">Standard ECMA-334 5th Edition</span></span>                                           |
| <span data-ttu-id="520d4-161">C# 3.0</span><span class="sxs-lookup"><span data-stu-id="520d4-161">C# 3.0</span></span>           | <span data-ttu-id="520d4-162">[下载 DOC][csharp-3]</span><span class="sxs-lookup"><span data-stu-id="520d4-162">[Download DOC][csharp-3]</span></span>   | <span data-ttu-id="520d4-163">C# 语言规范版本 3.0：Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="520d4-163">C# Language Specification Version 3.0: Microsoft Corporation</span></span>            |
| <span data-ttu-id="520d4-164">C# 2.0</span><span class="sxs-lookup"><span data-stu-id="520d4-164">C# 2.0</span></span>           | <span data-ttu-id="520d4-165">[下载 PDF][csharp-2]</span><span class="sxs-lookup"><span data-stu-id="520d4-165">[Download PDF][csharp-2]</span></span>   | <span data-ttu-id="520d4-166">标准 ECMA-334 第 4 版</span><span class="sxs-lookup"><span data-stu-id="520d4-166">Standard ECMA-334 4th Edition</span></span>                                           |
| <span data-ttu-id="520d4-167">C# 1.2</span><span class="sxs-lookup"><span data-stu-id="520d4-167">C# 1.2</span></span>           | <span data-ttu-id="520d4-168">[下载 DOC][csharp-1.2]</span><span class="sxs-lookup"><span data-stu-id="520d4-168">[Download DOC][csharp-1.2]</span></span> | <span data-ttu-id="520d4-169">C# 语言规范版本 1.2：Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="520d4-169">C# Language Specification Version 1.2: Microsoft Corporation</span></span>            |
| <span data-ttu-id="520d4-170">C# 1.0</span><span class="sxs-lookup"><span data-stu-id="520d4-170">C# 1.0</span></span>           | <span data-ttu-id="520d4-171">[下载 DOC][csharp-1]</span><span class="sxs-lookup"><span data-stu-id="520d4-171">[Download DOC][csharp-1]</span></span>   | <span data-ttu-id="520d4-172">C# 语言规范版本 1.0：Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="520d4-172">C# Language Specification Version 1.0: Microsoft Corporation</span></span>            |

[csharp-6]: /dotnet/csharp/language-reference/language-specification/introduction
[csharp-5]: https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-334.pdf
[csharp-3]: https://download.microsoft.com/download/3/8/8/388e7205-bc10-4226-b2a8-75351c669b09/CSharp%20Language%20Specification.doc
[csharp-2]: https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%204th%20edition%20June%202006.pdf
[csharp-1.2]: https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%202nd%20edition%20December%202002.pdf
[csharp-1]: https://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-334%201st%20edition%20December%202001.pdf

### <a name="minimum-sdk-version-needed-to-support-all-language-features"></a><span data-ttu-id="520d4-173">支持所有语言功能所需的最低 SDK 版本</span><span class="sxs-lookup"><span data-stu-id="520d4-173">Minimum SDK version needed to support all language features</span></span>

<span data-ttu-id="520d4-174">下表列出了支持相应语言版本的 C# 编译器的 SDK 的最低版本：</span><span class="sxs-lookup"><span data-stu-id="520d4-174">The following table lists the minimum versions of the SDK with the C# compiler that supports the corresponding language version:</span></span>

| <span data-ttu-id="520d4-175">C# 版本</span><span class="sxs-lookup"><span data-stu-id="520d4-175">C# version</span></span> | <span data-ttu-id="520d4-176">最低 SDK 版本</span><span class="sxs-lookup"><span data-stu-id="520d4-176">Minimum SDK version</span></span>                                                                  |
|------------|--------------------------------------------------------------------------------------|
| <span data-ttu-id="520d4-177">C# 8.0</span><span class="sxs-lookup"><span data-stu-id="520d4-177">C# 8.0</span></span>     | <span data-ttu-id="520d4-178">Microsoft Visual Studio/生成工具 2019，版本 16.3 或 .NET Core 3.0 SDK</span><span class="sxs-lookup"><span data-stu-id="520d4-178">Microsoft Visual Studio/Build Tools 2019, version 16.3, or .NET Core 3.0 SDK</span></span>         |
| <span data-ttu-id="520d4-179">C# 7.3</span><span class="sxs-lookup"><span data-stu-id="520d4-179">C# 7.3</span></span>     | <span data-ttu-id="520d4-180">Microsoft Visual Studio/生成工具 2017，版本 15.7</span><span class="sxs-lookup"><span data-stu-id="520d4-180">Microsoft Visual Studio/Build Tools 2017, version 15.7</span></span>                               |
| <span data-ttu-id="520d4-181">C# 7.2</span><span class="sxs-lookup"><span data-stu-id="520d4-181">C# 7.2</span></span>     | <span data-ttu-id="520d4-182">Microsoft Visual Studio/生成工具 2017，版本 15.5</span><span class="sxs-lookup"><span data-stu-id="520d4-182">Microsoft Visual Studio/Build Tools 2017, version 15.5</span></span>                               |
| <span data-ttu-id="520d4-183">C# 7.1</span><span class="sxs-lookup"><span data-stu-id="520d4-183">C# 7.1</span></span>     | <span data-ttu-id="520d4-184">Microsoft Visual Studio/生成工具 2017，版本 15.3</span><span class="sxs-lookup"><span data-stu-id="520d4-184">Microsoft Visual Studio/Build Tools 2017, version 15.3</span></span>                               |
| <span data-ttu-id="520d4-185">C# 7.0</span><span class="sxs-lookup"><span data-stu-id="520d4-185">C# 7.0</span></span>     | <span data-ttu-id="520d4-186">Microsoft Visual Studio/生成工具 2017</span><span class="sxs-lookup"><span data-stu-id="520d4-186">Microsoft Visual Studio/Build Tools 2017</span></span>                                             |
| <span data-ttu-id="520d4-187">C# 6</span><span class="sxs-lookup"><span data-stu-id="520d4-187">C# 6</span></span>       | <span data-ttu-id="520d4-188">Microsoft Visual Studio/生成工具 2015</span><span class="sxs-lookup"><span data-stu-id="520d4-188">Microsoft Visual Studio/Build Tools 2015</span></span>                                             |
| <span data-ttu-id="520d4-189">C# 5</span><span class="sxs-lookup"><span data-stu-id="520d4-189">C# 5</span></span>       | <span data-ttu-id="520d4-190">Microsoft Visual Studio/生成工具 2012 或捆绑的 .NET Framework 4.5 编译器</span><span class="sxs-lookup"><span data-stu-id="520d4-190">Microsoft Visual Studio/Build Tools 2012 or bundled .NET Framework 4.5 compiler</span></span>      |
| <span data-ttu-id="520d4-191">C# 4</span><span class="sxs-lookup"><span data-stu-id="520d4-191">C# 4</span></span>       | <span data-ttu-id="520d4-192">Microsoft Visual Studio/生成工具 2010 或捆绑的 .NET Framework 4.0 编译器</span><span class="sxs-lookup"><span data-stu-id="520d4-192">Microsoft Visual Studio/Build Tools 2010 or bundled .NET Framework 4.0 compiler</span></span>      |
| <span data-ttu-id="520d4-193">C# 3</span><span class="sxs-lookup"><span data-stu-id="520d4-193">C# 3</span></span>       | <span data-ttu-id="520d4-194">Microsoft Visual Studio/生成工具 2008 或捆绑的 .NET Framework 3.5 编译器</span><span class="sxs-lookup"><span data-stu-id="520d4-194">Microsoft Visual Studio/Build Tools 2008 or bundled .NET Framework 3.5 compiler</span></span>      |
| <span data-ttu-id="520d4-195">C# 2</span><span class="sxs-lookup"><span data-stu-id="520d4-195">C# 2</span></span>       | <span data-ttu-id="520d4-196">Microsoft Visual Studio/生成工具 2005 或捆绑的 .Net Framework 2.0 编译器</span><span class="sxs-lookup"><span data-stu-id="520d4-196">Microsoft Visual Studio/Build Tools 2005 or bundled .NET Framework 2.0 compiler</span></span>      |
| <span data-ttu-id="520d4-197">C# 1.0/1.2</span><span class="sxs-lookup"><span data-stu-id="520d4-197">C# 1.0/1.2</span></span> | <span data-ttu-id="520d4-198">Microsoft Visual Studio/生成工具 .NET 2002 或捆绑的 .NET Framework 1.0 编译器</span><span class="sxs-lookup"><span data-stu-id="520d4-198">Microsoft Visual Studio/Build Tools .NET 2002 or bundled .NET Framework 1.0 compiler</span></span> |

## <a name="nullable"></a><span data-ttu-id="520d4-199">Nullable</span><span class="sxs-lookup"><span data-stu-id="520d4-199">Nullable</span></span>

<span data-ttu-id="520d4-200">使用 Nullable 选项可指定可为空上下文。</span><span class="sxs-lookup"><span data-stu-id="520d4-200">The **Nullable** option lets you specify the nullable context.</span></span>

```xml
<Nullable>enable</Nullable>
```

<span data-ttu-id="520d4-201">参数必须为以下项之一：`enable`、`disable`、`warnings` 或 `annotations`。</span><span class="sxs-lookup"><span data-stu-id="520d4-201">The argument must be one of `enable`, `disable`, `warnings`, or `annotations`.</span></span> <span data-ttu-id="520d4-202">`enable` 参数启用可为空上下文。</span><span class="sxs-lookup"><span data-stu-id="520d4-202">The `enable` argument enables the nullable context.</span></span> <span data-ttu-id="520d4-203">指定 `disable` 将禁用可为空上下文。</span><span class="sxs-lookup"><span data-stu-id="520d4-203">Specifying `disable` will disable the nullable context.</span></span> <span data-ttu-id="520d4-204">如果提供 `warnings` 参数，将启用可为空警告上下文。</span><span class="sxs-lookup"><span data-stu-id="520d4-204">When providing the `warnings` argument the nullable warning context is enabled.</span></span> <span data-ttu-id="520d4-205">如果指定 `annotations` 参数，将启用可为空注释上下文。</span><span class="sxs-lookup"><span data-stu-id="520d4-205">When specifying the `annotations` argument, the nullable annotation context is enabled.</span></span>

<span data-ttu-id="520d4-206">流分析用于在可执行代码中推断变量的为空性。</span><span class="sxs-lookup"><span data-stu-id="520d4-206">Flow analysis is used to infer the nullability of variables within executable code.</span></span> <span data-ttu-id="520d4-207">推断出的变量的为空性与变量声明的为空性无关。</span><span class="sxs-lookup"><span data-stu-id="520d4-207">The inferred nullability of a variable is independent of the variable's declared nullability.</span></span> <span data-ttu-id="520d4-208">即使有条件地省略方法调用，也会对其进行分析。</span><span class="sxs-lookup"><span data-stu-id="520d4-208">Method calls are analyzed even when they're conditionally omitted.</span></span> <span data-ttu-id="520d4-209">例如，发布模式下的 <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="520d4-209">For instance, <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> in release mode.</span></span>

<span data-ttu-id="520d4-210">采用以下特性进行注释的方法调用也将影响流分析：</span><span class="sxs-lookup"><span data-stu-id="520d4-210">Invocation of methods annotated with the following attributes will also affect flow analysis:</span></span>

- <span data-ttu-id="520d4-211">简单的前提条件：<xref:System.Diagnostics.CodeAnalysis.AllowNullAttribute> 和 <xref:System.Diagnostics.CodeAnalysis.DisallowNullAttribute></span><span class="sxs-lookup"><span data-stu-id="520d4-211">Simple pre-conditions: <xref:System.Diagnostics.CodeAnalysis.AllowNullAttribute> and <xref:System.Diagnostics.CodeAnalysis.DisallowNullAttribute></span></span>
- <span data-ttu-id="520d4-212">简单的后置条件：<xref:System.Diagnostics.CodeAnalysis.MaybeNullAttribute> 和 <xref:System.Diagnostics.CodeAnalysis.NotNullAttribute></span><span class="sxs-lookup"><span data-stu-id="520d4-212">Simple post-conditions: <xref:System.Diagnostics.CodeAnalysis.MaybeNullAttribute> and <xref:System.Diagnostics.CodeAnalysis.NotNullAttribute></span></span>
- <span data-ttu-id="520d4-213">有条件后置条件：<xref:System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute> 和 <xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute></span><span class="sxs-lookup"><span data-stu-id="520d4-213">Conditional post-conditions: <xref:System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute> and <xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute></span></span>
- <span data-ttu-id="520d4-214"><xref:System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute>（例如 <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> 的 `DoesNotReturnIf(false)`）和 <xref:System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute></span><span class="sxs-lookup"><span data-stu-id="520d4-214"><xref:System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute> (for example, `DoesNotReturnIf(false)` for <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>) and <xref:System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute></span></span>
- <xref:System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute>
- <span data-ttu-id="520d4-215">成员后置条件：<xref:System.Diagnostics.CodeAnalysis.MemberNotNullAttribute.%23ctor(System.String)> 和 <xref:System.Diagnostics.CodeAnalysis.MemberNotNullAttribute.%23ctor(System.String[])></span><span class="sxs-lookup"><span data-stu-id="520d4-215">Member post-conditions: <xref:System.Diagnostics.CodeAnalysis.MemberNotNullAttribute.%23ctor(System.String)> and <xref:System.Diagnostics.CodeAnalysis.MemberNotNullAttribute.%23ctor(System.String[])></span></span>

> [!IMPORTANT]
> <span data-ttu-id="520d4-216">全局可为空上下文不适用于生成的代码文件。</span><span class="sxs-lookup"><span data-stu-id="520d4-216">The global nullable context does not apply for generated code files.</span></span> <span data-ttu-id="520d4-217">无论此设置如何，都会针对标记为“已生成”的任何源文件禁用可为空上下文。</span><span class="sxs-lookup"><span data-stu-id="520d4-217">Regardless of this setting, the nullable context is *disabled* for any source file marked as generated.</span></span> <span data-ttu-id="520d4-218">可采用四种方法将文件标记为“已生成”：</span><span class="sxs-lookup"><span data-stu-id="520d4-218">There are four ways a file is marked as generated:</span></span>
>
> 1. <span data-ttu-id="520d4-219">在 .editorconfig 中，在应用于该文件的部分中指定 `generated_code = true`。</span><span class="sxs-lookup"><span data-stu-id="520d4-219">In the .editorconfig, specify `generated_code = true` in a section that applies to that file.</span></span>
> 1. <span data-ttu-id="520d4-220">将 `<auto-generated>` 或 `<auto-generated/>` 放在文件顶部的注释中。</span><span class="sxs-lookup"><span data-stu-id="520d4-220">Put `<auto-generated>` or `<auto-generated/>` in a comment at the top of the file.</span></span> <span data-ttu-id="520d4-221">它可以位于该注释中的任意行上，但注释块必须是该文件中的第一个元素。</span><span class="sxs-lookup"><span data-stu-id="520d4-221">It can be on any line in that comment, but the comment block must be the first element in the file.</span></span>
> 1. <span data-ttu-id="520d4-222">文件名以 TemporaryGeneratedFile_ 开头</span><span class="sxs-lookup"><span data-stu-id="520d4-222">Start the file name with *TemporaryGeneratedFile_*</span></span>
> 1. <span data-ttu-id="520d4-223">文件名用以 .designer.cs、.generated.cs、.g.cs 或 .g.i.cs 结尾   。</span><span class="sxs-lookup"><span data-stu-id="520d4-223">End the file name with *.designer.cs*, *.generated.cs*, *.g.cs*, or *.g.i.cs*.</span></span>
>
> <span data-ttu-id="520d4-224">生成器可以选择使用 [`#nullable`](../preprocessor-directives.md#nullable-context) 预处理器指令。</span><span class="sxs-lookup"><span data-stu-id="520d4-224">Generators can opt-in using the [`#nullable`](../preprocessor-directives.md#nullable-context) preprocessor directive.</span></span>
