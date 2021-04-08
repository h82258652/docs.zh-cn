---
title: 如何为类或结构定义值相等性 - C# 编程指南
description: 了解如何为类或结构定义值相等性。 查看代码示例和可用资源。
ms.date: 03/26/2021
helpviewer_keywords:
- overriding Equals method [C#]
- object equivalence [C#]
- Equals method [C#], overriding
- value equality [C#]
- equivalence [C#]
ms.assetid: 4084581e-b931-498b-9534-cf7ef5b68690
ms.openlocfilehash: fd8e1e14650a836178534b44dc332215c0d0586a
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079448"
---
# <a name="how-to-define-value-equality-for-a-class-or-struct-c-programming-guide"></a><span data-ttu-id="05a67-104">如何为类或结构定义值相等性（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="05a67-104">How to define value equality for a class or struct (C# Programming Guide)</span></span>

<span data-ttu-id="05a67-105">[记录](../classes-and-structs/records.md)自动实现值相等性。</span><span class="sxs-lookup"><span data-stu-id="05a67-105">[Records](../classes-and-structs/records.md) automatically implement value equality.</span></span> <span data-ttu-id="05a67-106">当你的类型为数据建模并应实现值相等性时，请考虑定义 `record` 而不是 `class`。</span><span class="sxs-lookup"><span data-stu-id="05a67-106">Consider defining a `record` instead of a `class` when your type models data and should implement value equality.</span></span>

<span data-ttu-id="05a67-107">定义类或结构时，需确定为类型创建值相等性（或等效性）的自定义定义是否有意义。</span><span class="sxs-lookup"><span data-stu-id="05a67-107">When you define a class or struct, you decide whether it makes sense to create a custom definition of value equality (or equivalence) for the type.</span></span> <span data-ttu-id="05a67-108">通常，预期将类型的对象添加到集合时，或者这些对象主要用于存储一组字段或属性时，需实现值相等性。</span><span class="sxs-lookup"><span data-stu-id="05a67-108">Typically, you implement value equality when you expect to add objects of the type to a collection, or when their primary purpose is to store a set of fields or properties.</span></span> <span data-ttu-id="05a67-109">可以基于类型中所有字段和属性的比较结果来定义值相等性，也可以基于子集进行定义。</span><span class="sxs-lookup"><span data-stu-id="05a67-109">You can base your definition of value equality on a comparison of all the fields and properties in the type, or you can base the definition on a subset.</span></span>

<span data-ttu-id="05a67-110">在任何一种情况下，类和结构中的实现均应遵循 5 个等效性保证条件（对于以下规则，假设 `x`、`y` 和 `z` 都不为 null）：</span><span class="sxs-lookup"><span data-stu-id="05a67-110">In either case, and in both classes and structs, your implementation should follow the five guarantees of equivalence (for the following rules, assume that `x`, `y` and `z` are not null):</span></span>  
  
1. <span data-ttu-id="05a67-111">自反属性：`x.Equals(x)` 将返回 `true`。</span><span class="sxs-lookup"><span data-stu-id="05a67-111">The reflexive property: `x.Equals(x)` returns `true`.</span></span>
  
2. <span data-ttu-id="05a67-112">对称属性：`x.Equals(y)` 返回与 `y.Equals(x)` 相同的值。</span><span class="sxs-lookup"><span data-stu-id="05a67-112">The symmetric property: `x.Equals(y)` returns the same value as `y.Equals(x)`.</span></span>
  
3. <span data-ttu-id="05a67-113">可传递属性：如果 `(x.Equals(y) && y.Equals(z))` 返回 `true`，则 `x.Equals(z)` 将返回 `true`。</span><span class="sxs-lookup"><span data-stu-id="05a67-113">The transitive property: if `(x.Equals(y) && y.Equals(z))` returns `true`, then `x.Equals(z)` returns `true`.</span></span>
  
4. <span data-ttu-id="05a67-114">只要未修改 x 和 y 引用的对象，`x.Equals(y)` 的连续调用就将返回相同的值。</span><span class="sxs-lookup"><span data-stu-id="05a67-114">Successive invocations of `x.Equals(y)` return the same value as long as the objects referenced by x and y aren't modified.</span></span>  
  
5. <span data-ttu-id="05a67-115">任何非 null 值均不等于 null。</span><span class="sxs-lookup"><span data-stu-id="05a67-115">Any non-null value isn't equal to null.</span></span> <span data-ttu-id="05a67-116">然而，当 `x` 为 null 时，`x.Equals(y)` 将引发异常。</span><span class="sxs-lookup"><span data-stu-id="05a67-116">However, `x.Equals(y)` throws an exception when `x` is null.</span></span> <span data-ttu-id="05a67-117">这会违反规则 1 或 2，具体取决于 `Equals` 的参数。</span><span class="sxs-lookup"><span data-stu-id="05a67-117">That breaks rules 1 or 2, depending on the argument to `Equals`.</span></span>

<span data-ttu-id="05a67-118">定义的任何结构都已具有其从 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法的 <xref:System.ValueType?displayProperty=nameWithType> 替代中继承的值相等性的默认实现。</span><span class="sxs-lookup"><span data-stu-id="05a67-118">Any struct that you define already has a default implementation of value equality that it inherits from the <xref:System.ValueType?displayProperty=nameWithType> override of the <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> method.</span></span> <span data-ttu-id="05a67-119">此实现使用反射来检查类型中的所有字段和属性。</span><span class="sxs-lookup"><span data-stu-id="05a67-119">This implementation uses reflection to examine all the fields and properties in the type.</span></span> <span data-ttu-id="05a67-120">尽管此实现可生成正确的结果，但与专门为类型编写的自定义实现相比，它的速度相对较慢。</span><span class="sxs-lookup"><span data-stu-id="05a67-120">Although this implementation produces correct results, it is relatively slow compared to a custom implementation that you write specifically for the type.</span></span>  
  
<span data-ttu-id="05a67-121">类和结构的值相等性的实现详细信息有所不同。</span><span class="sxs-lookup"><span data-stu-id="05a67-121">The implementation details for value equality are different for classes and structs.</span></span> <span data-ttu-id="05a67-122">但是，类和结构都需要相同的基础步骤来实现相等性：</span><span class="sxs-lookup"><span data-stu-id="05a67-122">However, both classes and structs require the same basic steps for implementing equality:</span></span>  
  
1. <span data-ttu-id="05a67-123">替代[虚拟](../../language-reference/keywords/virtual.md) <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法。</span><span class="sxs-lookup"><span data-stu-id="05a67-123">Override the [virtual](../../language-reference/keywords/virtual.md) <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> method.</span></span> <span data-ttu-id="05a67-124">大多数情况下，`bool Equals( object obj )` 实现应只调入作为 <xref:System.IEquatable%601?displayProperty=nameWithType> 接口的实现的类型特定 `Equals` 方法。</span><span class="sxs-lookup"><span data-stu-id="05a67-124">In most cases, your implementation of `bool Equals( object obj )` should just call into the type-specific `Equals` method that is the implementation of the <xref:System.IEquatable%601?displayProperty=nameWithType> interface.</span></span> <span data-ttu-id="05a67-125">（请参阅步骤 2。）</span><span class="sxs-lookup"><span data-stu-id="05a67-125">(See step 2.)</span></span>  
  
2. <span data-ttu-id="05a67-126">通过提供类型特定的 `Equals` 方法实现 <xref:System.IEquatable%601?displayProperty=nameWithType> 接口。</span><span class="sxs-lookup"><span data-stu-id="05a67-126">Implement the <xref:System.IEquatable%601?displayProperty=nameWithType> interface by providing a type-specific `Equals` method.</span></span> <span data-ttu-id="05a67-127">实际的等效性比较将在此接口中执行。</span><span class="sxs-lookup"><span data-stu-id="05a67-127">This is where the actual equivalence comparison is performed.</span></span> <span data-ttu-id="05a67-128">例如，可能决定通过仅比较类型中的一两个字段来定义相等性。</span><span class="sxs-lookup"><span data-stu-id="05a67-128">For example, you might decide to define equality by comparing only one or two fields in your type.</span></span> <span data-ttu-id="05a67-129">不会从 `Equals` 引发异常。</span><span class="sxs-lookup"><span data-stu-id="05a67-129">Don't throw exceptions from `Equals`.</span></span> <span data-ttu-id="05a67-130">对于与继承相关的类：</span><span class="sxs-lookup"><span data-stu-id="05a67-130">For classes that are related by inheritance:</span></span>

   * <span data-ttu-id="05a67-131">此方法应仅检查类中声明的字段。</span><span class="sxs-lookup"><span data-stu-id="05a67-131">This method should examine only fields that are declared in the class.</span></span> <span data-ttu-id="05a67-132">它应调用 `base.Equals` 来检查基类中的字段。</span><span class="sxs-lookup"><span data-stu-id="05a67-132">It should call `base.Equals` to examine fields that are in the base class.</span></span> <span data-ttu-id="05a67-133">（如果类型直接从 <xref:System.Object> 中继承，则不会调用 `base.Equals`，因为 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 的 <xref:System.Object> 实现会执行引用相等性检查。）</span><span class="sxs-lookup"><span data-stu-id="05a67-133">(Don't call `base.Equals` if the type inherits directly from <xref:System.Object>, because the <xref:System.Object> implementation of <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> performs a reference equality check.)</span></span>

   * <span data-ttu-id="05a67-134">仅当要比较的变量的运行时类型相同时，才应将两个变量视为相等。</span><span class="sxs-lookup"><span data-stu-id="05a67-134">Two variables should be deemed equal only if the run-time types of the variables being compared are the same.</span></span> <span data-ttu-id="05a67-135">此外，如果变量的运行时和编译时类型不同，请确保使用运行时类型的 `Equals` 方法的 `IEquatable` 实现。</span><span class="sxs-lookup"><span data-stu-id="05a67-135">Also, make sure that the `IEquatable` implementation of the `Equals` method for the run-time type is used if the run-time and compile-time types of a variable are different.</span></span> <span data-ttu-id="05a67-136">确保始终正确比较运行时类型的一种策略是仅在 `sealed` 类中实现 `IEquatable`。</span><span class="sxs-lookup"><span data-stu-id="05a67-136">One strategy for making sure run-time types are always compared correctly is to implement `IEquatable` only in `sealed` classes.</span></span> <span data-ttu-id="05a67-137">有关详细信息，请参阅本文后续部分的[类示例](#class-example)。</span><span class="sxs-lookup"><span data-stu-id="05a67-137">For more information, see the [class example](#class-example) later in this article.</span></span>
  
3. <span data-ttu-id="05a67-138">可选但建议这样做：重载 [==](../../language-reference/operators/equality-operators.md#equality-operator-) 和 [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) 运算符。</span><span class="sxs-lookup"><span data-stu-id="05a67-138">Optional but recommended: Overload the [==](../../language-reference/operators/equality-operators.md#equality-operator-) and [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) operators.</span></span>  
  
4. <span data-ttu-id="05a67-139">替代 <xref:System.Object.GetHashCode%2A?displayProperty=nameWithType>，以便具有值相等性的两个对象生成相同的哈希代码。</span><span class="sxs-lookup"><span data-stu-id="05a67-139">Override <xref:System.Object.GetHashCode%2A?displayProperty=nameWithType> so that two objects that have value equality produce the same hash code.</span></span>  
  
5. <span data-ttu-id="05a67-140">可选：若要支持“大于”或“小于”定义，请为类型实现 <xref:System.IComparable%601> 接口，并同时重载 [<=](../../language-reference/operators/comparison-operators.md#less-than-or-equal-operator-) 和 [>=](../../language-reference/operators/comparison-operators.md#greater-than-or-equal-operator-) 运算符。</span><span class="sxs-lookup"><span data-stu-id="05a67-140">Optional: To support definitions for "greater than" or "less than," implement the <xref:System.IComparable%601> interface for your type, and also overload the [<=](../../language-reference/operators/comparison-operators.md#less-than-or-equal-operator-) and [>=](../../language-reference/operators/comparison-operators.md#greater-than-or-equal-operator-) operators.</span></span>  

> [!NOTE]
> <span data-ttu-id="05a67-141">从 C# 9.0 开始，可以使用记录来获取值相等性语义，而不需要任何不必要的样板代码。</span><span class="sxs-lookup"><span data-stu-id="05a67-141">Starting in C# 9.0, you can use records to get value equality semantics without any unnecessary boilerplate code.</span></span>

## <a name="class-example"></a><span data-ttu-id="05a67-142">类示例</span><span class="sxs-lookup"><span data-stu-id="05a67-142">Class example</span></span>

<span data-ttu-id="05a67-143">下面的示例演示如何在类（引用类型）中实现值相等性。</span><span class="sxs-lookup"><span data-stu-id="05a67-143">The following example shows how to implement value equality in a class (reference type).</span></span>

:::code language="csharp" source="snippets/how-to-define-value-equality-for-a-type/ValueEqualityClass/Program.cs":::

<span data-ttu-id="05a67-144">在类（引用类型）上，两种 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> 方法的默认实现均执行引用相等性比较，而不是值相等性检查。</span><span class="sxs-lookup"><span data-stu-id="05a67-144">On classes (reference types), the default implementation of both <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> methods performs a reference equality comparison, not a value equality check.</span></span> <span data-ttu-id="05a67-145">实施者替代虚方法时，目的是为其指定值相等性语义。</span><span class="sxs-lookup"><span data-stu-id="05a67-145">When an implementer overrides the virtual method, the purpose is to give it value equality semantics.</span></span>

<span data-ttu-id="05a67-146">即使类不重载 `==` 和 `!=` 运算符，也可将这些运算符与类一起使用。</span><span class="sxs-lookup"><span data-stu-id="05a67-146">The `==` and `!=` operators can be used with classes even if the class does not overload them.</span></span> <span data-ttu-id="05a67-147">但是，默认行为是执行引用相等性检查。</span><span class="sxs-lookup"><span data-stu-id="05a67-147">However, the default behavior is to perform a reference equality check.</span></span> <span data-ttu-id="05a67-148">在类中，如果重载 `Equals` 方法，则应重载 `==` 和 `!=` 运算符，但这并不是必需的。</span><span class="sxs-lookup"><span data-stu-id="05a67-148">In a class, if you overload the `Equals` method, you should overload the `==` and `!=` operators, but it is not required.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05a67-149">前面的示例代码可能无法按照预期的方式处理每个继承方案。</span><span class="sxs-lookup"><span data-stu-id="05a67-149">The preceding example code may not handle every inheritance scenario the way you expect.</span></span> <span data-ttu-id="05a67-150">考虑下列代码：</span><span class="sxs-lookup"><span data-stu-id="05a67-150">Consider the following code:</span></span>
>
> ```csharp
> TwoDPoint p1 = new ThreeDPoint(1, 2, 3);
> TwoDPoint p2 = new ThreeDPoint(1, 2, 4);
> Console.WriteLine(p1.Equals(p2)); // output: True
> ```
>
> <span data-ttu-id="05a67-151">根据此代码报告，尽管 `z` 值有所不同，但 `p1` 等于 `p2`。</span><span class="sxs-lookup"><span data-stu-id="05a67-151">This code reports that `p1` equals `p2` despite the difference in `z` values.</span></span> <span data-ttu-id="05a67-152">由于编译器会根据编译时类型选取 `IEquatable` 的 `TwoDPoint` 实现，因而会忽略该差异。</span><span class="sxs-lookup"><span data-stu-id="05a67-152">The difference is ignored because the compiler picks the `TwoDPoint` implementation of `IEquatable` based on the compile-time type.</span></span>
>
> <span data-ttu-id="05a67-153">`record` 类型的内置值相等性可以正确处理这类场景。</span><span class="sxs-lookup"><span data-stu-id="05a67-153">The built-in value equality of `record` types handles scenarios like this correctly.</span></span> <span data-ttu-id="05a67-154">如果 `TwoDPoint` 和 `ThreeDPoint` 是 `record` 类型，则 `p1.Equals(p2)` 的结果会是 `False`。</span><span class="sxs-lookup"><span data-stu-id="05a67-154">If `TwoDPoint` and `ThreeDPoint` were `record` types, the result of `p1.Equals(p2)` would be `False`.</span></span> <span data-ttu-id="05a67-155">有关详细信息，请参阅 [`record` 类型继承层次结果中的相等性](../../language-reference/builtin-types/record.md#equality-in-inheritance-hierarchies)。</span><span class="sxs-lookup"><span data-stu-id="05a67-155">For more information, see [Equality in `record` type inheritance hierarchies](../../language-reference/builtin-types/record.md#equality-in-inheritance-hierarchies).</span></span>

## <a name="struct-example"></a><span data-ttu-id="05a67-156">结构示例</span><span class="sxs-lookup"><span data-stu-id="05a67-156">Struct example</span></span>

<span data-ttu-id="05a67-157">下面的示例演示如何在结构（值类型）中实现值相等性：</span><span class="sxs-lookup"><span data-stu-id="05a67-157">The following example shows how to implement value equality in a struct (value type):</span></span>

:::code language="csharp" source="snippets/how-to-define-value-equality-for-a-type/ValueEqualityStruct/Program.cs":::
  
<span data-ttu-id="05a67-158">对于结构，<xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType>（<xref:System.ValueType?displayProperty=nameWithType> 中的替代版本）的默认实现通过使用反射来比较类型中每个字段的值，从而执行值相等性检查。</span><span class="sxs-lookup"><span data-stu-id="05a67-158">For structs, the default implementation of <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> (which is the overridden version in <xref:System.ValueType?displayProperty=nameWithType>) performs a value equality check by using reflection to compare the values of every field in the type.</span></span> <span data-ttu-id="05a67-159">实施者替代结构中的 `Equals` 虚方法时，目的是提供更高效的方法来执行值相等性检查，并选择根据结构字段或属性的某个子集来进行比较。</span><span class="sxs-lookup"><span data-stu-id="05a67-159">When an implementer overrides the virtual `Equals` method in a struct, the purpose is to provide a more efficient means of performing the value equality check and optionally to base the comparison on some subset of the struct's field or properties.</span></span>
  
<span data-ttu-id="05a67-160">除非结构显式重载了 [==](../../language-reference/operators/equality-operators.md#equality-operator-) 和 [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) 运算符，否则这些运算符无法对结构进行运算。</span><span class="sxs-lookup"><span data-stu-id="05a67-160">The [==](../../language-reference/operators/equality-operators.md#equality-operator-) and [!=](../../language-reference/operators/equality-operators.md#inequality-operator-) operators can't operate on a struct unless the struct explicitly overloads them.</span></span>

## <a name="see-also"></a><span data-ttu-id="05a67-161">请参阅</span><span class="sxs-lookup"><span data-stu-id="05a67-161">See also</span></span>

- [<span data-ttu-id="05a67-162">相等性比较</span><span class="sxs-lookup"><span data-stu-id="05a67-162">Equality comparisons</span></span>](equality-comparisons.md)
- [<span data-ttu-id="05a67-163">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="05a67-163">C# programming guide</span></span>](../index.md)
