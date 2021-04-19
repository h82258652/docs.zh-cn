---
title: C# 7.0 中的新增功能 - C# 指南
description: 大致了解 C# 语言的版本 7.0 中的新增功能。
ms.date: 10/02/2020
ms.assetid: fd41596d-d0c2-4816-b94d-c4d00a5d0243
ms.openlocfilehash: 0c57a030497d13b783960152612de27d5fc73c3c
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255020"
---
# <a name="whats-new-in-c-70-through-c-73"></a><span data-ttu-id="18b83-103">C# 7.0 - C# 7.3 中的新增功能</span><span class="sxs-lookup"><span data-stu-id="18b83-103">What's new in C# 7.0 through C# 7.3</span></span>

<span data-ttu-id="18b83-104">C# 7.0 - C# 7.3 为 C# 开发体验带来了大量功能和增量改进。</span><span class="sxs-lookup"><span data-stu-id="18b83-104">C# 7.0 through C# 7.3 brought a number of features and incremental improvements to your development experience with C#.</span></span> <span data-ttu-id="18b83-105">本文概述了新的语言功能和编译器选项。</span><span class="sxs-lookup"><span data-stu-id="18b83-105">This article provides an overview of the new language features and compiler options.</span></span> <span data-ttu-id="18b83-106">说明中描述了 C# 7.3 的行为，C# 7.3 是基于 .NET Framework 的应用程序支持的最新版本。</span><span class="sxs-lookup"><span data-stu-id="18b83-106">The descriptions describe the behavior for C# 7.3, which is the most recent version supported for .NET Framework-based applications.</span></span>

<span data-ttu-id="18b83-107">C# 7.1 中添加了[语言版本选择](../language-reference/configure-language-version.md)配置元素，因此你可以在项目文件中指定编译器语言版本。</span><span class="sxs-lookup"><span data-stu-id="18b83-107">The [language version selection](../language-reference/configure-language-version.md) configuration element was added with C# 7.1, which enables you to specify the compiler language version in your project file.</span></span>

<span data-ttu-id="18b83-108">C# 7.0-7.3 将这些功能和主题添加到了 C# 语言中：</span><span class="sxs-lookup"><span data-stu-id="18b83-108">C# 7.0-7.3 adds these features and themes to the C# language:</span></span>

