---
description: var - C# 参考
title: var - C# 参考
ms.date: 10/02/2020
f1_keywords:
- var
- var_CSharpKeyword
helpviewer_keywords:
- var keyword [C#]
ms.assetid: 0777850a-2691-4e3e-927f-0c850f5efe15
ms.openlocfilehash: b3590ebbcff0894b7864cd4a227a6ea068de42a2
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075506"
---
# <a name="var-c-reference"></a>var（C# 参考）

从 C# 3.0 开始，在方法范围内声明的变量可以具有隐式“类型”`var`。 隐式类型本地变量为强类型，就像用户已经自行声明该类型，但编译器决定类型一样。 `i` 的以下两个声明在功能上是等效的：

```csharp
var i = 10; // Implicitly typed.
int i = 10; // Explicitly typed.
```

> [!IMPORTANT]
> 在启用[可为空引用类型](../builtin-types/nullable-reference-types.md)的情况下使用 `var` 时，即使表达式类型不可为空，也始终表示可为空引用类型。

`var` 关键字的常见用途是用于构造函数调用表达式。 使用 `var`则不能在变量声明和对象实例化中重复类型名称，如下面的示例所示：

```csharp
var xs = new List<int>();
```

从 C# 9.0 开始，可以使用由目标确定类型的 [`new` 表达式](../operators/new-operator.md)作为替代方法：

```csharp
List<int> xs = new();
List<int>? ys = new();
```

在模式匹配中，在 [`var` 模式](../operators/patterns.md#var-pattern)中使用 `var` 关键字。

## <a name="example"></a>示例

下面的示例演示两个查询表达式。 在第一个表达式中，`var` 的使用是允许的，但不是必需的，因为查询结果的类型可以明确表述为 `IEnumerable<string>`。 不过，在第二个表达式中，`var` 允许结果是一系列匿名类型，且相应类型的名称只可供编译器本身访问。 如果使用 `var`，便无法为结果新建类。 请注意，在示例 #2 中，`foreach` 迭代变量 `item` 必须也为隐式类型。

[!code-csharp[csrefKeywordsTypes#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#18)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [隐式类型本地变量](../../programming-guide/classes-and-structs/implicitly-typed-local-variables.md)
- [LINQ 查询操作中的类型关系](../../programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md)
