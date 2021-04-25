---
title: switch 表达式 - C# 参考
description: 详细了解根据模式匹配提供类似 switch 的语义的 C# switch 表达式
ms.date: 04/16/2021
f1_keywords:
- switch_CSharpKeyword
helpviewer_keywords:
- switch expression [C#]
- pattern matching [C#]
ms.openlocfilehash: ce892598f364e4934613a797d9290fa385abd4c8
ms.sourcegitcommit: 23e2bc267872ce6ea22db4bf6478fe57bcba4bc8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2021
ms.locfileid: "107592261"
---
# <a name="switch-expression-c-reference"></a><span data-ttu-id="af90f-103">switch 表达式（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="af90f-103">switch expression (C# reference)</span></span>

<span data-ttu-id="af90f-104">从 C# 8.0 开始，可以使用 `switch` 表达式，根据与输入表达式匹配的模式，对候选表达式列表中的单个表达式进行求值。</span><span class="sxs-lookup"><span data-stu-id="af90f-104">Beginning with C# 8.0, you use the `switch` expression to evaluate a single expression from a list of candidate expressions based on a pattern match with an input expression.</span></span> <span data-ttu-id="af90f-105">有关在语句上下文中支持 `switch` 类语义的 `switch` 语句的信息，请参阅 [`switch` 语句](../keywords/switch.md)一文。</span><span class="sxs-lookup"><span data-stu-id="af90f-105">For information about the `switch` statement that supports `switch`-like semantics in a statement context, see the [`switch` statement](../keywords/switch.md) article.</span></span>

<span data-ttu-id="af90f-106">下面的示例演示了一个 `switch` 表达式，该表达式将在线地图中表示视觉方向的 [`enum`](../builtin-types/enum.md) 中的值转换为相应的基本方位：</span><span class="sxs-lookup"><span data-stu-id="af90f-106">The following example demonstrates a `switch` expression, which converts values of an [`enum`](../builtin-types/enum.md) representing visual directions in an online map to the corresponding cardinal directions:</span></span>

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="SnippetBasicStructure":::

<span data-ttu-id="af90f-107">上述示例展示了 `switch` 表达式的基本元素：</span><span class="sxs-lookup"><span data-stu-id="af90f-107">The preceding example shows the basic elements of a `switch` expression:</span></span>

- <span data-ttu-id="af90f-108">后跟 `switch` 关键字的表达式。</span><span class="sxs-lookup"><span data-stu-id="af90f-108">An expression followed by the `switch` keyword.</span></span> <span data-ttu-id="af90f-109">在上述示例中，这是 `direction` 方法参数。</span><span class="sxs-lookup"><span data-stu-id="af90f-109">In the preceding example, it's the `direction` method parameter.</span></span>
- <span data-ttu-id="af90f-110">`switch` expression arm，用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="af90f-110">The *`switch` expression arms*, separated by commas.</span></span> <span data-ttu-id="af90f-111">每个 `switch` expression arm 都包含一个模式、一个可选的 [case guard](#case-guards)、`=>` 标记和一个表达式  。</span><span class="sxs-lookup"><span data-stu-id="af90f-111">Each `switch` expression arm contains a *pattern*, an optional [*case guard*](#case-guards), the `=>` token, and an *expression*.</span></span>

<span data-ttu-id="af90f-112">在上述示例中，`switch` 表达式使用以下模式：</span><span class="sxs-lookup"><span data-stu-id="af90f-112">At the preceding example, a `switch` expression uses the following patterns:</span></span>

- <span data-ttu-id="af90f-113">[常数模式](patterns.md#constant-pattern)，用于处理 `Direction` 枚举的定义值。</span><span class="sxs-lookup"><span data-stu-id="af90f-113">A [constant pattern](patterns.md#constant-pattern) that is used to handle the defined values of the `Direction` enumeration.</span></span>
- <span data-ttu-id="af90f-114">[弃元模式](patterns.md#discard-pattern)用于处理没有相应的 `Direction` 枚举成员的任何整数值（例如 `(Direction)10`）。</span><span class="sxs-lookup"><span data-stu-id="af90f-114">A [discard pattern](patterns.md#discard-pattern) that is used to handle any integer value that doesn't have the corresponding member of the `Direction` enumeration (for example, `(Direction)10`).</span></span> <span data-ttu-id="af90f-115">这会使 `switch` 表达式[详尽](#non-exhaustive-switch-expressions)。</span><span class="sxs-lookup"><span data-stu-id="af90f-115">That makes the `switch` expression [exhaustive](#non-exhaustive-switch-expressions).</span></span>

<span data-ttu-id="af90f-116">有关 `switch` 表达式支持的模式的信息，请参阅[模式](patterns.md)。</span><span class="sxs-lookup"><span data-stu-id="af90f-116">For information about the patterns supported by a `switch` expression, see [Patterns](patterns.md).</span></span>

<span data-ttu-id="af90f-117">`switch` 表达式的结果是第一个 `switch` expression arm 的表达式的值，该 switch expression arm 的模式与范围表达式匹配，并且它的 case guard（如果存在）求值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="af90f-117">The result of a `switch` expression is the value of the expression of the first `switch` expression arm whose pattern matches the input expression and whose case guard, if present, evaluates to `true`.</span></span> <span data-ttu-id="af90f-118">`switch` expression arm 按文本顺序求值。</span><span class="sxs-lookup"><span data-stu-id="af90f-118">The `switch` expression arms are evaluated in text order.</span></span>

<span data-ttu-id="af90f-119">如果无法选择较低的 `switch` expression arm，编译器会发出错误，因为较高的 `switch` expression arm 匹配其所有值。</span><span class="sxs-lookup"><span data-stu-id="af90f-119">The compiler generates an error when a lower `switch` expression arm can't be chosen because a higher `switch` expression arm matches all its values.</span></span>

## <a name="case-guards"></a><span data-ttu-id="af90f-120">Case guard</span><span class="sxs-lookup"><span data-stu-id="af90f-120">Case guards</span></span>

<span data-ttu-id="af90f-121">模式或许表现力不够，无法指定用于计算 arm 的表达式的条件。</span><span class="sxs-lookup"><span data-stu-id="af90f-121">A pattern may be not expressive enough to specify the condition for the evaluation of an arm's expression.</span></span> <span data-ttu-id="af90f-122">在这种情况下，可以使用 case guard。</span><span class="sxs-lookup"><span data-stu-id="af90f-122">In such a case, you can use a case guard.</span></span> <span data-ttu-id="af90f-123">这是一个附加条件，必须与匹配模式同时满足。</span><span class="sxs-lookup"><span data-stu-id="af90f-123">That is an additional condition that must be satisfied together with a matched pattern.</span></span> <span data-ttu-id="af90f-124">可以在模式后面的 `when` 关键字之后指定一个 case guard，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="af90f-124">You specify a case guard after the `when` keyword that follows a pattern, as the following example shows:</span></span>

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="CaseGuardExample":::

<span data-ttu-id="af90f-125">上述示例使用带有嵌套 [var 模式](patterns.md#var-pattern)的[属性模式](patterns.md#property-pattern)。</span><span class="sxs-lookup"><span data-stu-id="af90f-125">The preceding example uses [property patterns](patterns.md#property-pattern) with nested [var patterns](patterns.md#var-pattern).</span></span>

## <a name="non-exhaustive-switch-expressions"></a><span data-ttu-id="af90f-126">非详尽的 switch 表达式</span><span class="sxs-lookup"><span data-stu-id="af90f-126">Non-exhaustive switch expressions</span></span>

<span data-ttu-id="af90f-127">如果 `switch` 表达式的模式均未捕获输入值，则运行时将引发异常。</span><span class="sxs-lookup"><span data-stu-id="af90f-127">If none of a `switch` expression's patterns matches an input value, the runtime throws an exception.</span></span> <span data-ttu-id="af90f-128">在 .NET Core 3.0 及更高版本中，异常是 <xref:System.Runtime.CompilerServices.SwitchExpressionException?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="af90f-128">In .NET Core 3.0 and later versions, the exception is a <xref:System.Runtime.CompilerServices.SwitchExpressionException?displayProperty=nameWithType>.</span></span> <span data-ttu-id="af90f-129">在 .NET Framework 中，异常是 <xref:System.InvalidOperationException>。</span><span class="sxs-lookup"><span data-stu-id="af90f-129">In .NET Framework, the exception is an <xref:System.InvalidOperationException>.</span></span> <span data-ttu-id="af90f-130">如果 `switch` 表达式未处理所有可能的输入值，则编译器会生成警告。</span><span class="sxs-lookup"><span data-stu-id="af90f-130">The compiler generates a warning if a `switch` expression doesn't handle all possible input values.</span></span>

> [!TIP]
> <span data-ttu-id="af90f-131">为了保证 `switch` 表达式处理所有可能的输入值，请为 `switch` expression arm 提供[弃元模式](patterns.md#discard-pattern)。</span><span class="sxs-lookup"><span data-stu-id="af90f-131">To guarantee that a `switch` expression handles all possible input values, provide a `switch` expression arm with a [discard pattern](patterns.md#discard-pattern).</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="af90f-132">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="af90f-132">C# language specification</span></span>

<span data-ttu-id="af90f-133">有关详细信息，请参阅[功能建议说明](~/_csharplang/proposals/csharp-8.0/patterns.md)的 [`switch` 表达式](~/_csharplang/proposals/csharp-8.0/patterns.md#switch-expression)部分。</span><span class="sxs-lookup"><span data-stu-id="af90f-133">For more information, see the [`switch` expression](~/_csharplang/proposals/csharp-8.0/patterns.md#switch-expression) section of the [feature proposal note](~/_csharplang/proposals/csharp-8.0/patterns.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="af90f-134">另请参阅</span><span class="sxs-lookup"><span data-stu-id="af90f-134">See also</span></span>

- [<span data-ttu-id="af90f-135">C# 参考</span><span class="sxs-lookup"><span data-stu-id="af90f-135">C# reference</span></span>](../index.md)
- [<span data-ttu-id="af90f-136">C# 运算符和表达式</span><span class="sxs-lookup"><span data-stu-id="af90f-136">C# operators and expressions</span></span>](index.md)
- [<span data-ttu-id="af90f-137">模式</span><span class="sxs-lookup"><span data-stu-id="af90f-137">Patterns</span></span>](patterns.md)
- [<span data-ttu-id="af90f-138">教程：使用模式匹配来构建类型驱动和数据驱动的算法</span><span class="sxs-lookup"><span data-stu-id="af90f-138">Tutorial: Use pattern matching to build type-driven and data-driven algorithms</span></span>](../../tutorials/pattern-matching.md)
- [<span data-ttu-id="af90f-139">`switch` 语句</span><span class="sxs-lookup"><span data-stu-id="af90f-139">`switch` statement</span></span>](../keywords/switch.md)
