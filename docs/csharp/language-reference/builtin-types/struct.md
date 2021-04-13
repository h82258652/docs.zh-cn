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
# <a name="structure-types-c-reference"></a>结构类型（C# 参考）

结构类型（“structure type”或“struct type”）是一种可封装数据和相关功能的[值类型](value-types.md)   。 使用 `struct` 关键字定义结构类型：

[!code-csharp[struct example](snippets/shared/StructType.cs#StructExample)]

结构类型具有值语义  。 也就是说，结构类型的变量包含类型的实例。 默认情况下，在分配中，通过将参数传递给方法并返回方法结果来复制变量值。 对于结构类型变量，将复制该类型的实例。 有关更多信息，请参阅[值类型](value-types.md)。

通常，可以使用结构类型来设计以数据为中心的较小类型，这些类型只有很少的行为或没有行为。 例如，.NET 使用结构类型来表示数字（[整数](integral-numeric-types.md)和[实数](floating-point-numeric-types.md)）、[布尔值](bool.md)、[Unicode 字符](char.md)以及[时间实例](xref:System.DateTime)。 如果侧重于类型的行为，请考虑定义一个[类](../keywords/class.md)。 类类型具有引用语义  。 也就是说，类类型的变量包含的是对类型的实例的引用，而不是实例本身。

由于结构类型具有值语义，因此建议定义不可变的  结构类型。

## <a name="readonly-struct"></a>`readonly` 结构

从 C# 7.2 开始，可以使用 `readonly` 修饰符来声明结构类型为不可变。 `readonly` 结构的所有数据成员都必须是只读的，如下所示：

- 任何字段声明都必须具有 [`readonly` 修饰符](../keywords/readonly.md)
- 任何属性（包括自动实现的属性）都必须是只读的。 在 C# 9.0 和更高版本中，属性可以具有 [`init` 访问器](../keywords/init.md)。

这样可以保证 `readonly` 结构的成员不会修改该结构的状态。 在 C# 8.0 及更高版本中，这意味着除构造函数外的其他实例成员是隐式 [`readonly`](#readonly-instance-members)。

> [!NOTE]
> 在 `readonly` 结构中，可变引用类型的数据成员仍可改变其自身的状态。 例如，不能替换 <xref:System.Collections.Generic.List%601> 实例，但可以向其中添加新元素。

下面的代码使用 init-only 属性资源库定义 `readonly` 结构，此内容在 C# 9.0 及更高版本中提供：

[!code-csharp[readonly struct](snippets/shared/StructType.cs#ReadonlyStruct)]

## <a name="readonly-instance-members"></a>`readonly` 实例成员

从 C#8.0 开始，还可以使用 `readonly` 修饰符声明实例成员不会修改结构的状态。 如果不能将整个结构类型声明为 `readonly`，可使用 `readonly` 修饰符标记不会修改结构状态的实例成员。

在 `readonly` 实例成员内，不能分配到结构的实例字段。 但是，`readonly` 成员可以调用非 `readonly` 成员。 在这种情况下，编译器将创建结构实例的副本，并调用该副本上的非 `readonly` 成员。 因此，不会修改原始结构实例。

通常，将 `readonly` 修饰符应用于以下类型的实例成员：

- 方法：

  [!code-csharp[readonly method](snippets/shared/StructType.cs#ReadonlyMethod)]

  还可以将 `readonly` 修饰符应用于可替代在 <xref:System.Object?displayProperty=nameWithType> 中声明的方法的方法：

  [!code-csharp[readonly override](snippets/shared/StructType.cs#ReadonlyOverride)]

- 属性和索引器：

  [!code-csharp[readonly property get](snippets/shared/StructType.cs#ReadonlyProperty)]

  如果需要将 `readonly` 修饰符应用于属性或索引器的两个访问器，请在属性或索引器的声明中应用它。

  > [!NOTE]
  > 编译器会将[自动实现的属性](../../programming-guide/classes-and-structs/auto-implemented-properties.md)的 `get` 访问器声明为 `readonly`，而不管属性声明中是否存在 `readonly` 修饰符。

  在 C# 9.0 和更高版本中，可以将 `readonly` 修饰符应用于具有 `init` 访问器的属性或索引器：

  :::code language="csharp" source="snippets/shared/StructType.cs" id="ReadonlyWithInit":::

不能将 `readonly` 修饰符应用于结构类型的静态成员。

编译器可以使用 `readonly` 修饰符进行性能优化。 有关详细信息，请参阅[编写安全有效的 C# 代码](../../write-safe-efficient-code.md)。

## <a name="limitations-with-the-design-of-a-structure-type"></a>结构类型的设计限制

设计结构类型时，具有与[类](../keywords/class.md)类型相同的功能，但有以下例外：

- 不能声明无参数构造函数。 每个结构类型都已经提供了一个隐式无参数构造函数，该构造函数生成类型的[默认值](default-values.md)。

- 不能在声明实例字段或属性时对它们进行初始化。 但是，可以在其声明中初始化[静态](../keywords/static.md)或[常量](../keywords/const.md)字段或静态属性。

- 结构类型的构造函数必须初始化该类型的所有实例字段。

- 结构类型不能从其他类或结构类型继承，也不能作为类的基础类型。 但是，结构类型可以实现[接口](../keywords/interface.md)。

- 不能在结构类型中声明[终结器](../../programming-guide/classes-and-structs/destructors.md)。

## <a name="instantiation-of-a-structure-type"></a>结构类型的实例化

在 C# 中，必须先初始化已声明的变量，然后才能使用该变量。 由于结构类型变量不能为 `null`（除非它是[可为空的值类型](nullable-value-types.md)的变量），因此，必须实例化相应类型的实例。 有多种方法可实现此目的。

通常，可使用 [`new`](../operators/new-operator.md) 运算符调用适当的构造函数来实例化结构类型。 每个结构类型都至少有一个构造函数。 这是一个隐式无参数构造函数，用于生成类型的[默认值](default-values.md)。 还可以使用[默认值表达式](../operators/default.md)来生成类型的默认值。

如果结构类型的所有实例字段都是可访问的，则还可以在不使用 `new` 运算符的情况下对其进行实例化。 在这种情况下，在首次使用实例之前必须初始化所有实例字段。 下面的示例演示如何执行此操作：

[!code-csharp[without new](snippets/shared/StructType.cs#WithoutNew)]

在处理[内置值类型](value-types.md#built-in-value-types)的情况下，请使用相应的文本来指定类型的值。

## <a name="passing-structure-type-variables-by-reference"></a>按引用传递结构类型变量

将结构类型变量作为参数传递给方法或从方法返回结构类型值时，将复制结构类型的整个实例。 这可能会影响高性能方案中涉及大型结构类型的代码的性能。 通过按引用传递结构类型变量，可以避免值复制操作。 使用 [`ref`](../keywords/ref.md#passing-an-argument-by-reference)、[`out`](../keywords/out-parameter-modifier.md) 或 [`in`](../keywords/in-parameter-modifier.md) 方法参数修饰符，指示必须按引用传递参数。 使用 [ref 返回值](../../programming-guide/classes-and-structs/ref-returns.md)按引用返回方法结果。 有关详细信息，请参阅[编写安全有效的 C# 代码](../../write-safe-efficient-code.md)。

## <a name="ref-struct"></a>`ref` 结构

从 C# 7.2 开始，可以在结构类型的声明中使用 `ref` 修饰符。 `ref` 结构类型的实例在堆栈上分配，并且不能转义到托管堆。 为了确保这一点，编译器将 `ref` 结构类型的使用限制如下：

- `ref` 结构不能是数组的元素类型。
- `ref` 结构不能是类或非 `ref` 结构的字段的声明类型。
- `ref` 结构不能实现接口。
- `ref` 结构不能被装箱为 <xref:System.ValueType?displayProperty=nameWithType> 或 <xref:System.Object?displayProperty=nameWithType>。
- `ref` 结构不能是类型参数。
- `ref` 结构变量不能由 [lambda 表达式](../operators/lambda-expressions.md)或[本地函数](../../programming-guide/classes-and-structs/local-functions.md)捕获。
- `ref` 结构变量不能在 [`async`](../keywords/async.md) 方法中使用。 但是，可以在同步方法中使用 `ref` 结构变量，例如，在返回 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 的方法中。
- `ref` 结构变量不能在[迭代器](../../iterators.md)中使用。

通常，如果需要一种同时包含 `ref` 结构类型的数据成员的类型，可以定义 `ref` 结构类型：

[!code-csharp[ref struct](snippets/shared/StructType.cs#RefStruct)]

若要将 `ref` 结构声明为 [`readonly`](#readonly-struct)，请在类型声明中组合使用 `readonly` 修饰符和 `ref` 修饰符（`readonly` 修饰符必须位于 `ref` 修饰符之前）：

[!code-csharp[readonly ref struct](snippets/shared/StructType.cs#ReadonlyRef)]

在 .NET 中，`ref` 结构的示例分别是 <xref:System.Span%601?displayProperty=nameWithType> 和 <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>。

## <a name="struct-constraint"></a>struct 约束

你还可在 [`struct` 约束](../../programming-guide/generics/constraints-on-type-parameters.md)中使用 `struct` 关键字，来指定类型参数为不可为 null 的值类型。 结构类型和[枚举](enum.md)类型都满足 `struct` 约束。

## <a name="conversions"></a>转换

对于任何结构类型（[`ref` struct](#ref-struct) 类型除外），都存在与 <xref:System.ValueType?displayProperty=nameWithType> 和 <xref:System.Object?displayProperty=nameWithType> 类型之间的[装箱和取消装箱](../../programming-guide/types/boxing-and-unboxing.md)相互转换。 还存在结构类型和它所实现的任何接口之间的装箱和取消装箱转换。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[结构](~/_csharplang/spec/structs.md)部分。

有关 C#7.2 及更高版本中引入的功能的更多信息，请参见以下功能建议说明：

- [只读结构](~/_csharplang/proposals/csharp-7.2/readonly-ref.md#readonly-structs)
- [只读实例成员](~/_csharplang/proposals/csharp-8.0/readonly-instance-members.md)
- [类 ref 类型的编译时安全性](~/_csharplang/proposals/csharp-7.2/span-safety.md)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [设计指南 - 在类和结构之间选择](../../../standard/design-guidelines/choosing-between-class-and-struct.md)
- [设计指南 - 结构设计](../../../standard/design-guidelines/struct.md)
- [类和结构](../../programming-guide/classes-and-structs/index.md)
