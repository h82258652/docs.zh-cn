---
description: '了解有关详细信息，请参阅 ULong 数据类型 (Visual Basic) '
title: ULong 数据类型
ms.date: 01/31/2018
f1_keywords:
- vb.ulong
helpviewer_keywords:
- numbers [Visual Basic], whole
- whole numbers
- integral data types [Visual Basic]
- integer numbers
- numbers [Visual Basic], integer
- integers [Visual Basic], data types
- integers [Visual Basic], types
- data types [Visual Basic], integral
- literal type characters [Visual Basic], UL
- ULong data type
- UL literal type characters [Visual Basic]
ms.assetid: 017e0702-774e-44ae-bedc-786b424ca84e
ms.openlocfilehash: 107100b4992be75dab574cbf274b53f506e267f3
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494592"
---
# <a name="ulong-data-type-visual-basic"></a><span data-ttu-id="96a10-103">ULong 数据类型 (Visual Basic) </span><span class="sxs-lookup"><span data-stu-id="96a10-103">ULong data type (Visual Basic)</span></span>

<span data-ttu-id="96a10-104">保留未签名的64位 (8 字节) 整数，范围介于0到18446744073709551615之间， (超过1.84 倍 10 ^ 19) 。</span><span class="sxs-lookup"><span data-stu-id="96a10-104">Holds unsigned 64-bit (8-byte) integers ranging in value from 0 through 18,446,744,073,709,551,615 (more than 1.84 times 10 ^ 19).</span></span>

## <a name="remarks"></a><span data-ttu-id="96a10-105">注解</span><span class="sxs-lookup"><span data-stu-id="96a10-105">Remarks</span></span>

<span data-ttu-id="96a10-106">使用 `ULong` 数据类型可包含太大的二进制数据 `UInteger` 或可能的最大无符号整数值。</span><span class="sxs-lookup"><span data-stu-id="96a10-106">Use the `ULong` data type to contain binary data too large for `UInteger`, or the largest possible unsigned integer values.</span></span>

<span data-ttu-id="96a10-107">`ULong` 的默认值为 0。</span><span class="sxs-lookup"><span data-stu-id="96a10-107">The default value of `ULong` is 0.</span></span>

## <a name="literal-assignments"></a><span data-ttu-id="96a10-108">文本赋值</span><span class="sxs-lookup"><span data-stu-id="96a10-108">Literal assignments</span></span>

