---
title: 显式接口实现 - C# 编程指南
description: 类可以在 C# 中实现包含具有相同签名的成员的接口。 显式实现创建特定于一个接口的类成员。
ms.date: 03/24/2021
helpviewer_keywords:
- explicit interfaces [C#]
- interfaces [C#], explicit
ms.openlocfilehash: 2ca7e8a10c009b01b43e0c09d25d2dd99cbee2ec
ms.sourcegitcommit: 80f38cb67bd02f51d5722fa13d0ea207e3b14a8e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105610858"
---
# <a name="explicit-interface-implementation-c-programming-guide"></a>显式接口实现（C# 编程指南）

如果一个[类](../../language-reference/keywords/class.md)实现的两个接口包含签名相同的成员，则在该类上实现此成员会导致这两个接口将此成员用作其实现。 如下示例中，所有对 `Paint` 的调用皆调用同一方法。 第一个示例定义类型：

[!code-csharp[DefineSimpleTypes](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#DefineTypes)]

下面的示例调用方法：

[!code-csharp[DefineSimpleTypes](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallMethods)]

但你可能不希望为这两个接口都调用相同的实现。 若要调用不同的实现，根据所使用的接口，可以显式实现接口成员。 显式接口实现是一个类成员，只通过指定接口进行调用。 通过在类成员前面加上接口名称和句点可命名该类成员。 例如：

[!code-csharp[DefineExplicitImplementation](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#ExplicitImplementation)]

类成员 `IControl.Paint` 仅通过 `IControl` 接口可用，`ISurface.Paint` 仅通过 `ISurface` 可用。 这两个方法实现相互独立，两者均不可直接在类上使用。 例如：

[!code-csharp[CallExplicitImplementation](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallExplicitImplementation)]

显式实现还用于处理两个接口分别声明名称相同的不同成员（例如属性和方法）的情况。 若要实现两个接口，类必须对属性 `P` 或方法 `P` 使用显式实现，或对二者同时使用，从而避免编译器错误。 例如：

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#NameCollision)]

显式接口实现没有访问修饰符，因为它不能作为其定义类型的成员进行访问。 而只能在通过接口实例调用时访问。 如果为显式接口实现指定访问修饰符，将收到编译器错误 [CS0106](../../language-reference/compiler-messages/cs0106.md)。 有关详细信息，请参阅 [`interface`（C# 参考）](../../language-reference/keywords/interface.md)。

从 [C# 8.0](../../whats-new/csharp-8.md#default-interface-methods) 开始，你可以为在接口中声明的成员定义一个实现。 如果类从接口继承方法实现，则只能通过接口类型的引用访问该方法。 继承的成员不会显示为公共接口的一部分。 下面的示例定义接口方法的默认实现：

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#DefaultImplementation)]

下面的示例调用默认实现：

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallDefaultImplementation)]

任何实现 `IControl` 接口的类都可以重写默认的 `Paint` 方法，作为公共方法或作为显式接口实现。

## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [类和结构](../classes-and-structs/index.md)
- [接口](./index.md)
- [继承](../classes-and-structs/inheritance.md)
