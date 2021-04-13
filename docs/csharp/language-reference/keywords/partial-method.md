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
# <a name="partial-method-c-reference"></a><span data-ttu-id="70dbf-103">分部方法（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="70dbf-103">partial method (C# Reference)</span></span>

<span data-ttu-id="70dbf-104">分部方法在分部类型的一部分中定义了签名，并在该类型的另一部分中定义了实现。</span><span class="sxs-lookup"><span data-stu-id="70dbf-104">A partial method has its signature defined in one part of a partial type, and its implementation defined in another part of the type.</span></span> <span data-ttu-id="70dbf-105">通过分部方法，类设计器可提供与事件处理程序类似的方法挂钩，以便开发者决定是否实现。</span><span class="sxs-lookup"><span data-stu-id="70dbf-105">Partial methods enable class designers to provide method hooks, similar to event handlers, that developers may decide to implement or not.</span></span> <span data-ttu-id="70dbf-106">如果开发者不提供实现，则编译器在编译时删除签名。</span><span class="sxs-lookup"><span data-stu-id="70dbf-106">If the developer does not supply an implementation, the compiler removes the signature at compile time.</span></span> <span data-ttu-id="70dbf-107">以下条件适用于分部方法：</span><span class="sxs-lookup"><span data-stu-id="70dbf-107">The following conditions apply to partial methods:</span></span>

- <span data-ttu-id="70dbf-108">声明必须以上下文关键字 [partial](../../language-reference/keywords/partial-type.md) 开头。</span><span class="sxs-lookup"><span data-stu-id="70dbf-108">Declarations must begin with the contextual keyword [partial](../../language-reference/keywords/partial-type.md).</span></span>

- <span data-ttu-id="70dbf-109">分部类型各部分中的签名必须匹配。</span><span class="sxs-lookup"><span data-stu-id="70dbf-109">Signatures in both parts of the partial type must match.</span></span>

<span data-ttu-id="70dbf-110">在以下情况下，不需要使用分部方法即可实现：</span><span class="sxs-lookup"><span data-stu-id="70dbf-110">A partial method isn't required to have an implementation in the following cases:</span></span>

- <span data-ttu-id="70dbf-111">没有任何可访问性修饰符（包括默认的 [专用](../../language-reference/keywords/private.md)）。</span><span class="sxs-lookup"><span data-stu-id="70dbf-111">It doesn't have any accessibility modifiers (including the default [private](../../language-reference/keywords/private.md)).</span></span>

- <span data-ttu-id="70dbf-112">返回 [void](../../language-reference/builtin-types/void.md)。</span><span class="sxs-lookup"><span data-stu-id="70dbf-112">It returns [void](../../language-reference/builtin-types/void.md).</span></span>

- <span data-ttu-id="70dbf-113">没有任何[输出](../../language-reference/keywords/out-parameter-modifier.md)参数。</span><span class="sxs-lookup"><span data-stu-id="70dbf-113">It doesn't have any [out](../../language-reference/keywords/out-parameter-modifier.md) parameters.</span></span>

- <span data-ttu-id="70dbf-114">没有以下任何修饰符：[virtual](../../language-reference/keywords/virtual.md)、[override](../../language-reference/keywords/override.md)、[sealed](../../language-reference/keywords/sealed.md)、[new](../../language-reference/keywords/new-modifier.md) 或 [extern](../../language-reference/keywords/extern.md)。</span><span class="sxs-lookup"><span data-stu-id="70dbf-114">It doesn't have any of the following modifiers [virtual](../../language-reference/keywords/virtual.md), [override](../../language-reference/keywords/override.md), [sealed](../../language-reference/keywords/sealed.md), [new](../../language-reference/keywords/new-modifier.md), or [extern](../../language-reference/keywords/extern.md).</span></span>

<span data-ttu-id="70dbf-115">任何不符合所有这些限制的方法（例如 `public virtual partial void` 方法）都必须提供实现。</span><span class="sxs-lookup"><span data-stu-id="70dbf-115">Any method that doesn't conform to all those restrictions (for example, `public virtual partial void` method), must provide an implementation.</span></span>

<span data-ttu-id="70dbf-116">下列示例显示在分部类的两个部分中定义的分部方法：</span><span class="sxs-lookup"><span data-stu-id="70dbf-116">The following example shows a partial method defined in two parts of a partial class:</span></span>

[!code-csharp[csrefKeywordsContextual#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#9)]

<span data-ttu-id="70dbf-117">分部方法还可用于与源生成器结合。</span><span class="sxs-lookup"><span data-stu-id="70dbf-117">Partial methods can also be useful in combination with source generators.</span></span> <span data-ttu-id="70dbf-118">例如，可使用以下模式定义 regex：</span><span class="sxs-lookup"><span data-stu-id="70dbf-118">For example a regex could be defined using the following pattern:</span></span>

```csharp
[RegexGenerated("(dog|cat|fish)")]
partial bool IsPetMatch(string input);
```

<span data-ttu-id="70dbf-119">有关详细信息，请参阅[分部类和方法](../../programming-guide/classes-and-structs/partial-classes-and-methods.md)。</span><span class="sxs-lookup"><span data-stu-id="70dbf-119">For more information, see [Partial Classes and Methods](../../programming-guide/classes-and-structs/partial-classes-and-methods.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="70dbf-120">请参阅</span><span class="sxs-lookup"><span data-stu-id="70dbf-120">See also</span></span>

- [<span data-ttu-id="70dbf-121">C# 参考</span><span class="sxs-lookup"><span data-stu-id="70dbf-121">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="70dbf-122">分部类型</span><span class="sxs-lookup"><span data-stu-id="70dbf-122">partial type</span></span>](partial-type.md)
