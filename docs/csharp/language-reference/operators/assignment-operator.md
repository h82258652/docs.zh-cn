---
description: 赋值运算符 - C# 参考
title: 赋值运算符 - C# 参考
ms.date: 09/10/2019
f1_keywords:
- =_CSharpKeyword
helpviewer_keywords:
- = operator [C#]
ms.assetid: d802a6d5-32f0-42b8-b180-12f5a081bfc1
ms.openlocfilehash: 702652cc7ae05d6a2bfd419ebce55399b9403a52
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075428"
---
# <a name="assignment-operators-c-reference"></a>赋值运算符（C# 参考）

赋值运算符 `=` 将其右操作数的值赋给变量、[属性](../../programming-guide/classes-and-structs/properties.md)或由其左操作数给出的[索引器](../../programming-guide/indexers/index.md)元素。 赋值表达式的结果是分配给左操作数的值。 右操作数类型必须与左操作数类型相同，或可隐式转换为左操作数的类型。

赋值运算符 `=` 为右联运算符，即形式的表达式

```csharp
a = b = c
```

计算结果为

```csharp
a = (b = c)
```

以下示例演示使用局部变量、属性和索引器元素作为其左操作数的赋值运算符的用法：

[!code-csharp-interactive[simple assignment](snippets/shared/AssignmentOperator.cs#Simple)]

## <a name="ref-assignment-operator"></a>ref 赋值运算符

从 C# 7.3 开始，可以使用 ref 赋值运算符 `= ref` 重新分配 [ref local](../keywords/ref.md#ref-locals) 或 [ref readonly local](../keywords/ref.md#ref-readonly-locals) 变量。 下面的示例演示 ref 赋值运算符的用法：

[!code-csharp[ref assignment operator](snippets/shared/AssignmentOperator.cs#RefAssignment)]

对于 ref 赋值运算符，其两个操作数的类型必须相同。

## <a name="compound-assignment"></a>复合赋值

对于二元运算符 `op`，窗体的复合赋值表达式

```csharp
x op= y
```

等效于

```csharp
x = x op y
```

不同的是 `x` 只计算一次。

[算术](arithmetic-operators.md#compound-assignment)、[布尔逻辑](boolean-logical-operators.md#compound-assignment)以及[逻辑位和移位](bitwise-and-shift-operators.md#compound-assignment)运算符支持复合赋值。

## <a name="null-coalescing-assignment"></a>Null 合并赋值

从 C# 8.0 开始，只有在左操作数计算为 `null` 时，才能使用 null 合并赋值运算符 `??=` 将其右操作数的值分配给左操作数。 有关详细信息，请参阅 [?? 和 ??= 运算符](null-coalescing-operator.md)一文。

## <a name="operator-overloadability"></a>运算符可重载性

用户定义类型不能[重载](operator-overloading.md)赋值运算符。 但是，用户定义类型可以定义到其他类型的隐式转换。 这样，可以将用户定义类型的值分配给其他类型的变量、属性或索引器元素。 有关详细信息，请参阅[用户定义转换运算符](user-defined-conversion-operators.md)。

用户定义类型不能显式重载复合赋值运算符。 但是，如果用户定义类型重载了二元运算符 `op`，则 `op=` 运算符（如果存在）也将被隐式重载。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[分配运算符](~/_csharplang/spec/expressions.md#assignment-operators)部分。

如需了解有关 ref 赋值运算符 `= ref` 的详细信息，请参阅[功能建议说明](~/_csharplang/proposals/csharp-7.3/ref-local-reassignment.md)。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [ref 关键字](../keywords/ref.md)