- [<span data-ttu-id="18b83-109">元组和弃元</span><span class="sxs-lookup"><span data-stu-id="18b83-109">Tuples and discards</span></span>](#tuples-and-discards)
  - <span data-ttu-id="18b83-110">可以创建包含多个公共字段的轻量级未命名类型。</span><span class="sxs-lookup"><span data-stu-id="18b83-110">You can create lightweight, unnamed types that contain multiple public fields.</span></span> <span data-ttu-id="18b83-111">编译器和 IDE 工具可理解这些类型的语义。</span><span class="sxs-lookup"><span data-stu-id="18b83-111">Compilers and IDE tools understand the semantics of these types.</span></span>
  - <span data-ttu-id="18b83-112">弃元是指在不关心所赋予的值时，赋值中使用的临时只写变量。</span><span class="sxs-lookup"><span data-stu-id="18b83-112">Discards are temporary, write-only variables used in assignments when you don't care about the value assigned.</span></span> <span data-ttu-id="18b83-113">在对元组和用户定义类型进行解构，以及在使用 `out` 参数调用方法时，它们的作用最大。</span><span class="sxs-lookup"><span data-stu-id="18b83-113">They're most useful when deconstructing tuples and user-defined types, as well as when calling methods with `out` parameters.</span></span>
- [<span data-ttu-id="18b83-114">模式匹配</span><span class="sxs-lookup"><span data-stu-id="18b83-114">Pattern Matching</span></span>](#pattern-matching)
  - <span data-ttu-id="18b83-115">可以基于任意类型和这些类型的成员的值创建分支逻辑。</span><span class="sxs-lookup"><span data-stu-id="18b83-115">You can create branching logic based on arbitrary types and values of the members of those types.</span></span>
- [<span data-ttu-id="18b83-116">`async` `Main` 方法</span><span class="sxs-lookup"><span data-stu-id="18b83-116">`async` `Main` method</span></span>](#async-main)
  - <span data-ttu-id="18b83-117">应用程序的入口点可以含有 `async` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="18b83-117">The entry point for an application can have the `async` modifier.</span></span>
- [<span data-ttu-id="18b83-118">本地函数</span><span class="sxs-lookup"><span data-stu-id="18b83-118">Local Functions</span></span>](#local-functions)
  - <span data-ttu-id="18b83-119">可以将函数嵌套在其他函数内，以限制其范围和可见性。</span><span class="sxs-lookup"><span data-stu-id="18b83-119">You can nest functions inside other functions to limit their scope and visibility.</span></span>
- [<span data-ttu-id="18b83-120">更多的 expression-bodied 成员</span><span class="sxs-lookup"><span data-stu-id="18b83-120">More expression-bodied members</span></span>](#more-expression-bodied-members)
  - <span data-ttu-id="18b83-121">可使用表达式创作的成员列表有所增长。</span><span class="sxs-lookup"><span data-stu-id="18b83-121">The list of members that can be authored using expressions has grown.</span></span>
- [<span data-ttu-id="18b83-122">`throw` 表达式</span><span class="sxs-lookup"><span data-stu-id="18b83-122">`throw` Expressions</span></span>](#throw-expressions)
  - <span data-ttu-id="18b83-123">可以在之前因为 `throw` 是语句而不被允许的代码构造中引发异常。</span><span class="sxs-lookup"><span data-stu-id="18b83-123">You can throw exceptions in code constructs that previously weren't allowed because `throw` was a statement.</span></span>
- [<span data-ttu-id="18b83-124">`default` 文本表达式</span><span class="sxs-lookup"><span data-stu-id="18b83-124">`default` literal expressions</span></span>](#default-literal-expressions)
  - <span data-ttu-id="18b83-125">在可以推断目标类型的情况下，可在默认值表达式中使用默认文本表达式。</span><span class="sxs-lookup"><span data-stu-id="18b83-125">You can use default literal expressions in default value expressions when the target type can be inferred.</span></span>
- [<span data-ttu-id="18b83-126">数字文本语法改进</span><span class="sxs-lookup"><span data-stu-id="18b83-126">Numeric literal syntax improvements</span></span>](#numeric-literal-syntax-improvements)
  - <span data-ttu-id="18b83-127">新令牌可提高数值常量的可读性。</span><span class="sxs-lookup"><span data-stu-id="18b83-127">New tokens improve readability for numeric constants.</span></span>
- [<span data-ttu-id="18b83-128">`out` 变量</span><span class="sxs-lookup"><span data-stu-id="18b83-128">`out` variables</span></span>](#out-variables)
  - <span data-ttu-id="18b83-129">可以将 `out` 值内联作为参数声明到使用这些参数的方法中。</span><span class="sxs-lookup"><span data-stu-id="18b83-129">You can declare `out` values inline as arguments to the method where they're used.</span></span>
- [<span data-ttu-id="18b83-130">非尾随命名参数</span><span class="sxs-lookup"><span data-stu-id="18b83-130">Non-trailing named arguments</span></span>](#non-trailing-named-arguments)
  - <span data-ttu-id="18b83-131">命名的参数可后接位置参数。</span><span class="sxs-lookup"><span data-stu-id="18b83-131">Named arguments can be followed by positional arguments.</span></span>
- [<span data-ttu-id="18b83-132">`private protected` 访问修饰符</span><span class="sxs-lookup"><span data-stu-id="18b83-132">`private protected` access modifier</span></span>](#private-protected-access-modifier)
  - <span data-ttu-id="18b83-133">`private protected` 访问修饰符允许访问同一程序集中的派生类。</span><span class="sxs-lookup"><span data-stu-id="18b83-133">The `private protected` access modifier enables access for derived classes in the same assembly.</span></span>
- [<span data-ttu-id="18b83-134">改进了重载解析</span><span class="sxs-lookup"><span data-stu-id="18b83-134">Improved overload resolution</span></span>](#improved-overload-candidates)
  - <span data-ttu-id="18b83-135">用于解决重载解析歧义的新规则。</span><span class="sxs-lookup"><span data-stu-id="18b83-135">New rules to resolve overload resolution ambiguity.</span></span>
- [<span data-ttu-id="18b83-136">编写安全高效代码的技巧</span><span class="sxs-lookup"><span data-stu-id="18b83-136">Techniques for writing safe efficient code</span></span>](#enabling-more-efficient-safe-code)
  - <span data-ttu-id="18b83-137">结合了多项语法改进，可使用引用语义处理值类型。</span><span class="sxs-lookup"><span data-stu-id="18b83-137">A combination of syntax improvements that enable working with value types using reference semantics.</span></span>

<span data-ttu-id="18b83-138">最后，编译器中添加了新的选项：</span><span class="sxs-lookup"><span data-stu-id="18b83-138">Finally, the compiler has new options:</span></span>

- <span data-ttu-id="18b83-139">控制[引用程序集生成](#reference-assembly-generation)的 `-refout` 和 `-refonly`。</span><span class="sxs-lookup"><span data-stu-id="18b83-139">`-refout` and `-refonly` that control [reference assembly generation](#reference-assembly-generation).</span></span>
- <span data-ttu-id="18b83-140">`-publicsign`，用于启用程序集的开放源代码软件 (OSS) 签名。</span><span class="sxs-lookup"><span data-stu-id="18b83-140">`-publicsign` to enable Open Source Software (OSS) signing of assemblies.</span></span>
- <span data-ttu-id="18b83-141">`-pathmap`用于提供源目录的映射。</span><span class="sxs-lookup"><span data-stu-id="18b83-141">`-pathmap` to provide a mapping for source directories.</span></span>

<span data-ttu-id="18b83-142">本文的其余部分概述了每个功能。</span><span class="sxs-lookup"><span data-stu-id="18b83-142">The remainder of this article provides an overview of each feature.</span></span> <span data-ttu-id="18b83-143">你将了解每项功能背后的原理和语法。</span><span class="sxs-lookup"><span data-stu-id="18b83-143">For each feature, you'll learn the reasoning behind it and the syntax.</span></span> <span data-ttu-id="18b83-144">可以使用 `dotnet try` 全局工具在环境中浏览这些功能：</span><span class="sxs-lookup"><span data-stu-id="18b83-144">You can explore these features in your environment using the `dotnet try` global tool:</span></span>

1. <span data-ttu-id="18b83-145">安装 [dotnet-try](https://github.com/dotnet/try/blob/main/DotNetTryLocal.md) 全局工具。</span><span class="sxs-lookup"><span data-stu-id="18b83-145">Install the [dotnet-try](https://github.com/dotnet/try/blob/main/DotNetTryLocal.md) global tool.</span></span>
1. <span data-ttu-id="18b83-146">克隆 [dotnet/try-samples](https://github.com/dotnet/try-samples) 存储库。</span><span class="sxs-lookup"><span data-stu-id="18b83-146">Clone the [dotnet/try-samples](https://github.com/dotnet/try-samples) repository.</span></span>
1. <span data-ttu-id="18b83-147">将当前目录设置为 try-samples 存储库的 csharp7 子目录 。</span><span class="sxs-lookup"><span data-stu-id="18b83-147">Set the current directory to the *csharp7* subdirectory for the *try-samples* repository.</span></span>
1. <span data-ttu-id="18b83-148">运行 `dotnet try`。</span><span class="sxs-lookup"><span data-stu-id="18b83-148">Run `dotnet try`.</span></span>

## <a name="tuples-and-discards"></a><span data-ttu-id="18b83-149">元组和弃元</span><span class="sxs-lookup"><span data-stu-id="18b83-149">Tuples and discards</span></span>

<span data-ttu-id="18b83-150">C# 为用于说明设计意图的类和结构提供了丰富的语法。</span><span class="sxs-lookup"><span data-stu-id="18b83-150">C# provides a rich syntax for classes and structs that is used to explain your design intent.</span></span> <span data-ttu-id="18b83-151">但是，这种丰富的语法有时会需要额外的工作，但益处却很少。</span><span class="sxs-lookup"><span data-stu-id="18b83-151">But sometimes that rich syntax requires extra work with minimal benefit.</span></span> <span data-ttu-id="18b83-152">你可能经常编写需要包含多个数据元素的简单结构的方法。</span><span class="sxs-lookup"><span data-stu-id="18b83-152">You may often write methods that need a simple structure containing more than one data element.</span></span> <span data-ttu-id="18b83-153">为了支持这些方案，已将元组添加到了 C#。</span><span class="sxs-lookup"><span data-stu-id="18b83-153">To support these scenarios *tuples* were added to C#.</span></span> <span data-ttu-id="18b83-154">元组是包含多个字段以表示数据成员的轻量级数据结构。</span><span class="sxs-lookup"><span data-stu-id="18b83-154">Tuples are lightweight data structures that contain multiple fields to represent the data members.</span></span> <span data-ttu-id="18b83-155">这些字段没有经过验证，并且你无法定义自己的方法。</span><span class="sxs-lookup"><span data-stu-id="18b83-155">The fields aren't validated, and you can't define your own methods.</span></span> <span data-ttu-id="18b83-156">C# 元组类型支持 `==` 和 `!=`。</span><span class="sxs-lookup"><span data-stu-id="18b83-156">C# tuple types support `==` and `!=`.</span></span> <span data-ttu-id="18b83-157">有关详细信息，</span><span class="sxs-lookup"><span data-stu-id="18b83-157">For more information.</span></span>

> [!NOTE]
> <span data-ttu-id="18b83-158">低于 C# 7.0 的版本中也提供元组，但它们效率低下且不具有语言支持。</span><span class="sxs-lookup"><span data-stu-id="18b83-158">Tuples were available before C# 7.0, but they were inefficient and had no language support.</span></span> <span data-ttu-id="18b83-159">这意味着元组元素只能作为 `Item1` 和 `Item2` 等引用。</span><span class="sxs-lookup"><span data-stu-id="18b83-159">This meant that tuple elements could only be referenced as `Item1`, `Item2` and so on.</span></span> <span data-ttu-id="18b83-160">C# 7.0 引入了对元组的语言支持，可利用更有效的新元组类型向元组字段赋予语义名称。</span><span class="sxs-lookup"><span data-stu-id="18b83-160">C# 7.0 introduces language support for tuples, which enables semantic names for the fields of a tuple using new, more efficient tuple types.</span></span>

<span data-ttu-id="18b83-161">可以通过为每个成员赋值来创建元组，并可选择为元组的每个成员提供语义名称：</span><span class="sxs-lookup"><span data-stu-id="18b83-161">You can create a tuple by assigning a value to each member, and optionally providing semantic names to each of the members of the tuple:</span></span>

[!code-csharp[NamedTuple](~/samples/snippets/csharp/new-in-7/program.cs#NamedTuple "Named tuple")]

<span data-ttu-id="18b83-162">`namedLetters` 元组包含称为 `Alpha` 和 `Beta` 的字段。</span><span class="sxs-lookup"><span data-stu-id="18b83-162">The `namedLetters` tuple contains fields referred to as `Alpha` and `Beta`.</span></span> <span data-ttu-id="18b83-163">这些名称仅存在于编译时且不保留，例如在运行时使用反射来检查元组时。</span><span class="sxs-lookup"><span data-stu-id="18b83-163">Those names exist only at compile time and aren't preserved, for example when inspecting the tuple using reflection at run time.</span></span>

<span data-ttu-id="18b83-164">在进行元组赋值时，还可以指定赋值右侧的字段的名称：</span><span class="sxs-lookup"><span data-stu-id="18b83-164">In a tuple assignment, you can also specify the names of the fields on the right-hand side of the assignment:</span></span>

[!code-csharp[ImplicitNamedTuple](~/samples/snippets/csharp/new-in-7/program.cs#ImplicitNamedTuple "Implicitly named tuple")]

<span data-ttu-id="18b83-165">在某些时候，你可能想要解包从方法返回的元组的成员。</span><span class="sxs-lookup"><span data-stu-id="18b83-165">There may be times when you want to unpackage the members of a tuple that were returned from a method.</span></span>  <span data-ttu-id="18b83-166">可通过为元组中的每个值声明单独的变量来实现此目的。</span><span class="sxs-lookup"><span data-stu-id="18b83-166">You can do that by declaring separate variables for each of the values in the tuple.</span></span> <span data-ttu-id="18b83-167">这种解包操作称为解构元组：</span><span class="sxs-lookup"><span data-stu-id="18b83-167">This unpackaging is called *deconstructing* the tuple:</span></span>

[!code-csharp[CallingWithDeconstructor](~/samples/snippets/csharp/new-in-7/program.cs#CallingWithDeconstructor "Deconstructing a tuple")]

<span data-ttu-id="18b83-168">还可以为 .NET 中的任何类型提供类似的析构。</span><span class="sxs-lookup"><span data-stu-id="18b83-168">You can also provide a similar deconstruction for any type in .NET.</span></span> <span data-ttu-id="18b83-169">编写 `Deconstruct` 方法，用作类的成员。</span><span class="sxs-lookup"><span data-stu-id="18b83-169">You write a `Deconstruct` method as a member of the class.</span></span> <span data-ttu-id="18b83-170">`Deconstruct` 方法为你要提取的每个属性提供一组 `out` 参数。</span><span class="sxs-lookup"><span data-stu-id="18b83-170">That `Deconstruct` method provides a set of `out` arguments for each of the properties you want to extract.</span></span> <span data-ttu-id="18b83-171">考虑提供析构函数方法的此 `Point` 类，该方法提取 `X` 和 `Y` 坐标：</span><span class="sxs-lookup"><span data-stu-id="18b83-171">Consider this `Point` class that provides a deconstructor method that extracts the `X` and `Y` coordinates:</span></span>

[!code-csharp[PointWithDeconstruction](~/samples/snippets/csharp/new-in-7/point.cs#PointWithDeconstruction "Point with deconstruction method")]

<span data-ttu-id="18b83-172">可以通过向元组分配 `Point` 来提取各个字段：</span><span class="sxs-lookup"><span data-stu-id="18b83-172">You can extract the individual fields by assigning a `Point` to a tuple:</span></span>

[!code-csharp[DeconstructPoint](~/samples/snippets/csharp/new-in-7/program.cs#DeconstructPoint "Deconstruct a point")]

<span data-ttu-id="18b83-173">在初始化元组时，许多时候，赋值操作右侧的变量名与用于元组元素的名称相同：元组元素的名称可通过用于初始化元组的变量进行推断：</span><span class="sxs-lookup"><span data-stu-id="18b83-173">Many times when you initialize a tuple, the variables used for the right side of the assignment are the same as the names you'd like for the tuple elements: The names of tuple elements can be inferred from the variables used to initialize the tuple:</span></span>

```csharp
int count = 5;
string label = "Colors used in the map";
var pair = (count, label); // element names are "count" and "label"
```

<span data-ttu-id="18b83-174">若要详细了解此功能，可以参阅[元组类型](../language-reference/builtin-types/value-tuples.md)一文。</span><span class="sxs-lookup"><span data-stu-id="18b83-174">You can learn more about this feature in the [Tuple types](../language-reference/builtin-types/value-tuples.md) article.</span></span>

<span data-ttu-id="18b83-175">通常，在进行元组解构或使用 `out` 参数调用方法时，必须定义一个其值无关紧要且你不打算使用的变量。</span><span class="sxs-lookup"><span data-stu-id="18b83-175">Often when deconstructing a tuple or calling a method with `out` parameters, you're forced to define a variable whose value you don't care about and don't intend to use.</span></span> <span data-ttu-id="18b83-176">为处理此情况，C# 增添了对弃元的支持。</span><span class="sxs-lookup"><span data-stu-id="18b83-176">C# adds support for *discards* to handle this scenario.</span></span> <span data-ttu-id="18b83-177">弃元是一个名为 `_`（下划线字符）的只写变量，可向单个变量赋予要放弃的所有值。</span><span class="sxs-lookup"><span data-stu-id="18b83-177">A discard is a write-only variable whose name is `_` (the underscore character); you can assign all of the values that you intend to discard to the single variable.</span></span> <span data-ttu-id="18b83-178">弃元类似于未赋值的变量；不可在代码中使用弃元（赋值语句除外）。</span><span class="sxs-lookup"><span data-stu-id="18b83-178">A discard is like an unassigned variable; apart from the assignment statement, the discard can't be used in code.</span></span>

<span data-ttu-id="18b83-179">在以下方案中支持弃元：</span><span class="sxs-lookup"><span data-stu-id="18b83-179">Discards are supported in the following scenarios:</span></span>

- <span data-ttu-id="18b83-180">在对元组或用户定义的类型进行解构时。</span><span class="sxs-lookup"><span data-stu-id="18b83-180">When deconstructing tuples or user-defined types.</span></span>
- <span data-ttu-id="18b83-181">在使用 [out](../language-reference/keywords/out-parameter-modifier.md) 参数调用方法时。</span><span class="sxs-lookup"><span data-stu-id="18b83-181">When calling methods with [out](../language-reference/keywords/out-parameter-modifier.md) parameters.</span></span>
- <span data-ttu-id="18b83-182">在使用 [is](../language-reference/keywords/is.md) 和 [switch](../language-reference/keywords/switch.md) 语句匹配操作的模式中。</span><span class="sxs-lookup"><span data-stu-id="18b83-182">In a pattern matching operation with the [is](../language-reference/keywords/is.md) and [switch](../language-reference/keywords/switch.md) statements.</span></span>
- <span data-ttu-id="18b83-183">在要将某赋值的值显式标识为弃元时用作独立标识符。</span><span class="sxs-lookup"><span data-stu-id="18b83-183">As a standalone identifier when you want to explicitly identify the value of an assignment as a discard.</span></span>

<span data-ttu-id="18b83-184">以下示例定义了 `QueryCityDataForYears` 方法，它返回一个包含两个不同年份的城市数据的六元组。</span><span class="sxs-lookup"><span data-stu-id="18b83-184">The following example defines a `QueryCityDataForYears` method that returns a 6-tuple that contains data for a city for two different years.</span></span> <span data-ttu-id="18b83-185">本例中，方法调用仅与此方法返回的两个人口值相关，因此在进行元组解构时，将元组中的其余值视为弃元。</span><span class="sxs-lookup"><span data-stu-id="18b83-185">The method call in the example is concerned only with the two population values returned by the method and so treats the remaining values in the tuple as discards when it deconstructs the tuple.</span></span>

[!code-csharp[Tuple-discard](~/samples/snippets/csharp/programming-guide/deconstructing-tuples/discard-tuple1.cs)]

<span data-ttu-id="18b83-186">有关详细信息，请参阅[弃元](../discards.md)。</span><span class="sxs-lookup"><span data-stu-id="18b83-186">For more information, see [Discards](../discards.md).</span></span>

## <a name="pattern-matching"></a><span data-ttu-id="18b83-187">模式匹配</span><span class="sxs-lookup"><span data-stu-id="18b83-187">Pattern matching</span></span>

<span data-ttu-id="18b83-188">模式匹配是一组功能，利用这些功能，你可以通过新的方式在代码中表示控制流。</span><span class="sxs-lookup"><span data-stu-id="18b83-188">*Pattern matching* is a set of features that enable new ways to express control flow in your code.</span></span> <span data-ttu-id="18b83-189">你可以测试变量的类型、值或其属性的值。</span><span class="sxs-lookup"><span data-stu-id="18b83-189">You can test variables for their type, values or the values of their properties.</span></span> <span data-ttu-id="18b83-190">这些方法可创建可读性更佳的代码流。</span><span class="sxs-lookup"><span data-stu-id="18b83-190">These techniques create more readable code flow.</span></span>

<span data-ttu-id="18b83-191">模式匹配支持 `is` 表达式和 `switch` 表达式。</span><span class="sxs-lookup"><span data-stu-id="18b83-191">Pattern matching supports `is` expressions and `switch` expressions.</span></span> <span data-ttu-id="18b83-192">每个表达式都允许检查对象及其属性以确定该对象是否满足所寻求的模式。</span><span class="sxs-lookup"><span data-stu-id="18b83-192">Each enables inspecting an object and its properties to determine if that object satisfies the sought pattern.</span></span> <span data-ttu-id="18b83-193">使用 `when` 关键字来指定模式的其他规则。</span><span class="sxs-lookup"><span data-stu-id="18b83-193">You use the `when` keyword to specify additional rules to the pattern.</span></span>

<span data-ttu-id="18b83-194">`is` 模式表达式扩展了常用 [`is` 运算符](../language-reference/keywords/is.md#pattern-matching-with-is)以查询关于其类型的对象，并在一条指令分配结果。</span><span class="sxs-lookup"><span data-stu-id="18b83-194">The `is` pattern expression extends the familiar [`is` operator](../language-reference/keywords/is.md#pattern-matching-with-is) to query an object about its type and assign the result in one instruction.</span></span> <span data-ttu-id="18b83-195">以下代码检查变量是否为 `int`，如果是，则将其添加到当前总和：</span><span class="sxs-lookup"><span data-stu-id="18b83-195">The following code checks if a variable is an `int`, and if so, adds it to the current sum:</span></span>

```csharp
if (input is int count)
    sum += count;
```

<span data-ttu-id="18b83-196">前面的小型示例演示了 `is` 表达式的增强功能。</span><span class="sxs-lookup"><span data-stu-id="18b83-196">The preceding small example demonstrates the enhancements to the `is` expression.</span></span> <span data-ttu-id="18b83-197">可以针对值类型和引用类型进行测试，并且可以将成功结果分配给类型正确的新变量。</span><span class="sxs-lookup"><span data-stu-id="18b83-197">You can test against value types as well as reference types, and you can assign the successful result to a new variable of the correct type.</span></span>

<span data-ttu-id="18b83-198">switch 匹配表达式具有常见的语法，它基于已包含在 C# 语言中的 `switch` 语句。</span><span class="sxs-lookup"><span data-stu-id="18b83-198">The switch match expression has a familiar syntax, based on the `switch` statement already part of the C# language.</span></span> <span data-ttu-id="18b83-199">更新后的 switch 语句有几个新构造：</span><span class="sxs-lookup"><span data-stu-id="18b83-199">The updated switch statement has several new constructs:</span></span>

- <span data-ttu-id="18b83-200">`switch` 表达式的控制类型不再局限于整数类型、`Enum` 类型、`string` 或与这些类型之一对应的可为 null 的类型。</span><span class="sxs-lookup"><span data-stu-id="18b83-200">The governing type of a `switch` expression is no longer restricted to integral types, `Enum` types, `string`, or a nullable type corresponding to one of those types.</span></span> <span data-ttu-id="18b83-201">可能会使用任何类型。</span><span class="sxs-lookup"><span data-stu-id="18b83-201">Any type may be used.</span></span>
- <span data-ttu-id="18b83-202">可以在每个 `case` 标签中测试 `switch` 表达式的类型。</span><span class="sxs-lookup"><span data-stu-id="18b83-202">You can test the type of the `switch` expression in each `case` label.</span></span> <span data-ttu-id="18b83-203">与 `is` 表达式一样，可以为该类型指定一个新变量。</span><span class="sxs-lookup"><span data-stu-id="18b83-203">As with the `is` expression, you may assign a new variable to that type.</span></span>
- <span data-ttu-id="18b83-204">可以添加 `when` 子句以进一步测试该变量的条件。</span><span class="sxs-lookup"><span data-stu-id="18b83-204">You may add a `when` clause to further test conditions on that variable.</span></span>
- <span data-ttu-id="18b83-205">`case` 标签的顺序现在很重要。</span><span class="sxs-lookup"><span data-stu-id="18b83-205">The order of `case` labels is now important.</span></span> <span data-ttu-id="18b83-206">执行匹配的第一个分支；其他将跳过。</span><span class="sxs-lookup"><span data-stu-id="18b83-206">The first branch to match is executed; others are skipped.</span></span>

<span data-ttu-id="18b83-207">以下代码演示了这些新功能：</span><span class="sxs-lookup"><span data-stu-id="18b83-207">The following code demonstrates these new features:</span></span>

```csharp
public static int SumPositiveNumbers(IEnumerable<object> sequence)
{
    int sum = 0;
    foreach (var i in sequence)
    {
        switch (i)
        {
            case 0:
                break;
            case IEnumerable<int> childSequence:
            {
                foreach(var item in childSequence)
                    sum += (item > 0) ? item : 0;
                break;
            }
            case int n when n > 0:
                sum += n;
                break;
            case null:
                throw new NullReferenceException("Null found in sequence");
            default:
                throw new InvalidOperationException("Unrecognized type");
        }
    }
    return sum;
}
```

- <span data-ttu-id="18b83-208">`case 0:` 是[常量模式](../language-reference/operators/patterns.md#constant-pattern)。</span><span class="sxs-lookup"><span data-stu-id="18b83-208">`case 0:` is a [constant pattern](../language-reference/operators/patterns.md#constant-pattern).</span></span>
- <span data-ttu-id="18b83-209">`case IEnumerable<int> childSequence:` 是[声明模式](../language-reference/operators/patterns.md#declaration-and-type-patterns)。</span><span class="sxs-lookup"><span data-stu-id="18b83-209">`case IEnumerable<int> childSequence:` is a [declaration pattern](../language-reference/operators/patterns.md#declaration-and-type-patterns).</span></span>
- <span data-ttu-id="18b83-210">`case int n when n > 0:` 是具有附加 `when` 条件的声明模式。</span><span class="sxs-lookup"><span data-stu-id="18b83-210">`case int n when n > 0:` is a declaration pattern with an additional `when` condition.</span></span>
- <span data-ttu-id="18b83-211">`case null:` 是 `null` 常量模式。</span><span class="sxs-lookup"><span data-stu-id="18b83-211">`case null:` is the `null` constant pattern.</span></span>
- <span data-ttu-id="18b83-212">`default:` 是常见的默认事例。</span><span class="sxs-lookup"><span data-stu-id="18b83-212">`default:` is the familiar default case.</span></span>

<span data-ttu-id="18b83-213">自 C# 7.1 起，`is` 和 `switch` 类型模式的模式表达式的类型可能为泛型类型参数。</span><span class="sxs-lookup"><span data-stu-id="18b83-213">Beginning with C# 7.1, the pattern expression for `is` and the `switch` type pattern may have the type of a generic type parameter.</span></span> <span data-ttu-id="18b83-214">这可能在检查 `struct` 或 `class` 类型且要避免装箱时最有用。</span><span class="sxs-lookup"><span data-stu-id="18b83-214">This can be most useful when checking types that may be either `struct` or `class` types, and you want to avoid boxing.</span></span>

<span data-ttu-id="18b83-215">可以在 [C# 中的模式匹配](../pattern-matching.md)中了解有关模式匹配的更多信息。</span><span class="sxs-lookup"><span data-stu-id="18b83-215">You can learn more about pattern matching in [Pattern Matching in C#](../pattern-matching.md).</span></span>

## <a name="async-main"></a><span data-ttu-id="18b83-216">异步 `main` 方法</span><span class="sxs-lookup"><span data-stu-id="18b83-216">Async main</span></span>

<span data-ttu-id="18b83-217">*异步 Main* 方法使你能够在 `Main` 方法中使用 `await` 关键字。</span><span class="sxs-lookup"><span data-stu-id="18b83-217">An *async main* method enables you to use `await` in your `Main` method.</span></span> <span data-ttu-id="18b83-218">在过去，需要编写：</span><span class="sxs-lookup"><span data-stu-id="18b83-218">Previously you would need to write:</span></span>

```csharp
static int Main()
{
    return DoAsyncWork().GetAwaiter().GetResult();
}
```

<span data-ttu-id="18b83-219">现在，您可以编写：</span><span class="sxs-lookup"><span data-stu-id="18b83-219">You can now write:</span></span>

```csharp
static async Task<int> Main()
{
    // This could also be replaced with the body
    // DoAsyncWork, including its await expressions:
    return await DoAsyncWork();
}
```

<span data-ttu-id="18b83-220">如果程序不返回退出代码，可以声明返回 <xref:System.Threading.Tasks.Task> 的 `Main` 方法:</span><span class="sxs-lookup"><span data-stu-id="18b83-220">If your program doesn't return an exit code, you can declare a `Main` method that returns a <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
static async Task Main()
{
    await SomeAsyncMethod();
}
```

<span data-ttu-id="18b83-221">如需了解更多详情，可以阅读编程指南中的[异步 Main](../programming-guide/main-and-command-args/index.md) 一文。</span><span class="sxs-lookup"><span data-stu-id="18b83-221">You can read more about the details in the [async main](../programming-guide/main-and-command-args/index.md) article in the programming guide.</span></span>

## <a name="local-functions"></a><span data-ttu-id="18b83-222">本地函数</span><span class="sxs-lookup"><span data-stu-id="18b83-222">Local functions</span></span>

<span data-ttu-id="18b83-223">许多类的设计都包括仅从一个位置调用的方法。</span><span class="sxs-lookup"><span data-stu-id="18b83-223">Many designs for classes include methods that are called from only one location.</span></span> <span data-ttu-id="18b83-224">这些额外的私有方法使每个方法保持小且集中。</span><span class="sxs-lookup"><span data-stu-id="18b83-224">These additional private methods keep each method small and focused.</span></span> <span data-ttu-id="18b83-225">本地函数使你能够在另一个方法的上下文内声明方法。</span><span class="sxs-lookup"><span data-stu-id="18b83-225">*Local functions* enable you to declare methods inside the context of another method.</span></span> <span data-ttu-id="18b83-226">本地函数使得类的阅读者更容易看到本地方法仅从声明它的上下文中调用。</span><span class="sxs-lookup"><span data-stu-id="18b83-226">Local functions make it easier for readers of the class to see that the local method is only called from the context in which it is declared.</span></span>

<span data-ttu-id="18b83-227">对于本地函数有两个常见的用例：公共迭代器方法和公共异步方法。</span><span class="sxs-lookup"><span data-stu-id="18b83-227">There are two common use cases for local functions: public iterator methods and public async methods.</span></span> <span data-ttu-id="18b83-228">这两种类型的方法都生成报告错误的时间晚于程序员期望时间的代码。</span><span class="sxs-lookup"><span data-stu-id="18b83-228">Both types of methods generate code that reports errors later than programmers might expect.</span></span> <span data-ttu-id="18b83-229">在迭代器方法中，只有在调用枚举返回的序列的代码时才会观察到任何异常。</span><span class="sxs-lookup"><span data-stu-id="18b83-229">In iterator methods, any exceptions are observed only when calling code that enumerates the returned sequence.</span></span> <span data-ttu-id="18b83-230">在异步方法中，只有当返回的 `Task` 处于等待状态时才会观察到任何异常。</span><span class="sxs-lookup"><span data-stu-id="18b83-230">In async methods, any exceptions are only observed when the returned `Task` is awaited.</span></span> <span data-ttu-id="18b83-231">以下示例演示如何使用本地函数将参数验证与迭代器实现分离：</span><span class="sxs-lookup"><span data-stu-id="18b83-231">The following example demonstrates separating parameter validation from the iterator implementation using a local function:</span></span>

[!code-csharp[22_IteratorMethodLocal](~/samples/snippets/csharp/new-in-7/Iterator.cs#IteratorMethodLocal "Iterator method with local function")]

<span data-ttu-id="18b83-232">可以对 `async` 方法采用相同的技术，以确保在异步工作开始之前引发由参数验证引起的异常：</span><span class="sxs-lookup"><span data-stu-id="18b83-232">The same technique can be employed with `async` methods to ensure that exceptions arising from argument validation are thrown before the asynchronous work begins:</span></span>

[!code-csharp[TaskExample](~/samples/snippets/csharp/new-in-7/AsyncWork.cs#TaskExample "Task returning method with local function")]

<span data-ttu-id="18b83-233">现在支持此语法：</span><span class="sxs-lookup"><span data-stu-id="18b83-233">This syntax is now supported:</span></span>

```csharp
[field: SomeThingAboutFieldAttribute]
public int SomeProperty { get; set; }
```

<span data-ttu-id="18b83-234">属性 `SomeThingAboutFieldAttribute` 应用于编译器生成的 `SomeProperty` 的支持字段。</span><span class="sxs-lookup"><span data-stu-id="18b83-234">The attribute `SomeThingAboutFieldAttribute` is applied to the compiler generated backing field for `SomeProperty`.</span></span> <span data-ttu-id="18b83-235">有关详细信息，请参阅 C# 编程指南中的[属性](../programming-guide/concepts/attributes/index.md)。</span><span class="sxs-lookup"><span data-stu-id="18b83-235">For more information, see [attributes](../programming-guide/concepts/attributes/index.md) in the C# programming guide.</span></span>

> [!NOTE]
> <span data-ttu-id="18b83-236">本地函数支持的某些设计也可以使用 lambda 表达式来完成。</span><span class="sxs-lookup"><span data-stu-id="18b83-236">Some of the designs that are supported by local functions can also be accomplished using *lambda expressions*.</span></span> <span data-ttu-id="18b83-237">有关详细信息，请参阅[本地函数与 Lambda 表达式](../programming-guide/classes-and-structs/local-functions.md#local-functions-vs-lambda-expressions)。</span><span class="sxs-lookup"><span data-stu-id="18b83-237">For more information, see [Local functions vs. lambda expressions](../programming-guide/classes-and-structs/local-functions.md#local-functions-vs-lambda-expressions).</span></span>

## <a name="more-expression-bodied-members"></a><span data-ttu-id="18b83-238">更多的 expression-bodied 成员</span><span class="sxs-lookup"><span data-stu-id="18b83-238">More expression-bodied members</span></span>

<span data-ttu-id="18b83-239">C# 6 为成员函数和只读属性引入了 expression-bodied 成员。</span><span class="sxs-lookup"><span data-stu-id="18b83-239">C# 6 introduced expression-bodied members for member functions and read-only properties.</span></span> <span data-ttu-id="18b83-240">C# 7.0 扩展了可作为表达式实现的允许的成员。</span><span class="sxs-lookup"><span data-stu-id="18b83-240">C# 7.0 expands the allowed members that can be implemented as expressions.</span></span> <span data-ttu-id="18b83-241">在 C# 7.0 中，你可以在属性和索引器上实现构造函数、终结器以及 `get` 和 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="18b83-241">In C# 7.0, you can implement *constructors*, *finalizers*, and `get` and `set` accessors on *properties* and *indexers*.</span></span> <span data-ttu-id="18b83-242">以下代码演示了每种情况的示例：</span><span class="sxs-lookup"><span data-stu-id="18b83-242">The following code shows examples of each:</span></span>

[!code-csharp[ExpressionBodiedMembers](~/samples/snippets/csharp/new-in-7/expressionmembers.cs#ExpressionBodiedEverything "new expression-bodied members")]

> [!NOTE]
> <span data-ttu-id="18b83-243">本示例不需要终结器，但显示它是为了演示语法。</span><span class="sxs-lookup"><span data-stu-id="18b83-243">This example does not need a finalizer, but it is shown to demonstrate the syntax.</span></span> <span data-ttu-id="18b83-244">不应在类中实现终结器，除非有必要发布非托管资源。</span><span class="sxs-lookup"><span data-stu-id="18b83-244">You should not implement a finalizer in your class unless it is necessary to  release unmanaged resources.</span></span> <span data-ttu-id="18b83-245">还应考虑使用 <xref:System.Runtime.InteropServices.SafeHandle> 类，而不是直接管理非托管资源。</span><span class="sxs-lookup"><span data-stu-id="18b83-245">You should also consider using the <xref:System.Runtime.InteropServices.SafeHandle> class instead of managing unmanaged resources directly.</span></span>

<span data-ttu-id="18b83-246">这些 expression-bodied 成员的新位置代表了 C# 语言的一个重要里程碑：这些功能由致力于开发开放源代码 [Roslyn](https://github.com/dotnet/Roslyn) 项目的社区成员实现。</span><span class="sxs-lookup"><span data-stu-id="18b83-246">These new locations for expression-bodied members represent an important milestone for the C# language: These features were implemented by community members working on the open-source [Roslyn](https://github.com/dotnet/Roslyn) project.</span></span>

<span data-ttu-id="18b83-247">将方法更改为 expression bodied 成员是[二进制兼容的更改](version-update-considerations.md#binary-compatible-changes)。</span><span class="sxs-lookup"><span data-stu-id="18b83-247">Changing a method to an expression bodied member is a [binary compatible change](version-update-considerations.md#binary-compatible-changes).</span></span>

## <a name="throw-expressions"></a><span data-ttu-id="18b83-248">引发表达式</span><span class="sxs-lookup"><span data-stu-id="18b83-248">Throw expressions</span></span>

<span data-ttu-id="18b83-249">在 C# 中，`throw` 始终是一个语句。</span><span class="sxs-lookup"><span data-stu-id="18b83-249">In C#, `throw` has always been a statement.</span></span> <span data-ttu-id="18b83-250">因为 `throw` 是一个语句而非表达式，所以在某些 C# 构造中无法使用它。</span><span class="sxs-lookup"><span data-stu-id="18b83-250">Because `throw` is a statement, not an expression, there were C# constructs where you couldn't use it.</span></span> <span data-ttu-id="18b83-251">它们包括条件表达式、null 合并表达式和一些 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="18b83-251">These included conditional expressions, null coalescing expressions, and some lambda expressions.</span></span> <span data-ttu-id="18b83-252">添加 expression-bodied 成员将添加更多位置，在这些位置中，`throw` 表达式会很有用。</span><span class="sxs-lookup"><span data-stu-id="18b83-252">The addition of expression-bodied members adds more locations where `throw` expressions would be useful.</span></span> <span data-ttu-id="18b83-253">为了可以编写这些构造，C# 7.0 引入了 [*throw 表达式*](../language-reference/keywords/throw.md#the-throw-expression)。</span><span class="sxs-lookup"><span data-stu-id="18b83-253">So that you can write any of these constructs, C# 7.0 introduces [*throw expressions*](../language-reference/keywords/throw.md#the-throw-expression).</span></span>

<span data-ttu-id="18b83-254">这使得编写更多基于表达式的代码变得更容易。</span><span class="sxs-lookup"><span data-stu-id="18b83-254">This addition makes it easier to write more expression-based code.</span></span> <span data-ttu-id="18b83-255">不需要其他语句来进行错误检查。</span><span class="sxs-lookup"><span data-stu-id="18b83-255">You don't need additional statements for error checking.</span></span>

## <a name="default-literal-expressions"></a><span data-ttu-id="18b83-256">默认文本表达式</span><span class="sxs-lookup"><span data-stu-id="18b83-256">Default literal expressions</span></span>

<span data-ttu-id="18b83-257">默认文本表达式是针对默认值表达式的一项增强功能。</span><span class="sxs-lookup"><span data-stu-id="18b83-257">Default literal expressions are an enhancement to default value expressions.</span></span> <span data-ttu-id="18b83-258">这些表达式将变量初始化为默认值。</span><span class="sxs-lookup"><span data-stu-id="18b83-258">These expressions initialize a variable to the default value.</span></span> <span data-ttu-id="18b83-259">过去会这么编写：</span><span class="sxs-lookup"><span data-stu-id="18b83-259">Where you previously would write:</span></span>

```csharp
Func<string, bool> whereClause = default(Func<string, bool>);
```

<span data-ttu-id="18b83-260">现在，可以省略掉初始化右侧的类型：</span><span class="sxs-lookup"><span data-stu-id="18b83-260">You can now omit the type on the right-hand side of the initialization:</span></span>

```csharp
Func<string, bool> whereClause = default;
```

<span data-ttu-id="18b83-261">有关详细信息，请参阅 [default 运算符](../language-reference/operators/default.md)一文中的 [default 文本](../language-reference/operators/default.md#default-literal)部分。</span><span class="sxs-lookup"><span data-stu-id="18b83-261">For more information, see the [default literal](../language-reference/operators/default.md#default-literal) section of the [default operator](../language-reference/operators/default.md) article.</span></span>

## <a name="numeric-literal-syntax-improvements"></a><span data-ttu-id="18b83-262">数字文本语法改进</span><span class="sxs-lookup"><span data-stu-id="18b83-262">Numeric literal syntax improvements</span></span>

<span data-ttu-id="18b83-263">误读的数值常量可能使第一次阅读代码时更难理解。</span><span class="sxs-lookup"><span data-stu-id="18b83-263">Misreading numeric constants can make it harder to understand code when reading it for the first time.</span></span> <span data-ttu-id="18b83-264">位掩码或其他符号值容易产生误解。</span><span class="sxs-lookup"><span data-stu-id="18b83-264">Bit masks or other symbolic values are prone to misunderstanding.</span></span> <span data-ttu-id="18b83-265">C# 7.0 包括两项新功能，可用于以最可读的方式写入数字来用于预期用途：二进制文本和数字分隔符 。</span><span class="sxs-lookup"><span data-stu-id="18b83-265">C# 7.0 includes two new features to write numbers in the most readable fashion for the intended use: *binary literals*, and *digit separators*.</span></span>

<span data-ttu-id="18b83-266">在创建位掩码时，或每当数字的二进制表示形式使代码最具可读性时，以二进制形式写入该数字：</span><span class="sxs-lookup"><span data-stu-id="18b83-266">For those times when you're creating bit masks, or whenever a binary representation of a number makes the most readable code, write that number in binary:</span></span>

[!code-csharp[ThousandSeparators](~/samples/snippets/csharp/new-in-7/Program.cs#ThousandSeparators "Thousands separators")]

<span data-ttu-id="18b83-267">常量开头的 `0b` 表示该数字以二进制数形式写入。</span><span class="sxs-lookup"><span data-stu-id="18b83-267">The `0b` at the beginning of the constant indicates that the number is written as a binary number.</span></span> <span data-ttu-id="18b83-268">二进制数可能会很长，因此通过引入 `_` 作为数字分隔符通常更易于查看位模式，如前面示例中的二进制常量所示。</span><span class="sxs-lookup"><span data-stu-id="18b83-268">Binary numbers can get long, so it's often easier to see the bit patterns by introducing the `_` as a digit separator, as shown in the binary constant in the preceding example.</span></span> <span data-ttu-id="18b83-269">数字分隔符可以出现在常量的任何位置。</span><span class="sxs-lookup"><span data-stu-id="18b83-269">The digit separator can appear anywhere in the constant.</span></span> <span data-ttu-id="18b83-270">对于十进制数字，通常将其用作千位分隔符。</span><span class="sxs-lookup"><span data-stu-id="18b83-270">For base 10 numbers, it is common to use it as a thousands separator.</span></span> <span data-ttu-id="18b83-271">十六进制和二进制文本可采用 `_` 开头：</span><span class="sxs-lookup"><span data-stu-id="18b83-271">Hex and binary numeric literals may begin with an `_`:</span></span>

[!code-csharp[LargeIntegers](~/samples/snippets/csharp/new-in-7/Program.cs#LargeIntegers "Large integer")]

<span data-ttu-id="18b83-272">数字分隔符也可以与 `decimal`、`float` 和 `double` 类型一起使用：</span><span class="sxs-lookup"><span data-stu-id="18b83-272">The digit separator can be used with `decimal`, `float`, and `double` types as well:</span></span>

[!code-csharp[OtherConstants](~/samples/snippets/csharp/new-in-7/Program.cs#OtherConstants "non-integral constants")]

<span data-ttu-id="18b83-273">综观来说，你可以声明可读性更强的数值常量。</span><span class="sxs-lookup"><span data-stu-id="18b83-273">Taken together, you can declare numeric constants with much more readability.</span></span>

## <a name="out-variables"></a><span data-ttu-id="18b83-274">`out` 变量</span><span class="sxs-lookup"><span data-stu-id="18b83-274">`out` variables</span></span>

<span data-ttu-id="18b83-275">支持 `out` 参数的现有语法已在 C# 7 中得到改进。</span><span class="sxs-lookup"><span data-stu-id="18b83-275">The existing syntax that supports `out` parameters has been improved in C# 7.</span></span> <span data-ttu-id="18b83-276">现在可以在方法调用的参数列表中声明 `out` 变量，而不是编写单独的声明语句：</span><span class="sxs-lookup"><span data-stu-id="18b83-276">You can now declare `out` variables in the argument list of a method call, rather than writing a separate declaration statement:</span></span>

[!code-csharp[OutVariableDeclarations](~/samples/snippets/csharp/new-in-7/program.cs#OutVariableDeclarations "Out variable declarations")]

<span data-ttu-id="18b83-277">为清楚起见，我们建议你指定 `out` 变量的类型，如前面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="18b83-277">You may want to specify the type of the `out` variable for clarity, as shown in the preceding example.</span></span> <span data-ttu-id="18b83-278">但是，该语言支持使用隐式类型的局部变量：</span><span class="sxs-lookup"><span data-stu-id="18b83-278">However, the language does support using an implicitly typed local variable:</span></span>

[!code-csharp[OutVarVariableDeclarations](~/samples/snippets/csharp/new-in-7/program.cs#OutVarVariableDeclarations "Implicitly typed Out variable")]

- <span data-ttu-id="18b83-279">代码更易于阅读。</span><span class="sxs-lookup"><span data-stu-id="18b83-279">The code is easier to read.</span></span>
  - <span data-ttu-id="18b83-280">在使用 out 变量的地方声明 out 变量，而不是在上面的代码行中声明。</span><span class="sxs-lookup"><span data-stu-id="18b83-280">You declare the out variable where you use it, not on a preceding line of code.</span></span>
- <span data-ttu-id="18b83-281">无需分配初始值。</span><span class="sxs-lookup"><span data-stu-id="18b83-281">No need to assign an initial value.</span></span>
  - <span data-ttu-id="18b83-282">通过在方法调用中使用 `out` 变量的位置声明该变量，使得在分配它之前不可能意外使用它。</span><span class="sxs-lookup"><span data-stu-id="18b83-282">By declaring the `out` variable where it's used in a method call, you can't accidentally use it before it is assigned.</span></span>

<span data-ttu-id="18b83-283">已对在 C# 7.0 中添加的允许 `out` 变量声明的语法进行了扩展，以包含字段初始值设定项、属性初始值设定项、构造函数初始值设定项和查询子句。</span><span class="sxs-lookup"><span data-stu-id="18b83-283">The syntax added in C# 7.0 to allow `out` variable declarations has been extended to include field initializers, property initializers, constructor initializers, and query clauses.</span></span> <span data-ttu-id="18b83-284">它允许使用如以下示例中所示的代码：</span><span class="sxs-lookup"><span data-stu-id="18b83-284">It enables code such as the following example:</span></span>

```csharp
public class B
{
   public B(int i, out int j)
   {
      j = i;
   }
}

public class D : B
{
   public D(int i) : base(i, out var j)
   {
      Console.WriteLine($"The value of 'j' is {j}");
   }
}
```

## <a name="non-trailing-named-arguments"></a><span data-ttu-id="18b83-285">非尾随命名参数</span><span class="sxs-lookup"><span data-stu-id="18b83-285">Non-trailing named arguments</span></span>

<span data-ttu-id="18b83-286">方法调用现可使用位于位置参数前面的命名参数（若这些命名参数的位置正确）。</span><span class="sxs-lookup"><span data-stu-id="18b83-286">Method calls may now use named arguments that precede positional arguments when those named arguments are in the correct positions.</span></span> <span data-ttu-id="18b83-287">如需了解详情，请参阅[命名参数和可选参数](../programming-guide/classes-and-structs/named-and-optional-arguments.md)。</span><span class="sxs-lookup"><span data-stu-id="18b83-287">For more information, see [Named and optional arguments](../programming-guide/classes-and-structs/named-and-optional-arguments.md).</span></span>

## <a name="private-protected-access-modifier"></a><span data-ttu-id="18b83-288">*private protected* 访问修饰符</span><span class="sxs-lookup"><span data-stu-id="18b83-288">*private protected* access modifier</span></span>

<span data-ttu-id="18b83-289">新的复合访问修饰符：`private protected` 指示可通过包含同一程序集中声明的类或派生类来访问成员。</span><span class="sxs-lookup"><span data-stu-id="18b83-289">A new compound access modifier: `private protected` indicates that a member may be accessed by containing class or derived classes that are declared in the same assembly.</span></span> <span data-ttu-id="18b83-290">虽然 `protected internal` 允许通过同一程序集中的类或派生类进行访问，但 `private protected` 限制对同一程序集中声明的派生类的访问。</span><span class="sxs-lookup"><span data-stu-id="18b83-290">While `protected internal` allows access by derived classes or classes that are in the same assembly, `private protected` limits access to derived types declared in the same assembly.</span></span>

<span data-ttu-id="18b83-291">如需了解详情，请参阅语言参考中的[访问修饰符](../language-reference/keywords/access-modifiers.md)。</span><span class="sxs-lookup"><span data-stu-id="18b83-291">For more information, see [access modifiers](../language-reference/keywords/access-modifiers.md) in the language reference.</span></span>

## <a name="improved-overload-candidates"></a><span data-ttu-id="18b83-292">改进了重载候选项</span><span class="sxs-lookup"><span data-stu-id="18b83-292">Improved overload candidates</span></span>

<span data-ttu-id="18b83-293">在每个版本中，对重载解析规则进行了更新，以解决多义方法调用具有“明显”选择的情况。</span><span class="sxs-lookup"><span data-stu-id="18b83-293">In every release, the overload resolution rules get updated to address situations where ambiguous method invocations have an "obvious" choice.</span></span> <span data-ttu-id="18b83-294">此版本添加了三个新规则，以帮助编译器选取明显的选择：</span><span class="sxs-lookup"><span data-stu-id="18b83-294">This release adds three new rules to help the compiler pick the obvious choice:</span></span>

1. <span data-ttu-id="18b83-295">当方法组同时包含实例和静态成员时，如果方法在不含实例接收器或上下文的情况下被调用，则编译器将丢弃实例成员。</span><span class="sxs-lookup"><span data-stu-id="18b83-295">When a method group contains both instance and static members, the compiler discards the instance members if the method was invoked without an instance receiver or context.</span></span> <span data-ttu-id="18b83-296">如果方法在含有实例接收器的情况下被调用，则编译器将丢弃静态成员。</span><span class="sxs-lookup"><span data-stu-id="18b83-296">The compiler discards the static members if the method was invoked with an instance receiver.</span></span> <span data-ttu-id="18b83-297">在没有接收器时，编译器将仅添加静态上下文中的静态成员，否则，将同时添加静态成员和实例成员。</span><span class="sxs-lookup"><span data-stu-id="18b83-297">When there is no receiver, the compiler includes only static members in a static context, otherwise both static and instance members.</span></span> <span data-ttu-id="18b83-298">当接收器是不明确的实例或类型时，编译器将同时添加两者。</span><span class="sxs-lookup"><span data-stu-id="18b83-298">When the receiver is ambiguously an instance or type, the compiler includes both.</span></span> <span data-ttu-id="18b83-299">静态上下文（其中隐式 `this` 实例接收器无法使用）包含未定义 `this` 的成员的正文（例如，静态成员），以及不能使用 `this` 的位置（例如，字段初始值设定项和构造函数初始值设定项）。</span><span class="sxs-lookup"><span data-stu-id="18b83-299">A static context, where an implicit `this` instance receiver cannot be used, includes the body of members where no `this` is defined, such as static members, as well as places where `this` cannot be used, such as field initializers and constructor-initializers.</span></span>
1. <span data-ttu-id="18b83-300">当一个方法组包含类型参数不满足其约束的某些泛型方法时，这些成员将从候选集中移除。</span><span class="sxs-lookup"><span data-stu-id="18b83-300">When a method group contains some generic methods whose type arguments do not satisfy their constraints, these members are removed from the candidate set.</span></span>
1. <span data-ttu-id="18b83-301">对于方法组转换，返回类型与委托的返回类型不匹配的候选方法将从集中移除。</span><span class="sxs-lookup"><span data-stu-id="18b83-301">For a method group conversion, candidate methods whose return type doesn't match up with the delegate's return type are removed from the set.</span></span>

<span data-ttu-id="18b83-302">你将注意到此更改，因为当你确定哪个方法更好时，你将发现多义方法重载具有更少的编译器错误。</span><span class="sxs-lookup"><span data-stu-id="18b83-302">You'll only notice this change because you'll find fewer compiler errors for ambiguous method overloads when you are sure which method is better.</span></span>

## <a name="enabling-more-efficient-safe-code"></a><span data-ttu-id="18b83-303">启用更高效的安全代码</span><span class="sxs-lookup"><span data-stu-id="18b83-303">Enabling more efficient safe code</span></span>

<span data-ttu-id="18b83-304">你应能够安全地编写性能与不安全代码一样好的 C# 代码。</span><span class="sxs-lookup"><span data-stu-id="18b83-304">You should be able to write C# code safely that performs as well as unsafe code.</span></span> <span data-ttu-id="18b83-305">安全代码可避免错误类，例如缓冲区溢出、杂散指针和其他内存访问错误。</span><span class="sxs-lookup"><span data-stu-id="18b83-305">Safe code avoids classes of errors, such as buffer overruns, stray pointers, and other memory access errors.</span></span> <span data-ttu-id="18b83-306">这些新功能扩展了可验证安全代码的功能。</span><span class="sxs-lookup"><span data-stu-id="18b83-306">These new features expand the capabilities of verifiable safe code.</span></span> <span data-ttu-id="18b83-307">努力使用安全结构编写更多代码。</span><span class="sxs-lookup"><span data-stu-id="18b83-307">Strive to write more of your code using safe constructs.</span></span> <span data-ttu-id="18b83-308">这些功能使其更容易实现。</span><span class="sxs-lookup"><span data-stu-id="18b83-308">These features make that easier.</span></span>

<span data-ttu-id="18b83-309">以下新增功能支持使安全代码获得更好的性能的主题：</span><span class="sxs-lookup"><span data-stu-id="18b83-309">The following new features support the theme of better performance for safe code:</span></span>

- <span data-ttu-id="18b83-310">无需固定即可访问固定的字段。</span><span class="sxs-lookup"><span data-stu-id="18b83-310">You can access fixed fields without pinning.</span></span>
- <span data-ttu-id="18b83-311">可以重新分配 `ref` 本地变量。</span><span class="sxs-lookup"><span data-stu-id="18b83-311">You can reassign `ref` local variables.</span></span>
- <span data-ttu-id="18b83-312">可以使用 `stackalloc` 数组上的初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="18b83-312">You can use initializers on `stackalloc` arrays.</span></span>
- <span data-ttu-id="18b83-313">可以对支持模式的任何类型使用 `fixed` 语句。</span><span class="sxs-lookup"><span data-stu-id="18b83-313">You can use `fixed` statements with any type that supports a pattern.</span></span>
- <span data-ttu-id="18b83-314">可以使用其他泛型约束。</span><span class="sxs-lookup"><span data-stu-id="18b83-314">You can use additional generic constraints.</span></span>
- <span data-ttu-id="18b83-315">针对实参的 `in` 修饰符，指定形参通过引用传递，但不通过调用方法修改。</span><span class="sxs-lookup"><span data-stu-id="18b83-315">The `in` modifier on parameters, to specify that an argument is passed by reference but not modified by the called method.</span></span> <span data-ttu-id="18b83-316">将 `in` 修饰符添加到参数是[源兼容的更改](version-update-considerations.md#source-compatible-changes)。</span><span class="sxs-lookup"><span data-stu-id="18b83-316">Adding the `in` modifier to an argument is a [source compatible change](version-update-considerations.md#source-compatible-changes).</span></span>
- <span data-ttu-id="18b83-317">针对方法返回的 `ref readonly` 修饰符，指示方法通过引用返回其值，但不允许写入该对象。</span><span class="sxs-lookup"><span data-stu-id="18b83-317">The `ref readonly` modifier on method returns, to indicate that a method returns its value by reference but doesn't allow writes to that object.</span></span> <span data-ttu-id="18b83-318">如果向某个值赋予返回值，则添加 `ref readonly` 修饰符是[源兼容的更改](version-update-considerations.md#source-compatible-changes)。</span><span class="sxs-lookup"><span data-stu-id="18b83-318">Adding the `ref readonly` modifier is a [source compatible change](version-update-considerations.md#source-compatible-changes), if the return is assigned to a value.</span></span> <span data-ttu-id="18b83-319">将 `readonly` 修饰符添加到现有的 `ref` 返回语句是[不兼容的更改](version-update-considerations.md#incompatible-changes)。</span><span class="sxs-lookup"><span data-stu-id="18b83-319">Adding the `readonly` modifier to an existing `ref` return statement is an [incompatible change](version-update-considerations.md#incompatible-changes).</span></span> <span data-ttu-id="18b83-320">它要求调用方更新 `ref` 本地变量的声明以包含 `readonly` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="18b83-320">It requires callers to update the declaration of `ref` local variables to include the `readonly` modifier.</span></span>
- <span data-ttu-id="18b83-321">`readonly struct` 声明，指示结构不可变，且应作为 `in` 参数传递到其成员方法。</span><span class="sxs-lookup"><span data-stu-id="18b83-321">The `readonly struct` declaration, to indicate that a struct is immutable and should be passed as an `in` parameter to its member methods.</span></span> <span data-ttu-id="18b83-322">将 `readonly` 修饰符添加到现有的结构声明是[二进制兼容的更改](version-update-considerations.md#binary-compatible-changes)。</span><span class="sxs-lookup"><span data-stu-id="18b83-322">Adding the `readonly` modifier to an existing struct declaration is a [binary compatible change](version-update-considerations.md#binary-compatible-changes).</span></span>
- <span data-ttu-id="18b83-323">`ref struct` 声明，指示结构类型直接访问托管的内存，且必须始终分配有堆栈。</span><span class="sxs-lookup"><span data-stu-id="18b83-323">The `ref struct` declaration, to indicate that a struct type accesses managed memory directly and must always be stack allocated.</span></span> <span data-ttu-id="18b83-324">将 `ref` 修饰符添加到现有 `struct` 声明是[不兼容的更改](version-update-considerations.md#incompatible-changes)。</span><span class="sxs-lookup"><span data-stu-id="18b83-324">Adding the `ref` modifier to an existing `struct` declaration is an [incompatible change](version-update-considerations.md#incompatible-changes).</span></span> <span data-ttu-id="18b83-325">`ref struct` 不能是类的成员，也不能用于可能在堆上分配的其他位置。</span><span class="sxs-lookup"><span data-stu-id="18b83-325">A `ref struct` cannot be a member of a class or used in other locations where it may be allocated on the heap.</span></span>

<span data-ttu-id="18b83-326">可以在[编写安全高效的代码](../write-safe-efficient-code.md)中详细了解所有这些更改。</span><span class="sxs-lookup"><span data-stu-id="18b83-326">You can read more about all these changes in [Write safe efficient code](../write-safe-efficient-code.md).</span></span>

### <a name="ref-locals-and-returns"></a><span data-ttu-id="18b83-327">Ref 局部变量和返回结果</span><span class="sxs-lookup"><span data-stu-id="18b83-327">Ref locals and returns</span></span>

<span data-ttu-id="18b83-328">此功能允许使用并返回对变量的引用的算法，这些变量在其他位置定义。</span><span class="sxs-lookup"><span data-stu-id="18b83-328">This feature enables algorithms that use and return references to variables defined elsewhere.</span></span> <span data-ttu-id="18b83-329">一个示例是使用大型矩阵并查找具有某些特征的单个位置。</span><span class="sxs-lookup"><span data-stu-id="18b83-329">One example is working with large matrices, and finding a single location with certain characteristics.</span></span> <span data-ttu-id="18b83-330">下面的方法在矩阵中向该存储返回“引用”：</span><span class="sxs-lookup"><span data-stu-id="18b83-330">The following method returns a **reference** to that storage in the matrix:</span></span>

[!code-csharp[FindReturningRef](~/samples/snippets/csharp/new-in-7/MatrixSearch.cs#FindReturningRef "Find returning by reference")]

<span data-ttu-id="18b83-331">可以将返回值声明为 `ref` 并在矩阵中修改该值，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="18b83-331">You can declare the return value as a `ref` and modify that value in the matrix, as shown in the following code:</span></span>

[!code-csharp[AssignRefReturn](~/samples/snippets/csharp/new-in-7/Program.cs#AssignRefReturn "Assign ref return")]

<span data-ttu-id="18b83-332">C# 语言还有多个规则，可保护你免于误用 `ref` 局部变量和返回结果：</span><span class="sxs-lookup"><span data-stu-id="18b83-332">The C# language has several rules that protect you from misusing the `ref` locals and returns:</span></span>

- <span data-ttu-id="18b83-333">必须将 `ref` 关键字添加到方法签名和方法中的所有 `return` 语句中。</span><span class="sxs-lookup"><span data-stu-id="18b83-333">You must add the `ref` keyword to the method signature and to all `return` statements in a method.</span></span>
  - <span data-ttu-id="18b83-334">这清楚地表明，该方法在整个方法中通过引用返回。</span><span class="sxs-lookup"><span data-stu-id="18b83-334">That makes it clear the method returns by reference throughout the method.</span></span>
- <span data-ttu-id="18b83-335">可以将 `ref return` 分配给值变量或 `ref` 变量。</span><span class="sxs-lookup"><span data-stu-id="18b83-335">A `ref return` may be assigned to a value variable, or a `ref` variable.</span></span>
  - <span data-ttu-id="18b83-336">调用方控制是否复制返回值。</span><span class="sxs-lookup"><span data-stu-id="18b83-336">The caller controls whether the return value is copied or not.</span></span> <span data-ttu-id="18b83-337">在分配返回值时省略 `ref` 修饰符表示调用方需要该值的副本，而不是对存储的引用。</span><span class="sxs-lookup"><span data-stu-id="18b83-337">Omitting the `ref` modifier when assigning the return value indicates that the caller wants a copy of the value, not a reference to the storage.</span></span>
- <span data-ttu-id="18b83-338">不可向 `ref` 本地变量赋予标准方法返回值。</span><span class="sxs-lookup"><span data-stu-id="18b83-338">You can't assign a standard method return value to a `ref` local variable.</span></span>
  - <span data-ttu-id="18b83-339">因为那将禁止类似 `ref int i = sequence.Count();` 这样的语句</span><span class="sxs-lookup"><span data-stu-id="18b83-339">That disallows statements like `ref int i = sequence.Count();`</span></span>
- <span data-ttu-id="18b83-340">不能将 `ref` 返回给其生存期不超出方法执行的变量。</span><span class="sxs-lookup"><span data-stu-id="18b83-340">You can't return a `ref` to a variable whose lifetime doesn't extend beyond the execution of the method.</span></span>
  - <span data-ttu-id="18b83-341">这意味着不可返回对本地变量或对类似作用域变量的引用。</span><span class="sxs-lookup"><span data-stu-id="18b83-341">That means you can't return a reference to a local variable or a variable with a similar scope.</span></span>
- <span data-ttu-id="18b83-342">`ref` 局部变量和返回结果不可用于异步方法。</span><span class="sxs-lookup"><span data-stu-id="18b83-342">`ref` locals and returns can't be used with async methods.</span></span>
  - <span data-ttu-id="18b83-343">编译器无法知道异步方法返回时，引用的变量是否已设置为其最终值。</span><span class="sxs-lookup"><span data-stu-id="18b83-343">The compiler can't know if the referenced variable has been set to its final value when the async method returns.</span></span>

<span data-ttu-id="18b83-344">添加 ref 局部变量和 ref 返回结果可通过避免复制值或多次执行取消引用操作，允许更为高效的算法。</span><span class="sxs-lookup"><span data-stu-id="18b83-344">The addition of ref locals and ref returns enables algorithms that are more efficient by avoiding copying values, or performing dereferencing operations multiple times.</span></span>

<span data-ttu-id="18b83-345">向返回值添加 `ref` 是[源兼容的更改](version-update-considerations.md#source-compatible-changes)。</span><span class="sxs-lookup"><span data-stu-id="18b83-345">Adding `ref` to the return value is a [source compatible change](version-update-considerations.md#source-compatible-changes).</span></span> <span data-ttu-id="18b83-346">现有代码会进行编译，但在分配时复制 ref 返回值。</span><span class="sxs-lookup"><span data-stu-id="18b83-346">Existing code compiles, but the ref return value is copied when assigned.</span></span> <span data-ttu-id="18b83-347">调用方必须将存储的返回值更新为 `ref` 局部变量，从而将返回值存储为引用。</span><span class="sxs-lookup"><span data-stu-id="18b83-347">Callers must update the storage for the return value to a `ref` local variable to store the return as a reference.</span></span>

<span data-ttu-id="18b83-348">现在，在对 `ref` 局部变量进行初始化后，可能会对其重新分配，以引用不同的实例。</span><span class="sxs-lookup"><span data-stu-id="18b83-348">Now, `ref` locals may be reassigned to refer to different instances after being initialized.</span></span> <span data-ttu-id="18b83-349">以下代码现在编译：</span><span class="sxs-lookup"><span data-stu-id="18b83-349">The following code now compiles:</span></span>

```csharp
ref VeryLargeStruct refLocal = ref veryLargeStruct; // initialization
refLocal = ref anotherVeryLargeStruct; // reassigned, refLocal refers to different storage.
```

<span data-ttu-id="18b83-350">有关详细信息，请参阅有关 [`ref` 返回和 `ref` 局部变量](../programming-guide/classes-and-structs/ref-returns.md)以及 [`foreach`](../language-reference/keywords/foreach-in.md) 的文章。</span><span class="sxs-lookup"><span data-stu-id="18b83-350">For more information, see the article on [`ref` returns and `ref` locals](../programming-guide/classes-and-structs/ref-returns.md), and the article on [`foreach`](../language-reference/keywords/foreach-in.md).</span></span>

<span data-ttu-id="18b83-351">有关详细信息，请参阅 [ref 关键字](../language-reference/keywords/ref.md)一文。</span><span class="sxs-lookup"><span data-stu-id="18b83-351">For more information, see the [ref keyword](../language-reference/keywords/ref.md) article.</span></span>

### <a name="conditional-ref-expressions"></a><span data-ttu-id="18b83-352">条件 `ref` 表达式</span><span class="sxs-lookup"><span data-stu-id="18b83-352">Conditional `ref` expressions</span></span>

<span data-ttu-id="18b83-353">最后，条件表达式可能生成 ref 结果而不是值。</span><span class="sxs-lookup"><span data-stu-id="18b83-353">Finally, the conditional expression may produce a ref result instead of a value result.</span></span> <span data-ttu-id="18b83-354">例如，你将编写以下内容以检索对两个数组之一中第一个元素的引用：</span><span class="sxs-lookup"><span data-stu-id="18b83-354">For example, you would write the following to retrieve a reference to the first element in one of two arrays:</span></span>

```csharp
ref var r = ref (arr != null ? ref arr[0] : ref otherArr[0]);
```

<span data-ttu-id="18b83-355">变量 `r` 是对 `arr` 或 `otherArr` 中第一个值的引用。</span><span class="sxs-lookup"><span data-stu-id="18b83-355">The variable `r` is a reference to the first value in either `arr` or `otherArr`.</span></span>

<span data-ttu-id="18b83-356">有关详细信息，请参阅语言参考中的[条件运算符 (?:)](../language-reference/operators/conditional-operator.md)。</span><span class="sxs-lookup"><span data-stu-id="18b83-356">For more information, see [conditional operator (?:)](../language-reference/operators/conditional-operator.md) in the language reference.</span></span>

### <a name="in-parameter-modifier"></a><span data-ttu-id="18b83-357">`in` 参数修饰符</span><span class="sxs-lookup"><span data-stu-id="18b83-357">`in` parameter modifier</span></span>

<span data-ttu-id="18b83-358">`in` 关键字补充了现有的 ref 和 out 关键字，以按引用传递参数。</span><span class="sxs-lookup"><span data-stu-id="18b83-358">The `in` keyword complements the existing ref and out keywords to pass arguments by reference.</span></span> <span data-ttu-id="18b83-359">`in` 关键字指定按引用传递参数，但调用的方法不修改值。</span><span class="sxs-lookup"><span data-stu-id="18b83-359">The `in` keyword specifies passing the argument by reference, but the called method doesn't modify the value.</span></span>

<span data-ttu-id="18b83-360">你可以声明按值或只读引用传递的重载，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="18b83-360">You may declare overloads that pass by value or by readonly reference, as shown in the following code:</span></span>

```csharp
static void M(S arg);
static void M(in S arg);
```

<span data-ttu-id="18b83-361">按值（前面示例中的第一个）传递的重载比按只读引用传递的重载更好。</span><span class="sxs-lookup"><span data-stu-id="18b83-361">The by value (first in the preceding example) overload is better than the by readonly reference version.</span></span> <span data-ttu-id="18b83-362">若要使用只读引用参数调用版本，必须在调用方法前添加 `in` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="18b83-362">To call the version with the readonly reference argument, you must include the `in` modifier when calling the method.</span></span>

<span data-ttu-id="18b83-363">有关详细信息，请参阅有关 [`in` 参数修饰符](../language-reference/keywords/in-parameter-modifier.md)的文章。</span><span class="sxs-lookup"><span data-stu-id="18b83-363">For more information, see the article on the [`in` parameter modifier](../language-reference/keywords/in-parameter-modifier.md).</span></span>

### <a name="more-types-support-the-fixed-statement"></a><span data-ttu-id="18b83-364">更多类型支持 `fixed` 语句</span><span class="sxs-lookup"><span data-stu-id="18b83-364">More types support the `fixed` statement</span></span>

<span data-ttu-id="18b83-365">`fixed` 语句支持有限的一组类型。</span><span class="sxs-lookup"><span data-stu-id="18b83-365">The `fixed` statement supported a limited set of types.</span></span> <span data-ttu-id="18b83-366">从 C# 7.3 开始，任何包含返回 `ref T` 或 `ref readonly T` 的 `GetPinnableReference()` 方法的类型均有可能为 `fixed`。</span><span class="sxs-lookup"><span data-stu-id="18b83-366">Starting with C# 7.3, any type that contains a `GetPinnableReference()` method that returns a `ref T` or `ref readonly T` may be `fixed`.</span></span> <span data-ttu-id="18b83-367">添加此功能意味着 `fixed` 可与 <xref:System.Span%601?displayProperty=nameWithType> 和相关类型配合使用。</span><span class="sxs-lookup"><span data-stu-id="18b83-367">Adding this feature means that `fixed` can be used with <xref:System.Span%601?displayProperty=nameWithType> and related types.</span></span>

<span data-ttu-id="18b83-368">有关详细信息，请参阅语言参考中的 [`fixed` 语句](../language-reference/keywords/fixed-statement.md)一文。</span><span class="sxs-lookup"><span data-stu-id="18b83-368">For more information, see the [`fixed` statement](../language-reference/keywords/fixed-statement.md) article in the language reference.</span></span>

### <a name="indexing-fixed-fields-does-not-require-pinning"></a><span data-ttu-id="18b83-369">索引 `fixed` 字段不需要进行固定</span><span class="sxs-lookup"><span data-stu-id="18b83-369">Indexing `fixed` fields does not require pinning</span></span>

<span data-ttu-id="18b83-370">请考虑此结构：</span><span class="sxs-lookup"><span data-stu-id="18b83-370">Consider this struct:</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}
```

<span data-ttu-id="18b83-371">在早期版本的 C# 中，需要固定变量才能访问属于 `myFixedField` 的整数之一。</span><span class="sxs-lookup"><span data-stu-id="18b83-371">In earlier versions of C#, you needed to pin a variable to access one of the integers that are part of `myFixedField`.</span></span> <span data-ttu-id="18b83-372">现在，以下代码进行编译，而不将变量 `p` 固定到单独的 `fixed` 语句中：</span><span class="sxs-lookup"><span data-stu-id="18b83-372">Now, the following code compiles without pinning the variable `p` inside a separate `fixed` statement:</span></span>

```csharp
class C
{
    static S s = new S();

    unsafe public void M()
    {
        int p = s.myFixedField[5];
    }
}
```

<span data-ttu-id="18b83-373">变量 `p` 访问 `myFixedField` 中的一个元素。</span><span class="sxs-lookup"><span data-stu-id="18b83-373">The variable `p` accesses one element in `myFixedField`.</span></span> <span data-ttu-id="18b83-374">无需声明单独的 `int*` 变量。</span><span class="sxs-lookup"><span data-stu-id="18b83-374">You don't need to declare a separate `int*` variable.</span></span> <span data-ttu-id="18b83-375">仍需要 `unsafe` 上下文。</span><span class="sxs-lookup"><span data-stu-id="18b83-375">You still need an `unsafe` context.</span></span> <span data-ttu-id="18b83-376">在早期版本的 C# 中，需要声明第二个固定的指针：</span><span class="sxs-lookup"><span data-stu-id="18b83-376">In earlier versions of C#, you need to declare a second fixed pointer:</span></span>

```csharp
class C
{
    static S s = new S();

    unsafe public void M()
    {
        fixed (int* ptr = s.myFixedField)
        {
            int p = ptr[5];
        }
    }
}
```

<span data-ttu-id="18b83-377">有关详细信息，请参阅有关 [`fixed` 语句](../language-reference/keywords/fixed-statement.md)的文章。</span><span class="sxs-lookup"><span data-stu-id="18b83-377">For more information, see the article on the [`fixed` statement](../language-reference/keywords/fixed-statement.md).</span></span>

### <a name="stackalloc-arrays-support-initializers"></a><span data-ttu-id="18b83-378">`stackalloc` 数组支持初始值设定项</span><span class="sxs-lookup"><span data-stu-id="18b83-378">`stackalloc` arrays support initializers</span></span>

<span data-ttu-id="18b83-379">当你对数组中的元素的值进行初始值设定时，你已能够指定该值：</span><span class="sxs-lookup"><span data-stu-id="18b83-379">You've been able to specify the values for elements in an array when you initialize it:</span></span>

```csharp
var arr = new int[3] {1, 2, 3};
var arr2 = new int[] {1, 2, 3};
```

<span data-ttu-id="18b83-380">现在，可向使用 `stackalloc` 进行声明的数组应用同一语法：</span><span class="sxs-lookup"><span data-stu-id="18b83-380">Now, that same syntax can be applied to arrays that are declared with `stackalloc`:</span></span>

```csharp
int* pArr = stackalloc int[3] {1, 2, 3};
int* pArr2 = stackalloc int[] {1, 2, 3};
Span<int> arr = stackalloc [] {1, 2, 3};
```

<span data-ttu-id="18b83-381">有关详细信息，请参阅[`stackalloc`运算符](../language-reference/operators/stackalloc.md)一文。</span><span class="sxs-lookup"><span data-stu-id="18b83-381">For more information, see the [`stackalloc` operator](../language-reference/operators/stackalloc.md) article.</span></span>

### <a name="enhanced-generic-constraints"></a><span data-ttu-id="18b83-382">增强的泛型约束</span><span class="sxs-lookup"><span data-stu-id="18b83-382">Enhanced generic constraints</span></span>

<span data-ttu-id="18b83-383">现在，可以将类型 <xref:System.Enum?displayProperty=nameWithType> 或 <xref:System.Delegate?displayProperty=nameWithType> 指定为类型参数的基类约束。</span><span class="sxs-lookup"><span data-stu-id="18b83-383">You can now specify the type <xref:System.Enum?displayProperty=nameWithType> or <xref:System.Delegate?displayProperty=nameWithType> as base class constraints for a type parameter.</span></span>

<span data-ttu-id="18b83-384">现在也可以使用新的 `unmanaged` 约束来指定类型参数必须是不可为 null 的[“非托管类型”](../language-reference/builtin-types/unmanaged-types.md)。</span><span class="sxs-lookup"><span data-stu-id="18b83-384">You can also use the new `unmanaged` constraint, to specify that a type parameter must be a non-nullable [unmanaged type](../language-reference/builtin-types/unmanaged-types.md).</span></span>

<span data-ttu-id="18b83-385">有关详细信息，请参阅有关 [`where` 泛型约束](../language-reference/keywords/where-generic-type-constraint.md)和[类型参数的约束](../programming-guide/generics/constraints-on-type-parameters.md)的文章。</span><span class="sxs-lookup"><span data-stu-id="18b83-385">For more information, see the articles on [`where` generic constraints](../language-reference/keywords/where-generic-type-constraint.md) and [constraints on type parameters](../programming-guide/generics/constraints-on-type-parameters.md).</span></span>

<span data-ttu-id="18b83-386">将这些约束添加到现有类型是[不兼容的更改](version-update-considerations.md#incompatible-changes)。</span><span class="sxs-lookup"><span data-stu-id="18b83-386">Adding these constraints to existing types is an [incompatible change](version-update-considerations.md#incompatible-changes).</span></span> <span data-ttu-id="18b83-387">封闭式泛型类型可能不再满足这些新约束的要求。</span><span class="sxs-lookup"><span data-stu-id="18b83-387">Closed generic types may no longer meet these new constraints.</span></span>

### <a name="generalized-async-return-types"></a><span data-ttu-id="18b83-388">通用的异步返回类型</span><span class="sxs-lookup"><span data-stu-id="18b83-388">Generalized async return types</span></span>

<span data-ttu-id="18b83-389">从异步方法返回 `Task` 对象可能在某些路径中导致性能瓶颈。</span><span class="sxs-lookup"><span data-stu-id="18b83-389">Returning a `Task` object from async methods can introduce performance bottlenecks in certain paths.</span></span> <span data-ttu-id="18b83-390">`Task` 是引用类型，因此使用它意味着分配对象。</span><span class="sxs-lookup"><span data-stu-id="18b83-390">`Task` is a reference type, so using it means allocating an object.</span></span> <span data-ttu-id="18b83-391">如果使用 `async` 修饰符声明的方法返回缓存结果或以同步方式完成，那么额外的分配在代码的性能关键部分可能要耗费相当长的时间。</span><span class="sxs-lookup"><span data-stu-id="18b83-391">In cases where a method declared with the `async` modifier returns a cached result, or completes synchronously, the extra allocations can become a significant time cost in performance critical sections of code.</span></span> <span data-ttu-id="18b83-392">如果这些分配发生在紧凑循环中，则成本会变高。</span><span class="sxs-lookup"><span data-stu-id="18b83-392">It can become costly if those allocations occur in tight loops.</span></span>

<span data-ttu-id="18b83-393">新语言功能意味着异步方法返回类型不限于 `Task`、`Task<T>` 和 `void`。</span><span class="sxs-lookup"><span data-stu-id="18b83-393">The new language feature means that async method return types aren't limited to `Task`, `Task<T>`, and `void`.</span></span> <span data-ttu-id="18b83-394">返回类型必须仍满足异步模式，这意味着 `GetAwaiter` 方法必须是可访问的。</span><span class="sxs-lookup"><span data-stu-id="18b83-394">The returned type must still satisfy the async pattern, meaning a `GetAwaiter` method must be accessible.</span></span> <span data-ttu-id="18b83-395">作为一个具体示例，已将 `ValueTask` 类型添加到 .NET 中，以使用这一新语言功能：</span><span class="sxs-lookup"><span data-stu-id="18b83-395">As one concrete example, the `ValueTask` type has been added to .NET to make use of this new language feature:</span></span>

[!code-csharp[UsingValueTask](~/samples/snippets/csharp/new-in-7/AsyncWork.cs#UsingValueTask "Using ValueTask")]

> [!NOTE]
> <span data-ttu-id="18b83-396">你需要添加 NuGet 包 [`System.Threading.Tasks.Extensions`](https://www.nuget.org/packages/System.Threading.Tasks.Extensions/) 才能使用 <xref:System.Threading.Tasks.ValueTask%601> 类型。</span><span class="sxs-lookup"><span data-stu-id="18b83-396">You need to add the NuGet package [`System.Threading.Tasks.Extensions`](https://www.nuget.org/packages/System.Threading.Tasks.Extensions/) > in order to use the <xref:System.Threading.Tasks.ValueTask%601> type.</span></span>

<span data-ttu-id="18b83-397">此增强功能对于库作者最有用，可避免在性能关键型代码中分配 `Task`。</span><span class="sxs-lookup"><span data-stu-id="18b83-397">This enhancement is most useful for library authors to avoid allocating a `Task` in performance critical code.</span></span>

## <a name="new-compiler-options"></a><span data-ttu-id="18b83-398">新的编译器选项</span><span class="sxs-lookup"><span data-stu-id="18b83-398">New compiler options</span></span>

<span data-ttu-id="18b83-399">新的编译器选项支持 C# 程序的新版本和 DevOps 方案。</span><span class="sxs-lookup"><span data-stu-id="18b83-399">New compiler options support new build and DevOps scenarios for C# programs.</span></span>

### <a name="reference-assembly-generation"></a><span data-ttu-id="18b83-400">引用程序集生成</span><span class="sxs-lookup"><span data-stu-id="18b83-400">Reference assembly generation</span></span>

<span data-ttu-id="18b83-401">有两种新的编译器选项可生成仅引用程序集：[ProduceReferenceAssembly](../language-reference/compiler-options/output.md#producereferenceassembly) 和 [ProduceOnlyReferenceAssembly](../language-reference/compiler-options/code-generation.md#produceonlyreferenceassembly)。</span><span class="sxs-lookup"><span data-stu-id="18b83-401">There are two new compiler options that generate *reference-only assemblies*: [**ProduceReferenceAssembly**](../language-reference/compiler-options/output.md#producereferenceassembly) and [**ProduceOnlyReferenceAssembly**](../language-reference/compiler-options/code-generation.md#produceonlyreferenceassembly).</span></span>
<span data-ttu-id="18b83-402">链接的文章详细介绍了这些选项和引用程序集。</span><span class="sxs-lookup"><span data-stu-id="18b83-402">The linked articles explain these options and reference assemblies in more detail.</span></span>

### <a name="public-or-open-source-signing"></a><span data-ttu-id="18b83-403">公共或开放源代码签名</span><span class="sxs-lookup"><span data-stu-id="18b83-403">Public or Open Source signing</span></span>

<span data-ttu-id="18b83-404">PublicSign 编译器选项指示编译器使用公钥对程序集进行签名。</span><span class="sxs-lookup"><span data-stu-id="18b83-404">The **PublicSign** compiler option instructs the compiler to sign the assembly using a public key.</span></span> <span data-ttu-id="18b83-405">程序集被标记为已签名，但签名取自公钥。</span><span class="sxs-lookup"><span data-stu-id="18b83-405">The assembly is marked as signed, but the signature is taken from the public key.</span></span> <span data-ttu-id="18b83-406">此选项使你能够使用公钥在开放源代码项目中构建签名的程序集。</span><span class="sxs-lookup"><span data-stu-id="18b83-406">This option enables you to build signed assemblies from open-source projects using a public key.</span></span>

<span data-ttu-id="18b83-407">有关详细信息，请参阅 [PublicSign 编译器选项](../language-reference/compiler-options/security.md#publicsign)一文。</span><span class="sxs-lookup"><span data-stu-id="18b83-407">For more information, see the [**PublicSign** compiler option](../language-reference/compiler-options/security.md#publicsign) article.</span></span>

### <a name="pathmap"></a><span data-ttu-id="18b83-408">pathmap</span><span class="sxs-lookup"><span data-stu-id="18b83-408">pathmap</span></span>

<span data-ttu-id="18b83-409">PathMap 编译器选项指示编译器将生成环境中的源路径替换为映射的源路径。</span><span class="sxs-lookup"><span data-stu-id="18b83-409">The **PathMap** compiler option instructs the compiler to replace source paths from the build environment with mapped source paths.</span></span> <span data-ttu-id="18b83-410">PathMap 选项控制由编译器编写入 PDB 文件或为 <xref:System.Runtime.CompilerServices.CallerFilePathAttribute> 编写的源路径。</span><span class="sxs-lookup"><span data-stu-id="18b83-410">The **PathMap** option controls the source path written by the compiler to PDB files or for the <xref:System.Runtime.CompilerServices.CallerFilePathAttribute>.</span></span>

<span data-ttu-id="18b83-411">有关详细信息，请参阅 [PathMap 编译器选项](../language-reference/compiler-options/advanced.md#pathmap)一文。</span><span class="sxs-lookup"><span data-stu-id="18b83-411">For more information, see the [**PathMap** compiler option](../language-reference/compiler-options/advanced.md#pathmap) article.</span></span>
