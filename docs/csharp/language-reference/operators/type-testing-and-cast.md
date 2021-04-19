---
title: 类型测试运算符和强制转换表达式 - C# 引用
description: 了解可用于检查表达式结果的类型并根据需要将其转换为另一种类型的 C# 运算符。
ms.date: 06/21/2019
author: pkulikov
f1_keywords:
- is_CSharpKeyword
- as_CSharpKeyword
- ()_CSharpKeyword
- typeof_CSharpKeyword
- as
- typeof
helpviewer_keywords:
- type-testing operators [C#]
- conversion operators [C#]
- type conversion [C#]
- is operator [C#]
- as operator [C#]
- cast operator [C#]
- cast expression [C#]
- () operator [C#]
- typeof operator [C#]
ms.openlocfilehash: f47074fda20c1bc2eda75184dd26c9de1c0e3701
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075311"
---
# <a name="type-testing-operators-and-cast-expression-c-reference"></a>类型测试运算符和强制转换表达式（C# 引用）

可以使用以下运算符和表达式来执行类型检查或类型转换：

- [is 运算符](#is-operator)：用于检查表达式的运行时类型是否与给定类型兼容
- [as 运算符](#as-operator)：用于将表达式显式转换为给定类型（如果其运行时类型与该类型兼容）
- [强制转换表达式](#cast-expression)：执行显式转换
- [typeof 运算符](#typeof-operator)：用于获取某个类型的 <xref:System.Type?displayProperty=nameWithType> 实例

## <a name="is-operator"></a>is 运算符

`is` 运算符检查表达式结果的运行时类型是否与给定类型兼容。 从 C# 7.0 开始，`is` 运算符还会对照某个模式测试表达式结果。

具有类型测试 `is` 运算符的表达式具有以下形式

```csharp
E is T
```

其中 `E` 是返回一个值的表达式，`T` 是类型或类型参数的名称。 `E` 不得为匿名方法或 Lambda 表达式。

如果 `E` 的结果为非 null 且可以通过引用转换、装箱转换或取消装箱转换来转换为类型 `T`，则 `E is T` 表达式将返回 `true`；否则，它将返回 `false`。 `is` 运算符不会考虑用户定义的转换。

以下示例演示，如果表达式结果的运行时类型派生自给定类型，即类型之间存在引用转换，`is` 运算符将返回 `true`：

[!code-csharp[is with reference conversion](snippets/shared/TypeTestingAndConversionOperators.cs#IsWithReferenceConversion)]

以下示例演示，`is` 运算符将考虑装箱和取消装箱转换，但不会考虑[数值转换](../builtin-types/numeric-conversions.md)：

[!code-csharp-interactive[is with int](snippets/shared/TypeTestingAndConversionOperators.cs#IsWithInt)]

有关 C# 转换的信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[转换](~/_csharplang/spec/conversions.md)一章。

### <a name="type-testing-with-pattern-matching"></a>有模式匹配的类型测试

从 C# 7.0 开始，`is` 运算符还会对照某个模式测试表达式结果。 下面的示例演示如何使用[声明模式](patterns.md#declaration-and-type-patterns)来检查表达式的运行时类型：

[!code-csharp-interactive[is with declaration pattern](snippets/shared/TypeTestingAndConversionOperators.cs#IsDeclarationPattern)]

若要了解受支持的模式，请参阅[模式](patterns.md)。

## <a name="as-operator"></a>as 运算符

`as` 运算符将表达式结果显式转换为给定的引用或可以为 null 值的类型。 如果无法进行转换，则 `as` 运算符返回 `null`。 与[强制转换表达式](#cast-expression) 不同，`as` 运算符永远不会引发异常。

形式如下的表达式

```csharp
E as T
```

其中，`E` 为返回值的表达式，`T` 为类型或类型参数的名称，生成相同的结果，

```csharp
E is T ? (T)(E) : (T)null
```

不同的是 `E` 只计算一次。

`as` 运算符仅考虑引用、可以为 null、装箱和取消装箱转换。 不能使用 `as` 运算符执行用户定义的转换。 为此，请使用[强制转换表达式](#cast-expression)。

下面的示例演示 `as` 运算符的用法：

[!code-csharp-interactive[as operator](snippets/shared/TypeTestingAndConversionOperators.cs#AsOperator)]

> [!NOTE]
> 如之前的示例所示，你需要将 `as` 表达式的结果与 `null` 进行比较，以检查转换是否成功。 从 C# 7.0 开始，你可以使用 [is 运算符](#type-testing-with-pattern-matching)测试转换是否成功，如果成功，则将其结果分配给新变量。

## <a name="cast-expression"></a>强制转换表达式

形式为 `(T)E` 的强制转换表达式将表达式 `E` 的结果显式转换为类型 `T`。 如果不存在从类型 `E` 到类型 `T` 的显式转换，则发生编译时错误。 在运行时，显式转换可能不会成功，强制转换表达式可能会引发异常。

下面的示例演示显式数值和引用转换：

[!code-csharp-interactive[cast expression](snippets/shared/TypeTestingAndConversionOperators.cs#Cast)]

有关支持的显式转换的信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[显式转换](~/_csharplang/spec/conversions.md#explicit-conversions)部分。 有关如何定义自定义显式或隐式类型转换的信息，请参阅[用户定义转换运算符](user-defined-conversion-operators.md)。

### <a name="other-usages-of-"></a>() 的其他用法

你还可以使用括号[调用方法或调用委托](member-access-operators.md#invocation-expression-)。

括号的其他用法是调整表达式中计算操作的顺序。 有关详细信息，请参阅 [C# 运算符](index.md)。

## <a name="typeof-operator"></a>typeof 运算符

`typeof` 运算符用于获取某个类型的 <xref:System.Type?displayProperty=nameWithType> 实例。 `typeof` 运算符的实参必须是类型或类型形参的名称，如以下示例所示：

[!code-csharp-interactive[typeof operator](snippets/shared/TypeTestingAndConversionOperators.cs#TypeOf)]

你还可以使用具有未绑定泛型类型的 `typeof` 运算符。 未绑定泛型类型的名称必须包含适当数量的逗号，且此数量小于类型参数的数量。 以下示例演示了具有未绑定泛型类型的 `typeof` 运算符的用法：

[!code-csharp-interactive[typeof unbound generic](snippets/shared/TypeTestingAndConversionOperators.cs#TypeOfUnboundGeneric)]

表达式不能为 `typeof` 运算符的参数。 若要获取表达式结果的运行时类型的 <xref:System.Type?displayProperty=nameWithType> 实例，请使用 <xref:System.Object.GetType%2A?displayProperty=nameWithType> 方法。

### <a name="type-testing-with-the-typeof-operator"></a>使用 `typeof` 运算符进行类型测试

使用 `typeof` 运算符来检查表达式结果的运行时类型是否与给定的类型完全匹配。 以下示例演示了使用 `typeof` 运算符和 [is 运算符](#is-operator)执行的类型检查之间的差异：

[!code-csharp[typeof vs is](snippets/shared/TypeTestingAndConversionOperators.cs#TypeCheckWithTypeOf)]

## <a name="operator-overloadability"></a>运算符可重载性

`is`、`as` 和 `typeof` 运算符无法进行重载。

用户定义的类型不能重载 `()` 运算符，但可以定义可由强制转换表达式执行的自定义类型转换。 有关详细信息，请参阅[用户定义转换运算符](user-defined-conversion-operators.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [is 运算符](~/_csharplang/spec/expressions.md#the-is-operator)
- [as 运算符](~/_csharplang/spec/expressions.md#the-as-operator)
- [强制转换表达式](~/_csharplang/spec/expressions.md#cast-expressions)
- [typeof 运算符](~/_csharplang/spec/expressions.md#the-typeof-operator)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [如何使用模式匹配以及 is 和 as 运算符安全地进行强制转换](../../how-to/safely-cast-using-pattern-matching-is-and-as-operators.md)
- [.NET 中的泛型](../../../standard/generics/index.md)
