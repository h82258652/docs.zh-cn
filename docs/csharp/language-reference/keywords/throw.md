---
description: throw - C# 参考
title: throw - C# 参考
ms.date: 03/02/2015
f1_keywords:
- throw
- throw_CSharpKeyword
helpviewer_keywords:
- throw statement [C#]
- throw expression [C#]
- throw keyword [C#]
ms.assetid: 5ac4feef-4b1a-4c61-aeb4-61d549e5dd42
ms.openlocfilehash: e081bd86e57d69c85ea018d371b3e489145e958d
ms.sourcegitcommit: 872ca41d1c26f39d0aef57cc365d09503bac780d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2021
ms.locfileid: "106288038"
---
# <a name="throw-c-reference"></a><span data-ttu-id="41947-103">throw（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="41947-103">throw (C# Reference)</span></span>

<span data-ttu-id="41947-104">发出程序执行期间出现异常的信号。</span><span class="sxs-lookup"><span data-stu-id="41947-104">Signals the occurrence of an exception during program execution.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="41947-105">备注</span><span class="sxs-lookup"><span data-stu-id="41947-105">Remarks</span></span>

<span data-ttu-id="41947-106">`throw` 的语法为：</span><span class="sxs-lookup"><span data-stu-id="41947-106">The syntax of `throw` is:</span></span>

```csharp
throw [e];
```

<span data-ttu-id="41947-107">`e` 是一个派生自 <xref:System.Exception?displayProperty=nameWithType> 的类的实例。</span><span class="sxs-lookup"><span data-stu-id="41947-107">where `e` is an instance of a class derived from <xref:System.Exception?displayProperty=nameWithType>.</span></span> <span data-ttu-id="41947-108">下例使用 `throw` 语句在传递给名为 `GetNumber` 的方法的参数与内部数组的有效索引不对应时引发  <xref:System.IndexOutOfRangeException> 。</span><span class="sxs-lookup"><span data-stu-id="41947-108">The following example uses the `throw` statement to throw an <xref:System.IndexOutOfRangeException> if the argument passed to a method named `GetNumber` does not correspond to a valid index of an internal array.</span></span>

[!code-csharp[csrefKeyword#1](~/samples/snippets/csharp/language-reference/keywords/throw/throw1/throw-1.cs#1)]

<span data-ttu-id="41947-109">然后方法调用方使用 `try-catch` 或 `try-catch-finally` 块来处理引发的异常。</span><span class="sxs-lookup"><span data-stu-id="41947-109">Method callers then use a `try-catch` or `try-catch-finally` block to handle the thrown exception.</span></span> <span data-ttu-id="41947-110">下例处理由 `GetNumber` 方法引发的异常。</span><span class="sxs-lookup"><span data-stu-id="41947-110">The following example handles the exception thrown by the `GetNumber` method.</span></span>

[!code-csharp[csrefKeyword#2](~/samples/snippets/csharp/language-reference/keywords/throw/throw1/throw-1.cs#2)]

## <a name="re-throwing-an-exception"></a><span data-ttu-id="41947-111">重新引发异常</span><span class="sxs-lookup"><span data-stu-id="41947-111">Re-throwing an exception</span></span>

<span data-ttu-id="41947-112">`throw` 也可以用于 `catch` 块，以重新引发在 `catch` 块中处理的异常。</span><span class="sxs-lookup"><span data-stu-id="41947-112">`throw` can also be used in a `catch` block to re-throw an exception handled in a `catch` block.</span></span>  <span data-ttu-id="41947-113">在这种情况下，`throw` 不采用异常操作数。</span><span class="sxs-lookup"><span data-stu-id="41947-113">In this case, `throw` does not take an exception operand.</span></span> <span data-ttu-id="41947-114">当方法将参数从调用方传递给其他库方法时，这是最有用的，库方法引发的异常必须传递给调用方。</span><span class="sxs-lookup"><span data-stu-id="41947-114">It is most useful when a method passes on an argument from a caller to some other library method, and the library method throws an exception that must be passed on to the caller.</span></span> <span data-ttu-id="41947-115">例如，以下示例重新引发在尝试检索未初始化字符串的第一个字符时引发的 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="41947-115">For example, the following example re-throws an <xref:System.NullReferenceException> that is thrown when attempting to retrieve the first character of an uninitialized string.</span></span>

[!code-csharp[csrefKeyword#3](~/samples/snippets/csharp/language-reference/keywords/throw/throw2/throw-2.cs#3)]

> [!IMPORTANT]
> <span data-ttu-id="41947-116">还可以使用 `catch` 块中的 `throw e` 语法来实例化传递给调用方的新异常。</span><span class="sxs-lookup"><span data-stu-id="41947-116">You can also use the `throw e` syntax in a `catch` block to instantiate a new exception that you pass on to the caller.</span></span> <span data-ttu-id="41947-117">在这种情况下，将不会保留可从 <xref:System.Exception.StackTrace> 属性获得的原始异常的堆栈跟踪。</span><span class="sxs-lookup"><span data-stu-id="41947-117">In this case, the stack trace of the original exception, which is available from the <xref:System.Exception.StackTrace> property, is not preserved.</span></span>

## <a name="the-throw-expression"></a><span data-ttu-id="41947-118">`throw` 表达式</span><span class="sxs-lookup"><span data-stu-id="41947-118">The `throw` expression</span></span>

<span data-ttu-id="41947-119">从 C# 7.0 开始，`throw` 可以用作表达式和语句。</span><span class="sxs-lookup"><span data-stu-id="41947-119">Starting with C# 7.0, `throw` can be used as an expression as well as a statement.</span></span> <span data-ttu-id="41947-120">这允许在以前不支持的上下文中引发异常。</span><span class="sxs-lookup"><span data-stu-id="41947-120">This allows an exception to be thrown in contexts that were previously unsupported.</span></span> <span data-ttu-id="41947-121">这些方法包括：</span><span class="sxs-lookup"><span data-stu-id="41947-121">These include:</span></span>

- <span data-ttu-id="41947-122">[条件运算符](../operators/conditional-operator.md)。</span><span class="sxs-lookup"><span data-stu-id="41947-122">[the conditional operator](../operators/conditional-operator.md).</span></span> <span data-ttu-id="41947-123">下例使用 `throw` 表达式在向方法传递空字符串数组时引发 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="41947-123">The following example uses a `throw` expression to throw an <xref:System.ArgumentException> if a method is passed an empty string array.</span></span> <span data-ttu-id="41947-124">在 C# 7.0 之前，此逻辑将需要显示在 `if`/`else` 语句中。</span><span class="sxs-lookup"><span data-stu-id="41947-124">Before C# 7.0, this logic would need to appear in an `if`/`else` statement.</span></span>

   [!code-csharp[csrefKeyword#4](~/samples/snippets/csharp/language-reference/keywords/throw/conditional/conditional.cs#1)]

- <span data-ttu-id="41947-125">[null 合并运算符](../operators/null-coalescing-operator.md)。</span><span class="sxs-lookup"><span data-stu-id="41947-125">[the null-coalescing operator](../operators/null-coalescing-operator.md).</span></span> <span data-ttu-id="41947-126">在以下示例中，如果分配给 `Name` 属性的字符串为 `null`，则将 `throw` 表达式与 null 合并运算符结合使用以引发异常。</span><span class="sxs-lookup"><span data-stu-id="41947-126">In the following example, a `throw` expression is used with a null-coalescing operator to throw an exception if the string assigned to a `Name` property is `null`.</span></span>

   [!code-csharp[csrefKeyword#5](~/samples/snippets/csharp/language-reference/keywords/throw/coalescing/coalescing.cs#1)]

- <span data-ttu-id="41947-127">expression-bodied [lambda](../operators/lambda-expressions.md) 或方法。</span><span class="sxs-lookup"><span data-stu-id="41947-127">an expression-bodied [lambda](../operators/lambda-expressions.md) or method.</span></span> <span data-ttu-id="41947-128">下例说明了 expression-bodied 方法，由于不支持对 <xref:System.DateTime> 值的转换，该方法引发 <xref:System.InvalidCastException>。</span><span class="sxs-lookup"><span data-stu-id="41947-128">The following example illustrates an expression-bodied method that throws an <xref:System.InvalidCastException> because a conversion to a <xref:System.DateTime> value is not supported.</span></span>

   [!code-csharp[csrefKeyword#6](~/samples/snippets/csharp/language-reference/keywords/throw/exp-bodied/exp-bodied.cs#1)]

## <a name="c-language-specification"></a><span data-ttu-id="41947-129">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="41947-129">C# language specification</span></span>

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a><span data-ttu-id="41947-130">请参阅</span><span class="sxs-lookup"><span data-stu-id="41947-130">See also</span></span>

- [<span data-ttu-id="41947-131">C# 参考</span><span class="sxs-lookup"><span data-stu-id="41947-131">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="41947-132">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="41947-132">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="41947-133">try-catch</span><span class="sxs-lookup"><span data-stu-id="41947-133">try-catch</span></span>](try-catch.md)
- [<span data-ttu-id="41947-134">C# 关键字</span><span class="sxs-lookup"><span data-stu-id="41947-134">C# Keywords</span></span>](index.md)
- [<span data-ttu-id="41947-135">如何：显式抛出异常</span><span class="sxs-lookup"><span data-stu-id="41947-135">How to: Explicitly Throw Exceptions</span></span>](../../../standard/exceptions/how-to-explicitly-throw-exceptions.md)
