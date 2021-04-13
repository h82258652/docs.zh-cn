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
# <a name="stackalloc-expression-c-reference"></a><span data-ttu-id="c5296-103">stackalloc 表达式（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="c5296-103">stackalloc expression (C# reference)</span></span>

<span data-ttu-id="c5296-104">`stackalloc` 表达式在堆栈上分配内存块。</span><span class="sxs-lookup"><span data-stu-id="c5296-104">A `stackalloc` expression allocates a block of memory on the stack.</span></span> <span data-ttu-id="c5296-105">该方法返回时，将自动丢弃在方法执行期间创建的堆栈中分配的内存块。</span><span class="sxs-lookup"><span data-stu-id="c5296-105">A stack allocated memory block created during the method execution is automatically discarded when that method returns.</span></span> <span data-ttu-id="c5296-106">不能显式释放使用 `stackalloc` 分配的内存。</span><span class="sxs-lookup"><span data-stu-id="c5296-106">You cannot explicitly free the memory allocated with `stackalloc`.</span></span> <span data-ttu-id="c5296-107">堆栈中分配的内存块不受[垃圾回收](../../../standard/garbage-collection/index.md)的影响，也不必通过 [`fixed` 语句](../keywords/fixed-statement.md)固定。</span><span class="sxs-lookup"><span data-stu-id="c5296-107">A stack allocated memory block is not subject to [garbage collection](../../../standard/garbage-collection/index.md) and doesn't have to be pinned with a [`fixed` statement](../keywords/fixed-statement.md).</span></span>

<span data-ttu-id="c5296-108">可以将 `stackalloc` 表达式的结果分配给以下任一类型的变量：</span><span class="sxs-lookup"><span data-stu-id="c5296-108">You can assign the result of a `stackalloc` expression to a variable of one of the following types:</span></span>

