---
description: 分部方法 - C# 参考
title: 分部方法 - C# 参考
ms.date: 03/23/2021
f1_keywords:
- partialmethod_CSharpKeyword
helpviewer_keywords:
- partial methods [C#]
ms.assetid: 43f40242-17e0-4452-8573-090503ad3137
ms.openlocfilehash: 240ad6f2f5792e4db9b032e36991f3c398f1d2f9
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111005"
---
# <a name="partial-method-c-reference"></a>分部方法（C# 参考）

分部方法在分部类型的一部分中定义了签名，并在该类型的另一部分中定义了实现。 通过分部方法，类设计器可提供与事件处理程序类似的方法挂钩，以便开发者决定是否实现。 如果开发者不提供实现，则编译器在编译时删除签名。 以下条件适用于分部方法：

- 声明必须以上下文关键字 [partial](../../language-reference/keywords/partial-type.md) 开头。

- 分部类型各部分中的签名必须匹配。

在以下情况下，不需要使用分部方法即可实现：

- 没有任何可访问性修饰符（包括默认的 [专用](../../language-reference/keywords/private.md)）。

- 返回 [void](../../language-reference/builtin-types/void.md)。

- 没有任何[输出](../../language-reference/keywords/out-parameter-modifier.md)参数。

- 没有以下任何修饰符：[virtual](../../language-reference/keywords/virtual.md)、[override](../../language-reference/keywords/override.md)、[sealed](../../language-reference/keywords/sealed.md)、[new](../../language-reference/keywords/new-modifier.md) 或 [extern](../../language-reference/keywords/extern.md)。

任何不符合所有这些限制的方法（例如 `public virtual partial void` 方法）都必须提供实现。

下列示例显示在分部类的两个部分中定义的分部方法：

[!code-csharp[csrefKeywordsContextual#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#9)]

分部方法还可用于与源生成器结合。 例如，可使用以下模式定义 regex：

```csharp
[RegexGenerated("(dog|cat|fish)")]
partial bool IsPetMatch(string input);
```

有关详细信息，请参阅[分部类和方法](../../programming-guide/classes-and-structs/partial-classes-and-methods.md)。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [分部类型](partial-type.md)
