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
# <a name="patterns-c-reference"></a>模式（C# 参考）

C# 在 C# 7.0 中引入了模式匹配。 自此之后，每个主要 C# 版本都扩展了模式匹配功能。 以下 C# 表达式和语句支持模式匹配：

- [`is`表达式](../keywords/is.md)
- `switch` [语句](../keywords/switch.md)
- `switch` [表达式](switch-expression.md)（在 C# 8.0 中引入）

在这些构造中，可将输入表达式与以下任一模式进行匹配：

- [声明模式](#declaration-and-type-patterns)：用于检查表达式的运行时类型，如果匹配成功，则将表达式结果分配给声明的变量。 在 C# 7.0 中引入。
- [类型模式](#declaration-and-type-patterns)：用于检查表达式的运行时类型。 在 C# 9.0 中引入。
- [常量模式](#constant-pattern)：用于测试表达式结果是否等于指定常量。 在 C# 7.0 中引入。
- [关系模式](#relational-patterns)：用于将表达式结果与指定常量进行比较。 在 C# 9.0 中引入。
- [逻辑模式](#logical-patterns)：用于测试表达式是否与模式的逻辑组合匹配。 在 C# 9.0 中引入。
- [属性模式](#property-pattern)：用于测试表达式的属性或字段是否与嵌套模式匹配。 在 C# 8.0 中引入。
- [位置模式](#positional-pattern)：用于解构表达式结果并测试结果值是否与嵌套模式匹配。 在 C# 8.0 中引入。
- [`var` 模式](#var-pattern)：用于匹配任何表达式并将其结果分配给声明的变量。 在 C# 7.0 中引入。
- [弃元模式](#discard-pattern)：用于匹配任何表达式。 在 C# 8.0 中引入。

[逻辑](#logical-patterns)、[属性](#property-pattern)和[位置](#positional-pattern)模式都是递归模式。 也就是说，它们可包含嵌套模式。

有关如何使用这些模式来生成数据驱动算法的示例，请参阅[教程：使用模式匹配来生成类型驱动的算法和数据驱动的算法](../../tutorials/pattern-matching.md)。

## <a name="declaration-and-type-patterns"></a>声明和类型模式

可使用声明和类型模式来检查表达式的运行时类型是否与给定类型兼容。 借助声明模式，还可声明新的局部变量。 当声明模式与表达式匹配时，将为该变量分配转换后的表达式结果，如以下示例所示：

:::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="BasicExample":::

从 C# 7.0 开始，类型为 `T` 的声明模式在表达式结果为非 NULL 且满足以下任一条件时与表达式匹配：

- 表达式结果的运行时类型为 `T`。

- 表达式结果的运行时类型派生自类型 `T` 或实现接口 `T`，或者存在从其到 `T` 的另一种[隐式引用转换](~/_csharplang/spec/conversions.md#implicit-reference-conversions)。 下面的示例演示满足此条件时的两种案例：

  :::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="ReferenceConversion":::

  在上述示例中，在第一次调用 `GetSourceLabel` 方法时，第一种模式与参数值匹配，因为参数的运行时类型 `int[]` 派生自 <xref:System.Array> 类型。 在第二次调用 `GetSourceLabel` 方法时，参数的运行时类型 <xref:System.Collections.Generic.List%601> 不是从 <xref:System.Array> 类型派生的，而是实现 <xref:System.Collections.Generic.ICollection%601> 接口。

- 表达式结果的运行时类型是具有基础类型 `T` 的[可为空的值类型](../builtin-types/nullable-value-types.md)。

- 存在从表达式结果的运行时类型到类型 `T` 的[装箱](../../programming-guide/types/boxing-and-unboxing.md#boxing)或[取消装箱](../../programming-guide/types/boxing-and-unboxing.md#unboxing)转换。

下面的示例演示最后两个条件：

:::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="NullableAndUnboxing":::

如果只想检查表达式类型，可使用弃元 `_` 代替变量名，如以下示例所示：

:::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="DiscardVariable":::

从 C# 9.0 开始，可对此使用类型模式，如以下示例所示：

:::code language="csharp" source="snippets/patterns/DeclarationAndTypePatterns.cs" id="TypePattern":::

类似于声明模式，当表达式结果为非 NULL 且其运行时类型满足上面列出的任一条件时，类型模式将与表达式匹配。

有关详细信息，请参阅功能方案说明的[声明模式](~/_csharplang/proposals/csharp-8.0/patterns.md#declaration-pattern)和[类型模式](~/_csharplang/proposals/csharp-9.0/patterns3.md#type-patterns)部分。

## <a name="constant-pattern"></a>常量模式

从 C# 7.0 开始，可使用常量模式来测试表达式结果是否等于指定的常量，如以下示例所示：

:::code language="csharp" source="snippets/patterns/ConstantPattern.cs" id="BasicExample":::

在常量模式中，可使用任何常量表达式，例如：

- [integer](../builtin-types/integral-numeric-types.md) 或 [floating-point](../builtin-types/floating-point-numeric-types.md) 数值文本
- [char](../builtin-types/char.md) 或 [string](../builtin-types/reference-types.md#the-string-type) 文本
- 布尔值 `true` 或 `false`
- [enum](../builtin-types/enum.md) 值
- 声明[常量](../keywords/const.md)字段或本地的名称
- `null`

常量模式用于检查 `null`，如以下示例所示：

:::code language="csharp" source="snippets/patterns/ConstantPattern.cs" id="NullCheck":::

编译器保证在计算表达式 `x is null` 时，不会调用用户重载的相等运算符 `==`。

从 C# 9.0 开始，可使用[否定](#logical-patterns) `null` 常量模式来检查非 NULL，如以下示例所示：

:::code language="csharp" source="snippets/patterns/ConstantPattern.cs" id="NonNullCheck":::

有关详细信息，请参阅功能建议说明的[常量模式](~/_csharplang/proposals/csharp-8.0/patterns.md#constant-pattern)部分。

## <a name="relational-patterns"></a>关系模式

从 C# 9.0 开始，可使用关系模式将表达式结果与常量进行比较，如以下示例所示：

:::code language="csharp" source="snippets/patterns/RelationalPatterns.cs" id="BasicExample":::

在关系模式中，可使用[关系运算符](comparison-operators.md) `<`、`>`、`<=` 或 `>=` 中的任何一个。 关系模式的右侧部分必须是常数表达式。 常数表达式可以是 [integer](../builtin-types/integral-numeric-types.md)、[floating-point](../builtin-types/floating-point-numeric-types.md)、[char](../builtin-types/char.md) 或 [enum](../builtin-types/enum.md) 类型。

要检查表达式结果是否在某个范围内，请将其与[合取 `and` 模式](#logical-patterns)匹配，如以下示例所示：

:::code language="csharp" source="snippets/patterns/RelationalPatterns.cs" id="WithCombinators":::

如果表达式结果为 `null` 或未能通过可为空或取消装箱转换转换为常量类型，则关系模式与表达式不匹配。

有关详细信息，请参阅功能建议说明的[关系模式](~/_csharplang/proposals/csharp-9.0/patterns3.md#relational-patterns)部分。

## <a name="logical-patterns"></a>逻辑模式

从 C# 9.0 开始，可使用 `not`、`and` 和 `or` 模式连结符来创建以下逻辑模式：

- 否定 `not` 模式在否定模式与表达式不匹配时与表达式匹配。 下面的示例说明如何否定[常量](#constant-pattern) `null` 模式来检查表达式是否为非空值：

  :::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="NotPattern":::

- 合取 `and` 模式在两个模式都与表达式匹配时与表达式匹配。 以下示例显示如何组合[关系模式](#relational-patterns)来检查值是否在某个范围内：

  :::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="AndPattern":::

- 析取 `or` 模式在任一模式与表达式匹配时与表达式匹配，如以下示例所示：

  :::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="OrPattern":::

如前面的示例所示，可在模式中重复使用模式连结符。

`and` 模式连结符的优先级高于 `or`。 要显式指定优先级，请使用括号，如以下示例所示：

:::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="WithParentheses":::

> [!NOTE]
> 检查模式的顺序是不确定的。 在运行时，可先检查 `or` 和 `and` 模式的右侧嵌套模式。

有关详细信息，请参阅功能建议说明的[模式连结符](~/_csharplang/proposals/csharp-9.0/patterns3.md#pattern-combinators)部分。

## <a name="property-pattern"></a>属性模式

从 C# 8.0 开始，可使用属性模式来将表达式的属性或字段与嵌套模式进行匹配，如以下示例所示：

:::code language="csharp" source="snippets/patterns/PropertyPattern.cs" id="BasicExample":::

当表达式结果为非 NULL 且每个嵌套模式都与表达式结果的相应属性或字段匹配时，属性模式将与表达式匹配。

还可将运行时类型检查和变量声明添加到属性模式，如以下示例所示：

:::code language="csharp" source="snippets/patterns/PropertyPattern.cs" id="WithTypeCheck":::

属性模式是一种递归模式。 也就是说，可以将任何模式用作嵌套模式。 使用属性模式将部分数据与嵌套模式进行匹配，如以下示例所示：

:::code language="csharp" source="snippets/patterns/PropertyPattern.cs" id="RecursivePropertyPattern":::

前面的示例使用 C# 9.0 及更高版本中提供的两个功能：`or` [模式连结符](#logical-patterns)和[记录类型](../builtin-types/record.md)。

有关详细信息，请参阅功能建议说明的[属性模式](~/_csharplang/proposals/csharp-8.0/patterns.md#property-pattern)部分。

## <a name="positional-pattern"></a>位置模式

从 C# 8.0 开始，可使用位置模式来解构表达式结果并将结果值与相应的嵌套模式匹配，如以下示例所示：

:::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="BasicExample":::

在前面的示例中，表达式的类型包含 [Deconstruct](../../deconstruct.md) 方法，该方法用于解构表达式结果。 还可将[元组类型](../builtin-types/value-tuples.md)的表达式与位置模式进行匹配。 这样，就可将多个输入与各种模式进行匹配，如以下示例所示：

:::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="MatchTuple":::

前面的示例使用在 C# 9.0 及更高版本中提供的[关系](#relational-patterns)和[逻辑](#logical-patterns)模式。

可在位置模式中使用元组元素的名称和 `Deconstruct` 参数，如以下示例所示：

:::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="UseIdentifiers":::

还可通过以下任一方式扩展位置模式：

- 添加运行时类型检查和变量声明，如以下示例所示：

  :::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="WithTypeCheck":::

  前面的示例使用隐式提供 `Deconstruct` 方法的[位置记录](../builtin-types/record.md#positional-syntax-for-property-definition)。

- 在位置模式中使用[属性模式](#property-pattern)，如以下示例所示：

  :::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="WithPropertyPattern":::

- 结合前面的两种用法，如以下示例所示：

  :::code language="csharp" source="snippets/patterns/PositionalPattern.cs" id="CompletePositionalPattern":::

位置模式是一种递归模式。 也就是说，可以将任何模式用作嵌套模式。

有关详细信息，请参阅功能建议说明的[位置模式](~/_csharplang/proposals/csharp-8.0/patterns.md#positional-pattern)部分。

## <a name="var-pattern"></a>`var` 模式

从 C# 7.0 开始，可使用 `var` 模式来匹配任何表达式（包括 `null`），并将其结果分配给新的局部变量，如以下示例所示：

:::code language="csharp" source="snippets/patterns/VarPattern.cs" id="KeepInterimResult":::

需要布尔表达式中的临时变量来保存中间计算的结果时，`var` 模式很有用。 当需要在 `switch` 表达式或语句的 `when` 大小写临界子句中执行其他检查时，也可使用 `var` 模式，如以下示例所示：

:::code language="csharp" source="snippets/patterns/VarPattern.cs" id="WithCaseGuards":::

在前面的示例中，模式 `var (x, y)` 等效于[位置模式](#positional-pattern) `(var x, var y)`。

在 `var` 模式中，声明变量的类型是与该模式匹配的表达式的编译时类型。

有关详细信息，请参阅功能建议说明的 [Var 模式](~/_csharplang/proposals/csharp-8.0/patterns.md#var-pattern)部分。

## <a name="discard-pattern"></a>弃元模式

从 C# 8.0 开始，可使用弃元模式 `_` 来匹配任何表达式，包括 `null`，如以下示例所示：

:::code language="csharp" source="snippets/patterns/DiscardPattern.cs" id="BasicExample":::

在前面的示例中，弃元模式用于处理 `null` 以及没有相应的 <xref:System.DayOfWeek> 枚举成员的任何整数值。 这可保证示例中的 `switch` 表达式可处理所有可能的输入值。 如果没有在 `switch` 表达式中使用弃元模式，并且该表达式的任何模式均与输入不匹配，则运行时[会引发异常](switch-expression.md#non-exhaustive-switch-expressions)。 如果 `switch` 表达式未处理所有可能的输入值，则编译器会生成警告。

弃元模式不能是 `is` 表达式或 `switch` 语句中的模式。 在这些案例中，要匹配任何表达式，请使用带有弃元 `var _` 的 [`var` 模式](#var-pattern)。

有关详细信息，请参阅功能建议说明的[弃元模式](~/_csharplang/proposals/csharp-8.0/patterns.md#discard-pattern)部分。

## <a name="parenthesized-pattern"></a>带括号模式

从 C# 9.0 开始，可在任何模式两边加上括号。 通常，这样做是为了强调或更改[逻辑模式](#logical-patterns)中的优先级，如以下示例所示：

:::code language="csharp" source="snippets/patterns/LogicalPatterns.cs" id="ChangedPrecedence":::

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅以下功能建议说明：

- [C# 7.0 的模式匹配](~/_csharplang/proposals/csharp-7.0/pattern-matching.md)
- [递归模式匹配（在 C# 8.0 中引入）](~/_csharplang/proposals/csharp-8.0/patterns.md)
- [C# 9.0 的模式匹配更改](~/_csharplang/proposals/csharp-9.0/patterns3.md)

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [教程：使用模式匹配来构建类型驱动和数据驱动的算法](../../tutorials/pattern-matching.md)