- <span data-ttu-id="c5296-109">从 C# 7.2 开始，<xref:System.Span%601?displayProperty=nameWithType> 或 <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> 将如下例所示：</span><span class="sxs-lookup"><span data-stu-id="c5296-109">Beginning with C# 7.2, <xref:System.Span%601?displayProperty=nameWithType> or <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>, as the following example shows:</span></span>

  [!code-csharp[stackalloc span](snippets/shared/StackallocOperator.cs#AssignToSpan)]

  <span data-ttu-id="c5296-110">将堆栈中分配的内存块分配给 <xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601> 变量时，不必使用 [unsafe](../keywords/unsafe.md) 上下文。</span><span class="sxs-lookup"><span data-stu-id="c5296-110">You don't have to use an [unsafe](../keywords/unsafe.md) context when you assign a stack allocated memory block to a <xref:System.Span%601> or <xref:System.ReadOnlySpan%601> variable.</span></span>

  <span data-ttu-id="c5296-111">使用这些类型时，可以在[条件表达式](conditional-operator.md)或赋值表达式中使用 `stackalloc` 表达式，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="c5296-111">When you work with those types, you can use a `stackalloc` expression in [conditional](conditional-operator.md) or assignment expressions, as the following example shows:</span></span>

  [!code-csharp[stackalloc expression](snippets/shared/StackallocOperator.cs#AsExpression)]

  <span data-ttu-id="c5296-112">从 C#8.0 开始，只要允许使用 <xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601> 变量，就可以在其他表达式中使用 `stackalloc` 表达式，如下例所示：</span><span class="sxs-lookup"><span data-stu-id="c5296-112">Beginning with C# 8.0, you can use a `stackalloc` expression inside other expressions whenever a <xref:System.Span%601> or <xref:System.ReadOnlySpan%601> variable is allowed, as the following example shows:</span></span>

  [!code-csharp[stackalloc in nested expressions](snippets/shared/StackallocOperator.cs#Nested)]

  > [!NOTE]
  > <span data-ttu-id="c5296-113">建议尽可能使用 <xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601> 类型来处理堆栈中分配的内存。</span><span class="sxs-lookup"><span data-stu-id="c5296-113">We recommend using <xref:System.Span%601> or <xref:System.ReadOnlySpan%601> types to work with stack allocated memory whenever possible.</span></span>

- <span data-ttu-id="c5296-114">[指针类型](../unsafe-code.md#pointer-types)，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="c5296-114">A [pointer type](../unsafe-code.md#pointer-types), as the following example shows:</span></span>

  [!code-csharp[stackalloc pointer](snippets/shared/StackallocOperator.cs#AssignToPointer)]

  <span data-ttu-id="c5296-115">如前面的示例所示，在使用指针类型时必须使用 `unsafe` 上下文。</span><span class="sxs-lookup"><span data-stu-id="c5296-115">As the preceding example shows, you must use an `unsafe` context when you work with pointer types.</span></span>

  <span data-ttu-id="c5296-116">对于指针类型，只能在局部变量声明中使用 `stackalloc` 表达式来初始化变量。</span><span class="sxs-lookup"><span data-stu-id="c5296-116">In the case of pointer types, you can use a `stackalloc` expression only in a local variable declaration to initialize the variable.</span></span>

<span data-ttu-id="c5296-117">堆栈上可用的内存量存在限制。</span><span class="sxs-lookup"><span data-stu-id="c5296-117">The amount of memory available on the stack is limited.</span></span> <span data-ttu-id="c5296-118">如果在堆栈上分配过多的内存，会引发 <xref:System.StackOverflowException>。</span><span class="sxs-lookup"><span data-stu-id="c5296-118">If you allocate too much memory on the stack, a <xref:System.StackOverflowException> is thrown.</span></span> <span data-ttu-id="c5296-119">为避免这种情况，请遵循以下规则：</span><span class="sxs-lookup"><span data-stu-id="c5296-119">To avoid that, follow the rules below:</span></span>

- <span data-ttu-id="c5296-120">限制使用 `stackalloc` 分配的内存量：</span><span class="sxs-lookup"><span data-stu-id="c5296-120">Limit the amount of memory you allocate with `stackalloc`:</span></span>

  [!code-csharp[limit stackalloc](snippets/shared/StackallocOperator.cs#LimitStackalloc)]

  <span data-ttu-id="c5296-121">由于堆栈上可用的内存量取决于执行代码的环境，因此在定义实际限值时请持保守态度。</span><span class="sxs-lookup"><span data-stu-id="c5296-121">Because the amount of memory available on the stack depends on the environment in which the code is executed, be conservative when you define the actual limit value.</span></span>

- <span data-ttu-id="c5296-122">避免在循环内使用 `stackalloc`。</span><span class="sxs-lookup"><span data-stu-id="c5296-122">Avoid using `stackalloc` inside loops.</span></span> <span data-ttu-id="c5296-123">在循环外分配内存块，然后在循环内重用它。</span><span class="sxs-lookup"><span data-stu-id="c5296-123">Allocate the memory block outside a loop and reuse it inside the loop.</span></span>

<span data-ttu-id="c5296-124">新分配的内存的内容未定义。</span><span class="sxs-lookup"><span data-stu-id="c5296-124">The content of the newly allocated memory is undefined.</span></span> <span data-ttu-id="c5296-125">在使用之前应对其进行初始化。</span><span class="sxs-lookup"><span data-stu-id="c5296-125">You should initialize it before the use.</span></span> <span data-ttu-id="c5296-126">例如，可以使用 <xref:System.Span%601.Clear%2A?displayProperty=nameWithType> 方法将所有项设置为 `T` 类型的默认值。</span><span class="sxs-lookup"><span data-stu-id="c5296-126">For example, you can use the <xref:System.Span%601.Clear%2A?displayProperty=nameWithType> method that sets all the items to the default value of type `T`.</span></span>

<span data-ttu-id="c5296-127">从 C# 7.3 开始，可以使用数组初始值设定项语法来定义新分配内存的内容。</span><span class="sxs-lookup"><span data-stu-id="c5296-127">Beginning with C# 7.3, you can use array initializer syntax to define the content of the newly allocated memory.</span></span> <span data-ttu-id="c5296-128">下面的示例演示执行此操作的各种方法：</span><span class="sxs-lookup"><span data-stu-id="c5296-128">The following example demonstrates various ways to do that:</span></span>

[!code-csharp[stackalloc initialization](snippets/shared/StackallocOperator.cs#StackallocInit)]

<span data-ttu-id="c5296-129">在表达式 `stackalloc T[E]` 中，`T` 必须是[非托管类型](../builtin-types/unmanaged-types.md)，并且 `E` 的计算结果必须为非负 [int](../builtin-types/integral-numeric-types.md) 值。</span><span class="sxs-lookup"><span data-stu-id="c5296-129">In expression `stackalloc T[E]`, `T` must be an [unmanaged type](../builtin-types/unmanaged-types.md) and `E` must evaluate to a non-negative [int](../builtin-types/integral-numeric-types.md) value.</span></span>

## <a name="security"></a><span data-ttu-id="c5296-130">安全性</span><span class="sxs-lookup"><span data-stu-id="c5296-130">Security</span></span>

<span data-ttu-id="c5296-131">使用 `stackalloc` 会自动启用公共语言运行时 (CLR) 中的缓冲区溢出检测功能。</span><span class="sxs-lookup"><span data-stu-id="c5296-131">The use of `stackalloc` automatically enables buffer overrun detection features in the common language runtime (CLR).</span></span> <span data-ttu-id="c5296-132">如果检测到缓冲区溢出，则将尽快终止进程，以便将执行恶意代码的可能性降到最低。</span><span class="sxs-lookup"><span data-stu-id="c5296-132">If a buffer overrun is detected, the process is terminated as quickly as possible to minimize the chance that malicious code is executed.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="c5296-133">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="c5296-133">C# language specification</span></span>

<span data-ttu-id="c5296-134">有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)以及[允许嵌入上下文中的 `stackalloc`](~/_csharplang/proposals/csharp-8.0/nested-stackalloc.md) 功能建议说明的[堆栈分配](~/_csharplang/spec/unsafe-code.md#stack-allocation)部分。</span><span class="sxs-lookup"><span data-stu-id="c5296-134">For more information, see the [Stack allocation](~/_csharplang/spec/unsafe-code.md#stack-allocation) section of the [C# language specification](~/_csharplang/spec/introduction.md) and the [Permit `stackalloc` in nested contexts](~/_csharplang/proposals/csharp-8.0/nested-stackalloc.md) feature proposal note.</span></span>

## <a name="see-also"></a><span data-ttu-id="c5296-135">请参阅</span><span class="sxs-lookup"><span data-stu-id="c5296-135">See also</span></span>

- [<span data-ttu-id="c5296-136">C# 参考</span><span class="sxs-lookup"><span data-stu-id="c5296-136">C# reference</span></span>](../index.md)
- [<span data-ttu-id="c5296-137">C# 运算符和表达式</span><span class="sxs-lookup"><span data-stu-id="c5296-137">C# operators and expressions</span></span>](index.md)
- [<span data-ttu-id="c5296-138">指针相关的运算符</span><span class="sxs-lookup"><span data-stu-id="c5296-138">Pointer related operators</span></span>](pointer-related-operators.md)
- [<span data-ttu-id="c5296-139">指针类型</span><span class="sxs-lookup"><span data-stu-id="c5296-139">Pointer types</span></span>](../unsafe-code.md#pointer-types)
- [<span data-ttu-id="c5296-140">内存和跨度相关类型</span><span class="sxs-lookup"><span data-stu-id="c5296-140">Memory and span-related types</span></span>](../../../standard/memory-and-spans/index.md)
- [<span data-ttu-id="c5296-141">stackalloc 注意事项</span><span class="sxs-lookup"><span data-stu-id="c5296-141">Dos and Don'ts of stackalloc</span></span>](https://vcsjones.dev/2020/02/24/stackalloc/)
