---
description: 详细了解 C# 中的 void 关键字
title: void - C# 参考
ms.date: 02/11/2020
f1_keywords:
- void_CSharpKeyword
- void
- (void)
helpviewer_keywords:
- void keyword [C#]
ms.assetid: 0d2d8a95-fe20-4fbd-bf5d-c1e54bce71d4
ms.openlocfilehash: 4a6afbd88f9cabc6818cdc8ba34f14ae18b195a4
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497380"
---
# <a name="void-c-reference"></a>void（C# 参考）

可以将 `void` 用作[方法](../../programming-guide/classes-and-structs/methods.md)（或[本地函数](../../programming-guide/classes-and-structs/local-functions.md)）的返回类型来指定该方法不返回值。

[!code-csharp[void method](snippets/shared/VoidType.cs#VoidExample)]

还可以将 `void` 用作引用类型来声明指向未知类型的指针。 有关详细信息，请参阅[指针类型](../unsafe-code.md#pointer-types)。

不能将 `void` 用作变量的类型。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- <xref:System.Void?displayProperty=nameWithType>
