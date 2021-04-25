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
# <a name="-operator-c-reference"></a>?: 运算符（C# 参考）

条件运算符 (`?:`) 也称为三元条件运算符，用于计算布尔表达式，并根据布尔表达式的计算结果为 `true` 还是 `false` 来返回两个表达式中的一个结果，如以下示例所示：

:::code language="csharp" interactive="try-dotnet-method" source="snippets/shared/ConditionalOperator.cs" id="BasicExample":::

如上述示例所示，条件运算符的语法如下所示：

```csharp
condition ? consequent : alternative
```

`condition` 表达式的计算结果必须为 `true` 或 `false`。 若 `condition` 的计算结果为 `true`，将计算 `consequent`，其结果成为运算结果。 若 `condition` 的计算结果为 `false`，将计算 `alternative`，其结果成为运算结果。 只会计算 `consequent` 或 `alternative`。

从 C# 9.0 开始，条件表达式由目标确定类型。 也就是说，如果条件表达式的目标类型是已知的，则 `consequent` 和 `alternative` 的类型必须可隐式转换为目标类型，如以下示例所示：

[!code-csharp[target-typed conditional](snippets/shared/ConditionalOperator.cs#TargetTyped)]

如果条件表达式的目标类型是未知的（例如使用 [`var`](../keywords/var.md) 关键字时）或者采用 C# 8.0 及更早版本，则 `consequent` 和 `alternative` 的类型必须相同，或者必须存在从一种类型到另一种类型的隐式转换：

[!code-csharp[not target-typed conditional](snippets/shared/ConditionalOperator.cs#NotTargetTyped)]

条件运算符为右联运算符，即形式的表达式

```csharp
a ? b : c ? d : e
```

计算结果为

```csharp
a ? b : (c ? d : e)
```

> [!TIP]
> 可以使用以下助记键设备记住条件运算符的计算方式：
>
> ```text
> is this condition true ? yes : no
> ```

## <a name="conditional-ref-expression"></a>ref 条件表达式

从 C# 7.2 开始，可通过 ref 条件表达式有条件地分配 [ref local](../keywords/ref.md#ref-locals) 或 [ref readonly local](../keywords/ref.md#ref-readonly-locals) 变量。 还可以使用 ref 条件表达式作为[引用返回值](../keywords/ref.md#reference-return-values)或 [`ref` 方法参数](../keywords/ref.md#passing-an-argument-by-reference)。

ref 条件表达式的语法如下所示：

```csharp
condition ? ref consequent : ref alternative
```

ref 条件表达式与原始的条件运算符相似，仅计算两个表达式其中之一：`consequent` 或 `alternative`。

在 ref 条件表达式中，`consequent` 和 `alternative` 的类型必须相同。 ref 条件表达式不由目标确定类型。

下面的示例演示 ref 条件表达式的用法：

[!code-csharp-interactive[conditional ref](snippets/shared/ConditionalOperator.cs#ConditionalRef)]

## <a name="conditional-operator-and-an-ifelse-statement"></a>条件运算符和 `if..else` 语句

需要根据条件计算值时，使用条件运算符而不是 [if-else](../keywords/if-else.md) 语句可以使代码更简洁。 下面的示例演示了将整数归类为负数或非负数的两种方法：

[!code-csharp[conditional and if-else](snippets/shared/ConditionalOperator.cs#CompareWithIf)]

## <a name="operator-overloadability"></a>运算符可重载性

用户定义类型不能重载条件运算符。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[条件运算符](~/_csharplang/spec/expressions.md#conditional-operator)部分。

有关 C# 7.2 及更高版本中添加的功能的详细信息，请参阅以下功能建议说明：

- [ref 条件表达式 (C# 7.2)](~/_csharplang/proposals/csharp-7.2/conditional-ref.md)
- [目标类型的条件表达式 (C# 9.0)](~/_csharplang/proposals/csharp-9.0/target-typed-conditional-expression.md)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [if-else 语句](../keywords/if-else.md)
- [?. 和 ?[] 运算符](member-access-operators.md#null-conditional-operators--and-)
- [?? 和 ??= 运算符](null-coalescing-operator.md)
- [ref 关键字](../keywords/ref.md)
