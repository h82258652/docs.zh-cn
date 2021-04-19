---
title: C# 8.0 中的新增功能 - C# 指南
description: 简要介绍 C# 8.0 中提供的新功能。
ms.date: 04/07/2020
ms.openlocfilehash: fa31276175733211c74ac54affc8ec222896c87f
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255345"
---
# <a name="whats-new-in-c-80"></a><span data-ttu-id="4e9d6-103">C# 8.0 中的新增功能</span><span class="sxs-lookup"><span data-stu-id="4e9d6-103">What's new in C# 8.0</span></span>

<span data-ttu-id="4e9d6-104">C# 8.0 向 C# 语言添加了以下功能和增强功能：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-104">C# 8.0 adds the following features and enhancements to the C# language:</span></span>

- [<span data-ttu-id="4e9d6-105">Readonly 成员</span><span class="sxs-lookup"><span data-stu-id="4e9d6-105">Readonly members</span></span>](#readonly-members)
- [<span data-ttu-id="4e9d6-106">默认接口方法</span><span class="sxs-lookup"><span data-stu-id="4e9d6-106">Default interface methods</span></span>](#default-interface-methods)
- <span data-ttu-id="4e9d6-107">[模式匹配增强功能](#more-patterns-in-more-places)：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-107">[Pattern matching enhancements](#more-patterns-in-more-places):</span></span>
  - [<span data-ttu-id="4e9d6-108">Switch 表达式</span><span class="sxs-lookup"><span data-stu-id="4e9d6-108">Switch expressions</span></span>](#switch-expressions)
  - [<span data-ttu-id="4e9d6-109">属性模式</span><span class="sxs-lookup"><span data-stu-id="4e9d6-109">Property patterns</span></span>](#property-patterns)
  - [<span data-ttu-id="4e9d6-110">元组模式</span><span class="sxs-lookup"><span data-stu-id="4e9d6-110">Tuple patterns</span></span>](#tuple-patterns)
  - [<span data-ttu-id="4e9d6-111">位置模式</span><span class="sxs-lookup"><span data-stu-id="4e9d6-111">Positional patterns</span></span>](#positional-patterns)
- [<span data-ttu-id="4e9d6-112">Using 声明</span><span class="sxs-lookup"><span data-stu-id="4e9d6-112">Using declarations</span></span>](#using-declarations)
- [<span data-ttu-id="4e9d6-113">静态本地函数</span><span class="sxs-lookup"><span data-stu-id="4e9d6-113">Static local functions</span></span>](#static-local-functions)
- [<span data-ttu-id="4e9d6-114">可处置的 ref 结构</span><span class="sxs-lookup"><span data-stu-id="4e9d6-114">Disposable ref structs</span></span>](#disposable-ref-structs)
- [<span data-ttu-id="4e9d6-115">可为空引用类型</span><span class="sxs-lookup"><span data-stu-id="4e9d6-115">Nullable reference types</span></span>](#nullable-reference-types)
- [<span data-ttu-id="4e9d6-116">异步流</span><span class="sxs-lookup"><span data-stu-id="4e9d6-116">Asynchronous streams</span></span>](#asynchronous-streams)
- [<span data-ttu-id="4e9d6-117">异步可释放</span><span class="sxs-lookup"><span data-stu-id="4e9d6-117">Asynchronous disposable</span></span>](#asynchronous-disposable)
- [<span data-ttu-id="4e9d6-118">索引和范围</span><span class="sxs-lookup"><span data-stu-id="4e9d6-118">Indices and ranges</span></span>](#indices-and-ranges)
- [<span data-ttu-id="4e9d6-119">Null 合并赋值</span><span class="sxs-lookup"><span data-stu-id="4e9d6-119">Null-coalescing assignment</span></span>](#null-coalescing-assignment)
- [<span data-ttu-id="4e9d6-120">非托管构造类型</span><span class="sxs-lookup"><span data-stu-id="4e9d6-120">Unmanaged constructed types</span></span>](#unmanaged-constructed-types)
- [<span data-ttu-id="4e9d6-121">嵌套表达式中的 Stackalloc</span><span class="sxs-lookup"><span data-stu-id="4e9d6-121">Stackalloc in nested expressions</span></span>](#stackalloc-in-nested-expressions)
- [<span data-ttu-id="4e9d6-122">内插逐字字符串的增强功能</span><span class="sxs-lookup"><span data-stu-id="4e9d6-122">Enhancement of interpolated verbatim strings</span></span>](#enhancement-of-interpolated-verbatim-strings)

<span data-ttu-id="4e9d6-123">“.NET Core 3.x”和“.NET Standard 2.1”支持 C# 8.0 。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-123">C# 8.0 is supported on **.NET Core 3.x** and **.NET Standard 2.1**.</span></span> <span data-ttu-id="4e9d6-124">有关详细信息，请参阅 [C# 语言版本控制](../language-reference/configure-language-version.md)。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-124">For more information, see [C# language versioning](../language-reference/configure-language-version.md).</span></span>

<span data-ttu-id="4e9d6-125">本文的剩余部分将简要介绍这些功能。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-125">The remainder of this article briefly describes these features.</span></span> <span data-ttu-id="4e9d6-126">如果有详细讲解的文章，则将提供指向这些教程和概述的链接。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-126">Where in-depth articles are available, links to those tutorials and overviews are provided.</span></span> <span data-ttu-id="4e9d6-127">可以使用 `dotnet try` 全局工具在环境中浏览这些功能：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-127">You can explore these features in your environment using the `dotnet try` global tool:</span></span>

1. <span data-ttu-id="4e9d6-128">安装 [dotnet-try](https://github.com/dotnet/try/blob/main/DotNetTryLocal.md) 全局工具。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-128">Install the [dotnet-try](https://github.com/dotnet/try/blob/main/DotNetTryLocal.md) global tool.</span></span>
1. <span data-ttu-id="4e9d6-129">克隆 [dotnet/try-samples](https://github.com/dotnet/try-samples) 存储库。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-129">Clone the [dotnet/try-samples](https://github.com/dotnet/try-samples) repository.</span></span>
1. <span data-ttu-id="4e9d6-130">将当前目录设置为 try-samples 存储库的 csharp8 子目录 。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-130">Set the current directory to the *csharp8* subdirectory for the *try-samples* repository.</span></span>
1. <span data-ttu-id="4e9d6-131">运行 `dotnet try`。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-131">Run `dotnet try`.</span></span>

## <a name="readonly-members"></a><span data-ttu-id="4e9d6-132">Readonly 成员</span><span class="sxs-lookup"><span data-stu-id="4e9d6-132">Readonly members</span></span>

<span data-ttu-id="4e9d6-133">可将 `readonly` 修饰符应用于结构的成员。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-133">You can apply the `readonly` modifier to members of a struct.</span></span> <span data-ttu-id="4e9d6-134">它指示该成员不会修改状态。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-134">It indicates that the member doesn't modify state.</span></span> <span data-ttu-id="4e9d6-135">这比将 `readonly` 修饰符应用于 `struct` 声明更精细。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-135">It's more granular than applying the `readonly` modifier to a `struct` declaration.</span></span>  <span data-ttu-id="4e9d6-136">请考虑以下可变结构：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-136">Consider the following mutable struct:</span></span>

```csharp
public struct Point
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Distance => Math.Sqrt(X * X + Y * Y);

    public override string ToString() =>
        $"({X}, {Y}) is {Distance} from the origin";
}
```

<span data-ttu-id="4e9d6-137">与大多数结构一样，`ToString()` 方法不会修改状态。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-137">Like most structs, the `ToString()` method doesn't modify state.</span></span> <span data-ttu-id="4e9d6-138">可以通过将 `readonly` 修饰符添加到 `ToString()` 的声明来对此进行指示：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-138">You could indicate that by adding the `readonly` modifier to the declaration of `ToString()`:</span></span>

```csharp
public readonly override string ToString() =>
    $"({X}, {Y}) is {Distance} from the origin";
```

<span data-ttu-id="4e9d6-139">上述更改会生成编译器警告，因为 `ToString` 访问未标记为 `readonly` 的 `Distance` 属性：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-139">The preceding change generates a compiler warning, because `ToString` accesses the `Distance` property, which isn't marked `readonly`:</span></span>

```console
warning CS8656: Call to non-readonly member 'Point.Distance.get' from a 'readonly' member results in an implicit copy of 'this'
```

<span data-ttu-id="4e9d6-140">需要创建防御性副本时，编译器会发出警告。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-140">The compiler warns you when it needs to create a defensive copy.</span></span>  <span data-ttu-id="4e9d6-141">`Distance` 属性不会更改状态，因此可以通过将 `readonly` 修饰符添加到声明来修复此警告：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-141">The `Distance` property doesn't change state, so you can fix this warning by adding the `readonly` modifier to the declaration:</span></span>

```csharp
public readonly double Distance => Math.Sqrt(X * X + Y * Y);
```

<span data-ttu-id="4e9d6-142">请注意，`readonly` 修饰符对于只读属性是必需的。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-142">Notice that the `readonly` modifier is necessary on a read-only property.</span></span> <span data-ttu-id="4e9d6-143">编译器会假设 `get` 访问器可以修改状态；必须显式声明 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-143">The compiler doesn't assume `get` accessors don't modify state; you must declare `readonly` explicitly.</span></span> <span data-ttu-id="4e9d6-144">自动实现的属性是一个例外；编译器会将所有自动实现的 Getter 视为 `readonly`，因此，此处无需向 `X` 和 `Y` 属性添加 `readonly` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-144">Auto-implemented properties are an exception; the compiler will treat all auto-implemented getters as `readonly`, so here there's no need to add the `readonly` modifier to the `X` and `Y` properties.</span></span>

<span data-ttu-id="4e9d6-145">编译器确实会强制执行 `readonly` 成员不修改状态的规则。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-145">The compiler does enforce the rule that `readonly` members don't modify state.</span></span> <span data-ttu-id="4e9d6-146">除非删除 `readonly` 修饰符，否则不会编译以下方法：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-146">The following method won't compile unless you remove the `readonly` modifier:</span></span>

```csharp
public readonly void Translate(int xOffset, int yOffset)
{
    X += xOffset;
    Y += yOffset;
}
```

<span data-ttu-id="4e9d6-147">通过此功能，可以指定设计意图，使编译器可以强制执行该意图，并基于该意图进行优化。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-147">This feature lets you specify your design intent so the compiler can enforce it, and make optimizations based on that intent.</span></span>

<span data-ttu-id="4e9d6-148">有关详细信息，请参阅[结构类型](../language-reference/builtin-types/struct.md)一文中的 [`readonly` 实例成员](../language-reference/builtin-types/struct.md#readonly-instance-members)部分。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-148">For more information, see the [`readonly` instance members](../language-reference/builtin-types/struct.md#readonly-instance-members) section of the [Structure types](../language-reference/builtin-types/struct.md) article.</span></span>

## <a name="default-interface-methods"></a><span data-ttu-id="4e9d6-149">默认接口方法</span><span class="sxs-lookup"><span data-stu-id="4e9d6-149">Default interface methods</span></span>

<span data-ttu-id="4e9d6-150">现在可以将成员添加到接口，并为这些成员提供实现。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-150">You can now add members to interfaces and provide an implementation for those members.</span></span> <span data-ttu-id="4e9d6-151">借助此语言功能，API 作者可以将方法添加到以后版本的接口中，而不会破坏与该接口当前实现的源或二进制文件兼容性。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-151">This language feature enables API authors to add methods to an interface in later versions without breaking source or binary compatibility with existing implementations of that interface.</span></span> <span data-ttu-id="4e9d6-152">现有的实现继承默认实现。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-152">Existing implementations *inherit* the default implementation.</span></span> <span data-ttu-id="4e9d6-153">此功能使 C# 与面向 Android 或 Swift 的 API 进行互操作，此类 API 支持类似功能。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-153">This feature also enables C# to interoperate with APIs that target Android or Swift, which support similar features.</span></span> <span data-ttu-id="4e9d6-154">默认接口方法还支持类似于“特征”语言功能的方案。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-154">Default interface methods also enable scenarios similar to a "traits" language feature.</span></span>

<span data-ttu-id="4e9d6-155">默认接口方法会影响很多方案和语言元素。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-155">Default interface methods affect many scenarios and language elements.</span></span> <span data-ttu-id="4e9d6-156">我们的第一个教程介绍如何[使用默认实现更新接口](./tutorials/default-interface-methods-versions.md)。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-156">Our first tutorial covers [updating an interface with default implementations](./tutorials/default-interface-methods-versions.md).</span></span>

## <a name="more-patterns-in-more-places"></a><span data-ttu-id="4e9d6-157">在更多位置中使用更多模式</span><span class="sxs-lookup"><span data-stu-id="4e9d6-157">More patterns in more places</span></span>

<span data-ttu-id="4e9d6-158">模式匹配提供了在相关但不同类型的数据中提供形状相关功能的工具。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-158">**Pattern matching** gives tools to provide shape-dependent functionality across related but different kinds of data.</span></span> <span data-ttu-id="4e9d6-159">C# 7.0 通过使用 [`is`](../language-reference/keywords/is.md) 表达式和 [`switch`](../language-reference/keywords/switch.md) 语句引入了类型模式和常量模式的语法。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-159">C# 7.0 introduced syntax for type patterns and constant patterns by using the [`is`](../language-reference/keywords/is.md) expression and the [`switch`](../language-reference/keywords/switch.md) statement.</span></span> <span data-ttu-id="4e9d6-160">这些功能代表了支持数据和功能分离的编程范例的初步尝试。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-160">These features represented the first tentative steps toward supporting programming paradigms where data and functionality live apart.</span></span> <span data-ttu-id="4e9d6-161">随着行业转向更多微服务和其他基于云的体系结构，还需要其他语言工具。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-161">As the industry moves toward more microservices and other cloud-based architectures, other language tools are needed.</span></span>

<span data-ttu-id="4e9d6-162">C# 8.0 扩展了此词汇表，这样就可以在代码中的更多位置使用更多模式表达式。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-162">C# 8.0 expands this vocabulary so you can use more pattern expressions in more places in your code.</span></span> <span data-ttu-id="4e9d6-163">当数据和功能分离时，请考虑使用这些功能。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-163">Consider these features when your data and functionality are separate.</span></span> <span data-ttu-id="4e9d6-164">当算法依赖于对象运行时类型以外的事实时，请考虑使用模式匹配。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-164">Consider pattern matching when your algorithms depend on a fact other than the runtime type of an object.</span></span> <span data-ttu-id="4e9d6-165">这些技术提供了另一种表达设计的方式。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-165">These techniques provide another way to express designs.</span></span>

<span data-ttu-id="4e9d6-166">除了可以在新位置使用新模式之外，C# 8.0 还添加了“递归模式”。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-166">In addition to new patterns in new places, C# 8.0 adds **recursive patterns**.</span></span> <span data-ttu-id="4e9d6-167">递归模式是可以包含其他模式的模式。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-167">Recursive patterns are patterns that can contain other patterns.</span></span>

### <a name="switch-expressions"></a><span data-ttu-id="4e9d6-168">switch 表达式</span><span class="sxs-lookup"><span data-stu-id="4e9d6-168">Switch expressions</span></span>

<span data-ttu-id="4e9d6-169">通常情况下，[`switch`](../language-reference/keywords/switch.md) 语句在其每个 `case` 块中生成一个值。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-169">Often, a [`switch`](../language-reference/keywords/switch.md) statement produces a value in each of its `case` blocks.</span></span> <span data-ttu-id="4e9d6-170">借助 Switch 表达式，可以使用更简洁的表达式语法。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-170">**Switch expressions** enable you to use more concise expression syntax.</span></span> <span data-ttu-id="4e9d6-171">只有些许重复的 `case` 和 `break` 关键字和大括号。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-171">There are fewer repetitive `case` and `break` keywords, and fewer curly braces.</span></span>  <span data-ttu-id="4e9d6-172">以下面列出彩虹颜色的枚举为例：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-172">As an example, consider the following enum that lists the colors of the rainbow:</span></span>

```csharp
public enum Rainbow
{
    Red,
    Orange,
    Yellow,
    Green,
    Blue,
    Indigo,
    Violet
}
```

<span data-ttu-id="4e9d6-173">如果应用定义了通过 `R`、`G` 和 `B` 组件构造而成的 `RGBColor` 类型，可使用以下包含 switch 表达式的方法，将 `Rainbow` 转换为 RGB 值：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-173">If your application defined an `RGBColor` type that is constructed from the `R`, `G` and `B` components, you could convert a `Rainbow` value to its RGB values using the following method containing a switch expression:</span></span>

```csharp
public static RGBColor FromRainbow(Rainbow colorBand) =>
    colorBand switch
    {
        Rainbow.Red    => new RGBColor(0xFF, 0x00, 0x00),
        Rainbow.Orange => new RGBColor(0xFF, 0x7F, 0x00),
        Rainbow.Yellow => new RGBColor(0xFF, 0xFF, 0x00),
        Rainbow.Green  => new RGBColor(0x00, 0xFF, 0x00),
        Rainbow.Blue   => new RGBColor(0x00, 0x00, 0xFF),
        Rainbow.Indigo => new RGBColor(0x4B, 0x00, 0x82),
        Rainbow.Violet => new RGBColor(0x94, 0x00, 0xD3),
        _              => throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand)),
    };
```

<span data-ttu-id="4e9d6-174">这里有几个语法改进：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-174">There are several syntax improvements here:</span></span>

- <span data-ttu-id="4e9d6-175">变量位于 `switch` 关键字之前。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-175">The variable comes before the `switch` keyword.</span></span> <span data-ttu-id="4e9d6-176">不同的顺序使得在视觉上可以很轻松地区分 switch 表达式和 switch 语句。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-176">The different order makes it visually easy to distinguish the switch expression from the switch statement.</span></span>
- <span data-ttu-id="4e9d6-177">将 `case` 和 `:` 元素替换为 `=>`。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-177">The `case` and `:` elements are replaced with `=>`.</span></span> <span data-ttu-id="4e9d6-178">它更简洁，更直观。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-178">It's more concise and intuitive.</span></span>
- <span data-ttu-id="4e9d6-179">将 `default` 事例替换为 `_` 弃元。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-179">The `default` case is replaced with a `_` discard.</span></span>
- <span data-ttu-id="4e9d6-180">正文是表达式，不是语句。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-180">The bodies are expressions, not statements.</span></span>

<span data-ttu-id="4e9d6-181">将其与使用经典 `switch` 语句的等效代码进行对比：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-181">Contrast that with the equivalent code using the classic `switch` statement:</span></span>

```csharp
public static RGBColor FromRainbowClassic(Rainbow colorBand)
{
    switch (colorBand)
    {
        case Rainbow.Red:
            return new RGBColor(0xFF, 0x00, 0x00);
        case Rainbow.Orange:
            return new RGBColor(0xFF, 0x7F, 0x00);
        case Rainbow.Yellow:
            return new RGBColor(0xFF, 0xFF, 0x00);
        case Rainbow.Green:
            return new RGBColor(0x00, 0xFF, 0x00);
        case Rainbow.Blue:
            return new RGBColor(0x00, 0x00, 0xFF);
        case Rainbow.Indigo:
            return new RGBColor(0x4B, 0x00, 0x82);
        case Rainbow.Violet:
            return new RGBColor(0x94, 0x00, 0xD3);
        default:
            throw new ArgumentException(message: "invalid enum value", paramName: nameof(colorBand));
    };
}
```

<span data-ttu-id="4e9d6-182">有关详细信息，请参阅 [`switch` 表达式](../language-reference/operators/switch-expression.md)。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-182">For more information, see [`switch` expression](../language-reference/operators/switch-expression.md).</span></span>

### <a name="property-patterns"></a><span data-ttu-id="4e9d6-183">属性模式</span><span class="sxs-lookup"><span data-stu-id="4e9d6-183">Property patterns</span></span>

<span data-ttu-id="4e9d6-184">借助属性模式，可以匹配所检查的对象的属性。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-184">The **property pattern** enables you to match on properties of the object examined.</span></span> <span data-ttu-id="4e9d6-185">请看一个电子商务网站的示例，该网站必须根据买家地址计算销售税。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-185">Consider an eCommerce site that must compute sales tax based on the buyer's address.</span></span> <span data-ttu-id="4e9d6-186">这种计算不是 `Address` 类的核心职责。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-186">That computation isn't a core responsibility of an `Address` class.</span></span> <span data-ttu-id="4e9d6-187">它会随时间变化，可能比地址格式的更改更频繁。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-187">It will change over time, likely more often than address format changes.</span></span> <span data-ttu-id="4e9d6-188">销售税的金额取决于地址的 `State` 属性。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-188">The amount of sales tax depends on the `State` property of the address.</span></span> <span data-ttu-id="4e9d6-189">下面的方法使用属性模式从地址和价格计算销售税：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-189">The following method uses the property pattern to compute the sales tax from the address and the price:</span></span>

```csharp
public static decimal ComputeSalesTax(Address location, decimal salePrice) =>
    location switch
    {
        { State: "WA" } => salePrice * 0.06M,
        { State: "MN" } => salePrice * 0.075M,
        { State: "MI" } => salePrice * 0.05M,
        // other cases removed for brevity...
        _ => 0M
    };
```

<span data-ttu-id="4e9d6-190">模式匹配为表达此算法创建了简洁的语法。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-190">Pattern matching creates a concise syntax for expressing this algorithm.</span></span>

<span data-ttu-id="4e9d6-191">有关详细信息，请参阅[模式](../language-reference/operators/patterns.md)一文的[属性模式](../language-reference/operators/patterns.md#property-pattern)部分。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-191">For more information, see the [Property pattern](../language-reference/operators/patterns.md#property-pattern) section of the [Patterns](../language-reference/operators/patterns.md) article.</span></span>

### <a name="tuple-patterns"></a><span data-ttu-id="4e9d6-192">元组模式</span><span class="sxs-lookup"><span data-stu-id="4e9d6-192">Tuple patterns</span></span>

<span data-ttu-id="4e9d6-193">一些算法依赖于多个输入。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-193">Some algorithms depend on multiple inputs.</span></span> <span data-ttu-id="4e9d6-194">使用元组模式，可根据表示为[元组](../language-reference/builtin-types/value-tuples.md)的多个值进行切换。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-194">**Tuple patterns** allow you to switch based on multiple values expressed as a [tuple](../language-reference/builtin-types/value-tuples.md).</span></span> <span data-ttu-id="4e9d6-195">以下代码显示了游戏“rock, paper, scissors（石头剪刀布）”的切换表达式：：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-195">The following code shows a switch expression for the game *rock, paper, scissors*:</span></span>

```csharp
public static string RockPaperScissors(string first, string second)
    => (first, second) switch
    {
        ("rock", "paper") => "rock is covered by paper. Paper wins.",
        ("rock", "scissors") => "rock breaks scissors. Rock wins.",
        ("paper", "rock") => "paper covers rock. Paper wins.",
        ("paper", "scissors") => "paper is cut by scissors. Scissors wins.",
        ("scissors", "rock") => "scissors is broken by rock. Rock wins.",
        ("scissors", "paper") => "scissors cuts paper. Scissors wins.",
        (_, _) => "tie"
    };
```

<span data-ttu-id="4e9d6-196">消息指示获胜者。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-196">The messages indicate the winner.</span></span> <span data-ttu-id="4e9d6-197">弃元表示平局(石头剪刀布游戏)的三种组合或其他文本输入。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-197">The discard case represents the three combinations for ties, or other text inputs.</span></span>

### <a name="positional-patterns"></a><span data-ttu-id="4e9d6-198">位置模式</span><span class="sxs-lookup"><span data-stu-id="4e9d6-198">Positional patterns</span></span>

<span data-ttu-id="4e9d6-199">某些类型包含 `Deconstruct` 方法，该方法将其属性解构为离散变量。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-199">Some types include a `Deconstruct` method that deconstructs its properties into discrete variables.</span></span> <span data-ttu-id="4e9d6-200">如果可以访问 `Deconstruct` 方法，就可以使用位置模式检查对象的属性并将这些属性用于模式。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-200">When a `Deconstruct` method is accessible, you can use **positional patterns** to inspect properties of the object and use those properties for a pattern.</span></span>  <span data-ttu-id="4e9d6-201">考虑以下 `Point` 类，其中包含用于为 `X` 和 `Y` 创建离散变量的 `Deconstruct` 方法：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-201">Consider the following `Point` class that includes a `Deconstruct` method to create discrete variables for `X` and `Y`:</span></span>

```csharp
public class Point
{
    public int X { get; }
    public int Y { get; }

    public Point(int x, int y) => (X, Y) = (x, y);

    public void Deconstruct(out int x, out int y) =>
        (x, y) = (X, Y);
}
```

<span data-ttu-id="4e9d6-202">此外，请考虑以下表示象限的各种位置的枚举：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-202">Additionally, consider the following enum that represents various positions of a quadrant:</span></span>

```csharp
public enum Quadrant
{
    Unknown,
    Origin,
    One,
    Two,
    Three,
    Four,
    OnBorder
}
```

<span data-ttu-id="4e9d6-203">下面的方法使用位置模式来提取 `x` 和 `y` 的值。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-203">The following method uses the **positional pattern** to extract the values of `x` and `y`.</span></span> <span data-ttu-id="4e9d6-204">然后，它使用 `when` 子句来确定该点的 `Quadrant`：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-204">Then, it uses a `when` clause to determine the `Quadrant` of the point:</span></span>

```csharp
static Quadrant GetQuadrant(Point point) => point switch
{
    (0, 0) => Quadrant.Origin,
    var (x, y) when x > 0 && y > 0 => Quadrant.One,
    var (x, y) when x < 0 && y > 0 => Quadrant.Two,
    var (x, y) when x < 0 && y < 0 => Quadrant.Three,
    var (x, y) when x > 0 && y < 0 => Quadrant.Four,
    var (_, _) => Quadrant.OnBorder,
    _ => Quadrant.Unknown
};
```

<span data-ttu-id="4e9d6-205">当 `x` 或 `y` 为 0（但不是两者同时为 0）时，前一个开关中的弃元模式匹配。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-205">The discard pattern in the preceding switch matches when either `x` or `y` is 0, but not both.</span></span> <span data-ttu-id="4e9d6-206">Switch 表达式必须要么生成值，要么引发异常。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-206">A switch expression must either produce a value or throw an exception.</span></span> <span data-ttu-id="4e9d6-207">如果这些情况都不匹配，则 switch 表达式将引发异常。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-207">If none of the cases match, the switch expression throws an exception.</span></span> <span data-ttu-id="4e9d6-208">如果没有在 switch 表达式中涵盖所有可能的情况，编译器将生成一个警告。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-208">The compiler generates a warning for you if you don't cover all possible cases in your switch expression.</span></span>

<span data-ttu-id="4e9d6-209">可在此[模式匹配高级教程](../tutorials/pattern-matching.md)中探索模式匹配方法。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-209">You can explore pattern matching techniques in this [advanced tutorial on pattern matching](../tutorials/pattern-matching.md).</span></span> <span data-ttu-id="4e9d6-210">有关位置模式的详细信息，请参阅[模式](../language-reference/operators/patterns.md)的[位置模式](../language-reference/operators/patterns.md#positional-pattern)部分。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-210">For more information about a positional pattern, see the [Positional pattern](../language-reference/operators/patterns.md#positional-pattern) section of the [Patterns](../language-reference/operators/patterns.md) article.</span></span>

## <a name="using-declarations"></a><span data-ttu-id="4e9d6-211">using 声明</span><span class="sxs-lookup"><span data-stu-id="4e9d6-211">Using declarations</span></span>

<span data-ttu-id="4e9d6-212">using 声明是前面带 `using` 关键字的变量声明。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-212">A **using declaration** is a variable declaration preceded by the `using` keyword.</span></span> <span data-ttu-id="4e9d6-213">它指示编译器声明的变量应在封闭范围的末尾进行处理。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-213">It tells the compiler that the variable being declared should be disposed at the end of the enclosing scope.</span></span> <span data-ttu-id="4e9d6-214">以下面编写文本文件的代码为例：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-214">For example, consider the following code that writes a text file:</span></span>

```csharp
static int WriteLinesToFile(IEnumerable<string> lines)
{
    using var file = new System.IO.StreamWriter("WriteLines2.txt");
    int skippedLines = 0;
    foreach (string line in lines)
    {
        if (!line.Contains("Second"))
        {
            file.WriteLine(line);
        }
        else
        {
            skippedLines++;
        }
    }
    // Notice how skippedLines is in scope here.
    return skippedLines;
    // file is disposed here
}
```

<span data-ttu-id="4e9d6-215">在前面的示例中，当到达方法的右括号时，将对该文件进行处理。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-215">In the preceding example, the file is disposed when the closing brace for the method is reached.</span></span> <span data-ttu-id="4e9d6-216">这是声明 `file` 的范围的末尾。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-216">That's the end of the scope in which `file` is declared.</span></span> <span data-ttu-id="4e9d6-217">前面的代码相当于下面使用经典 [using 语句](../language-reference/keywords/using-statement.md)的代码：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-217">The preceding code is equivalent to the following code that uses the classic [using statement](../language-reference/keywords/using-statement.md):</span></span>

```csharp
static int WriteLinesToFile(IEnumerable<string> lines)
{
    using (var file = new System.IO.StreamWriter("WriteLines2.txt"))
    {
        int skippedLines = 0;
        foreach (string line in lines)
        {
            if (!line.Contains("Second"))
            {
                file.WriteLine(line);
            }
            else
            {
                skippedLines++;
            }
        }
        return skippedLines;
    } // file is disposed here
}
```

<span data-ttu-id="4e9d6-218">在前面的示例中，当到达与 `using` 语句关联的右括号时，将对该文件进行处理。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-218">In the preceding example, the file is disposed when the closing brace associated with the `using` statement is reached.</span></span>

<span data-ttu-id="4e9d6-219">在这两种情况下，编译器将生成对 `Dispose()` 的调用。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-219">In both cases, the compiler generates the call to `Dispose()`.</span></span> <span data-ttu-id="4e9d6-220">如果 `using` 语句中的表达式不可用，编译器将生成一个错误。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-220">The compiler generates an error if the expression in the `using` statement isn't disposable.</span></span>

## <a name="static-local-functions"></a><span data-ttu-id="4e9d6-221">静态本地函数</span><span class="sxs-lookup"><span data-stu-id="4e9d6-221">Static local functions</span></span>

<span data-ttu-id="4e9d6-222">现在可以向本地函数添加 `static` 修饰符，以确保[本地函数](../programming-guide/classes-and-structs/local-functions.md)不会从封闭范围捕获（引用）任何变量。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-222">You can now add the `static` modifier to [local functions](../programming-guide/classes-and-structs/local-functions.md) to ensure that local function doesn't capture (reference) any variables from the enclosing scope.</span></span> <span data-ttu-id="4e9d6-223">这样做会生成 `CS8421`，“静态本地函数不能包含对 \<variable> 的引用。”</span><span class="sxs-lookup"><span data-stu-id="4e9d6-223">Doing so generates `CS8421`, "A static local function can't contain a reference to \<variable>."</span></span>

<span data-ttu-id="4e9d6-224">考虑下列代码。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-224">Consider the following code.</span></span> <span data-ttu-id="4e9d6-225">本地函数 `LocalFunction` 访问在封闭范围（方法 `M`）中声明的变量 `y`。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-225">The local function `LocalFunction` accesses the variable `y`, declared in the enclosing scope (the method `M`).</span></span> <span data-ttu-id="4e9d6-226">因此，不能用 `static` 修饰符来声明 `LocalFunction`：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-226">Therefore, `LocalFunction` can't be declared with the `static` modifier:</span></span>

```csharp
int M()
{
    int y;
    LocalFunction();
    return y;

    void LocalFunction() => y = 0;
}
```

<span data-ttu-id="4e9d6-227">下面的代码包含一个静态本地函数。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-227">The following code contains a static local function.</span></span> <span data-ttu-id="4e9d6-228">它可以是静态的，因为它不访问封闭范围中的任何变量：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-228">It can be static because it doesn't access any variables in the enclosing scope:</span></span>

```csharp
int M()
{
    int y = 5;
    int x = 7;
    return Add(x, y);

    static int Add(int left, int right) => left + right;
}
```

## <a name="disposable-ref-structs"></a><span data-ttu-id="4e9d6-229">可处置的 ref 结构</span><span class="sxs-lookup"><span data-stu-id="4e9d6-229">Disposable ref structs</span></span>

<span data-ttu-id="4e9d6-230">用 `ref` 修饰符声明的 `struct` 可能无法实现任何接口，因此无法实现 <xref:System.IDisposable>。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-230">A `struct` declared with the `ref` modifier may not implement any interfaces and so can't implement <xref:System.IDisposable>.</span></span> <span data-ttu-id="4e9d6-231">因此，要能够处理 `ref struct`，它必须有一个可访问的 `void Dispose()` 方法。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-231">Therefore, to enable a `ref struct` to be disposed, it must have an accessible `void Dispose()` method.</span></span> <span data-ttu-id="4e9d6-232">此功能同样适用于 `readonly ref struct` 声明。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-232">This feature also applies to `readonly ref struct` declarations.</span></span>

## <a name="nullable-reference-types"></a><span data-ttu-id="4e9d6-233">可为空引用类型</span><span class="sxs-lookup"><span data-stu-id="4e9d6-233">Nullable reference types</span></span>

<span data-ttu-id="4e9d6-234">在可为空注释上下文中，引用类型的任何变量都被视为不可为空引用类型。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-234">Inside a nullable annotation context, any variable of a reference type is considered to be a **nonnullable reference type**.</span></span> <span data-ttu-id="4e9d6-235">若要指示一个变量可能为 null，必须在类型名称后面附加 `?`，以将该变量声明为可为空引用类型。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-235">If you want to indicate that a variable may be null, you must append the type name with the `?` to declare the variable as a **nullable reference type**.</span></span>

<span data-ttu-id="4e9d6-236">对于不可为空引用类型，编译器使用流分析来确保在声明时将本地变量初始化为非 Null 值。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-236">For nonnullable reference types, the compiler uses flow analysis to ensure that local variables are initialized to a non-null value when declared.</span></span> <span data-ttu-id="4e9d6-237">字段必须在构造过程中初始化。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-237">Fields must be initialized during construction.</span></span> <span data-ttu-id="4e9d6-238">如果没有通过调用任何可用的构造函数或通过初始化表达式来设置变量，编译器将生成警告。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-238">The compiler generates a warning if the variable isn't set by a call to any of the available constructors or by an initializer.</span></span> <span data-ttu-id="4e9d6-239">此外，不能向不可为空引用类型分配一个可以为 Null 的值。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-239">Furthermore, nonnullable reference types can't be assigned a value that could be null.</span></span>

<span data-ttu-id="4e9d6-240">不对可为空引用类型进行检查以确保它们没有被赋予 Null 值或初始化为 Null。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-240">Nullable reference types aren't checked to ensure they aren't assigned or initialized to null.</span></span> <span data-ttu-id="4e9d6-241">不过，编译器使用流分析来确保可为空引用类型的任何变量在被访问或分配给不可为空引用类型之前，都会对其 Null 性进行检查。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-241">However, the compiler uses flow analysis to ensure that any variable of a nullable reference type is checked against null before it's accessed or assigned to a nonnullable reference type.</span></span>

<span data-ttu-id="4e9d6-242">可以在[可为空引用类型](../nullable-references.md)的概述中了解该功能的更多信息。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-242">You can learn more about the feature in the overview of [nullable reference types](../nullable-references.md).</span></span> <span data-ttu-id="4e9d6-243">可以在此[可为空引用类型教程](tutorials/nullable-reference-types.md)中的新应用程序中自行尝试。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-243">Try it yourself in a new application in this [nullable reference types tutorial](tutorials/nullable-reference-types.md).</span></span> <span data-ttu-id="4e9d6-244">在[迁移应用程序以使用可为空引用类型教程](tutorials/upgrade-to-nullable-references.md)中了解迁移现有代码库以使用可为空引用类型的步骤。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-244">Learn about the steps to migrate an existing codebase to make use of nullable reference types in the [migrating an application to use nullable reference types tutorial](tutorials/upgrade-to-nullable-references.md).</span></span>

## <a name="asynchronous-streams"></a><span data-ttu-id="4e9d6-245">异步流</span><span class="sxs-lookup"><span data-stu-id="4e9d6-245">Asynchronous streams</span></span>

<span data-ttu-id="4e9d6-246">从 C# 8.0 开始，可以创建并以异步方式使用流。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-246">Starting with C# 8.0, you can create and consume streams asynchronously.</span></span> <span data-ttu-id="4e9d6-247">返回异步流的方法有三个属性：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-247">A method that returns an asynchronous stream has three properties:</span></span>

1. <span data-ttu-id="4e9d6-248">它是用 `async` 修饰符声明的。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-248">It's declared with the `async` modifier.</span></span>
1. <span data-ttu-id="4e9d6-249">它将返回 <xref:System.Collections.Generic.IAsyncEnumerable%601>。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-249">It returns an <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span>
1. <span data-ttu-id="4e9d6-250">该方法包含用于在异步流中返回连续元素的 `yield return` 语句。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-250">The method contains `yield return` statements to return successive elements in the asynchronous stream.</span></span>

<span data-ttu-id="4e9d6-251">使用异步流需要在枚举流元素时在 `foreach` 关键字前面添加 `await` 关键字。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-251">Consuming an asynchronous stream requires you to add the `await` keyword before the `foreach` keyword when you enumerate the elements of the stream.</span></span> <span data-ttu-id="4e9d6-252">添加 `await` 关键字需要枚举异步流的方法，以使用 `async` 修饰符进行声明并返回 `async` 方法允许的类型。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-252">Adding the `await` keyword requires the method that enumerates the asynchronous stream to be declared with the `async` modifier and to return a type allowed for an `async` method.</span></span> <span data-ttu-id="4e9d6-253">通常这意味着返回 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601>。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-253">Typically that means returning a <xref:System.Threading.Tasks.Task> or <xref:System.Threading.Tasks.Task%601>.</span></span> <span data-ttu-id="4e9d6-254">也可以为 <xref:System.Threading.Tasks.ValueTask> 或 <xref:System.Threading.Tasks.ValueTask%601>。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-254">It can also be a <xref:System.Threading.Tasks.ValueTask> or <xref:System.Threading.Tasks.ValueTask%601>.</span></span> <span data-ttu-id="4e9d6-255">方法既可以使用异步流，也可以生成异步流，这意味着它将返回 <xref:System.Collections.Generic.IAsyncEnumerable%601>。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-255">A method can both consume and produce an asynchronous stream, which means it would return an <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="4e9d6-256">下面的代码生成一个从 0 到 19 的序列，在生成每个数字之间等待 100 毫秒：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-256">The following code generates a sequence from 0 to 19, waiting 100 ms between generating each number:</span></span>

```csharp
public static async System.Collections.Generic.IAsyncEnumerable<int> GenerateSequence()
{
    for (int i = 0; i < 20; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}
```

<span data-ttu-id="4e9d6-257">可以使用 `await foreach` 语句来枚举序列：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-257">You would enumerate the sequence using the `await foreach` statement:</span></span>

```csharp
await foreach (var number in GenerateSequence())
{
    Console.WriteLine(number);
}
```

<span data-ttu-id="4e9d6-258">可以在[创建和使用异步流](tutorials/generate-consume-asynchronous-stream.md)的教程中自行尝试异步流。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-258">You can try asynchronous streams yourself in our tutorial on [creating and consuming async streams](tutorials/generate-consume-asynchronous-stream.md).</span></span> <span data-ttu-id="4e9d6-259">默认情况下，在捕获的上下文中处理流元素。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-259">By default, stream elements are processed in the captured context.</span></span> <span data-ttu-id="4e9d6-260">如果要禁用上下文捕获，请使用 <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait%2A?displayProperty=nameWithType> 扩展方法。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-260">If you want to disable capturing of the context, use the <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait%2A?displayProperty=nameWithType> extension method.</span></span> <span data-ttu-id="4e9d6-261">有关同步上下文并捕获当前上下文的详细信息，请参阅有关[使用基于任务的异步模式](../../standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md)的文章。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-261">For more information about synchronization contexts and capturing the current context, see the article on [consuming the Task-based asynchronous pattern](../../standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md).</span></span>

## <a name="asynchronous-disposable"></a><span data-ttu-id="4e9d6-262">异步可释放</span><span class="sxs-lookup"><span data-stu-id="4e9d6-262">Asynchronous disposable</span></span>

<span data-ttu-id="4e9d6-263">从 C# 8.0 开始，语言支持实现 <xref:System.IAsyncDisposable?displayProperty=nameWithType> 接口的异步可释放类型。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-263">Starting with C# 8.0, the language supports asynchronous disposable types that implement the <xref:System.IAsyncDisposable?displayProperty=nameWithType> interface.</span></span> <span data-ttu-id="4e9d6-264">可使用 `await using` 语句来处理异步可释放对象。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-264">You use the `await using` statement to work with an asynchronously disposable object.</span></span> <span data-ttu-id="4e9d6-265">有关详细信息，请参阅[实现 DisposeAsync 方法](../../standard/garbage-collection/implementing-disposeasync.md)。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-265">For more information, see the [Implement a DisposeAsync method](../../standard/garbage-collection/implementing-disposeasync.md) article.</span></span>

## <a name="indices-and-ranges"></a><span data-ttu-id="4e9d6-266">索引和范围</span><span class="sxs-lookup"><span data-stu-id="4e9d6-266">Indices and ranges</span></span>

<span data-ttu-id="4e9d6-267">索引和范围为访问序列中的单个元素或范围提供了简洁的语法。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-267">Indices and ranges provide a succinct syntax for accessing single elements or ranges in a sequence.</span></span>

<span data-ttu-id="4e9d6-268">此语言支持依赖于两个新类型和两个新运算符：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-268">This language support relies on two new types, and two new operators:</span></span>

- <span data-ttu-id="4e9d6-269"><xref:System.Index?displayProperty=nameWithType> 表示一个序列索引。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-269"><xref:System.Index?displayProperty=nameWithType> represents an index into a sequence.</span></span>
- <span data-ttu-id="4e9d6-270">来自末尾运算符 `^` 的索引，指定一个索引与序列末尾相关。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-270">The index from end operator `^`, which specifies that an index is relative to the end of the sequence.</span></span>
- <span data-ttu-id="4e9d6-271"><xref:System.Range?displayProperty=nameWithType> 表示序列的子范围。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-271"><xref:System.Range?displayProperty=nameWithType> represents a sub range of a sequence.</span></span>
- <span data-ttu-id="4e9d6-272">范围运算符 `..`，用于指定范围的开始和末尾，就像操作数一样。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-272">The range operator `..`, which specifies the start and end of a range as its operands.</span></span>

<span data-ttu-id="4e9d6-273">让我们从索引规则开始。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-273">Let's start with the rules for indexes.</span></span> <span data-ttu-id="4e9d6-274">请考虑数组 `sequence`。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-274">Consider an array `sequence`.</span></span> <span data-ttu-id="4e9d6-275">`0` 索引与 `sequence[0]` 相同。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-275">The `0` index is the same as `sequence[0]`.</span></span> <span data-ttu-id="4e9d6-276">`^0` 索引与 `sequence[sequence.Length]` 相同。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-276">The `^0` index is the same as `sequence[sequence.Length]`.</span></span> <span data-ttu-id="4e9d6-277">请注意，`sequence[^0]` 不会引发异常，就像 `sequence[sequence.Length]` 一样。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-277">Note that `sequence[^0]` does throw an exception, just as `sequence[sequence.Length]` does.</span></span> <span data-ttu-id="4e9d6-278">对于任何数字 `n`，索引 `^n` 与 `sequence.Length - n` 相同。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-278">For any number `n`, the index `^n` is the same as `sequence.Length - n`.</span></span>

<span data-ttu-id="4e9d6-279">范围指定范围的开始和末尾 。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-279">A range specifies the *start* and *end* of a range.</span></span> <span data-ttu-id="4e9d6-280">包括此范围的开始，但不包括此范围的末尾，这表示此范围包含开始但不包含末尾 。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-280">The start of the range is inclusive, but the end of the range is exclusive, meaning the *start* is included in the range but the *end* isn't included in the range.</span></span> <span data-ttu-id="4e9d6-281">范围 `[0..^0]` 表示整个范围，就像 `[0..sequence.Length]` 表示整个范围。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-281">The range `[0..^0]` represents the entire range, just as `[0..sequence.Length]` represents the entire range.</span></span>

<span data-ttu-id="4e9d6-282">请看以下几个示例。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-282">Let's look at a few examples.</span></span> <span data-ttu-id="4e9d6-283">请考虑以下数组，用其顺数索引和倒数索引进行注释：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-283">Consider the following array, annotated with its index from the start and from the end:</span></span>

```csharp
var words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (or words.Length) ^0
```

<span data-ttu-id="4e9d6-284">可以使用 `^1` 索引检索最后一个词：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-284">You can retrieve the last word with the `^1` index:</span></span>

```csharp
Console.WriteLine($"The last word is {words[^1]}");
// writes "dog"
```

<span data-ttu-id="4e9d6-285">以下代码创建了一个包含单词“quick”、“brown”和“fox”的子范围。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-285">The following code creates a subrange with the words "quick", "brown", and "fox".</span></span> <span data-ttu-id="4e9d6-286">它包括 `words[1]` 到 `words[3]`。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-286">It includes `words[1]` through `words[3]`.</span></span> <span data-ttu-id="4e9d6-287">元素 `words[4]` 不在该范围内。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-287">The element `words[4]` isn't in the range.</span></span>

```csharp
var quickBrownFox = words[1..4];
```

<span data-ttu-id="4e9d6-288">以下代码使用“lazy”和“dog”创建一个子范围。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-288">The following code creates a subrange with "lazy" and "dog".</span></span> <span data-ttu-id="4e9d6-289">它包括 `words[^2]` 和 `words[^1]`。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-289">It includes `words[^2]` and `words[^1]`.</span></span> <span data-ttu-id="4e9d6-290">末尾索引 `words[^0]` 不包括在内：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-290">The end index `words[^0]` isn't included:</span></span>

```csharp
var lazyDog = words[^2..^0];
```

<span data-ttu-id="4e9d6-291">下面的示例为开始和/或结束创建了开放范围：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-291">The following examples create ranges that are open ended for the start, end, or both:</span></span>

```csharp
var allWords = words[..]; // contains "The" through "dog".
var firstPhrase = words[..4]; // contains "The" through "fox"
var lastPhrase = words[6..]; // contains "the", "lazy" and "dog"
```

<span data-ttu-id="4e9d6-292">此外可以将范围声明为变量：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-292">You can also declare ranges as variables:</span></span>

```csharp
Range phrase = 1..4;
```

<span data-ttu-id="4e9d6-293">然后可以在 `[` 和 `]` 字符中使用该范围：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-293">The range can then be used inside the `[` and `]` characters:</span></span>

```csharp
var text = words[phrase];
```

<span data-ttu-id="4e9d6-294">不仅数组支持索引和范围。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-294">Not only arrays support indices and ranges.</span></span> <span data-ttu-id="4e9d6-295">还可以将索引和范围用于 [string](../language-reference/builtin-types/reference-types.md#the-string-type)、<xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601>。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-295">You can also use indices and ranges with [string](../language-reference/builtin-types/reference-types.md#the-string-type), <xref:System.Span%601>, or <xref:System.ReadOnlySpan%601>.</span></span> <span data-ttu-id="4e9d6-296">有关详细信息，请参阅[索引和范围的类型支持](tutorials/ranges-indexes.md#type-support-for-indices-and-ranges)。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-296">For more information, see [Type support for indices and ranges](tutorials/ranges-indexes.md#type-support-for-indices-and-ranges).</span></span>

<span data-ttu-id="4e9d6-297">可在有关[索引和范围](tutorials/ranges-indexes.md)的教程中详细了解索引和范围。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-297">You can explore more about indices and ranges in the tutorial on [indices and ranges](tutorials/ranges-indexes.md).</span></span>

## <a name="null-coalescing-assignment"></a><span data-ttu-id="4e9d6-298">Null 合并赋值</span><span class="sxs-lookup"><span data-stu-id="4e9d6-298">Null-coalescing assignment</span></span>

<span data-ttu-id="4e9d6-299">C# 8.0 引入了 null 合并赋值运算符 `??=`。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-299">C# 8.0 introduces the null-coalescing assignment operator `??=`.</span></span> <span data-ttu-id="4e9d6-300">仅当左操作数计算为 `null` 时，才能使用运算符 `??=` 将其右操作数的值分配给左操作数。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-300">You can use the `??=` operator to assign the value of its right-hand operand to its left-hand operand only if the left-hand operand evaluates to `null`.</span></span>

```csharp
List<int> numbers = null;
int? i = null;

numbers ??= new List<int>();
numbers.Add(i ??= 17);
numbers.Add(i ??= 20);

Console.WriteLine(string.Join(" ", numbers));  // output: 17 17
Console.WriteLine(i);  // output: 17
```

<span data-ttu-id="4e9d6-301">有关详细信息，请参阅 [?? 和 ??= 运算符](../language-reference/operators/null-coalescing-operator.md)一文。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-301">For more information, see the [?? and ??= operators](../language-reference/operators/null-coalescing-operator.md) article.</span></span>

## <a name="unmanaged-constructed-types"></a><span data-ttu-id="4e9d6-302">非托管构造类型</span><span class="sxs-lookup"><span data-stu-id="4e9d6-302">Unmanaged constructed types</span></span>

<span data-ttu-id="4e9d6-303">在 C# 7.3 及更低版本中，构造类型（包含至少一个类型参数的类型）不能为[非托管类型](../language-reference/builtin-types/unmanaged-types.md)。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-303">In C# 7.3 and earlier, a constructed type (a type that includes at least one type argument) can't be an [unmanaged type](../language-reference/builtin-types/unmanaged-types.md).</span></span> <span data-ttu-id="4e9d6-304">从 C# 8.0 开始，如果构造的值类型仅包含非托管类型的字段，则该类型不受管理。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-304">Starting with C# 8.0, a constructed value type is unmanaged if it contains fields of unmanaged types only.</span></span>

<span data-ttu-id="4e9d6-305">例如，假设泛型 `Coords<T>` 类型有以下定义：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-305">For example, given the following definition of the generic `Coords<T>` type:</span></span>

```csharp
public struct Coords<T>
{
    public T X;
    public T Y;
}
```

<span data-ttu-id="4e9d6-306">`Coords<int>` 类型为 C# 8.0 及更高版本中的非托管类型。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-306">the `Coords<int>` type is an unmanaged type in C# 8.0 and later.</span></span> <span data-ttu-id="4e9d6-307">与任何非托管类型一样，可以创建指向此类型的变量的指针，或针对此类型的实例[在堆栈上分配内存块](../language-reference/operators/stackalloc.md)：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-307">Like for any unmanaged type, you can create a pointer to a variable of this type or [allocate a block of memory on the stack](../language-reference/operators/stackalloc.md) for instances of this type:</span></span>

```csharp
Span<Coords<int>> coordinates = stackalloc[]
{
    new Coords<int> { X = 0, Y = 0 },
    new Coords<int> { X = 0, Y = 3 },
    new Coords<int> { X = 4, Y = 0 }
};
```

<span data-ttu-id="4e9d6-308">有关详细信息，请参阅[非托管类型](../language-reference/builtin-types/unmanaged-types.md)。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-308">For more information, see [Unmanaged types](../language-reference/builtin-types/unmanaged-types.md).</span></span>

## <a name="stackalloc-in-nested-expressions"></a><span data-ttu-id="4e9d6-309">嵌套表达式中的 stackalloc</span><span class="sxs-lookup"><span data-stu-id="4e9d6-309">Stackalloc in nested expressions</span></span>

<span data-ttu-id="4e9d6-310">从 C# 8.0 开始，如果 [stackalloc](../language-reference/operators/stackalloc.md) 表达式的结果为 <xref:System.Span%601?displayProperty=nameWithType> 或 <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> 类型，则可以在其他表达式中使用 `stackalloc` 表达式：</span><span class="sxs-lookup"><span data-stu-id="4e9d6-310">Starting with C# 8.0, if the result of a [stackalloc](../language-reference/operators/stackalloc.md) expression is of the <xref:System.Span%601?displayProperty=nameWithType> or <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> type, you can use the `stackalloc` expression in other expressions:</span></span>

```csharp
Span<int> numbers = stackalloc[] { 1, 2, 3, 4, 5, 6 };
var ind = numbers.IndexOfAny(stackalloc[] { 2, 4, 6, 8 });
Console.WriteLine(ind);  // output: 1
```

## <a name="enhancement-of-interpolated-verbatim-strings"></a><span data-ttu-id="4e9d6-311">内插逐字字符串的增强功能</span><span class="sxs-lookup"><span data-stu-id="4e9d6-311">Enhancement of interpolated verbatim strings</span></span>

<span data-ttu-id="4e9d6-312">[内插](../language-reference/tokens/interpolated.md)逐字字符串中 `$` 和 `@` 标记的顺序可以任意安排：`$@"..."` 和 `@$"..."` 均为有效的内插逐字字符串。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-312">Order of the `$` and `@` tokens in [interpolated](../language-reference/tokens/interpolated.md) verbatim strings can be any: both `$@"..."` and `@$"..."` are valid interpolated verbatim strings.</span></span> <span data-ttu-id="4e9d6-313">在早期 C# 版本中，`$` 标记必须出现在 `@` 标记之前。</span><span class="sxs-lookup"><span data-stu-id="4e9d6-313">In earlier C# versions, the `$` token must appear before the `@` token.</span></span>
