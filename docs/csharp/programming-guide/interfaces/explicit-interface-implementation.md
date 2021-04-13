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
# <a name="explicit-interface-implementation-c-programming-guide"></a><span data-ttu-id="bb364-104">显式接口实现（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="bb364-104">Explicit Interface Implementation (C# Programming Guide)</span></span>

<span data-ttu-id="bb364-105">如果一个[类](../../language-reference/keywords/class.md)实现的两个接口包含签名相同的成员，则在该类上实现此成员会导致这两个接口将此成员用作其实现。</span><span class="sxs-lookup"><span data-stu-id="bb364-105">If a [class](../../language-reference/keywords/class.md) implements two interfaces that contain a member with the same signature, then implementing that member on the class will cause both interfaces to use that member as their implementation.</span></span> <span data-ttu-id="bb364-106">如下示例中，所有对 `Paint` 的调用皆调用同一方法。</span><span class="sxs-lookup"><span data-stu-id="bb364-106">In the following example, all the calls to `Paint` invoke the same method.</span></span> <span data-ttu-id="bb364-107">第一个示例定义类型：</span><span class="sxs-lookup"><span data-stu-id="bb364-107">This first sample defines the types:</span></span>

[!code-csharp[DefineSimpleTypes](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#DefineTypes)]

<span data-ttu-id="bb364-108">下面的示例调用方法：</span><span class="sxs-lookup"><span data-stu-id="bb364-108">The following sample calls the methods:</span></span>

[!code-csharp[DefineSimpleTypes](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallMethods)]

<span data-ttu-id="bb364-109">但你可能不希望为这两个接口都调用相同的实现。</span><span class="sxs-lookup"><span data-stu-id="bb364-109">But you might not want the same implementation to be called for both interfaces.</span></span> <span data-ttu-id="bb364-110">若要调用不同的实现，根据所使用的接口，可以显式实现接口成员。</span><span class="sxs-lookup"><span data-stu-id="bb364-110">To call a different implementation depending on which interface is in use, you can implement an interface member explicitly.</span></span> <span data-ttu-id="bb364-111">显式接口实现是一个类成员，只通过指定接口进行调用。</span><span class="sxs-lookup"><span data-stu-id="bb364-111">An explicit interface implementation is a class member that is only called through the specified interface.</span></span> <span data-ttu-id="bb364-112">通过在类成员前面加上接口名称和句点可命名该类成员。</span><span class="sxs-lookup"><span data-stu-id="bb364-112">Name the class member by prefixing it with the name of the interface and a period.</span></span> <span data-ttu-id="bb364-113">例如：</span><span class="sxs-lookup"><span data-stu-id="bb364-113">For example:</span></span>

[!code-csharp[DefineExplicitImplementation](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#ExplicitImplementation)]

<span data-ttu-id="bb364-114">类成员 `IControl.Paint` 仅通过 `IControl` 接口可用，`ISurface.Paint` 仅通过 `ISurface` 可用。</span><span class="sxs-lookup"><span data-stu-id="bb364-114">The class member `IControl.Paint` is only available through the `IControl` interface, and `ISurface.Paint` is only available through `ISurface`.</span></span> <span data-ttu-id="bb364-115">这两个方法实现相互独立，两者均不可直接在类上使用。</span><span class="sxs-lookup"><span data-stu-id="bb364-115">Both method implementations are separate, and neither are available directly on the class.</span></span> <span data-ttu-id="bb364-116">例如：</span><span class="sxs-lookup"><span data-stu-id="bb364-116">For example:</span></span>

[!code-csharp[CallExplicitImplementation](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallExplicitImplementation)]

<span data-ttu-id="bb364-117">显式实现还用于处理两个接口分别声明名称相同的不同成员（例如属性和方法）的情况。</span><span class="sxs-lookup"><span data-stu-id="bb364-117">Explicit implementation is also used to resolve cases where two interfaces each declare different members of the same name such as a property and a method.</span></span> <span data-ttu-id="bb364-118">若要实现两个接口，类必须对属性 `P` 或方法 `P` 使用显式实现，或对二者同时使用，从而避免编译器错误。</span><span class="sxs-lookup"><span data-stu-id="bb364-118">To implement both interfaces, a class has to use explicit implementation either for the property `P`, or the method `P`, or both, to avoid a compiler error.</span></span> <span data-ttu-id="bb364-119">例如：</span><span class="sxs-lookup"><span data-stu-id="bb364-119">For example:</span></span>

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#NameCollision)]

<span data-ttu-id="bb364-120">显式接口实现没有访问修饰符，因为它不能作为其定义类型的成员进行访问。</span><span class="sxs-lookup"><span data-stu-id="bb364-120">An explicit interface implementation doesn't have an access modifier since it isn't accessible as a member of the type it's defined in.</span></span> <span data-ttu-id="bb364-121">而只能在通过接口实例调用时访问。</span><span class="sxs-lookup"><span data-stu-id="bb364-121">Instead, it's only accessible when called through an instance of the interface.</span></span> <span data-ttu-id="bb364-122">如果为显式接口实现指定访问修饰符，将收到编译器错误 [CS0106](../../language-reference/compiler-messages/cs0106.md)。</span><span class="sxs-lookup"><span data-stu-id="bb364-122">If you specify an access modifier for an explicit interface implementation, you get compiler error [CS0106](../../language-reference/compiler-messages/cs0106.md).</span></span> <span data-ttu-id="bb364-123">有关详细信息，请参阅 [`interface`（C# 参考）](../../language-reference/keywords/interface.md)。</span><span class="sxs-lookup"><span data-stu-id="bb364-123">For more information, see [`interface` (C# Reference)](../../language-reference/keywords/interface.md).</span></span>

<span data-ttu-id="bb364-124">从 [C# 8.0](../../whats-new/csharp-8.md#default-interface-methods) 开始，你可以为在接口中声明的成员定义一个实现。</span><span class="sxs-lookup"><span data-stu-id="bb364-124">Beginning with [C# 8.0](../../whats-new/csharp-8.md#default-interface-methods), you can define an implementation for members declared in an interface.</span></span> <span data-ttu-id="bb364-125">如果类从接口继承方法实现，则只能通过接口类型的引用访问该方法。</span><span class="sxs-lookup"><span data-stu-id="bb364-125">If a class inherits a method implementation from an interface, that method is only accessible through a reference of the interface type.</span></span> <span data-ttu-id="bb364-126">继承的成员不会显示为公共接口的一部分。</span><span class="sxs-lookup"><span data-stu-id="bb364-126">The inherited member doesn't appear as part of the public interface.</span></span> <span data-ttu-id="bb364-127">下面的示例定义接口方法的默认实现：</span><span class="sxs-lookup"><span data-stu-id="bb364-127">The following sample defines a default implementation for an interface method:</span></span>

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#DefaultImplementation)]

<span data-ttu-id="bb364-128">下面的示例调用默认实现：</span><span class="sxs-lookup"><span data-stu-id="bb364-128">The following sample invokes the default implementation:</span></span>

[!code-csharp[NameCollisions](~/samples/snippets/csharp/interfaces/ExplicitImplementation.cs#CallDefaultImplementation)]

<span data-ttu-id="bb364-129">任何实现 `IControl` 接口的类都可以重写默认的 `Paint` 方法，作为公共方法或作为显式接口实现。</span><span class="sxs-lookup"><span data-stu-id="bb364-129">Any class that implements the `IControl` interface can override the default `Paint` method, either as a public method, or as an explicit interface implementation.</span></span>

## <a name="see-also"></a><span data-ttu-id="bb364-130">另请参阅</span><span class="sxs-lookup"><span data-stu-id="bb364-130">See also</span></span>

- [<span data-ttu-id="bb364-131">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="bb364-131">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="bb364-132">类和结构</span><span class="sxs-lookup"><span data-stu-id="bb364-132">Classes and Structs</span></span>](../classes-and-structs/index.md)
- [<span data-ttu-id="bb364-133">接口</span><span class="sxs-lookup"><span data-stu-id="bb364-133">Interfaces</span></span>](./index.md)
- [<span data-ttu-id="bb364-134">继承</span><span class="sxs-lookup"><span data-stu-id="bb364-134">Inheritance</span></span>](../classes-and-structs/inheritance.md)
