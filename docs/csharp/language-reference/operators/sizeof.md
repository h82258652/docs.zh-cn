---
description: sizeof 运算符 - C# 参考
title: sizeof 运算符 - C# 参考
ms.date: 07/25/2019
f1_keywords:
- sizeof_CSharpKeyword
- sizeof
helpviewer_keywords:
- sizeof keyword [C#]
ms.assetid: c548592c-677c-4f40-a4ce-e613f7529141
ms.openlocfilehash: 6685cdb4fba2460ee4a47b004aa6911ab76235a4
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497367"
---
# <a name="sizeof-operator-c-reference"></a><span data-ttu-id="00606-103">sizeof 运算符（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="00606-103">sizeof operator (C# reference)</span></span>

<span data-ttu-id="00606-104">`sizeof` 运算符返回给定类型的变量所占用的字节数。</span><span class="sxs-lookup"><span data-stu-id="00606-104">The `sizeof` operator returns the number of bytes occupied by a variable of a given type.</span></span> <span data-ttu-id="00606-105">`sizeof` 运算符的参数必须是一个[非托管类型](../builtin-types/unmanaged-types.md)的名称，或是一个[限定](../../programming-guide/generics/constraints-on-type-parameters.md#unmanaged-constraint)为非托管类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="00606-105">The argument to the `sizeof` operator must be the name of an [unmanaged type](../builtin-types/unmanaged-types.md) or a type parameter that is [constrained](../../programming-guide/generics/constraints-on-type-parameters.md#unmanaged-constraint) to be an unmanaged type.</span></span>

<span data-ttu-id="00606-106">`sizeof` 运算符需要[不安全](../keywords/unsafe.md)上下文。</span><span class="sxs-lookup"><span data-stu-id="00606-106">The `sizeof` operator requires an [unsafe](../keywords/unsafe.md) context.</span></span> <span data-ttu-id="00606-107">但下表中的表达式在编译时被计算为相应的常数值，并不需要“不安全”的上下文：</span><span class="sxs-lookup"><span data-stu-id="00606-107">However, the expressions presented in the following table are evaluated in compile time to the corresponding constant values and don't require an unsafe context:</span></span>

|<span data-ttu-id="00606-108">Expression</span><span class="sxs-lookup"><span data-stu-id="00606-108">Expression</span></span>|<span data-ttu-id="00606-109">常量值</span><span class="sxs-lookup"><span data-stu-id="00606-109">Constant value</span></span>|
|---------|---------------|
|`sizeof(sbyte)`|<span data-ttu-id="00606-110">1</span><span class="sxs-lookup"><span data-stu-id="00606-110">1</span></span>|
|`sizeof(byte)`|<span data-ttu-id="00606-111">1</span><span class="sxs-lookup"><span data-stu-id="00606-111">1</span></span>|
|`sizeof(short)`|<span data-ttu-id="00606-112">2</span><span class="sxs-lookup"><span data-stu-id="00606-112">2</span></span>|
|`sizeof(ushort)`|<span data-ttu-id="00606-113">2</span><span class="sxs-lookup"><span data-stu-id="00606-113">2</span></span>|
|`sizeof(int)`|<span data-ttu-id="00606-114">4</span><span class="sxs-lookup"><span data-stu-id="00606-114">4</span></span>|
|`sizeof(uint)`|<span data-ttu-id="00606-115">4</span><span class="sxs-lookup"><span data-stu-id="00606-115">4</span></span>|
|`sizeof(long)`|<span data-ttu-id="00606-116">8</span><span class="sxs-lookup"><span data-stu-id="00606-116">8</span></span>|
|`sizeof(ulong)`|<span data-ttu-id="00606-117">8</span><span class="sxs-lookup"><span data-stu-id="00606-117">8</span></span>|
|`sizeof(char)`|<span data-ttu-id="00606-118">2</span><span class="sxs-lookup"><span data-stu-id="00606-118">2</span></span>|
|`sizeof(float)`|<span data-ttu-id="00606-119">4</span><span class="sxs-lookup"><span data-stu-id="00606-119">4</span></span>|
|`sizeof(double)`|<span data-ttu-id="00606-120">8</span><span class="sxs-lookup"><span data-stu-id="00606-120">8</span></span>|
|`sizeof(decimal)`|<span data-ttu-id="00606-121">16</span><span class="sxs-lookup"><span data-stu-id="00606-121">16</span></span>|
|`sizeof(bool)`|<span data-ttu-id="00606-122">1</span><span class="sxs-lookup"><span data-stu-id="00606-122">1</span></span>|

<span data-ttu-id="00606-123">下列情况也不需要使用不安全的上下文：`sizeof` 运算符的操作数是[枚举](../builtin-types/enum.md)类型的名称。</span><span class="sxs-lookup"><span data-stu-id="00606-123">You also don't need to use an unsafe context when the operand of the `sizeof` operator is the name of an [enum](../builtin-types/enum.md) type.</span></span>

<span data-ttu-id="00606-124">下面的示例演示 `sizeof` 运算符的用法：</span><span class="sxs-lookup"><span data-stu-id="00606-124">The following example demonstrates the usage of the `sizeof` operator:</span></span>

[!code-csharp[sizeof examples](snippets/shared/SizeOfOperator.cs)]

<span data-ttu-id="00606-125">`sizeof` 运算符返回公共语言运行时将在托管内存中分配的字节数。</span><span class="sxs-lookup"><span data-stu-id="00606-125">The `sizeof` operator returns a number of bytes that would be allocated by the common language runtime in managed memory.</span></span> <span data-ttu-id="00606-126">对于[结构](../builtin-types/struct.md)类型，该值包括了填充（如有），如前例所示。</span><span class="sxs-lookup"><span data-stu-id="00606-126">For [struct](../builtin-types/struct.md) types, that value includes any padding, as the preceding example demonstrates.</span></span> <span data-ttu-id="00606-127">`sizeof` 运算符的结果可能异于 <xref:System.Runtime.InteropServices.Marshal.SizeOf%2A?displayProperty=nameWithType> 方法的结果，该方法返回某个类型在 *非托管* 内存中的大小。</span><span class="sxs-lookup"><span data-stu-id="00606-127">The result of the `sizeof` operator might differ from the result of the <xref:System.Runtime.InteropServices.Marshal.SizeOf%2A?displayProperty=nameWithType> method, which returns the size of a type in *unmanaged* memory.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="00606-128">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="00606-128">C# language specification</span></span>

<span data-ttu-id="00606-129">有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的 [sizeof 运算符](~/_csharplang/spec/unsafe-code.md#the-sizeof-operator)部分。</span><span class="sxs-lookup"><span data-stu-id="00606-129">For more information, see [The sizeof operator](~/_csharplang/spec/unsafe-code.md#the-sizeof-operator) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="00606-130">另请参阅</span><span class="sxs-lookup"><span data-stu-id="00606-130">See also</span></span>

- [<span data-ttu-id="00606-131">C# 参考</span><span class="sxs-lookup"><span data-stu-id="00606-131">C# reference</span></span>](../index.md)
- [<span data-ttu-id="00606-132">C# 运算符和表达式</span><span class="sxs-lookup"><span data-stu-id="00606-132">C# operators and expressions</span></span>](index.md)
- [<span data-ttu-id="00606-133">指针相关的运算符</span><span class="sxs-lookup"><span data-stu-id="00606-133">Pointer related operators</span></span>](pointer-related-operators.md)
- [<span data-ttu-id="00606-134">指针类型</span><span class="sxs-lookup"><span data-stu-id="00606-134">Pointer types</span></span>](../unsafe-code.md#pointer-types)
- [<span data-ttu-id="00606-135">内存和跨度相关类型</span><span class="sxs-lookup"><span data-stu-id="00606-135">Memory and span-related types</span></span>](../../../standard/memory-and-spans/index.md)
- [<span data-ttu-id="00606-136">.NET 中的泛型</span><span class="sxs-lookup"><span data-stu-id="00606-136">Generics in .NET</span></span>](../../../standard/generics/index.md)
