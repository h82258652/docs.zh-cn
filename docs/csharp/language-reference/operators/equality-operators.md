---
title: 相等运算符 - C# 参考
description: 了解 C# 相等比较运算符和 C# 类型相等。
ms.date: 10/30/2020
author: pkulikov
f1_keywords:
- ==_CSharpKeyword
- '!=_CSharpKeyword'
helpviewer_keywords:
- comparison operators [C#]
- relational operators [C#]
- equality operator [C#]
- equals operator [C#]
- == operator [C#]
- inequality operator [C#]
- not equals operator [C#]
- '!= operator [C#]'
ms.openlocfilehash: ee45869dff19712d8e4ef30f2fc01c2d4633acf1
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255449"
---
# <a name="equality-operators-c-reference"></a>相等运算符（C# 参考）

[`==`（相等）](#equality-operator-) 和 [`!=`（不等）](#inequality-operator-) 运算符检查其操作数是否相等。

## <a name="equality-operator-"></a>相等运算符 ==

如果操作数相等，等于运算符 `==` 返回 `true`，否则返回 `false`。

### <a name="value-types-equality"></a>值类型的相等性

如果[内置值类型](../builtin-types/value-types.md#built-in-value-types)的值相等，则其操作数相等：

[!code-csharp-interactive[value types equality](snippets/shared/EqualityOperators.cs#ValueTypesEquality)]

> [!NOTE]
> 对于 `==`、[`<`、`>`、`<=` 和 `>=`](comparison-operators.md) 运算符，如果任何操作数不是数字（<xref:System.Double.NaN?displayProperty=nameWithType> 或 <xref:System.Single.NaN?displayProperty=nameWithType>），则运算的结果为 `false`。 这意味着 `NaN` 值不大于、小于或等于任何其他 `double`（或 `float`）值，包括 `NaN`。 有关更多信息和示例，请参阅 <xref:System.Double.NaN?displayProperty=nameWithType> 或 <xref:System.Single.NaN?displayProperty=nameWithType> 参考文章。

如果基本整数类型的相应值相等，则相同[枚举](../builtin-types/enum.md)类型的两个操作数相等。

用户定义的 [struct](../builtin-types/struct.md) 类型默认情况下不支持 `==` 运算符。 要支持 `==` 运算符，用户定义的结构必须[重载](operator-overloading.md)它。

从 C# 7.3 开始，`==` 和 `!=` 运算符由 C# [元组](../builtin-types/value-tuples.md)支持。 有关详细信息，请参阅[元组类型](../builtin-types/value-tuples.md)一文的[元组相等](../builtin-types/value-tuples.md#tuple-equality)部分。

### <a name="reference-types-equality"></a>引用类型的相等性

默认情况下，如果两个非记录引用类型操作符引用同一对象，则这两个操作符相等：

[!code-csharp[reference type equality](snippets/shared/EqualityOperators.cs#ReferenceTypesEquality)]

如示例所示，默认情况下，用户定义的引用类型支持 `==` 运算符。 但是，引用类型可重载 `==` 运算符。 如果引用类型重载 `==` 运算符，使用 <xref:System.Object.ReferenceEquals%2A?displayProperty=nameWithType> 方法来检查该类型的两个引用是否引用同一对象。

### <a name="record-types-equality"></a>记录类型相等性

在 C# 9.0 和更高版本中提供，[记录类型](../builtin-types/record.md)支持 `==` 和 `!=` 运算符，这些运算符默认提供值相等性语义。 也就是说，当两个记录操作数均为 `null` 或所有字段的对应值和自动实现的属性相等时，两个记录操作数都相等。

:::code language="csharp" source="snippets/shared/EqualityOperators.cs" id="RecordTypesEquality":::

如前面的示例所示，对于非记录引用类型成员，比较的是它们的引用值，而不是所引用的实例。

### <a name="string-equality"></a>字符串相等性

如果两个字符串均为 `null` 或者两个字符串实例具有相等长度且在每个字符位置有相同字符，则这两个[字符串](../builtin-types/reference-types.md#the-string-type)操作数相等：

[!code-csharp-interactive[string equality](snippets/shared/EqualityOperators.cs#StringEquality)]

这就是区分大小写的序号比较。 有关字符串比较的详细信息，请参阅[如何在 C# 中比较字符串](../../how-to/compare-strings.md)。

### <a name="delegate-equality"></a>委托相等

当两个[委托](../../programming-guide/delegates/index.md)操作数都是 `null` 或它们的调用列表长度相同并且在每个位置具有相同的条目时，运行时类型相同的两个委托操作数是相等的：

[!code-csharp-interactive[delegate equality](snippets/shared/EqualityOperators.cs#DelegateEquality)]

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[委托相等运算符](~/_csharplang/spec/expressions.md#delegate-equality-operators)部分。

通过计算语义上相同的 [Lambda 表达式](lambda-expressions.md)生成的委托不相等，如以下示例所示：

[!code-csharp-interactive[from identical lambdas](snippets/shared/EqualityOperators.cs#IdenticalLambdas)]

## <a name="inequality-operator-"></a>不等运算符 !=

如果操作数不相等，不等于运算符 `!=` 返回 `true`，否则返回 `false`。 对于[内置类型](../builtin-types/built-in-types.md)的操作数，表达式 `x != y` 生成与表达式 `!(x == y)` 相同的结果。 有关类型相等性的更多信息，请参阅[相等运算符](#equality-operator-)部分。

下面的示例演示 `!=` 运算符的用法：

[!code-csharp-interactive[non-equality examples](snippets/shared/EqualityOperators.cs#NonEquality)]

## <a name="operator-overloadability"></a>运算符可重载性

用户定义类型可以[重载](operator-overloading.md)`==` 和 `!=` 运算符。 如果某类型重载这两个运算符之一，它还必须重载另一个运算符。

记录类型不能显式重载 `==` 和 `!=` 运算符。 如果需要更改记录类型 `T` 的 `==` 和 `!=` 运算符的行为，请使用以下签名实现 <xref:System.IEquatable%601.Equals%2A?displayProperty=nameWithType> 方法：

```csharp
public virtual bool Equals(T? other);
```

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[关系和类型测试运算符](~/_csharplang/spec/expressions.md#relational-and-type-testing-operators)部分。

有关记录类型相等性的详细信息，请参阅[记录功能建议附注](~/_csharplang/proposals/csharp-9.0/records.md)中的[相等性成员](~/_csharplang/proposals/csharp-9.0/records.md#equality-members)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- <xref:System.IEquatable%601?displayProperty=nameWithType>
- <xref:System.Object.Equals%2A?displayProperty=nameWithType>
- <xref:System.Object.ReferenceEquals%2A?displayProperty=nameWithType>
- [相等性比较](../../programming-guide/statements-expressions-operators/equality-comparisons.md)
- [比较运算符](comparison-operators.md)
