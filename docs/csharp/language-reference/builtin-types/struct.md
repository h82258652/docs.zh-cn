---
title: 结构类型 - C# 参考
description: 了解 C# 中的结构类型
ms.date: 10/23/2020
f1_keywords:
- struct_CSharpKeyword
helpviewer_keywords:
- struct keyword [C#]
- struct type [C#]
- structure type [C#]
ms.assetid: ff3dd9b7-dc93-4720-8855-ef5558f65c7c
ms.openlocfilehash: 3aaba057da6214992864cf8e907b0c06ec93264c
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255085"
---
# <a name="structure-types-c-reference"></a><span data-ttu-id="047d6-103">结构类型（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="047d6-103">Structure types (C# reference)</span></span>

<span data-ttu-id="047d6-104">结构类型（“structure type”或“struct type”）是一种可封装数据和相关功能的[值类型](value-types.md)   。</span><span class="sxs-lookup"><span data-stu-id="047d6-104">A *structure type* (or *struct type*) is a [value type](value-types.md) that can encapsulate data and related functionality.</span></span> <span data-ttu-id="047d6-105">使用 `struct` 关键字定义结构类型：</span><span class="sxs-lookup"><span data-stu-id="047d6-105">You use the `struct` keyword to define a structure type:</span></span>

[!code-csharp[struct example](snippets/shared/StructType.cs#StructExample)]

<span data-ttu-id="047d6-106">结构类型具有值语义  。</span><span class="sxs-lookup"><span data-stu-id="047d6-106">Structure types have *value semantics*.</span></span> <span data-ttu-id="047d6-107">也就是说，结构类型的变量包含类型的实例。</span><span class="sxs-lookup"><span data-stu-id="047d6-107">That is, a variable of a structure type contains an instance of the type.</span></span> <span data-ttu-id="047d6-108">默认情况下，在分配中，通过将参数传递给方法并返回方法结果来复制变量值。</span><span class="sxs-lookup"><span data-stu-id="047d6-108">By default, variable values are copied on assignment, passing an argument to a method, and returning a method result.</span></span> <span data-ttu-id="047d6-109">对于结构类型变量，将复制该类型的实例。</span><span class="sxs-lookup"><span data-stu-id="047d6-109">In the case of a structure-type variable, an instance of the type is copied.</span></span> <span data-ttu-id="047d6-110">有关更多信息，请参阅[值类型](value-types.md)。</span><span class="sxs-lookup"><span data-stu-id="047d6-110">For more information, see [Value types](value-types.md).</span></span>

<span data-ttu-id="047d6-111">通常，可以使用结构类型来设计以数据为中心的较小类型，这些类型只有很少的行为或没有行为。</span><span class="sxs-lookup"><span data-stu-id="047d6-111">Typically, you use structure types to design small data-centric types that provide little or no behavior.</span></span> <span data-ttu-id="047d6-112">例如，.NET 使用结构类型来表示数字（[整数](integral-numeric-types.md)和[实数](floating-point-numeric-types.md)）、[布尔值](bool.md)、[Unicode 字符](char.md)以及[时间实例](xref:System.DateTime)。</span><span class="sxs-lookup"><span data-stu-id="047d6-112">For example, .NET uses structure types to represent a number (both [integer](integral-numeric-types.md) and [real](floating-point-numeric-types.md)), a [Boolean value](bool.md), a [Unicode character](char.md), a [time instance](xref:System.DateTime).</span></span> <span data-ttu-id="047d6-113">如果侧重于类型的行为，请考虑定义一个[类](../keywords/class.md)。</span><span class="sxs-lookup"><span data-stu-id="047d6-113">If you're focused on the behavior of a type, consider defining a [class](../keywords/class.md).</span></span> <span data-ttu-id="047d6-114">类类型具有引用语义  。</span><span class="sxs-lookup"><span data-stu-id="047d6-114">Class types have *reference semantics*.</span></span> <span data-ttu-id="047d6-115">也就是说，类类型的变量包含的是对类型的实例的引用，而不是实例本身。</span><span class="sxs-lookup"><span data-stu-id="047d6-115">That is, a variable of a class type contains a reference to an instance of the type, not the instance itself.</span></span>

<span data-ttu-id="047d6-116">由于结构类型具有值语义，因此建议定义不可变的  结构类型。</span><span class="sxs-lookup"><span data-stu-id="047d6-116">Because structure types have value semantics, we recommend you to define *immutable* structure types.</span></span>

## <a name="readonly-struct"></a><span data-ttu-id="047d6-117">`readonly` 结构</span><span class="sxs-lookup"><span data-stu-id="047d6-117">`readonly` struct</span></span>

<span data-ttu-id="047d6-118">从 C# 7.2 开始，可以使用 `readonly` 修饰符来声明结构类型为不可变。</span><span class="sxs-lookup"><span data-stu-id="047d6-118">Beginning with C# 7.2, you use the `readonly` modifier to declare that a structure type is immutable.</span></span> <span data-ttu-id="047d6-119">`readonly` 结构的所有数据成员都必须是只读的，如下所示：</span><span class="sxs-lookup"><span data-stu-id="047d6-119">All data members of a `readonly` struct must be read-only as follows:</span></span>

- <span data-ttu-id="047d6-120">任何字段声明都必须具有 [`readonly` 修饰符](../keywords/readonly.md)</span><span class="sxs-lookup"><span data-stu-id="047d6-120">Any field declaration must have the [`readonly` modifier](../keywords/readonly.md)</span></span>
- <span data-ttu-id="047d6-121">任何属性（包括自动实现的属性）都必须是只读的。</span><span class="sxs-lookup"><span data-stu-id="047d6-121">Any property, including auto-implemented ones, must be read-only.</span></span> <span data-ttu-id="047d6-122">在 C# 9.0 和更高版本中，属性可以具有 [`init` 访问器](../keywords/init.md)。</span><span class="sxs-lookup"><span data-stu-id="047d6-122">In C# 9.0 and later, a property may have an [`init` accessor](../keywords/init.md).</span></span>

<span data-ttu-id="047d6-123">这样可以保证 `readonly` 结构的成员不会修改该结构的状态。</span><span class="sxs-lookup"><span data-stu-id="047d6-123">That guarantees that no member of a `readonly` struct modifies the state of the struct.</span></span> <span data-ttu-id="047d6-124">在 C# 8.0 及更高版本中，这意味着除构造函数外的其他实例成员是隐式 [`readonly`](#readonly-instance-members)。</span><span class="sxs-lookup"><span data-stu-id="047d6-124">In C# 8.0 and later, that means that other instance members except constructors are implicitly [`readonly`](#readonly-instance-members).</span></span>

> [!NOTE]
> <span data-ttu-id="047d6-125">在 `readonly` 结构中，可变引用类型的数据成员仍可改变其自身的状态。</span><span class="sxs-lookup"><span data-stu-id="047d6-125">In a `readonly` struct, a data member of a mutable reference type still can mutate its own state.</span></span> <span data-ttu-id="047d6-126">例如，不能替换 <xref:System.Collections.Generic.List%601> 实例，但可以向其中添加新元素。</span><span class="sxs-lookup"><span data-stu-id="047d6-126">For example, you can't replace a <xref:System.Collections.Generic.List%601> instance, but you can add new elements to it.</span></span>

<span data-ttu-id="047d6-127">下面的代码使用 init-only 属性资源库定义 `readonly` 结构，此内容在 C# 9.0 及更高版本中提供：</span><span class="sxs-lookup"><span data-stu-id="047d6-127">The following code defines a `readonly` struct with init-only property setters, available in C# 9.0 and later:</span></span>

[!code-csharp[readonly struct](snippets/shared/StructType.cs#ReadonlyStruct)]

## <a name="readonly-instance-members"></a><span data-ttu-id="047d6-128">`readonly` 实例成员</span><span class="sxs-lookup"><span data-stu-id="047d6-128">`readonly` instance members</span></span>

<span data-ttu-id="047d6-129">从 C#8.0 开始，还可以使用 `readonly` 修饰符声明实例成员不会修改结构的状态。</span><span class="sxs-lookup"><span data-stu-id="047d6-129">Beginning with C# 8.0, you can also use the `readonly` modifier to declare that an instance member doesn't modify the state of a struct.</span></span> <span data-ttu-id="047d6-130">如果不能将整个结构类型声明为 `readonly`，可使用 `readonly` 修饰符标记不会修改结构状态的实例成员。</span><span class="sxs-lookup"><span data-stu-id="047d6-130">If you can't declare the whole structure type as `readonly`, use the `readonly` modifier to mark the instance members that don't modify the state of the struct.</span></span>

<span data-ttu-id="047d6-131">在 `readonly` 实例成员内，不能分配到结构的实例字段。</span><span class="sxs-lookup"><span data-stu-id="047d6-131">Within a `readonly` instance member, you can't assign to structure's instance fields.</span></span> <span data-ttu-id="047d6-132">但是，`readonly` 成员可以调用非 `readonly` 成员。</span><span class="sxs-lookup"><span data-stu-id="047d6-132">However, a `readonly` member can call a non-`readonly` member.</span></span> <span data-ttu-id="047d6-133">在这种情况下，编译器将创建结构实例的副本，并调用该副本上的非 `readonly` 成员。</span><span class="sxs-lookup"><span data-stu-id="047d6-133">In that case the compiler creates a copy of the structure instance and calls the non-`readonly` member on that copy.</span></span> <span data-ttu-id="047d6-134">因此，不会修改原始结构实例。</span><span class="sxs-lookup"><span data-stu-id="047d6-134">As a result, the original structure instance is not modified.</span></span>

<span data-ttu-id="047d6-135">通常，将 `readonly` 修饰符应用于以下类型的实例成员：</span><span class="sxs-lookup"><span data-stu-id="047d6-135">Typically, you apply the `readonly` modifier to the following kinds of instance members:</span></span>

- <span data-ttu-id="047d6-136">方法：</span><span class="sxs-lookup"><span data-stu-id="047d6-136">methods:</span></span>

  [!code-csharp[readonly method](snippets/shared/StructType.cs#ReadonlyMethod)]

  <span data-ttu-id="047d6-137">还可以将 `readonly` 修饰符应用于可替代在 <xref:System.Object?displayProperty=nameWithType> 中声明的方法的方法：</span><span class="sxs-lookup"><span data-stu-id="047d6-137">You can also apply the `readonly` modifier to methods that override methods declared in <xref:System.Object?displayProperty=nameWithType>:</span></span>

  [!code-csharp[readonly override](snippets/shared/StructType.cs#ReadonlyOverride)]

- <span data-ttu-id="047d6-138">属性和索引器：</span><span class="sxs-lookup"><span data-stu-id="047d6-138">properties and indexers:</span></span>

  [!code-csharp[readonly property get](snippets/shared/StructType.cs#ReadonlyProperty)]

  <span data-ttu-id="047d6-139">如果需要将 `readonly` 修饰符应用于属性或索引器的两个访问器，请在属性或索引器的声明中应用它。</span><span class="sxs-lookup"><span data-stu-id="047d6-139">If you need to apply the `readonly` modifier to both accessors of a property or indexer, apply it in the declaration of the property or indexer.</span></span>

  > [!NOTE]
  > <span data-ttu-id="047d6-140">编译器会将[自动实现的属性](../../programming-guide/classes-and-structs/auto-implemented-properties.md)的 `get` 访问器声明为 `readonly`，而不管属性声明中是否存在 `readonly` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="047d6-140">The compiler declares a `get` accessor of an [auto-implemented property](../../programming-guide/classes-and-structs/auto-implemented-properties.md) as `readonly`, regardless of presence of the `readonly` modifier in a property declaration.</span></span>

  <span data-ttu-id="047d6-141">在 C# 9.0 和更高版本中，可以将 `readonly` 修饰符应用于具有 `init` 访问器的属性或索引器：</span><span class="sxs-lookup"><span data-stu-id="047d6-141">In C# 9.0 and later, you may apply the `readonly` modifier to a property or indexer with an `init` accessor:</span></span>

  :::code language="csharp" source="snippets/shared/StructType.cs" id="ReadonlyWithInit":::

<span data-ttu-id="047d6-142">不能将 `readonly` 修饰符应用于结构类型的静态成员。</span><span class="sxs-lookup"><span data-stu-id="047d6-142">You can't apply the `readonly` modifier to static members of a structure type.</span></span>

<span data-ttu-id="047d6-143">编译器可以使用 `readonly` 修饰符进行性能优化。</span><span class="sxs-lookup"><span data-stu-id="047d6-143">The compiler may make use of the `readonly` modifier for performance optimizations.</span></span> <span data-ttu-id="047d6-144">有关详细信息，请参阅[编写安全有效的 C# 代码](../../write-safe-efficient-code.md)。</span><span class="sxs-lookup"><span data-stu-id="047d6-144">For more information, see [Write safe and efficient C# code](../../write-safe-efficient-code.md).</span></span>

## <a name="limitations-with-the-design-of-a-structure-type"></a><span data-ttu-id="047d6-145">结构类型的设计限制</span><span class="sxs-lookup"><span data-stu-id="047d6-145">Limitations with the design of a structure type</span></span>

<span data-ttu-id="047d6-146">设计结构类型时，具有与[类](../keywords/class.md)类型相同的功能，但有以下例外：</span><span class="sxs-lookup"><span data-stu-id="047d6-146">When you design a structure type, you have the same capabilities as with a [class](../keywords/class.md) type, with the following exceptions:</span></span>

- <span data-ttu-id="047d6-147">不能声明无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="047d6-147">You can't declare a parameterless constructor.</span></span> <span data-ttu-id="047d6-148">每个结构类型都已经提供了一个隐式无参数构造函数，该构造函数生成类型的[默认值](default-values.md)。</span><span class="sxs-lookup"><span data-stu-id="047d6-148">Every structure type already provides an implicit parameterless constructor that produces the [default value](default-values.md) of the type.</span></span>

- <span data-ttu-id="047d6-149">不能在声明实例字段或属性时对它们进行初始化。</span><span class="sxs-lookup"><span data-stu-id="047d6-149">You can't initialize an instance field or property at its declaration.</span></span> <span data-ttu-id="047d6-150">但是，可以在其声明中初始化[静态](../keywords/static.md)或[常量](../keywords/const.md)字段或静态属性。</span><span class="sxs-lookup"><span data-stu-id="047d6-150">However, you can initialize a [static](../keywords/static.md) or [const](../keywords/const.md) field or a static property at its declaration.</span></span>

- <span data-ttu-id="047d6-151">结构类型的构造函数必须初始化该类型的所有实例字段。</span><span class="sxs-lookup"><span data-stu-id="047d6-151">A constructor of a structure type must initialize all instance fields of the type.</span></span>

- <span data-ttu-id="047d6-152">结构类型不能从其他类或结构类型继承，也不能作为类的基础类型。</span><span class="sxs-lookup"><span data-stu-id="047d6-152">A structure type can't inherit from other class or structure type and it can't be the base of a class.</span></span> <span data-ttu-id="047d6-153">但是，结构类型可以实现[接口](../keywords/interface.md)。</span><span class="sxs-lookup"><span data-stu-id="047d6-153">However, a structure type can implement [interfaces](../keywords/interface.md).</span></span>

- <span data-ttu-id="047d6-154">不能在结构类型中声明[终结器](../../programming-guide/classes-and-structs/destructors.md)。</span><span class="sxs-lookup"><span data-stu-id="047d6-154">You can't declare a [finalizer](../../programming-guide/classes-and-structs/destructors.md) within a structure type.</span></span>

## <a name="instantiation-of-a-structure-type"></a><span data-ttu-id="047d6-155">结构类型的实例化</span><span class="sxs-lookup"><span data-stu-id="047d6-155">Instantiation of a structure type</span></span>

<span data-ttu-id="047d6-156">在 C# 中，必须先初始化已声明的变量，然后才能使用该变量。</span><span class="sxs-lookup"><span data-stu-id="047d6-156">In C#, you must initialize a declared variable before it can be used.</span></span> <span data-ttu-id="047d6-157">由于结构类型变量不能为 `null`（除非它是[可为空的值类型](nullable-value-types.md)的变量），因此，必须实例化相应类型的实例。</span><span class="sxs-lookup"><span data-stu-id="047d6-157">Because a structure-type variable can't be `null` (unless it's a variable of a [nullable value type](nullable-value-types.md)), you must instantiate an instance of the corresponding type.</span></span> <span data-ttu-id="047d6-158">有多种方法可实现此目的。</span><span class="sxs-lookup"><span data-stu-id="047d6-158">There are several ways to do that.</span></span>

<span data-ttu-id="047d6-159">通常，可使用 [`new`](../operators/new-operator.md) 运算符调用适当的构造函数来实例化结构类型。</span><span class="sxs-lookup"><span data-stu-id="047d6-159">Typically, you instantiate a structure type by calling an appropriate constructor with the [`new`](../operators/new-operator.md) operator.</span></span> <span data-ttu-id="047d6-160">每个结构类型都至少有一个构造函数。</span><span class="sxs-lookup"><span data-stu-id="047d6-160">Every structure type has at least one constructor.</span></span> <span data-ttu-id="047d6-161">这是一个隐式无参数构造函数，用于生成类型的[默认值](default-values.md)。</span><span class="sxs-lookup"><span data-stu-id="047d6-161">That's an implicit parameterless constructor, which produces the [default value](default-values.md) of the type.</span></span> <span data-ttu-id="047d6-162">还可以使用[默认值表达式](../operators/default.md)来生成类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="047d6-162">You can also use a [default value expression](../operators/default.md) to produce the default value of a type.</span></span>

<span data-ttu-id="047d6-163">如果结构类型的所有实例字段都是可访问的，则还可以在不使用 `new` 运算符的情况下对其进行实例化。</span><span class="sxs-lookup"><span data-stu-id="047d6-163">If all instance fields of a structure type are accessible, you can also instantiate it without the `new` operator.</span></span> <span data-ttu-id="047d6-164">在这种情况下，在首次使用实例之前必须初始化所有实例字段。</span><span class="sxs-lookup"><span data-stu-id="047d6-164">In that case you must initialize all instance fields before the first use of the instance.</span></span> <span data-ttu-id="047d6-165">下面的示例演示如何执行此操作：</span><span class="sxs-lookup"><span data-stu-id="047d6-165">The following example shows how to do that:</span></span>

[!code-csharp[without new](snippets/shared/StructType.cs#WithoutNew)]

<span data-ttu-id="047d6-166">在处理[内置值类型](value-types.md#built-in-value-types)的情况下，请使用相应的文本来指定类型的值。</span><span class="sxs-lookup"><span data-stu-id="047d6-166">In the case of the [built-in value types](value-types.md#built-in-value-types), use the corresponding literals to specify a value of the type.</span></span>

## <a name="passing-structure-type-variables-by-reference"></a><span data-ttu-id="047d6-167">按引用传递结构类型变量</span><span class="sxs-lookup"><span data-stu-id="047d6-167">Passing structure-type variables by reference</span></span>

<span data-ttu-id="047d6-168">将结构类型变量作为参数传递给方法或从方法返回结构类型值时，将复制结构类型的整个实例。</span><span class="sxs-lookup"><span data-stu-id="047d6-168">When you pass a structure-type variable to a method as an argument or return a structure-type value from a method, the whole instance of a structure type is copied.</span></span> <span data-ttu-id="047d6-169">这可能会影响高性能方案中涉及大型结构类型的代码的性能。</span><span class="sxs-lookup"><span data-stu-id="047d6-169">That can affect the performance of your code in high-performance scenarios that involve large structure types.</span></span> <span data-ttu-id="047d6-170">通过按引用传递结构类型变量，可以避免值复制操作。</span><span class="sxs-lookup"><span data-stu-id="047d6-170">You can avoid value copying by passing a structure-type variable by reference.</span></span> <span data-ttu-id="047d6-171">使用 [`ref`](../keywords/ref.md#passing-an-argument-by-reference)、[`out`](../keywords/out-parameter-modifier.md) 或 [`in`](../keywords/in-parameter-modifier.md) 方法参数修饰符，指示必须按引用传递参数。</span><span class="sxs-lookup"><span data-stu-id="047d6-171">Use the [`ref`](../keywords/ref.md#passing-an-argument-by-reference), [`out`](../keywords/out-parameter-modifier.md), or [`in`](../keywords/in-parameter-modifier.md) method parameter modifiers to indicate that an argument must be passed by reference.</span></span> <span data-ttu-id="047d6-172">使用 [ref 返回值](../../programming-guide/classes-and-structs/ref-returns.md)按引用返回方法结果。</span><span class="sxs-lookup"><span data-stu-id="047d6-172">Use [ref returns](../../programming-guide/classes-and-structs/ref-returns.md) to return a method result by reference.</span></span> <span data-ttu-id="047d6-173">有关详细信息，请参阅[编写安全有效的 C# 代码](../../write-safe-efficient-code.md)。</span><span class="sxs-lookup"><span data-stu-id="047d6-173">For more information, see [Write safe and efficient C# code](../../write-safe-efficient-code.md).</span></span>

## <a name="ref-struct"></a><span data-ttu-id="047d6-174">`ref` 结构</span><span class="sxs-lookup"><span data-stu-id="047d6-174">`ref` struct</span></span>

<span data-ttu-id="047d6-175">从 C# 7.2 开始，可以在结构类型的声明中使用 `ref` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="047d6-175">Beginning with C# 7.2, you can use the `ref` modifier in the declaration of a structure type.</span></span> <span data-ttu-id="047d6-176">`ref` 结构类型的实例在堆栈上分配，并且不能转义到托管堆。</span><span class="sxs-lookup"><span data-stu-id="047d6-176">Instances of a `ref` struct type are allocated on the stack and can't escape to the managed heap.</span></span> <span data-ttu-id="047d6-177">为了确保这一点，编译器将 `ref` 结构类型的使用限制如下：</span><span class="sxs-lookup"><span data-stu-id="047d6-177">To ensure that, the compiler limits the usage of `ref` struct types as follows:</span></span>

- <span data-ttu-id="047d6-178">`ref` 结构不能是数组的元素类型。</span><span class="sxs-lookup"><span data-stu-id="047d6-178">A `ref` struct can't be the element type of an array.</span></span>
- <span data-ttu-id="047d6-179">`ref` 结构不能是类或非 `ref` 结构的字段的声明类型。</span><span class="sxs-lookup"><span data-stu-id="047d6-179">A `ref` struct can't be a declared type of a field of a class or a non-`ref` struct.</span></span>
- <span data-ttu-id="047d6-180">`ref` 结构不能实现接口。</span><span class="sxs-lookup"><span data-stu-id="047d6-180">A `ref` struct can't implement interfaces.</span></span>
- <span data-ttu-id="047d6-181">`ref` 结构不能被装箱为 <xref:System.ValueType?displayProperty=nameWithType> 或 <xref:System.Object?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="047d6-181">A `ref` struct can't be boxed to <xref:System.ValueType?displayProperty=nameWithType> or <xref:System.Object?displayProperty=nameWithType>.</span></span>
- <span data-ttu-id="047d6-182">`ref` 结构不能是类型参数。</span><span class="sxs-lookup"><span data-stu-id="047d6-182">A `ref` struct can't be a type argument.</span></span>
- <span data-ttu-id="047d6-183">`ref` 结构变量不能由 [lambda 表达式](../operators/lambda-expressions.md)或[本地函数](../../programming-guide/classes-and-structs/local-functions.md)捕获。</span><span class="sxs-lookup"><span data-stu-id="047d6-183">A `ref` struct variable can't be captured by a [lambda expression](../operators/lambda-expressions.md) or a [local function](../../programming-guide/classes-and-structs/local-functions.md).</span></span>
- <span data-ttu-id="047d6-184">`ref` 结构变量不能在 [`async`](../keywords/async.md) 方法中使用。</span><span class="sxs-lookup"><span data-stu-id="047d6-184">A `ref` struct variable can't be used in an [`async`](../keywords/async.md) method.</span></span> <span data-ttu-id="047d6-185">但是，可以在同步方法中使用 `ref` 结构变量，例如，在返回 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 的方法中。</span><span class="sxs-lookup"><span data-stu-id="047d6-185">However, you can use `ref` struct variables in synchronous methods, for example, in those that return <xref:System.Threading.Tasks.Task> or <xref:System.Threading.Tasks.Task%601>.</span></span>
- <span data-ttu-id="047d6-186">`ref` 结构变量不能在[迭代器](../../iterators.md)中使用。</span><span class="sxs-lookup"><span data-stu-id="047d6-186">A `ref` struct variable can't be used in [iterators](../../iterators.md).</span></span>

<span data-ttu-id="047d6-187">通常，如果需要一种同时包含 `ref` 结构类型的数据成员的类型，可以定义 `ref` 结构类型：</span><span class="sxs-lookup"><span data-stu-id="047d6-187">Typically, you define a `ref` struct type when you need a type that also includes data members of `ref` struct types:</span></span>

[!code-csharp[ref struct](snippets/shared/StructType.cs#RefStruct)]

<span data-ttu-id="047d6-188">若要将 `ref` 结构声明为 [`readonly`](#readonly-struct)，请在类型声明中组合使用 `readonly` 修饰符和 `ref` 修饰符（`readonly` 修饰符必须位于 `ref` 修饰符之前）：</span><span class="sxs-lookup"><span data-stu-id="047d6-188">To declare a `ref` struct as [`readonly`](#readonly-struct), combine the `readonly` and `ref` modifiers in the type declaration (the `readonly` modifier must come before the `ref` modifier):</span></span>

[!code-csharp[readonly ref struct](snippets/shared/StructType.cs#ReadonlyRef)]

<span data-ttu-id="047d6-189">在 .NET 中，`ref` 结构的示例分别是 <xref:System.Span%601?displayProperty=nameWithType> 和 <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="047d6-189">In .NET, examples of a `ref` struct are <xref:System.Span%601?displayProperty=nameWithType> and <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>.</span></span>

## <a name="struct-constraint"></a><span data-ttu-id="047d6-190">struct 约束</span><span class="sxs-lookup"><span data-stu-id="047d6-190">struct constraint</span></span>

<span data-ttu-id="047d6-191">你还可在 [`struct` 约束](../../programming-guide/generics/constraints-on-type-parameters.md)中使用 `struct` 关键字，来指定类型参数为不可为 null 的值类型。</span><span class="sxs-lookup"><span data-stu-id="047d6-191">You also use the `struct` keyword in the [`struct` constraint](../../programming-guide/generics/constraints-on-type-parameters.md) to specify that a type parameter is a non-nullable value type.</span></span> <span data-ttu-id="047d6-192">结构类型和[枚举](enum.md)类型都满足 `struct` 约束。</span><span class="sxs-lookup"><span data-stu-id="047d6-192">Both structure and [enumeration](enum.md) types satisfy the `struct` constraint.</span></span>

## <a name="conversions"></a><span data-ttu-id="047d6-193">转换</span><span class="sxs-lookup"><span data-stu-id="047d6-193">Conversions</span></span>

<span data-ttu-id="047d6-194">对于任何结构类型（[`ref` struct](#ref-struct) 类型除外），都存在与 <xref:System.ValueType?displayProperty=nameWithType> 和 <xref:System.Object?displayProperty=nameWithType> 类型之间的[装箱和取消装箱](../../programming-guide/types/boxing-and-unboxing.md)相互转换。</span><span class="sxs-lookup"><span data-stu-id="047d6-194">For any structure type (except [`ref` struct](#ref-struct) types), there exist [boxing and unboxing](../../programming-guide/types/boxing-and-unboxing.md) conversions to and from the <xref:System.ValueType?displayProperty=nameWithType> and <xref:System.Object?displayProperty=nameWithType> types.</span></span> <span data-ttu-id="047d6-195">还存在结构类型和它所实现的任何接口之间的装箱和取消装箱转换。</span><span class="sxs-lookup"><span data-stu-id="047d6-195">There exist also boxing and unboxing conversions between a structure type and any interface that it implements.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="047d6-196">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="047d6-196">C# language specification</span></span>

<span data-ttu-id="047d6-197">有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[结构](~/_csharplang/spec/structs.md)部分。</span><span class="sxs-lookup"><span data-stu-id="047d6-197">For more information, see the [Structs](~/_csharplang/spec/structs.md) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

<span data-ttu-id="047d6-198">有关 C#7.2 及更高版本中引入的功能的更多信息，请参见以下功能建议说明：</span><span class="sxs-lookup"><span data-stu-id="047d6-198">For more information about features introduced in C# 7.2 and later, see the following feature proposal notes:</span></span>

- [<span data-ttu-id="047d6-199">只读结构</span><span class="sxs-lookup"><span data-stu-id="047d6-199">Readonly structs</span></span>](~/_csharplang/proposals/csharp-7.2/readonly-ref.md#readonly-structs)
- [<span data-ttu-id="047d6-200">只读实例成员</span><span class="sxs-lookup"><span data-stu-id="047d6-200">Readonly instance members</span></span>](~/_csharplang/proposals/csharp-8.0/readonly-instance-members.md)
- [<span data-ttu-id="047d6-201">类 ref 类型的编译时安全性</span><span class="sxs-lookup"><span data-stu-id="047d6-201">Compile-time safety for ref-like types</span></span>](~/_csharplang/proposals/csharp-7.2/span-safety.md)

## <a name="see-also"></a><span data-ttu-id="047d6-202">请参阅</span><span class="sxs-lookup"><span data-stu-id="047d6-202">See also</span></span>

- [<span data-ttu-id="047d6-203">C# 参考</span><span class="sxs-lookup"><span data-stu-id="047d6-203">C# reference</span></span>](../index.md)
- [<span data-ttu-id="047d6-204">设计指南 - 在类和结构之间选择</span><span class="sxs-lookup"><span data-stu-id="047d6-204">Design guidelines - Choosing between class and struct</span></span>](../../../standard/design-guidelines/choosing-between-class-and-struct.md)
- [<span data-ttu-id="047d6-205">设计指南 - 结构设计</span><span class="sxs-lookup"><span data-stu-id="047d6-205">Design guidelines - Struct design</span></span>](../../../standard/design-guidelines/struct.md)
- [<span data-ttu-id="047d6-206">类和结构</span><span class="sxs-lookup"><span data-stu-id="047d6-206">Classes and structs</span></span>](../../programming-guide/classes-and-structs/index.md)
