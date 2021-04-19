---
title: 模式 - C# 参考
description: 了解 C# 模式匹配表达式和语句支持的模式 - C# 参考
ms.date: 04/05/2021
f1_keywords:
- and_CSharpKeyword
- or_CSharpKeyword
- not_CSharpKeyword
helpviewer_keywords:
- pattern matching [C#]
- and keyword [C#]
- or keyword [C#]
- not keyword [C#]
ms.openlocfilehash: cac2208d41bca2c6befecffbe4bb0b8ca43042bc
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106499074"
---
# <a name="patterns-c-reference"></a><span data-ttu-id="945d5-103">模式（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="945d5-103">Patterns (C# reference)</span></span>

<span data-ttu-id="945d5-104">C# 在 C# 7.0 中引入了模式匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-104">C# introduced pattern matching in C# 7.0.</span></span> <span data-ttu-id="945d5-105">自此之后，每个主要 C# 版本都扩展了模式匹配功能。</span><span class="sxs-lookup"><span data-stu-id="945d5-105">Since then, each major C# version extends pattern matching capabilities.</span></span> <span data-ttu-id="945d5-106">以下 C# 表达式和语句支持模式匹配：</span><span class="sxs-lookup"><span data-stu-id="945d5-106">The following C# expressions and statements support pattern matching:</span></span>

- [<span data-ttu-id="945d5-107">`is`表达式</span><span class="sxs-lookup"><span data-stu-id="945d5-107">`is` expression</span></span>](../keywords/is.md)
- <span data-ttu-id="945d5-108">`switch` [语句](../keywords/switch.md)</span><span class="sxs-lookup"><span data-stu-id="945d5-108">`switch` [statement](../keywords/switch.md)</span></span>
- <span data-ttu-id="945d5-109">`switch` [表达式](switch-expression.md)（在 C# 8.0 中引入）</span><span class="sxs-lookup"><span data-stu-id="945d5-109">`switch` [expression](switch-expression.md) (introduced in C# 8.0)</span></span>

<span data-ttu-id="945d5-110">在这些构造中，可将输入表达式与以下任一模式进行匹配：</span><span class="sxs-lookup"><span data-stu-id="945d5-110">In those constructs, you can match an input expression against any of the following patterns:</span></span>

- <span data-ttu-id="945d5-111">[声明模式](#declaration-and-type-patterns)：用于检查表达式的运行时类型，如果匹配成功，则将表达式结果分配给声明的变量。</span><span class="sxs-lookup"><span data-stu-id="945d5-111">[Declaration pattern](#declaration-and-type-patterns): to check the runtime type of an expression and, if a match succeeds, assign an expression result to a declared variable.</span></span> <span data-ttu-id="945d5-112">在 C# 7.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="945d5-112">Introduced in C# 7.0.</span></span>
- <span data-ttu-id="945d5-113">[类型模式](#declaration-and-type-patterns)：用于检查表达式的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="945d5-113">[Type pattern](#declaration-and-type-patterns): to check the runtime type of an expression.</span></span> <span data-ttu-id="945d5-114">在 C# 9.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="945d5-114">Introduced in C# 9.0.</span></span>
- <span data-ttu-id="945d5-115">[常量模式](#constant-pattern)：用于测试表达式结果是否等于指定常量。</span><span class="sxs-lookup"><span data-stu-id="945d5-115">[Constant pattern](#constant-pattern): to test if an expression result equals a specified constant.</span></span> <span data-ttu-id="945d5-116">在 C# 7.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="945d5-116">Introduced in C# 7.0.</span></span>
- <span data-ttu-id="945d5-117">[关系模式](#relational-patterns)：用于将表达式结果与指定常量进行比较。</span><span class="sxs-lookup"><span data-stu-id="945d5-117">[Relational patterns](#relational-patterns): to compare an expression result with a specified constant.</span></span> <span data-ttu-id="945d5-118">在 C# 9.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="945d5-118">Introduced in C# 9.0.</span></span>
- <span data-ttu-id="945d5-119">[逻辑模式](#logical-patterns)：用于测试表达式是否与模式的逻辑组合匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-119">[Logical patterns](#logical-patterns): to test if an expression matches a logical combination of patterns.</span></span> <span data-ttu-id="945d5-120">在 C# 9.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="945d5-120">Introduced in C# 9.0.</span></span>
- <span data-ttu-id="945d5-121">[属性模式](#property-pattern)：用于测试表达式的属性或字段是否与嵌套模式匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-121">[Property pattern](#property-pattern): to test if an expression's properties or fields match nested patterns.</span></span> <span data-ttu-id="945d5-122">在 C# 8.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="945d5-122">Introduced in C# 8.0.</span></span>
- <span data-ttu-id="945d5-123">[位置模式](#positional-pattern)：用于解构表达式结果并测试结果值是否与嵌套模式匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-123">[Positional pattern](#positional-pattern): to deconstruct an expression result and test if the resulting values match nested patterns.</span></span> <span data-ttu-id="945d5-124">在 C# 8.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="945d5-124">Introduced in C# 8.0.</span></span>
- <span data-ttu-id="945d5-125">[`var` 模式](#var-pattern)：用于匹配任何表达式并将其结果分配给声明的变量。</span><span class="sxs-lookup"><span data-stu-id="945d5-125">[`var` pattern](#var-pattern): to match any expression and assign its result to a declared variable.</span></span> <span data-ttu-id="945d5-126">在 C# 7.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="945d5-126">Introduced in C# 7.0.</span></span>
- <span data-ttu-id="945d5-127">[弃元模式](#discard-pattern)：用于匹配任何表达式。</span><span class="sxs-lookup"><span data-stu-id="945d5-127">[Discard pattern](#discard-pattern): to match any expression.</span></span> <span data-ttu-id="945d5-128">在 C# 8.0 中引入。</span><span class="sxs-lookup"><span data-stu-id="945d5-128">Introduced in C# 8.0.</span></span>

<span data-ttu-id="945d5-129">[逻辑](#logical-patterns)、[属性](#property-pattern)和[位置](#positional-pattern)模式都是递归模式。</span><span class="sxs-lookup"><span data-stu-id="945d5-129">[Logical](#logical-patterns), [property](#property-pattern), and [positional](#positional-pattern) patterns are *recursive* patterns.</span></span> <span data-ttu-id="945d5-130">也就是说，它们可包含嵌套模式。</span><span class="sxs-lookup"><span data-stu-id="945d5-130">That is, they can contain *nested* patterns.</span></span>

<span data-ttu-id="945d5-131">有关如何使用这些模式来生成数据驱动算法的示例，请参阅[教程：使用模式匹配来生成类型驱动的算法和数据驱动的算法](../../tutorials/pattern-matching.md)。</span><span class="sxs-lookup"><span data-stu-id="945d5-131">For the example of how to use those patterns to build a data-driven algorithm, see [Tutorial: Use pattern matching to build type-driven and data-driven algorithms](../../tutorials/pattern-matching.md).</span></span>

## <a name="declaration-and-type-patterns"></a><span data-ttu-id="945d5-132">声明和类型模式</span><span class="sxs-lookup"><span data-stu-id="945d5-132">Declaration and type patterns</span></span>

<span data-ttu-id="945d5-133">可使用声明和类型模式来检查表达式的运行时类型是否与给定类型兼容。</span><span class="sxs-lookup"><span data-stu-id="945d5-133">You use declaration and type patterns to check if the runtime type of an expression is compatible with a given type.</span></span> <span data-ttu-id="945d5-134">借助声明模式，还可声明新的局部变量。</span><span class="sxs-lookup"><span data-stu-id="945d5-134">With a declaration pattern, you can also declare a new local variable.</span></span> <span data-ttu-id="945d5-135">当声明模式与表达式匹配时，将为该变量分配转换后的表达式结果，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-135">When a declaration pattern matches an expression, that variable is assigned a converted expression result, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="BasicExample":::

<span data-ttu-id="945d5-136">从 C# 7.0 开始，类型为 `T` 的声明模式在表达式结果为非 NULL 且满足以下任一条件时与表达式匹配：</span><span class="sxs-lookup"><span data-stu-id="945d5-136">Beginning with C# 7.0, a *declaration pattern* with type `T` matches an expression when an expression result is non-null and any of the following conditions are true:</span></span>

- <span data-ttu-id="945d5-137">表达式结果的运行时类型为 `T`。</span><span class="sxs-lookup"><span data-stu-id="945d5-137">The runtime type of an expression result is `T`.</span></span>

- <span data-ttu-id="945d5-138">表达式结果的运行时类型派生自类型 `T` 或实现接口 `T`，或者存在从其到 `T` 的另一种[隐式引用转换](~/_csharplang/spec/conversions.md#implicit-reference-conversions)。</span><span class="sxs-lookup"><span data-stu-id="945d5-138">The runtime type of an expression result derives from type `T` or implements interface `T` or another [implicit reference conversion](~/_csharplang/spec/conversions.md#implicit-reference-conversions) exists from it to `T`.</span></span> <span data-ttu-id="945d5-139">下面的示例演示满足此条件时的两种案例：</span><span class="sxs-lookup"><span data-stu-id="945d5-139">The following example demonstrates two cases when this condition is true:</span></span>

  :::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="ReferenceConversion":::

  <span data-ttu-id="945d5-140">在上述示例中，在第一次调用 `GetSourceLabel` 方法时，第一种模式与参数值匹配，因为参数的运行时类型 `int[]` 派生自 <xref:System.Array> 类型。</span><span class="sxs-lookup"><span data-stu-id="945d5-140">In the preceding example, at the first call to the `GetSourceLabel` method, the first pattern matches an argument value because the argument's runtime type `int[]` derives from the <xref:System.Array> type.</span></span> <span data-ttu-id="945d5-141">在第二次调用 `GetSourceLabel` 方法时，参数的运行时类型 <xref:System.Collections.Generic.List%601> 不是从 <xref:System.Array> 类型派生的，而是实现 <xref:System.Collections.Generic.ICollection%601> 接口。</span><span class="sxs-lookup"><span data-stu-id="945d5-141">At the second call to the `GetSourceLabel` method, the argument's runtime type <xref:System.Collections.Generic.List%601> doesn't derive from the <xref:System.Array> type but implements the <xref:System.Collections.Generic.ICollection%601> interface.</span></span>

- <span data-ttu-id="945d5-142">表达式结果的运行时类型是具有基础类型 `T` 的[可为空的值类型](../builtin-types/nullable-value-types.md)。</span><span class="sxs-lookup"><span data-stu-id="945d5-142">The runtime type of an expression result is a [nullable value type](../builtin-types/nullable-value-types.md) with the underlying type `T`.</span></span>

- <span data-ttu-id="945d5-143">存在从表达式结果的运行时类型到类型 `T` 的[装箱](../../programming-guide/types/boxing-and-unboxing.md#boxing)或[取消装箱](../../programming-guide/types/boxing-and-unboxing.md#unboxing)转换。</span><span class="sxs-lookup"><span data-stu-id="945d5-143">A [boxing](../../programming-guide/types/boxing-and-unboxing.md#boxing) or [unboxing](../../programming-guide/types/boxing-and-unboxing.md#unboxing) conversion exists from the runtime type of an expression result to type `T`.</span></span>

<span data-ttu-id="945d5-144">下面的示例演示最后两个条件：</span><span class="sxs-lookup"><span data-stu-id="945d5-144">The following example demonstrates the last two conditions:</span></span>

:::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="NullableAndUnboxing":::

<span data-ttu-id="945d5-145">如果只想检查表达式类型，可使用弃元 `_` 代替变量名，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-145">If you want to check only the type of an expression, you can use a discard `_` in place of a variable's name, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="DiscardVariable":::

<span data-ttu-id="945d5-146">从 C# 9.0 开始，可对此使用类型模式，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-146">Beginning with C# 9.0, for that purpose you can use a *type pattern*, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="TypePattern":::

<span data-ttu-id="945d5-147">类似于声明模式，当表达式结果为非 NULL 且其运行时类型满足上面列出的任一条件时，类型模式将与表达式匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-147">Like a declaration pattern, a type pattern matches an expression when an expression result is non-null and its runtime type satisfies any of the conditions listed above.</span></span>

<span data-ttu-id="945d5-148">有关详细信息，请参阅功能方案说明的[声明模式](~/_csharplang/proposals/csharp-8.0/patterns.md#declaration-pattern)和[类型模式](~/_csharplang/proposals/csharp-9.0/patterns3.md#type-patterns)部分。</span><span class="sxs-lookup"><span data-stu-id="945d5-148">For more information, see the [Declaration pattern](~/_csharplang/proposals/csharp-8.0/patterns.md#declaration-pattern) and [Type pattern](~/_csharplang/proposals/csharp-9.0/patterns3.md#type-patterns) sections of the feature proposal notes.</span></span>

## <a name="constant-pattern"></a><span data-ttu-id="945d5-149">常量模式</span><span class="sxs-lookup"><span data-stu-id="945d5-149">Constant pattern</span></span>

<span data-ttu-id="945d5-150">从 C# 7.0 开始，可使用常量模式来测试表达式结果是否等于指定的常量，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-150">Beginning with C# 7.0, you use a *constant pattern* to test if an expression result equals a specified constant, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/ConstantPattern.cs" id="BasicExample":::

<span data-ttu-id="945d5-151">在常量模式中，可使用任何常量表达式，例如：</span><span class="sxs-lookup"><span data-stu-id="945d5-151">In a constant pattern, you can use any constant expression, such as:</span></span>

- <span data-ttu-id="945d5-152">[integer](../builtin-types/integral-numeric-types.md) 或 [floating-point](../builtin-types/floating-point-numeric-types.md) 数值文本</span><span class="sxs-lookup"><span data-stu-id="945d5-152">an [integer](../builtin-types/integral-numeric-types.md) or [floating-point](../builtin-types/floating-point-numeric-types.md) numerical literal</span></span>
- <span data-ttu-id="945d5-153">[char](../builtin-types/char.md) 或 [string](../builtin-types/reference-types.md#the-string-type) 文本</span><span class="sxs-lookup"><span data-stu-id="945d5-153">a [char](../builtin-types/char.md) or a [string](../builtin-types/reference-types.md#the-string-type) literal</span></span>
- <span data-ttu-id="945d5-154">布尔值 `true` 或 `false`</span><span class="sxs-lookup"><span data-stu-id="945d5-154">a Boolean value `true` or `false`</span></span>
- <span data-ttu-id="945d5-155">[enum](../builtin-types/enum.md) 值</span><span class="sxs-lookup"><span data-stu-id="945d5-155">an [enum](../builtin-types/enum.md) value</span></span>
- <span data-ttu-id="945d5-156">声明[常量](../keywords/const.md)字段或本地的名称</span><span class="sxs-lookup"><span data-stu-id="945d5-156">the name of a declared [const](../keywords/const.md) field or local</span></span>
- `null`

<span data-ttu-id="945d5-157">常量模式用于检查 `null`，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-157">Use a constant pattern to check for `null`, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/ConstantPattern.cs" id="NullCheck":::

<span data-ttu-id="945d5-158">编译器保证在计算表达式 `x is null` 时，不会调用用户重载的相等运算符 `==`。</span><span class="sxs-lookup"><span data-stu-id="945d5-158">The compiler guarantees that no user-overloaded equality operator `==` is invoked when expression `x is null` is evaluated.</span></span>

<span data-ttu-id="945d5-159">从 C# 9.0 开始，可使用[否定](#logical-patterns) `null` 常量模式来检查非 NULL，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-159">Beginning with C# 9.0, you can use a [negated](#logical-patterns) `null` constant pattern to check for non-null, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/ConstantPattern.cs" id="NonNullCheck":::

<span data-ttu-id="945d5-160">有关详细信息，请参阅功能建议说明的[常量模式](~/_csharplang/proposals/csharp-8.0/patterns.md#constant-pattern)部分。</span><span class="sxs-lookup"><span data-stu-id="945d5-160">For more information, see the [Constant pattern](~/_csharplang/proposals/csharp-8.0/patterns.md#constant-pattern) section of the feature proposal note.</span></span>

## <a name="relational-patterns"></a><span data-ttu-id="945d5-161">关系模式</span><span class="sxs-lookup"><span data-stu-id="945d5-161">Relational patterns</span></span>

<span data-ttu-id="945d5-162">从 C# 9.0 开始，可使用关系模式将表达式结果与常量进行比较，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-162">Beginning with C# 9.0, you use a *relational pattern* to compare an expression result with a constant, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/RelationalPatterns.cs" id="BasicExample":::

<span data-ttu-id="945d5-163">在关系模式中，可使用[关系运算符](comparison-operators.md) `<`、`>`、`<=` 或 `>=` 中的任何一个。</span><span class="sxs-lookup"><span data-stu-id="945d5-163">In a relational pattern, you can use any of the [relational operators](comparison-operators.md) `<`, `>`, `<=`, or `>=`.</span></span> <span data-ttu-id="945d5-164">关系模式的右侧部分必须是常数表达式。</span><span class="sxs-lookup"><span data-stu-id="945d5-164">The right-hand part of a relational pattern must be a constant expression.</span></span> <span data-ttu-id="945d5-165">常数表达式可以是 [integer](../builtin-types/integral-numeric-types.md)、[floating-point](../builtin-types/floating-point-numeric-types.md)、[char](../builtin-types/char.md) 或 [enum](../builtin-types/enum.md) 类型。</span><span class="sxs-lookup"><span data-stu-id="945d5-165">The constant expression can be of an [integer](../builtin-types/integral-numeric-types.md), [floating-point](../builtin-types/floating-point-numeric-types.md), [char](../builtin-types/char.md), or [enum](../builtin-types/enum.md) type.</span></span>

<span data-ttu-id="945d5-166">要检查表达式结果是否在某个范围内，请将其与[合取 `and` 模式](#logical-patterns)匹配，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-166">To check if an expression result is in a certain range, match it against a [conjunctive `and` pattern](#logical-patterns), as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/RelationalPatterns.cs" id="WithCombinators":::

<span data-ttu-id="945d5-167">如果表达式结果为 `null` 或未能通过可为空或取消装箱转换转换为常量类型，则关系模式与表达式不匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-167">If an expression result is `null` or fails to convert to the type of a constant by a nullable or unboxing conversion, a relational pattern doesn't match an expression.</span></span>

<span data-ttu-id="945d5-168">有关详细信息，请参阅功能建议说明的[关系模式](~/_csharplang/proposals/csharp-9.0/patterns3.md#relational-patterns)部分。</span><span class="sxs-lookup"><span data-stu-id="945d5-168">For more information, see the [Relational patterns](~/_csharplang/proposals/csharp-9.0/patterns3.md#relational-patterns) section of the feature proposal note.</span></span>

## <a name="logical-patterns"></a><span data-ttu-id="945d5-169">逻辑模式</span><span class="sxs-lookup"><span data-stu-id="945d5-169">Logical patterns</span></span>

<span data-ttu-id="945d5-170">从 C# 9.0 开始，可使用 `not`、`and` 和 `or` 模式连结符来创建以下逻辑模式：</span><span class="sxs-lookup"><span data-stu-id="945d5-170">Beginning with C# 9.0, you use the `not`, `and`, and `or` pattern combinators to create the following *logical patterns*:</span></span>

- <span data-ttu-id="945d5-171">否定 `not` 模式在否定模式与表达式不匹配时与表达式匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-171">*Negation* `not` pattern that matches an expression when the negated pattern doesn't match the expression.</span></span> <span data-ttu-id="945d5-172">下面的示例说明如何否定[常量](#constant-pattern) `null` 模式来检查表达式是否为非空值：</span><span class="sxs-lookup"><span data-stu-id="945d5-172">The following example shows how you can negate a [constant](#constant-pattern) `null` pattern to check if an expression is non-null:</span></span>

  :::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="NotPattern":::

- <span data-ttu-id="945d5-173">合取 `and` 模式在两个模式都与表达式匹配时与表达式匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-173">*Conjunctive* `and` pattern that matches an expression when both patterns match the expression.</span></span> <span data-ttu-id="945d5-174">以下示例显示如何组合[关系模式](#relational-patterns)来检查值是否在某个范围内：</span><span class="sxs-lookup"><span data-stu-id="945d5-174">The following example shows how you can combine [relational patterns](#relational-patterns) to check if a value is in a certain range:</span></span>

  :::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="AndPattern":::

- <span data-ttu-id="945d5-175">析取 `or` 模式在任一模式与表达式匹配时与表达式匹配，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-175">*Disjunctive* `or` pattern that matches an expression when either of patterns matches the expression, as the following example shows:</span></span>

  :::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="OrPattern":::

<span data-ttu-id="945d5-176">如前面的示例所示，可在模式中重复使用模式连结符。</span><span class="sxs-lookup"><span data-stu-id="945d5-176">As the preceding example shows, you can repeatedly use the pattern combinators in a pattern.</span></span>

<span data-ttu-id="945d5-177">`and` 模式连结符的优先级高于 `or`。</span><span class="sxs-lookup"><span data-stu-id="945d5-177">The `and` pattern combinator has higher precedence than `or`.</span></span> <span data-ttu-id="945d5-178">要显式指定优先级，请使用括号，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-178">To explicitly specify the precedence, use parentheses, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="WithParentheses":::

> [!NOTE]
> <span data-ttu-id="945d5-179">检查模式的顺序是不确定的。</span><span class="sxs-lookup"><span data-stu-id="945d5-179">The order in which patterns are checked is undefined.</span></span> <span data-ttu-id="945d5-180">在运行时，可先检查 `or` 和 `and` 模式的右侧嵌套模式。</span><span class="sxs-lookup"><span data-stu-id="945d5-180">At run time, the right-hand nested patterns of `or` and `and` patterns can be checked first.</span></span>

<span data-ttu-id="945d5-181">有关详细信息，请参阅功能建议说明的[模式连结符](~/_csharplang/proposals/csharp-9.0/patterns3.md#pattern-combinators)部分。</span><span class="sxs-lookup"><span data-stu-id="945d5-181">For more information, see the [Pattern combinators](~/_csharplang/proposals/csharp-9.0/patterns3.md#pattern-combinators) section of the feature proposal note.</span></span>

## <a name="property-pattern"></a><span data-ttu-id="945d5-182">属性模式</span><span class="sxs-lookup"><span data-stu-id="945d5-182">Property pattern</span></span>

<span data-ttu-id="945d5-183">从 C# 8.0 开始，可使用属性模式来将表达式的属性或字段与嵌套模式进行匹配，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-183">Beginning with C# 8.0, you use a *property pattern* to match an expression's properties or fields against nested patterns, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/PropertyPattern.cs" id="BasicExample":::

<span data-ttu-id="945d5-184">当表达式结果为非 NULL 且每个嵌套模式都与表达式结果的相应属性或字段匹配时，属性模式将与表达式匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-184">A property pattern matches an expression when an expression result is non-null and every nested pattern matches the corresponding property or field of the expression result.</span></span>

<span data-ttu-id="945d5-185">还可将运行时类型检查和变量声明添加到属性模式，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-185">You can also add a runtime type check and a variable declaration to a property pattern, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/PropertyPattern.cs" id="WithTypeCheck":::

<span data-ttu-id="945d5-186">属性模式是一种递归模式。</span><span class="sxs-lookup"><span data-stu-id="945d5-186">A property pattern is a recursive pattern.</span></span> <span data-ttu-id="945d5-187">也就是说，可以将任何模式用作嵌套模式。</span><span class="sxs-lookup"><span data-stu-id="945d5-187">That is, you can use any pattern as a nested pattern.</span></span> <span data-ttu-id="945d5-188">使用属性模式将部分数据与嵌套模式进行匹配，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-188">Use a property pattern to match parts of data against nested patterns, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/PropertyPattern.cs" id="RecursivePropertyPattern":::

<span data-ttu-id="945d5-189">前面的示例使用 C# 9.0 及更高版本中提供的两个功能：`or` [模式连结符](#logical-patterns)和[记录类型](../builtin-types/record.md)。</span><span class="sxs-lookup"><span data-stu-id="945d5-189">The preceding example uses two features available in C# 9.0 and later: `or` [pattern combinator](#logical-patterns) and [record types](../builtin-types/record.md).</span></span>

<span data-ttu-id="945d5-190">有关详细信息，请参阅功能建议说明的[属性模式](~/_csharplang/proposals/csharp-8.0/patterns.md#property-pattern)部分。</span><span class="sxs-lookup"><span data-stu-id="945d5-190">For more information, see the [Property pattern](~/_csharplang/proposals/csharp-8.0/patterns.md#property-pattern) section of the feature proposal note.</span></span>

## <a name="positional-pattern"></a><span data-ttu-id="945d5-191">位置模式</span><span class="sxs-lookup"><span data-stu-id="945d5-191">Positional pattern</span></span>

<span data-ttu-id="945d5-192">从 C# 8.0 开始，可使用位置模式来解构表达式结果并将结果值与相应的嵌套模式匹配，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-192">Beginning with C# 8.0, you use a *positional pattern* to deconstruct an expression result and match the resulting values against the corresponding nested patterns, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="BasicExample":::

<span data-ttu-id="945d5-193">在前面的示例中，表达式的类型包含 [Deconstruct](../../deconstruct.md) 方法，该方法用于解构表达式结果。</span><span class="sxs-lookup"><span data-stu-id="945d5-193">At the preceding example, the type of an expression contains the [Deconstruct](../../deconstruct.md) method, which is used to deconstruct an expression result.</span></span> <span data-ttu-id="945d5-194">还可将[元组类型](../builtin-types/value-tuples.md)的表达式与位置模式进行匹配。</span><span class="sxs-lookup"><span data-stu-id="945d5-194">You can also match expressions of [tuple types](../builtin-types/value-tuples.md) against positional patterns.</span></span> <span data-ttu-id="945d5-195">这样，就可将多个输入与各种模式进行匹配，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-195">In that way, you can match multiple inputs against various patterns, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="MatchTuple":::

<span data-ttu-id="945d5-196">前面的示例使用在 C# 9.0 及更高版本中提供的[关系](#relational-patterns)和[逻辑](#logical-patterns)模式。</span><span class="sxs-lookup"><span data-stu-id="945d5-196">The preceding example uses [relational](#relational-patterns) and [logical](#logical-patterns) patterns, which are available in C# 9.0 and later.</span></span>

<span data-ttu-id="945d5-197">可在位置模式中使用元组元素的名称和 `Deconstruct` 参数，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-197">You can use the names of tuple elements and `Deconstruct` parameters in a positional pattern, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="UseIdentifiers":::

<span data-ttu-id="945d5-198">还可通过以下任一方式扩展位置模式：</span><span class="sxs-lookup"><span data-stu-id="945d5-198">You can also extend a positional pattern in any of the following ways:</span></span>

- <span data-ttu-id="945d5-199">添加运行时类型检查和变量声明，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-199">Add a runtime type check and a variable declaration, as the following example shows:</span></span>

  :::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="WithTypeCheck":::

  <span data-ttu-id="945d5-200">前面的示例使用隐式提供 `Deconstruct` 方法的[位置记录](../builtin-types/record.md#positional-syntax-for-property-definition)。</span><span class="sxs-lookup"><span data-stu-id="945d5-200">The preceding example uses [positional records](../builtin-types/record.md#positional-syntax-for-property-definition) that implicitly provide the `Deconstruct` method.</span></span>

- <span data-ttu-id="945d5-201">在位置模式中使用[属性模式](#property-pattern)，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-201">Use a [property pattern](#property-pattern) within a positional pattern, as the following example shows:</span></span>

  :::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="WithPropertyPattern":::

- <span data-ttu-id="945d5-202">结合前面的两种用法，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-202">Combine two preceding usages, as the following example shows:</span></span>

  :::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="CompletePositionalPattern":::

<span data-ttu-id="945d5-203">位置模式是一种递归模式。</span><span class="sxs-lookup"><span data-stu-id="945d5-203">A positional pattern is a recursive pattern.</span></span> <span data-ttu-id="945d5-204">也就是说，可以将任何模式用作嵌套模式。</span><span class="sxs-lookup"><span data-stu-id="945d5-204">That is, you can use any pattern as a nested pattern.</span></span>

<span data-ttu-id="945d5-205">有关详细信息，请参阅功能建议说明的[位置模式](~/_csharplang/proposals/csharp-8.0/patterns.md#positional-pattern)部分。</span><span class="sxs-lookup"><span data-stu-id="945d5-205">For more information, see the [Positional pattern](~/_csharplang/proposals/csharp-8.0/patterns.md#positional-pattern) section of the feature proposal note.</span></span>

## <a name="var-pattern"></a><span data-ttu-id="945d5-206">`var` 模式</span><span class="sxs-lookup"><span data-stu-id="945d5-206">`var` pattern</span></span>

<span data-ttu-id="945d5-207">从 C# 7.0 开始，可使用 `var` 模式来匹配任何表达式（包括 `null`），并将其结果分配给新的局部变量，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-207">Beginning with C# 7.0, you use a *`var` pattern* to match any expression, including `null`, and assign its result to a new local variable, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/VarPattern.cs" id="KeepInterimResult":::

<span data-ttu-id="945d5-208">需要布尔表达式中的临时变量来保存中间计算的结果时，`var` 模式很有用。</span><span class="sxs-lookup"><span data-stu-id="945d5-208">A `var` pattern is useful when you need a temporary variable within a Boolean expression to hold the result of intermediate calculations.</span></span> <span data-ttu-id="945d5-209">当需要在 `switch` 表达式或语句的 `when` 大小写临界子句中执行其他检查时，也可使用 `var` 模式，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-209">You can also use a `var` pattern when you need to perform additional checks in `when` case guards of a `switch` expression or statement, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/VarPattern.cs" id="WithCaseGuards":::

<span data-ttu-id="945d5-210">在前面的示例中，模式 `var (x, y)` 等效于[位置模式](#positional-pattern) `(var x, var y)`。</span><span class="sxs-lookup"><span data-stu-id="945d5-210">In the preceding example, pattern `var (x, y)` is equivalent to a [positional pattern](#positional-pattern) `(var x, var y)`.</span></span>

<span data-ttu-id="945d5-211">在 `var` 模式中，声明变量的类型是与该模式匹配的表达式的编译时类型。</span><span class="sxs-lookup"><span data-stu-id="945d5-211">In a `var` pattern, the type of a declared variable is the compile-time type of the expression that is matched against the pattern.</span></span>

<span data-ttu-id="945d5-212">有关详细信息，请参阅功能建议说明的 [Var 模式](~/_csharplang/proposals/csharp-8.0/patterns.md#var-pattern)部分。</span><span class="sxs-lookup"><span data-stu-id="945d5-212">For more information, see the [Var pattern](~/_csharplang/proposals/csharp-8.0/patterns.md#var-pattern) section of the feature proposal note.</span></span>

## <a name="discard-pattern"></a><span data-ttu-id="945d5-213">弃元模式</span><span class="sxs-lookup"><span data-stu-id="945d5-213">Discard pattern</span></span>

<span data-ttu-id="945d5-214">从 C# 8.0 开始，可使用弃元模式 `_` 来匹配任何表达式，包括 `null`，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-214">Beginning with C# 8.0, you use a *discard pattern* `_` to match any expression, including `null`, as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/DiscardPattern.cs" id="BasicExample":::

<span data-ttu-id="945d5-215">在前面的示例中，弃元模式用于处理 `null` 以及没有相应的 <xref:System.DayOfWeek> 枚举成员的任何整数值。</span><span class="sxs-lookup"><span data-stu-id="945d5-215">In the preceding example, a discard pattern is used to handle `null` and any integer value that doesn't have the corresponding member of the <xref:System.DayOfWeek> enumeration.</span></span> <span data-ttu-id="945d5-216">这可保证示例中的 `switch` 表达式可处理所有可能的输入值。</span><span class="sxs-lookup"><span data-stu-id="945d5-216">That guarantees that a `switch` expression in the example handles all possible input values.</span></span> <span data-ttu-id="945d5-217">如果没有在 `switch` 表达式中使用弃元模式，并且该表达式的任何模式均与输入不匹配，则运行时[会引发异常](switch-expression.md#non-exhaustive-switch-expressions)。</span><span class="sxs-lookup"><span data-stu-id="945d5-217">If you don't use a discard pattern in a `switch` expression and none of the expression's patterns matches an input, the runtime [throws an exception](switch-expression.md#non-exhaustive-switch-expressions).</span></span> <span data-ttu-id="945d5-218">如果 `switch` 表达式未处理所有可能的输入值，则编译器会生成警告。</span><span class="sxs-lookup"><span data-stu-id="945d5-218">The compiler generates a warning if a `switch` expression doesn't handle all possible input values.</span></span>

<span data-ttu-id="945d5-219">弃元模式不能是 `is` 表达式或 `switch` 语句中的模式。</span><span class="sxs-lookup"><span data-stu-id="945d5-219">A discard pattern cannot be a pattern in an `is` expression or a `switch` statement.</span></span> <span data-ttu-id="945d5-220">在这些案例中，要匹配任何表达式，请使用带有弃元 `var _` 的 [`var` 模式](#var-pattern)。</span><span class="sxs-lookup"><span data-stu-id="945d5-220">In those cases, to match any expression, use a [`var` pattern](#var-pattern) with a discard: `var _`.</span></span>

<span data-ttu-id="945d5-221">有关详细信息，请参阅功能建议说明的[弃元模式](~/_csharplang/proposals/csharp-8.0/patterns.md#discard-pattern)部分。</span><span class="sxs-lookup"><span data-stu-id="945d5-221">For more information, see the [Discard pattern](~/_csharplang/proposals/csharp-8.0/patterns.md#discard-pattern) section of the feature proposal note.</span></span>

## <a name="parenthesized-pattern"></a><span data-ttu-id="945d5-222">带括号模式</span><span class="sxs-lookup"><span data-stu-id="945d5-222">Parenthesized pattern</span></span>

<span data-ttu-id="945d5-223">从 C# 9.0 开始，可在任何模式两边加上括号。</span><span class="sxs-lookup"><span data-stu-id="945d5-223">Beginning with C# 9.0, you can put parentheses around any pattern.</span></span> <span data-ttu-id="945d5-224">通常，这样做是为了强调或更改[逻辑模式](#logical-patterns)中的优先级，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="945d5-224">Typically, you do that to emphasize or change the precedence in [logical patterns](#logical-patterns), as the following example shows:</span></span>

:::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="ChangedPrecedence":::

## <a name="c-language-specification"></a><span data-ttu-id="945d5-225">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="945d5-225">C# language specification</span></span>

<span data-ttu-id="945d5-226">有关详细信息，请参阅以下功能建议说明：</span><span class="sxs-lookup"><span data-stu-id="945d5-226">For more information, see the following feature proposal notes:</span></span>

- [<span data-ttu-id="945d5-227">C# 7.0 的模式匹配</span><span class="sxs-lookup"><span data-stu-id="945d5-227">Pattern matching for C# 7.0</span></span>](~/_csharplang/proposals/csharp-7.0/pattern-matching.md)
- [<span data-ttu-id="945d5-228">递归模式匹配（在 C# 8.0 中引入）</span><span class="sxs-lookup"><span data-stu-id="945d5-228">Recursive pattern matching (introduced in C# 8.0)</span></span>](~/_csharplang/proposals/csharp-8.0/patterns.md)
- [<span data-ttu-id="945d5-229">C# 9.0 的模式匹配更改</span><span class="sxs-lookup"><span data-stu-id="945d5-229">Pattern-matching changes for C# 9.0</span></span>](~/_csharplang/proposals/csharp-9.0/patterns3.md)

## <a name="see-also"></a><span data-ttu-id="945d5-230">另请参阅</span><span class="sxs-lookup"><span data-stu-id="945d5-230">See also</span></span>

- [<span data-ttu-id="945d5-231">C# 参考</span><span class="sxs-lookup"><span data-stu-id="945d5-231">C# reference</span></span>](../index.md)
- [<span data-ttu-id="945d5-232">C# 运算符和表达式</span><span class="sxs-lookup"><span data-stu-id="945d5-232">C# operators and expressions</span></span>](index.md)
- [<span data-ttu-id="945d5-233">教程：使用模式匹配来构建类型驱动和数据驱动的算法</span><span class="sxs-lookup"><span data-stu-id="945d5-233">Tutorial: Use pattern matching to build type-driven and data-driven algorithms</span></span>](../../tutorials/pattern-matching.md)
