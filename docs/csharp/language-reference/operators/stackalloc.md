---
description: stackalloc 表达式 - C# 参考
title: stackalloc 表达式 - C# 参考
ms.date: 03/13/2020
f1_keywords:
- stackalloc_CSharpKeyword
helpviewer_keywords:
- stackalloc expression [C#]
ms.openlocfilehash: 42867ff1b5acffaf62639a31a5bdd3b4055e252a
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497458"
---
# <a name="stackalloc-expression-c-reference"></a>stackalloc 表达式（C# 参考）

`stackalloc` 表达式在堆栈上分配内存块。 该方法返回时，将自动丢弃在方法执行期间创建的堆栈中分配的内存块。 不能显式释放使用 `stackalloc` 分配的内存。 堆栈中分配的内存块不受[垃圾回收](../../../standard/garbage-collection/index.md)的影响，也不必通过 [`fixed` 语句](../keywords/fixed-statement.md)固定。

可以将 `stackalloc` 表达式的结果分配给以下任一类型的变量：

- 从 C# 7.2 开始，<xref:System.Span%601?displayProperty=nameWithType> 或 <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> 将如下例所示：

  [!code-csharp[stackalloc span](snippets/shared/StackallocOperator.cs#AssignToSpan)]

  将堆栈中分配的内存块分配给 <xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601> 变量时，不必使用 [unsafe](../keywords/unsafe.md) 上下文。

  使用这些类型时，可以在[条件表达式](conditional-operator.md)或赋值表达式中使用 `stackalloc` 表达式，如以下示例所示：

  [!code-csharp[stackalloc expression](snippets/shared/StackallocOperator.cs#AsExpression)]

  从 C#8.0 开始，只要允许使用 <xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601> 变量，就可以在其他表达式中使用 `stackalloc` 表达式，如下例所示：

  [!code-csharp[stackalloc in nested expressions](snippets/shared/StackallocOperator.cs#Nested)]

  > [!NOTE]
  > 建议尽可能使用 <xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601> 类型来处理堆栈中分配的内存。

- [指针类型](../unsafe-code.md#pointer-types)，如以下示例所示：

  [!code-csharp[stackalloc pointer](snippets/shared/StackallocOperator.cs#AssignToPointer)]

  如前面的示例所示，在使用指针类型时必须使用 `unsafe` 上下文。

  对于指针类型，只能在局部变量声明中使用 `stackalloc` 表达式来初始化变量。

堆栈上可用的内存量存在限制。 如果在堆栈上分配过多的内存，会引发 <xref:System.StackOverflowException>。 为避免这种情况，请遵循以下规则：

- 限制使用 `stackalloc` 分配的内存量：

  [!code-csharp[limit stackalloc](snippets/shared/StackallocOperator.cs#LimitStackalloc)]

  由于堆栈上可用的内存量取决于执行代码的环境，因此在定义实际限值时请持保守态度。

- 避免在循环内使用 `stackalloc`。 在循环外分配内存块，然后在循环内重用它。

新分配的内存的内容未定义。 在使用之前应对其进行初始化。 例如，可以使用 <xref:System.Span%601.Clear%2A?displayProperty=nameWithType> 方法将所有项设置为 `T` 类型的默认值。

从 C# 7.3 开始，可以使用数组初始值设定项语法来定义新分配内存的内容。 下面的示例演示执行此操作的各种方法：

[!code-csharp[stackalloc initialization](snippets/shared/StackallocOperator.cs#StackallocInit)]

在表达式 `stackalloc T[E]` 中，`T` 必须是[非托管类型](../builtin-types/unmanaged-types.md)，并且 `E` 的计算结果必须为非负 [int](../builtin-types/integral-numeric-types.md) 值。

## <a name="security"></a>安全性

使用 `stackalloc` 会自动启用公共语言运行时 (CLR) 中的缓冲区溢出检测功能。 如果检测到缓冲区溢出，则将尽快终止进程，以便将执行恶意代码的可能性降到最低。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)以及[允许嵌入上下文中的 `stackalloc`](~/_csharplang/proposals/csharp-8.0/nested-stackalloc.md) 功能建议说明的[堆栈分配](~/_csharplang/spec/unsafe-code.md#stack-allocation)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [指针相关的运算符](pointer-related-operators.md)
- [指针类型](../unsafe-code.md#pointer-types)
- [内存和跨度相关类型](../../../standard/memory-and-spans/index.md)
- [stackalloc 注意事项](https://vcsjones.dev/2020/02/24/stackalloc/)
