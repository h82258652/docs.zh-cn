---
description: 了解 C# 中的非托管类型
title: 非托管类型 - C# 参考
ms.date: 09/06/2019
helpviewer_keywords:
- unmanaged type [C#]
ms.openlocfilehash: 13e8d4238a85201d46acabdf3103bdc7254ecfe8
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497393"
---
# <a name="unmanaged-types-c-reference"></a>非托管类型（C# 参考）

如果某个类型是以下类型之一，则它是非托管类型  ：

- `sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal` 或 `bool`
- 任何[枚举](enum.md)类型
- 任何[指针](../unsafe-code.md#pointer-types)类型
- 任何用户定义的 [struct](struct.md) 类型，只包含非托管类型的字段，并且在 C# 7.3 及更早版本中，不是构造类型（包含至少一个类型参数的类型）

从 C# 7.3 开始，可使用 [`unmanaged` 约束](../../programming-guide/generics/constraints-on-type-parameters.md#unmanaged-constraint)指定：类型参数为“非指针、不可为 null 的非托管类型”。

从 C# 8.0 开始，仅包含非托管类型的字段的 *构造* 结构类型也是非托管类型，如以下示例所示：

[!code-csharp[unmanaged constructed types](snippets/shared/UnmanagedTypes.cs#ProgramExample)]

泛型结构可以是非托管类型的源，也可以是不是非托管构造类型的源。 前面的示例定义一个泛型结构 `Coords<T>`，并提供非托管构造类型的示例。 不是非托管类型情况的示例是 `Coords<object>`。 它不是非托管性质，因为它具有不是非托管性质的 `object` 类型的字段。 如果你希望所有  构造类型都是非托管类型，请在泛型结构的定义中使用 `unmanaged` 约束：

[!code-csharp[unmanaged constraint in type definition](snippets/shared/UnmanagedTypes.cs#AlwaysUnmanaged)]

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[指针类型](~/_csharplang/spec/unsafe-code.md#pointer-types)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [指针类型](../unsafe-code.md#pointer-types)
- [内存和跨度相关类型](../../../standard/memory-and-spans/index.md)
- [sizeof 运算符](../operators/sizeof.md)
- [stackalloc](../operators/stackalloc.md)
