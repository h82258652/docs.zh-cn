---
description: '?: 运算符 - C# 参考'
title: '?: 运算符 - C# 参考'
ms.date: 09/17/2020
f1_keywords:
- ?:_CSharpKeyword
- ?_CSharpKeyword
- :_CSharpKeyword
helpviewer_keywords:
- '?: operator [C#]'
- conditional operator (?:) [C#]
ms.assetid: e83a17f1-7500-48ba-8bee-2fbc4c847af4
ms.openlocfilehash: 84a740f3afeca94a706b4120b8cd8f191f49a048
ms.sourcegitcommit: bbc724b72fb6c978905ac715e4033efa291f84dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "107369564"
---
# <a name="-operator-c-reference"></a><span data-ttu-id="6c125-103">?: 运算符（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="6c125-103">?: operator (C# reference)</span></span>

<span data-ttu-id="6c125-104">条件运算符 (`?:`) 也称为三元条件运算符，用于计算布尔表达式，并根据布尔表达式的计算结果为 `true` 还是 `false` 来返回两个表达式中的一个结果，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="6c125-104">The conditional operator `?:`, also known as the ternary conditional operator, evaluates a Boolean expression and returns the result of one of the two expressions, depending on whether the Boolean expression evaluates to `true` or `false`, as the following example shows:</span></span>

:::code language="csharp" interactive="try-dotnet-method" source="snippets/shared/ConditionalOperator.cs" id="BasicExample":::

<span data-ttu-id="6c125-105">如上述示例所示，条件运算符的语法如下所示：</span><span class="sxs-lookup"><span data-stu-id="6c125-105">As the preceding example shows, the syntax for the conditional operator is as follows:</span></span>

```csharp
condition ? consequent : alternative
```

<span data-ttu-id="6c125-106">`condition` 表达式的计算结果必须为 `true` 或 `false`。</span><span class="sxs-lookup"><span data-stu-id="6c125-106">The `condition` expression must evaluate to `true` or `false`.</span></span> <span data-ttu-id="6c125-107">若 `condition` 的计算结果为 `true`，将计算 `consequent`，其结果成为运算结果。</span><span class="sxs-lookup"><span data-stu-id="6c125-107">If `condition` evaluates to `true`, the `consequent` expression is evaluated, and its result becomes the result of the operation.</span></span> <span data-ttu-id="6c125-108">若 `condition` 的计算结果为 `false`，将计算 `alternative`，其结果成为运算结果。</span><span class="sxs-lookup"><span data-stu-id="6c125-108">If `condition` evaluates to `false`, the `alternative` expression is evaluated, and its result becomes the result of the operation.</span></span> <span data-ttu-id="6c125-109">只会计算 `consequent` 或 `alternative`。</span><span class="sxs-lookup"><span data-stu-id="6c125-109">Only `consequent` or `alternative` is evaluated.</span></span>

<span data-ttu-id="6c125-110">从 C# 9.0 开始，条件表达式由目标确定类型。</span><span class="sxs-lookup"><span data-stu-id="6c125-110">Beginning with C# 9.0, conditional expressions are target-typed.</span></span> <span data-ttu-id="6c125-111">也就是说，如果条件表达式的目标类型是已知的，则 `consequent` 和 `alternative` 的类型必须可隐式转换为目标类型，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="6c125-111">That is, if a target type of a conditional expression is known, the types of `consequent` and `alternative` must be implicitly convertible to the target type, as the following example shows:</span></span>

[!code-csharp[target-typed conditional](snippets/shared/ConditionalOperator.cs#TargetTyped)]

<span data-ttu-id="6c125-112">如果条件表达式的目标类型是未知的（例如使用 [`var`](../keywords/var.md) 关键字时）或者采用 C# 8.0 及更早版本，则 `consequent` 和 `alternative` 的类型必须相同，或者必须存在从一种类型到另一种类型的隐式转换：</span><span class="sxs-lookup"><span data-stu-id="6c125-112">If a target type of a conditional expression is unknown (for example, when you use the [`var`](../keywords/var.md) keyword) or in C# 8.0 and earlier, the type of `consequent` and `alternative` must be the same or there must be an implicit conversion from one type to the other:</span></span>

[!code-csharp[not target-typed conditional](snippets/shared/ConditionalOperator.cs#NotTargetTyped)]

<span data-ttu-id="6c125-113">条件运算符为右联运算符，即形式的表达式</span><span class="sxs-lookup"><span data-stu-id="6c125-113">The conditional operator is right-associative, that is, an expression of the form</span></span>

```csharp
a ? b : c ? d : e
```

<span data-ttu-id="6c125-114">计算结果为</span><span class="sxs-lookup"><span data-stu-id="6c125-114">is evaluated as</span></span>

```csharp
a ? b : (c ? d : e)
```

> [!TIP]
> <span data-ttu-id="6c125-115">可以使用以下助记键设备记住条件运算符的计算方式：</span><span class="sxs-lookup"><span data-stu-id="6c125-115">You can use the following mnemonic device to remember how the conditional operator is evaluated:</span></span>
>
> ```text
> is this condition true ? yes : no
> ```

## <a name="conditional-ref-expression"></a><span data-ttu-id="6c125-116">ref 条件表达式</span><span class="sxs-lookup"><span data-stu-id="6c125-116">Conditional ref expression</span></span>

<span data-ttu-id="6c125-117">从 C# 7.2 开始，可通过 ref 条件表达式有条件地分配 [ref local](../keywords/ref.md#ref-locals) 或 [ref readonly local](../keywords/ref.md#ref-readonly-locals) 变量。</span><span class="sxs-lookup"><span data-stu-id="6c125-117">Beginning with C# 7.2, a [ref local](../keywords/ref.md#ref-locals) or [ref readonly local](../keywords/ref.md#ref-readonly-locals) variable can be assigned conditionally with a conditional ref expression.</span></span> <span data-ttu-id="6c125-118">还可以使用 ref 条件表达式作为[引用返回值](../keywords/ref.md#reference-return-values)或 [`ref` 方法参数](../keywords/ref.md#passing-an-argument-by-reference)。</span><span class="sxs-lookup"><span data-stu-id="6c125-118">You can also use a conditional ref expression as a [reference return value](../keywords/ref.md#reference-return-values) or as a [`ref` method argument](../keywords/ref.md#passing-an-argument-by-reference).</span></span>

<span data-ttu-id="6c125-119">ref 条件表达式的语法如下所示：</span><span class="sxs-lookup"><span data-stu-id="6c125-119">The syntax for a conditional ref expression is as follows:</span></span>

```csharp
condition ? ref consequent : ref alternative
```

<span data-ttu-id="6c125-120">ref 条件表达式与原始的条件运算符相似，仅计算两个表达式其中之一：`consequent` 或 `alternative`。</span><span class="sxs-lookup"><span data-stu-id="6c125-120">Like the original conditional operator, a conditional ref expression evaluates only one of the two expressions: either `consequent` or `alternative`.</span></span>

<span data-ttu-id="6c125-121">在 ref 条件表达式中，`consequent` 和 `alternative` 的类型必须相同。</span><span class="sxs-lookup"><span data-stu-id="6c125-121">In the case of a conditional ref expression, the type of `consequent` and `alternative` must be the same.</span></span> <span data-ttu-id="6c125-122">ref 条件表达式不由目标确定类型。</span><span class="sxs-lookup"><span data-stu-id="6c125-122">Conditional ref expressions are not target-typed.</span></span>

<span data-ttu-id="6c125-123">下面的示例演示 ref 条件表达式的用法：</span><span class="sxs-lookup"><span data-stu-id="6c125-123">The following example demonstrates the usage of a conditional ref expression:</span></span>

[!code-csharp-interactive[conditional ref](snippets/shared/ConditionalOperator.cs#ConditionalRef)]

## <a name="conditional-operator-and-an-ifelse-statement"></a><span data-ttu-id="6c125-124">条件运算符和 `if..else` 语句</span><span class="sxs-lookup"><span data-stu-id="6c125-124">Conditional operator and an `if..else` statement</span></span>

<span data-ttu-id="6c125-125">需要根据条件计算值时，使用条件运算符而不是 [if-else](../keywords/if-else.md) 语句可以使代码更简洁。</span><span class="sxs-lookup"><span data-stu-id="6c125-125">Use of the conditional operator instead of an [if-else](../keywords/if-else.md) statement might result in more concise code in cases when you need conditionally to compute a value.</span></span> <span data-ttu-id="6c125-126">下面的示例演示了将整数归类为负数或非负数的两种方法：</span><span class="sxs-lookup"><span data-stu-id="6c125-126">The following example demonstrates two ways to classify an integer as negative or nonnegative:</span></span>

[!code-csharp[conditional and if-else](snippets/shared/ConditionalOperator.cs#CompareWithIf)]

## <a name="operator-overloadability"></a><span data-ttu-id="6c125-127">运算符可重载性</span><span class="sxs-lookup"><span data-stu-id="6c125-127">Operator overloadability</span></span>

<span data-ttu-id="6c125-128">用户定义类型不能重载条件运算符。</span><span class="sxs-lookup"><span data-stu-id="6c125-128">A user-defined type cannot overload the conditional operator.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="6c125-129">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="6c125-129">C# language specification</span></span>

<span data-ttu-id="6c125-130">有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[条件运算符](~/_csharplang/spec/expressions.md#conditional-operator)部分。</span><span class="sxs-lookup"><span data-stu-id="6c125-130">For more information, see the [Conditional operator](~/_csharplang/spec/expressions.md#conditional-operator) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

<span data-ttu-id="6c125-131">有关 C# 7.2 及更高版本中添加的功能的详细信息，请参阅以下功能建议说明：</span><span class="sxs-lookup"><span data-stu-id="6c125-131">For more information about features added in C# 7.2 and later, see the following feature proposal notes:</span></span>

- [<span data-ttu-id="6c125-132">ref 条件表达式 (C# 7.2)</span><span class="sxs-lookup"><span data-stu-id="6c125-132">Conditional ref expressions (C# 7.2)</span></span>](~/_csharplang/proposals/csharp-7.2/conditional-ref.md)
- [<span data-ttu-id="6c125-133">目标类型的条件表达式 (C# 9.0)</span><span class="sxs-lookup"><span data-stu-id="6c125-133">Target-typed conditional expression (C# 9.0)</span></span>](~/_csharplang/proposals/csharp-9.0/target-typed-conditional-expression.md)

## <a name="see-also"></a><span data-ttu-id="6c125-134">请参阅</span><span class="sxs-lookup"><span data-stu-id="6c125-134">See also</span></span>

- [<span data-ttu-id="6c125-135">C# 参考</span><span class="sxs-lookup"><span data-stu-id="6c125-135">C# reference</span></span>](../index.md)
- [<span data-ttu-id="6c125-136">C# 运算符和表达式</span><span class="sxs-lookup"><span data-stu-id="6c125-136">C# operators and expressions</span></span>](index.md)
- [<span data-ttu-id="6c125-137">if-else 语句</span><span class="sxs-lookup"><span data-stu-id="6c125-137">if-else statement</span></span>](../keywords/if-else.md)
- <span data-ttu-id="6c125-138">[?. 和 ?[] 运算符](member-access-operators.md#null-conditional-operators--and-)</span><span class="sxs-lookup"><span data-stu-id="6c125-138">[?. and ?[] operators](member-access-operators.md#null-conditional-operators--and-)</span></span>
- [<span data-ttu-id="6c125-139">?? 和 ??= 运算符</span><span class="sxs-lookup"><span data-stu-id="6c125-139">?? and ??= operators</span></span>](null-coalescing-operator.md)
- [<span data-ttu-id="6c125-140">ref 关键字</span><span class="sxs-lookup"><span data-stu-id="6c125-140">ref keyword</span></span>](../keywords/ref.md)
