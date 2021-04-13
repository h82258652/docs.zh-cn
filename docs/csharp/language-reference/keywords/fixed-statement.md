---
description: fixed 语句 - C# 参考
title: fixed 语句 - C# 参考
ms.date: 05/10/2018
f1_keywords:
- fixed_CSharpKeyword
- fixed
helpviewer_keywords:
- fixed keyword [C#]
ms.openlocfilehash: 2af9c4311e8326d6df933ec5d4c38354fe021e5c
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497562"
---
# <a name="fixed-statement-c-reference"></a><span data-ttu-id="29d7b-103">fixed 语句（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="29d7b-103">fixed Statement (C# Reference)</span></span>

<span data-ttu-id="29d7b-104">`fixed` 语句可防止垃圾回收器重新定位可移动的变量。</span><span class="sxs-lookup"><span data-stu-id="29d7b-104">The `fixed` statement prevents the garbage collector from relocating a movable variable.</span></span> <span data-ttu-id="29d7b-105">`fixed` 语句仅允许存在于[不安全的](unsafe.md)上下文中。</span><span class="sxs-lookup"><span data-stu-id="29d7b-105">The `fixed` statement is only permitted in an [unsafe](unsafe.md) context.</span></span> <span data-ttu-id="29d7b-106">还可以使用 `fixed` 关键字创建[固定大小的缓冲区](../unsafe-code.md#fixed-size-buffers)。</span><span class="sxs-lookup"><span data-stu-id="29d7b-106">You can also use the `fixed` keyword to create [fixed size buffers](../unsafe-code.md#fixed-size-buffers).</span></span>

<span data-ttu-id="29d7b-107">`fixed` 语句将为托管变量设置一个指针，并在该语句的执行过程中“单边锁定”该变量。</span><span class="sxs-lookup"><span data-stu-id="29d7b-107">The `fixed` statement sets a pointer to a managed variable and "pins" that variable during the execution of the statement.</span></span> <span data-ttu-id="29d7b-108">仅可在 `fixed` 上下文中使用指向可移动托管变量的指针。</span><span class="sxs-lookup"><span data-stu-id="29d7b-108">Pointers to movable managed variables are useful only in a `fixed` context.</span></span> <span data-ttu-id="29d7b-109">如果没有 `fixed` 上下文，垃圾回收可能会不可预测地重定位变量。</span><span class="sxs-lookup"><span data-stu-id="29d7b-109">Without a `fixed` context, garbage collection could relocate the variables unpredictably.</span></span> <span data-ttu-id="29d7b-110">C# 编译器只允许将指针分配给 `fixed` 语句中的托管变量。</span><span class="sxs-lookup"><span data-stu-id="29d7b-110">The C# compiler only lets you assign a pointer to a managed variable in a `fixed` statement.</span></span>

[!code-csharp[Accessing fixed memory](snippets/FixedKeywordExamples.cs#1)]

<span data-ttu-id="29d7b-111">可以通过使用一个数组、字符串、固定大小的缓冲区或变量的地址来初始化指针。</span><span class="sxs-lookup"><span data-stu-id="29d7b-111">You can initialize a pointer by using an array, a string, a fixed-size buffer, or the address of a variable.</span></span> <span data-ttu-id="29d7b-112">以下示例演示变量地址、数组和字符串的使用方式：</span><span class="sxs-lookup"><span data-stu-id="29d7b-112">The following example illustrates the use of variable addresses, arrays, and strings:</span></span>

[!code-csharp[Initializing fixed size buffers](snippets/FixedKeywordExamples.cs#2)]

<span data-ttu-id="29d7b-113">从 C# 7.3 开始，`fixed` 语句可在数组、字符串、固定大小缓冲区或非托管变量以外的其他类型上执行。</span><span class="sxs-lookup"><span data-stu-id="29d7b-113">Starting with C# 7.3, the `fixed` statement operates on additional types beyond arrays, strings, fixed size buffers, or unmanaged variables.</span></span> <span data-ttu-id="29d7b-114">实施名为 `GetPinnableReference` 的方法的任何类型都可以被固定。</span><span class="sxs-lookup"><span data-stu-id="29d7b-114">Any type that implements a method named `GetPinnableReference` can be pinned.</span></span> <span data-ttu-id="29d7b-115">`GetPinnableReference` 必须返回[非托管类型](../builtin-types/unmanaged-types.md) 的 `ref` 变量。</span><span class="sxs-lookup"><span data-stu-id="29d7b-115">The `GetPinnableReference` must return a `ref` variable of an [unmanaged type](../builtin-types/unmanaged-types.md).</span></span> <span data-ttu-id="29d7b-116">.NET Core 2.0 中引入的 .NET 类型 <xref:System.Span%601?displayProperty=nameWithType> 和 <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> 使用此模式，并且可以固定。</span><span class="sxs-lookup"><span data-stu-id="29d7b-116">The .NET types <xref:System.Span%601?displayProperty=nameWithType> and <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> introduced in .NET Core 2.0 make use of this pattern and can be pinned.</span></span> <span data-ttu-id="29d7b-117">下面的示例对此进行了演示：</span><span class="sxs-lookup"><span data-stu-id="29d7b-117">This is shown in the following example:</span></span>

[!code-csharp[Accessing fixed memory](snippets/FixedKeywordExamples.cs#FixedSpan)]

<span data-ttu-id="29d7b-118">如果正在创建应加入此模式的类型，请参阅 <xref:System.Span%601.GetPinnableReference?displayProperty=nameWithType> 以查看有关实施此模式的示例。</span><span class="sxs-lookup"><span data-stu-id="29d7b-118">If you are creating types that should participate in this pattern, see <xref:System.Span%601.GetPinnableReference?displayProperty=nameWithType> for an example of implementing the pattern.</span></span>

<span data-ttu-id="29d7b-119">如果它们都是同一类型，则可以在一个语句中初始化多个指针：</span><span class="sxs-lookup"><span data-stu-id="29d7b-119">Multiple pointers can be initialized in one statement if they are all the same type:</span></span>

```csharp
fixed (byte* ps = srcarray, pd = dstarray) {...}
```

<span data-ttu-id="29d7b-120">若要初始化不同类型的指针，只需嵌套 `fixed` 语句，如下面的示例中所示。</span><span class="sxs-lookup"><span data-stu-id="29d7b-120">To initialize pointers of different types, simply nest `fixed` statements, as shown in the following example.</span></span>

[!code-csharp[Initializing multiple pointers](snippets/FixedKeywordExamples.cs#3)]

<span data-ttu-id="29d7b-121">执行该语句中的代码之后，任何固定的变量都将被解锁并受垃圾回收的约束。</span><span class="sxs-lookup"><span data-stu-id="29d7b-121">After the code in the statement is executed, any pinned variables are unpinned and subject to garbage collection.</span></span> <span data-ttu-id="29d7b-122">因此，请勿指向 `fixed` 语句之外的那些变量。</span><span class="sxs-lookup"><span data-stu-id="29d7b-122">Therefore, do not point to those variables outside the `fixed` statement.</span></span> <span data-ttu-id="29d7b-123">在 `fixed` 语句中声明的变量的作用域为该语句，使此操作更容易：</span><span class="sxs-lookup"><span data-stu-id="29d7b-123">The variables declared in the `fixed` statement are scoped to that statement, making this easier:</span></span>

```csharp
fixed (byte* ps = srcarray, pd = dstarray)
{
   ...
}
// ps and pd are no longer in scope here.
```

<span data-ttu-id="29d7b-124">在 `fixed` 语句中初始化的指针为只读变量。</span><span class="sxs-lookup"><span data-stu-id="29d7b-124">Pointers initialized in `fixed` statements are readonly variables.</span></span> <span data-ttu-id="29d7b-125">如果想要修改指针值，必须声明第二个指针变量，并修改它。</span><span class="sxs-lookup"><span data-stu-id="29d7b-125">If you want to modify the pointer value, you must declare a second pointer variable, and modify that.</span></span> <span data-ttu-id="29d7b-126">不能修改在 `fixed` 语句中声明的变量：</span><span class="sxs-lookup"><span data-stu-id="29d7b-126">The variable declared in the `fixed` statement cannot be modified:</span></span>

```csharp
fixed (byte* ps = srcarray, pd = dstarray)
{
    byte* pSourceCopy = ps;
    pSourceCopy++; // point to the next element.
    ps++; // invalid: cannot modify ps, as it is declared in the fixed statement.
}
```

<span data-ttu-id="29d7b-127">可以在堆栈上分配内存，在这种情况下，内存不受垃圾回收的约束，因此不需要固定。</span><span class="sxs-lookup"><span data-stu-id="29d7b-127">You can allocate memory on the stack, where it is not subject to garbage collection and therefore does not need to be pinned.</span></span> <span data-ttu-id="29d7b-128">为此，请使用 [`stackalloc` 表达式](../operators/stackalloc.md)。</span><span class="sxs-lookup"><span data-stu-id="29d7b-128">To do that, use a [`stackalloc` expression](../operators/stackalloc.md).</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="29d7b-129">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="29d7b-129">C# language specification</span></span>

<span data-ttu-id="29d7b-130">有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [fixed 语句](~/_csharplang/spec/unsafe-code.md#the-fixed-statement)部分。</span><span class="sxs-lookup"><span data-stu-id="29d7b-130">For more information, see [The fixed statement](~/_csharplang/spec/unsafe-code.md#the-fixed-statement) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="29d7b-131">请参阅</span><span class="sxs-lookup"><span data-stu-id="29d7b-131">See also</span></span>

- [<span data-ttu-id="29d7b-132">C# 参考</span><span class="sxs-lookup"><span data-stu-id="29d7b-132">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="29d7b-133">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="29d7b-133">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="29d7b-134">C# 关键字</span><span class="sxs-lookup"><span data-stu-id="29d7b-134">C# Keywords</span></span>](index.md)
- [<span data-ttu-id="29d7b-135">unsafe</span><span class="sxs-lookup"><span data-stu-id="29d7b-135">unsafe</span></span>](unsafe.md)
- [<span data-ttu-id="29d7b-136">指针类型</span><span class="sxs-lookup"><span data-stu-id="29d7b-136">Pointer types</span></span>](../unsafe-code.md#pointer-types)
- [<span data-ttu-id="29d7b-137">固定大小的缓冲区</span><span class="sxs-lookup"><span data-stu-id="29d7b-137">Fixed Size Buffers</span></span>](../unsafe-code.md#fixed-size-buffers)