<span data-ttu-id="96a10-109">可以声明和初始化 `ULong` 变量，方法是向其分配十进制文本、十六进制文本、八进制文本，或者从 Visual Basic 2017) 二进制文本开始 (。</span><span class="sxs-lookup"><span data-stu-id="96a10-109">You can declare and initialize a `ULong` variable by assigning it a decimal literal, a hexadecimal literal, an octal literal, or (starting with Visual Basic 2017) a binary literal.</span></span> <span data-ttu-id="96a10-110">如果整数文本在 `ULong` 范围之外（即，如果它小于 <xref:System.UInt64.MinValue?displayProperty=nameWithType> 或大于 <xref:System.UInt64.MaxValue?displayProperty=nameWithType>），会发生编译错误。</span><span class="sxs-lookup"><span data-stu-id="96a10-110">If the integer literal is outside the range of `ULong` (that is, if it is less than <xref:System.UInt64.MinValue?displayProperty=nameWithType> or greater than <xref:System.UInt64.MaxValue?displayProperty=nameWithType>, a compilation error occurs.</span></span>

<span data-ttu-id="96a10-111">在以下示例中，表示为十进制、十六进制和二进制文本且等于 7,934,076,125 的整数被分配给 `ULong` 值。</span><span class="sxs-lookup"><span data-stu-id="96a10-111">In the following example, integers equal to 7,934,076,125 that are represented as decimal, hexadecimal, and binary literals are assigned to `ULong` values.</span></span>

[!code-vb[ULong](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#ULong)]

> [!NOTE]
> <span data-ttu-id="96a10-112">使用前缀 `&h` 或 `&H` 表示十六进制文本，使用前缀 `&b` 或表示 `&B` 二进制文本，使用前缀 `&o` 或 `&O` 表示八进制文本。</span><span class="sxs-lookup"><span data-stu-id="96a10-112">You use the prefix `&h` or `&H` to denote a hexadecimal literal, the prefix `&b` or `&B` to denote a binary literal, and the prefix `&o` or `&O` to denote an octal literal.</span></span> <span data-ttu-id="96a10-113">十进制文本没有前缀。</span><span class="sxs-lookup"><span data-stu-id="96a10-113">Decimal literals have no prefix.</span></span>

<span data-ttu-id="96a10-114">从 Visual Basic 2017 开始，还可以使用下划线字符 `_` 作为数字分隔符来增强可读性，如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="96a10-114">Starting with Visual Basic 2017, you can also use the underscore character, `_`, as a digit separator to enhance readability, as the following example shows.</span></span>

[!code-vb[ULong](../../../../samples/snippets/visualbasic/language-reference/data-types/numeric-literals.vb#LongS)]

<span data-ttu-id="96a10-115">从 Visual Basic 15.5 开始，还可以使用下划线字符 (`_`) 作为前缀和十六进制、二进制或八进制数字之间的前导分隔符。</span><span class="sxs-lookup"><span data-stu-id="96a10-115">Starting with Visual Basic 15.5, you can also use the underscore character (`_`) as a leading separator between the prefix and the hexadecimal, binary, or octal digits.</span></span> <span data-ttu-id="96a10-116">例如：</span><span class="sxs-lookup"><span data-stu-id="96a10-116">For example:</span></span>

```vb
Dim number As ULong = &H_F9AC_0326_1489_D68C
```

[!INCLUDE [supporting-underscores](../../../../includes/vb-separator-langversion.md)]

<span data-ttu-id="96a10-117">数字文本还可以包含 `UL` 或 `ul` [类型字符](../../programming-guide/language-features/data-types/type-characters.md) 来表示 `ULong` 数据类型，如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="96a10-117">Numeric literals can also include the `UL` or `ul` [type character](../../programming-guide/language-features/data-types/type-characters.md) to denote the `ULong` data type, as the following example shows.</span></span>

```vb
Dim number = &H_00_00_0A_96_2F_AC_14_D7ul
```

## <a name="programming-tips"></a><span data-ttu-id="96a10-118">编程提示</span><span class="sxs-lookup"><span data-stu-id="96a10-118">Programming tips</span></span>

- <span data-ttu-id="96a10-119">**负数。**</span><span class="sxs-lookup"><span data-stu-id="96a10-119">**Negative Numbers.**</span></span> <span data-ttu-id="96a10-120">由于 `ULong` 是一个无符号类型，因此它不能表示负数。</span><span class="sxs-lookup"><span data-stu-id="96a10-120">Because `ULong` is an unsigned type, it cannot represent a negative number.</span></span> <span data-ttu-id="96a10-121">如果 `-` 对计算结果为类型的表达式使用一元减号 () 运算符 `ULong` ，Visual Basic 会将表达式转换为 `Decimal` 第一个表达式。</span><span class="sxs-lookup"><span data-stu-id="96a10-121">If you use the unary minus (`-`) operator on an expression that evaluates to type `ULong`, Visual Basic converts the expression to `Decimal` first.</span></span>

- <span data-ttu-id="96a10-122">**CLS 遵从性。**</span><span class="sxs-lookup"><span data-stu-id="96a10-122">**CLS Compliance.**</span></span> <span data-ttu-id="96a10-123">`ULong`数据类型不是[公共语言规范](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/) (CLS) 的一部分，因此符合 cls 的代码不能使用使用它的组件。</span><span class="sxs-lookup"><span data-stu-id="96a10-123">The `ULong` data type is not part of the [Common Language Specification](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/) (CLS), so CLS-compliant code cannot consume a component that uses it.</span></span>

- <span data-ttu-id="96a10-124">**互操作注意事项。**</span><span class="sxs-lookup"><span data-stu-id="96a10-124">**Interop Considerations.**</span></span> <span data-ttu-id="96a10-125">如果你与不是为 .NET Framework 编写的组件（如自动化或 COM 对象）进行交互，请记住，如这样的类型 `ulong` 可以在其他环境中具有不同的数据宽度 (32 位) 。</span><span class="sxs-lookup"><span data-stu-id="96a10-125">If you are interfacing with components not written for the .NET Framework, for example Automation or COM objects, keep in mind that types such as `ulong` can have a different data width (32 bits) in other environments.</span></span> <span data-ttu-id="96a10-126">如果要将32位参数传递给此类组件，请 `UInteger` `ULong` 在托管的 Visual Basic 代码中将其声明为而不是。</span><span class="sxs-lookup"><span data-stu-id="96a10-126">If you are passing a 32-bit argument to such a component, declare it as `UInteger` instead of `ULong` in your managed Visual Basic code.</span></span>

- <span data-ttu-id="96a10-127">**扩大.**</span><span class="sxs-lookup"><span data-stu-id="96a10-127">**Widening.**</span></span> <span data-ttu-id="96a10-128">`ULong`数据类型扩大到 `Decimal` 、 `Single` 和 `Double` 。</span><span class="sxs-lookup"><span data-stu-id="96a10-128">The `ULong` data type widens to `Decimal`, `Single`, and `Double`.</span></span> <span data-ttu-id="96a10-129">这意味着你可以转换 `ULong` 为这些类型中的任何一种，而不会遇到 <xref:System.OverflowException?displayProperty=nameWithType> 错误。</span><span class="sxs-lookup"><span data-stu-id="96a10-129">This means you can convert `ULong` to any of these types without encountering a <xref:System.OverflowException?displayProperty=nameWithType> error.</span></span>

- <span data-ttu-id="96a10-130">**键入字符。**</span><span class="sxs-lookup"><span data-stu-id="96a10-130">**Type Characters.**</span></span> <span data-ttu-id="96a10-131">将文本类型字符追加 `UL` 到文本会将其强制转换为 `ULong` 数据类型。</span><span class="sxs-lookup"><span data-stu-id="96a10-131">Appending the literal type characters `UL` to a literal forces it to the `ULong` data type.</span></span> <span data-ttu-id="96a10-132">`ULong` 没有标识符类型字符。</span><span class="sxs-lookup"><span data-stu-id="96a10-132">`ULong` has no identifier type character.</span></span>

- <span data-ttu-id="96a10-133">**Framework 类型。**</span><span class="sxs-lookup"><span data-stu-id="96a10-133">**Framework Type.**</span></span> <span data-ttu-id="96a10-134">.NET Framework 中的对应类型是 <xref:System.UInt64?displayProperty=nameWithType> 结构。</span><span class="sxs-lookup"><span data-stu-id="96a10-134">The corresponding type in the .NET Framework is the <xref:System.UInt64?displayProperty=nameWithType> structure.</span></span>

## <a name="see-also"></a><span data-ttu-id="96a10-135">另请参阅</span><span class="sxs-lookup"><span data-stu-id="96a10-135">See also</span></span>

- <xref:System.UInt64>
- [<span data-ttu-id="96a10-136">数据类型</span><span class="sxs-lookup"><span data-stu-id="96a10-136">Data Types</span></span>](index.md)
- [<span data-ttu-id="96a10-137">Type Conversion Functions</span><span class="sxs-lookup"><span data-stu-id="96a10-137">Type Conversion Functions</span></span>](../functions/type-conversion-functions.md)
- [<span data-ttu-id="96a10-138">转换摘要</span><span class="sxs-lookup"><span data-stu-id="96a10-138">Conversion Summary</span></span>](../keywords/conversion-summary.md)
- [<span data-ttu-id="96a10-139">如何：调用需要使用无符号类型的 Windows 函数</span><span class="sxs-lookup"><span data-stu-id="96a10-139">How to: Call a Windows Function that Takes Unsigned Types</span></span>](../../programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [<span data-ttu-id="96a10-140">有效使用数据类型</span><span class="sxs-lookup"><span data-stu-id="96a10-140">Efficient Use of Data Types</span></span>](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
