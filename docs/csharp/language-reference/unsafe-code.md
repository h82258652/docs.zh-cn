---
title: 不安全代码、数据指针和函数指针
description: 了解不安全代码、指针和函数指针。 C# 要求声明不安全的上下文，以使用这些功能直接操作内存或函数指针。
ms.date: 04/01/2021
helpviewer_keywords:
- security [C#], type safety
- C# language, unsafe code
- type safety [C#]
- unsafe keyword [C#]
- unsafe code [C#]
- C# language, pointers
- pointers [C#], about pointers
ms.assetid: b0fcca10-a92d-4f2a-835b-b0ccae6739ee
ms.openlocfilehash: 9c55fc48b5805ba38dcf289cb5e03626cf3e96ec
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106499078"
---
# <a name="unsafe-code-and-pointer-types"></a><span data-ttu-id="0f0fc-104">不安全代码和指针类型</span><span class="sxs-lookup"><span data-stu-id="0f0fc-104">Unsafe code and pointer types</span></span>

<span data-ttu-id="0f0fc-105">你编写的大多数 C# 代码都是“可验证的安全代码”。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-105">Most of the C# code you write is "verifiably safe code."</span></span> <span data-ttu-id="0f0fc-106">可验证的安全代码表示 .NET 工具可以验证代码是否安全。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-106">*Verifiably safe code* means .NET tools can verify that the code is safe.</span></span> <span data-ttu-id="0f0fc-107">通常，安全代码不会直接使用指针访问内存，</span><span class="sxs-lookup"><span data-stu-id="0f0fc-107">In general, safe code doesn't directly access memory using pointers.</span></span> <span data-ttu-id="0f0fc-108">也不会分配原始内存，</span><span class="sxs-lookup"><span data-stu-id="0f0fc-108">It also doesn't allocate raw memory.</span></span> <span data-ttu-id="0f0fc-109">而是创建托管对象。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-109">It creates managed objects instead.</span></span>

<span data-ttu-id="0f0fc-110">C# 支持 [`unsafe`](keywords/unsafe.md) 上下文，你可在其中编写不可验证的代码。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-110">C# supports an [`unsafe`](keywords/unsafe.md) context, in which you may write *unverifiable* code.</span></span> <span data-ttu-id="0f0fc-111">在 `unsafe` 上下文中，代码可使用指针、分配和释放内存块，以及使用函数指针调用方法。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-111">In an `unsafe` context, code may use pointers, allocate and free blocks of memory, and call methods using function pointers.</span></span> <span data-ttu-id="0f0fc-112">C# 中的不安全代码不一定是危险的，它只是其安全性不可验证的代码。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-112">Unsafe code in C# isn't necessarily dangerous; it's just code whose safety cannot be verified.</span></span>

<span data-ttu-id="0f0fc-113">不安全代码具有以下属性：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-113">Unsafe code has the following properties:</span></span>

- <span data-ttu-id="0f0fc-114">可将方法、类型和代码块定义为不安全。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-114">Methods, types, and code blocks can be defined as unsafe.</span></span>
- <span data-ttu-id="0f0fc-115">在某些情况下，通过移除数组绑定检查，不安全代码可提高应用程序的性能。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-115">In some cases, unsafe code may increase an application's performance by removing array bounds checks.</span></span>
- <span data-ttu-id="0f0fc-116">调用需要指针的本机函数时，需使用不安全代码。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-116">Unsafe code is required when you call native functions that require pointers.</span></span>
- <span data-ttu-id="0f0fc-117">使用不安全代码将引发安全风险和稳定性风险。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-117">Using unsafe code introduces security and stability risks.</span></span>
- <span data-ttu-id="0f0fc-118">必须使用 [AllowUnsafeBlocks](compiler-options/language.md#allowunsafeblocks) 编译器选项来编译包含不安全块的代码。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-118">The code that contains unsafe blocks must be compiled with the [**AllowUnsafeBlocks**](compiler-options/language.md#allowunsafeblocks) compiler option.</span></span>

## <a name="pointer-types"></a><span data-ttu-id="0f0fc-119">指针类型</span><span class="sxs-lookup"><span data-stu-id="0f0fc-119">Pointer types</span></span>

<span data-ttu-id="0f0fc-120">在不安全的上下文中，类型除了是值类型或引用类型外，还可以是指针类型。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-120">In an unsafe context, a type may be a pointer type, in addition to a value type, or a reference type.</span></span> <span data-ttu-id="0f0fc-121">指针类型声明采用下列形式之一：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-121">A pointer type declaration takes one of the following forms:</span></span>

``` csharp
type* identifier;
void* identifier; //allowed but not recommended
```

<span data-ttu-id="0f0fc-122">在指针类型中的 `*` 之前指定的类型被称为“referent 类型”  。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-122">The type specified before the `*` in a pointer type is called the **referent type**.</span></span> <span data-ttu-id="0f0fc-123">只有[非托管类型](builtin-types/unmanaged-types.md)可为引用类型。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-123">Only an [unmanaged type](builtin-types/unmanaged-types.md) can be a referent type.</span></span>

<span data-ttu-id="0f0fc-124">指针类型不从[对象](builtin-types/reference-types.md)继承，并且指针类型与 `object` 之间不存在转换。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-124">Pointer types don't inherit from [object](builtin-types/reference-types.md) and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="0f0fc-125">此外，装箱和取消装箱不支持指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-125">Also, boxing and unboxing don't support pointers.</span></span> <span data-ttu-id="0f0fc-126">但是，你可在不同的指针类型之间以及指针类型和整型之间进行转换。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-126">However, you can convert between different pointer types and between pointer types and integral types.</span></span>

<span data-ttu-id="0f0fc-127">当在同一声明中声明多个指针时，星号 (`*`) 仅与基础类型一起写入，</span><span class="sxs-lookup"><span data-stu-id="0f0fc-127">When you declare multiple pointers in the same declaration, you write the asterisk (`*`) together with the underlying type only.</span></span> <span data-ttu-id="0f0fc-128">而不是用作每个指针名称的前缀。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-128">It isn't used as a prefix to each pointer name.</span></span> <span data-ttu-id="0f0fc-129">例如：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-129">For example:</span></span>

```csharp
int* p1, p2, p3;   // Ok
int *p1, *p2, *p3;   // Invalid in C#
```

<span data-ttu-id="0f0fc-130">指针不能指向引用或包含引用的[结构](builtin-types/struct.md)，因为无法对对象引用进行垃圾回收，即使有指针指向它也是如此。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-130">A pointer can't point to a reference or to a [struct](builtin-types/struct.md) that contains references, because an object reference can be garbage collected even if a pointer is pointing to it.</span></span> <span data-ttu-id="0f0fc-131">垃圾回收器并不跟踪是否有任何类型的指针指向对象。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-131">The garbage collector doesn't keep track of whether an object is being pointed to by any pointer types.</span></span>

<span data-ttu-id="0f0fc-132">`MyType*` 类型的指针变量的值为 `MyType` 类型的变量的地址。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-132">The value of the pointer variable of type `MyType*` is the address of a variable of type `MyType`.</span></span> <span data-ttu-id="0f0fc-133">下面是指针类型声明的示例：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-133">The following are examples of pointer type declarations:</span></span>

- <span data-ttu-id="0f0fc-134">`int* p`：`p` 是指向整数的指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-134">`int* p`: `p` is a pointer to an integer.</span></span>
- <span data-ttu-id="0f0fc-135">`int** p`：`p` 是指向整数的指针的指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-135">`int** p`: `p` is a pointer to a pointer to an integer.</span></span>
- <span data-ttu-id="0f0fc-136">`int*[] p`：`p` 是指向整数的指针的一维数组。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-136">`int*[] p`: `p` is a single-dimensional array of pointers to integers.</span></span>
- <span data-ttu-id="0f0fc-137">`char* p`：`p` 是指向字符的指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-137">`char* p`: `p` is a pointer to a char.</span></span>
- <span data-ttu-id="0f0fc-138">`void* p`：`p` 是指向未知类型的指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-138">`void* p`: `p` is a pointer to an unknown type.</span></span>

<span data-ttu-id="0f0fc-139">指针间接寻址运算符 `*` 可用于访问位于指针变量所指向的位置的内容。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-139">The pointer indirection operator `*` can be used to access the contents at the location pointed to by the pointer variable.</span></span> <span data-ttu-id="0f0fc-140">例如，请考虑以下声明：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-140">For example, consider the following declaration:</span></span>

```csharp
int* myVariable;
```

<span data-ttu-id="0f0fc-141">表达式 `*myVariable` 表示在 `int` 中包含的地址处找到的 `myVariable` 变量。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-141">The expression `*myVariable` denotes the `int` variable found at the address contained in `myVariable`.</span></span>

<span data-ttu-id="0f0fc-142">关于 [`fixed` 语句](keywords/fixed-statement.md) 的文章中有几个指针示例。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-142">There are several examples of pointers in the articles on the [`fixed` statement](keywords/fixed-statement.md).</span></span> <span data-ttu-id="0f0fc-143">下面的示例使用 `unsafe` 关键字和 `fixed` 语句，并显示如何递增内部指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-143">The following example uses the `unsafe` keyword and the `fixed` statement, and shows how to increment an interior pointer.</span></span>  <span data-ttu-id="0f0fc-144">你可将此代码粘贴到控制台应用程序的 Main 函数中来运行它。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-144">You can paste this code into the Main function of a console application to run it.</span></span> <span data-ttu-id="0f0fc-145">这些示例必须使用 [AllowUnsafeBlocks](compiler-options/language.md#allowunsafeblocks) 编译器选项集进行编译。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-145">These examples must be compiled with the [**AllowUnsafeBlocks**](compiler-options/language.md#allowunsafeblocks) compiler option set.</span></span>

:::code language="csharp" source="snippets/unsafe-code/FixedKeywordExamples.cs" ID="5":::

<span data-ttu-id="0f0fc-146">你无法对 `void*` 类型的指针应用间接寻址运算符。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-146">You can't apply the indirection operator to a pointer of type `void*`.</span></span> <span data-ttu-id="0f0fc-147">但是，你可以使用强制转换将 void 指针转换为任何其他指针类型，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-147">However, you can use a cast to convert a void pointer to any other pointer type, and vice versa.</span></span>

<span data-ttu-id="0f0fc-148">指针可以为 `null`。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-148">A pointer can be `null`.</span></span> <span data-ttu-id="0f0fc-149">将间接寻址运算符应用于 null 指针将导致由实现定义的行为。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-149">Applying the indirection operator to a null pointer causes an implementation-defined behavior.</span></span>

<span data-ttu-id="0f0fc-150">在方法之间传递指针会导致未定义的行为。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-150">Passing pointers between methods can cause undefined behavior.</span></span> <span data-ttu-id="0f0fc-151">考虑这种方法，该方法通过 `in`、`out` 或 `ref` 参数或作为函数结果返回一个指向局部变量的指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-151">Consider a method that returns a pointer to a local variable through an `in`, `out`, or `ref` parameter or as the function result.</span></span> <span data-ttu-id="0f0fc-152">如果已在固定块中设置指针，则它指向的变量不再是固定的。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-152">If the pointer was set in a fixed block, the variable to which it points may no longer be fixed.</span></span>

<span data-ttu-id="0f0fc-153">下表列出了可在不安全的上下文中对指针执行的运算符和语句：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-153">The following table lists the operators and statements that can operate on pointers in an unsafe context:</span></span>

|<span data-ttu-id="0f0fc-154">运算符/语句</span><span class="sxs-lookup"><span data-stu-id="0f0fc-154">Operator/Statement</span></span>|<span data-ttu-id="0f0fc-155">使用</span><span class="sxs-lookup"><span data-stu-id="0f0fc-155">Use</span></span>|
|-------------------------|---------|
|`*`|<span data-ttu-id="0f0fc-156">执行指针间接寻址。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-156">Performs pointer indirection.</span></span>|
|`->`|<span data-ttu-id="0f0fc-157">通过指针访问结构的成员。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-157">Accesses a member of a struct through a pointer.</span></span>|
|`[]`|<span data-ttu-id="0f0fc-158">为指针建立索引。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-158">Indexes a pointer.</span></span>|
|`&`|<span data-ttu-id="0f0fc-159">获取变量的地址。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-159">Obtains the address of a variable.</span></span>|
|<span data-ttu-id="0f0fc-160">`++` 和 `--`</span><span class="sxs-lookup"><span data-stu-id="0f0fc-160">`++` and `--`</span></span>|<span data-ttu-id="0f0fc-161">递增和递减指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-161">Increments and decrements pointers.</span></span>|
|<span data-ttu-id="0f0fc-162">`+` 和 `-`</span><span class="sxs-lookup"><span data-stu-id="0f0fc-162">`+` and `-`</span></span>|<span data-ttu-id="0f0fc-163">执行指针算法。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-163">Performs pointer arithmetic.</span></span>|
|<span data-ttu-id="0f0fc-164">`==`、`!=`、`<`、`>`、`<=` 和 `>=`</span><span class="sxs-lookup"><span data-stu-id="0f0fc-164">`==`, `!=`, `<`, `>`, `<=`, and `>=`</span></span>|<span data-ttu-id="0f0fc-165">比较指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-165">Compares pointers.</span></span>|
|[`stackalloc`](operators/stackalloc.md)|<span data-ttu-id="0f0fc-166">在堆栈上分配内存。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-166">Allocates memory on the stack.</span></span>|
|[<span data-ttu-id="0f0fc-167">`fixed` 语句</span><span class="sxs-lookup"><span data-stu-id="0f0fc-167">`fixed` statement</span></span>](keywords/fixed-statement.md)|<span data-ttu-id="0f0fc-168">临时固定变量以便找到其地址。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-168">Temporarily fixes a variable so that its address may be found.</span></span>|

<span data-ttu-id="0f0fc-169">要详细了解与指针相关的运算符，请参阅[与指针相关的运算符](operators/pointer-related-operators.md)。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-169">For more information about pointer-related operators, see [Pointer-related operators](operators/pointer-related-operators.md).</span></span>

<span data-ttu-id="0f0fc-170">任何指针类型都可以隐式转换为 `void*` 类型。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-170">Any pointer type can be implicitly converted to a `void*` type.</span></span> <span data-ttu-id="0f0fc-171">可以为任何指针类型分配值 `null`。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-171">Any pointer type can be assigned the value `null`.</span></span> <span data-ttu-id="0f0fc-172">可以使用强制转换表达式将任何指针类型显式转换为任何其他指针类型。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-172">Any pointer type can be explicitly converted to any other pointer type using a cast expression.</span></span> <span data-ttu-id="0f0fc-173">也可以将任何整数类型转换为指针类型，或将任何指针类型转换为整数类型。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-173">You can also convert any integral type to a pointer type, or any pointer type to an integral type.</span></span> <span data-ttu-id="0f0fc-174">这些转换需要显式转换。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-174">These conversions require an explicit cast.</span></span>

<span data-ttu-id="0f0fc-175">以下示例将 `int*` 转换为 `byte*`。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-175">The following example converts an `int*` to a `byte*`.</span></span> <span data-ttu-id="0f0fc-176">请注意，指针指向变量的最低寻址字节。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-176">Notice that the pointer points to the lowest addressed byte of the variable.</span></span> <span data-ttu-id="0f0fc-177">如果结果连续递增，直到达到 `int` 的大小（4 字节），可显示变量的其余字节。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-177">When you successively increment the result, up to the size of `int` (4 bytes), you can display the remaining bytes of the variable.</span></span>

:::code language="csharp" source="snippets/unsafe-code/Conversions.cs" ID="Conversion":::

## <a name="fixed-size-buffers"></a><span data-ttu-id="0f0fc-178">固定大小的缓冲区</span><span class="sxs-lookup"><span data-stu-id="0f0fc-178">Fixed-size buffers</span></span>

<span data-ttu-id="0f0fc-179">在 C# 中，可以使用 [fixed](keywords/fixed-statement.md) 语句来创建在数据结构中具有固定大小的数组的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-179">In C#, you can use the [fixed](keywords/fixed-statement.md) statement to create a buffer with a fixed size array in a data structure.</span></span> <span data-ttu-id="0f0fc-180">当编写与其他语言或平台的数据源进行互操作的方法时，固定大小的缓冲区很有用。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-180">Fixed size buffers are useful when you write methods that interoperate with data sources from other languages or platforms.</span></span> <span data-ttu-id="0f0fc-181">固定的数组可以采用允许用于常规结构成员的任何属性或修饰符。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-181">The fixed array can take any attributes or modifiers that are allowed for regular struct members.</span></span> <span data-ttu-id="0f0fc-182">唯一的限制是数组类型必须为 `bool`、`byte`、`char`、`short`、`int`, `long`、`sbyte`、`ushort`、`uint`、`ulong`、`float` 或 `double`。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-182">The only restriction is that the array type must be `bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, or `double`.</span></span>

```csharp
private fixed char name[30];
```

<span data-ttu-id="0f0fc-183">在安全代码中，包含数组的 C# 结构不包含该数组的元素，</span><span class="sxs-lookup"><span data-stu-id="0f0fc-183">In safe code, a C# struct that contains an array doesn't contain the array elements.</span></span> <span data-ttu-id="0f0fc-184">而该结构包含对这些元素的引用。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-184">The struct contains a reference to the elements instead.</span></span> <span data-ttu-id="0f0fc-185">当在[不安全的](keywords/unsafe.md)代码块中使用数组时，可以在[结构](builtin-types/struct.md)中嵌入固定大小的数组。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-185">You can embed an array of fixed size in a [struct](builtin-types/struct.md) when it's used in an [unsafe](keywords/unsafe.md) code block.</span></span>

<span data-ttu-id="0f0fc-186">以下 `struct` 的大小不依赖于数组中的元素数，因为 `pathName` 是一个引用：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-186">The size of the following `struct` doesn't depend on the number of elements in the array, since `pathName` is a reference:</span></span>

:::code language="csharp" source="snippets/unsafe-code/FixedKeywordExamples.cs" ID="6":::

<span data-ttu-id="0f0fc-187">`struct` 可以在不安全代码中包含嵌入的数组。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-187">A `struct` can contain an embedded array in unsafe code.</span></span> <span data-ttu-id="0f0fc-188">在下面的示例中，`fixedBuffer` 数组具有固定的大小。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-188">In the following example, the `fixedBuffer` array has a fixed size.</span></span> <span data-ttu-id="0f0fc-189">使用 `fixed` 语句建立指向第一个元素的指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-189">You use a `fixed` statement to establish a pointer to the first element.</span></span> <span data-ttu-id="0f0fc-190">通过此指针访问数组的元素。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-190">You access the elements of the array through this pointer.</span></span> <span data-ttu-id="0f0fc-191">`fixed` 语句将 `fixedBuffer` 实例字段固定到内存中的特定位置。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-191">The `fixed` statement pins the `fixedBuffer` instance field to a specific location in memory.</span></span>

:::code language="csharp" source="snippets/unsafe-code/FixedKeywordExamples.cs" ID="7":::

<span data-ttu-id="0f0fc-192">包含 128 个元素的 `char` 数组的大小为 256 个字节。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-192">The size of the 128 element `char` array is 256 bytes.</span></span> <span data-ttu-id="0f0fc-193">在固定大小的 [char](builtin-types/char.md) 缓冲区中，每个字符总是占用 2 个字节，不考虑编码。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-193">Fixed size [char](builtin-types/char.md) buffers always take 2 bytes per character, regardless of the encoding.</span></span> <span data-ttu-id="0f0fc-194">甚至在使用 `CharSet = CharSet.Auto` 或 `CharSet = CharSet.Ansi` 将 char 缓冲区封送到 API 方法或结构时，此数组大小也是相同的。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-194">This array size is the same even when char buffers are marshaled to API methods or structs with `CharSet = CharSet.Auto` or `CharSet = CharSet.Ansi`.</span></span> <span data-ttu-id="0f0fc-195">有关更多信息，请参见<xref:System.Runtime.InteropServices.CharSet>。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-195">For more information, see <xref:System.Runtime.InteropServices.CharSet>.</span></span>

<span data-ttu-id="0f0fc-196">前面的示例演示访问未固定的 `fixed` 字段，此功能从 C# 7.3 开始提供。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-196">The  preceding example demonstrates accessing `fixed` fields without pinning, which is available starting with C# 7.3.</span></span>

<span data-ttu-id="0f0fc-197">另一常见的固定大小的数组是 [bool](builtin-types/bool.md) 数组。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-197">Another common fixed-size array is the [bool](builtin-types/bool.md) array.</span></span> <span data-ttu-id="0f0fc-198">`bool` 数组中的元素大小始终为 1 个字节。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-198">The elements in a `bool` array are always 1 byte in size.</span></span> <span data-ttu-id="0f0fc-199">`bool` 数组不适用于创建位数组或缓冲区。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-199">`bool` arrays aren't appropriate for creating bit arrays or buffers.</span></span>

<span data-ttu-id="0f0fc-200">固定大小的缓冲区使用 <xref:System.Runtime.CompilerServices.UnsafeValueTypeAttribute?displayProperty=nameWithType> 进行编译，它指示公共语言运行时 (CLR) 某个类型包含可能溢出的非托管数组。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-200">Fixed size buffers are compiled with the <xref:System.Runtime.CompilerServices.UnsafeValueTypeAttribute?displayProperty=nameWithType>, which instructs the common language runtime (CLR) that a type contains an unmanaged array that can potentially overflow.</span></span> <span data-ttu-id="0f0fc-201">使用 [stackalloc](operators/stackalloc.md) 分配的内存还会在 CLR 中自动启用缓冲区溢出检测功能。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-201">Memory allocated using [stackalloc](operators/stackalloc.md) also automatically enables buffer overrun detection features in the CLR.</span></span> <span data-ttu-id="0f0fc-202">前面的示例演示如何在 `unsafe struct` 中存在固定大小的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-202">The previous example shows how a fixed size buffer could exist in an `unsafe struct`.</span></span>

```csharp
internal unsafe struct Buffer
{
    public fixed char fixedBuffer[128];
}
```

<span data-ttu-id="0f0fc-203">为 `Buffer` 生成 C# 的编译器的特性如下：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-203">The compiler-generated C# for `Buffer` is attributed as follows:</span></span>

```csharp
internal struct Buffer
{
    [StructLayout(LayoutKind.Sequential, Size = 256)]
    [CompilerGenerated]
    [UnsafeValueType]
    public struct <fixedBuffer>e__FixedBuffer
    {
        public char FixedElementField;
    }

    [FixedBuffer(typeof(char), 128)]
    public <fixedBuffer>e__FixedBuffer fixedBuffer;
}
```

<span data-ttu-id="0f0fc-204">固定大小的缓冲区与常规数组的区别体现在以下方面：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-204">Fixed size buffers differ from regular arrays in the following ways:</span></span>

- <span data-ttu-id="0f0fc-205">只能在 `unsafe` 上下文中使用。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-205">May only be used in an `unsafe` context.</span></span>
- <span data-ttu-id="0f0fc-206">只能是结构的实例字段。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-206">May only be instance fields of structs.</span></span>
- <span data-ttu-id="0f0fc-207">它们始终是矢量或一维数组。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-207">They're always vectors, or one-dimensional arrays.</span></span>
- <span data-ttu-id="0f0fc-208">声明应包括长度，如 `fixed char id[8]`。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-208">The declaration should include the length, such as `fixed char id[8]`.</span></span> <span data-ttu-id="0f0fc-209">不能使用 `fixed char id[]`。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-209">You can't use `fixed char id[]`.</span></span>

## <a name="how-to-use-pointers-to-copy-an-array-of-bytes"></a><span data-ttu-id="0f0fc-210">如何使用指针来复制字节数组</span><span class="sxs-lookup"><span data-stu-id="0f0fc-210">How to use pointers to copy an array of bytes</span></span>

<span data-ttu-id="0f0fc-211">下面的示例使用指针将字节从一个数组复制到另一个数组。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-211">The following example uses pointers to copy bytes from one array to another.</span></span>

<span data-ttu-id="0f0fc-212">此示例使用 [unsafe](keywords/unsafe.md) 关键字，使你可以在 `Copy` 方法中使用指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-212">This example uses the [unsafe](keywords/unsafe.md) keyword, which enables you to use pointers in the `Copy` method.</span></span> <span data-ttu-id="0f0fc-213">[fixed](keywords/fixed-statement.md) 语句用于声明指向源数组和目标数组的指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-213">The [fixed](keywords/fixed-statement.md) statement is used to declare pointers to the source and destination arrays.</span></span> <span data-ttu-id="0f0fc-214">`fixed` 语句将源数组和目标数组的位置固定在内存中，以便它们不会被垃圾回收所移动。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-214">The `fixed` statement *pins* the location of the source and destination arrays in memory so that they will not be moved by garbage collection.</span></span> <span data-ttu-id="0f0fc-215">当完成 `fixed` 块后，将取消固定数组的内存块。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-215">The memory blocks for the arrays are unpinned when the `fixed` block is completed.</span></span> <span data-ttu-id="0f0fc-216">因为此示例中的 `Copy` 方法使用 `unsafe` 关键字，所以必须使用 [AllowUnsafeBlocks](compiler-options/language.md#allowunsafeblocks) 编译器选项对其进行编译。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-216">Because the `Copy` method in this example uses the `unsafe` keyword, it must be compiled with the [**AllowUnsafeBlocks**](compiler-options/language.md#allowunsafeblocks) compiler option.</span></span>

<span data-ttu-id="0f0fc-217">此示例使用索引而非第二个非托管的指针访问这两个数组的元素。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-217">This example accesses the elements of both arrays using indices rather than a second unmanaged pointer.</span></span> <span data-ttu-id="0f0fc-218">`pSource` 和 `pTarget` 指针的声明固定数组。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-218">The declaration of the `pSource` and `pTarget` pointers pins the arrays.</span></span> <span data-ttu-id="0f0fc-219">从 C# 7.3 开始可以使用此功能。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-219">This feature is available starting with C# 7.3.</span></span>

:::code language="csharp" source="snippets/unsafe-code/FixedKeywordExamples.cs" ID="8":::

## <a name="function-pointers"></a><span data-ttu-id="0f0fc-220">函数指针</span><span class="sxs-lookup"><span data-stu-id="0f0fc-220">Function pointers</span></span>

<span data-ttu-id="0f0fc-221">C# 提供 [`delegate`](builtin-types/reference-types.md#the-delegate-type) 类型来定义安全函数指针对象。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-221">C# provides [`delegate`](builtin-types/reference-types.md#the-delegate-type) types to define safe function pointer objects.</span></span> <span data-ttu-id="0f0fc-222">调用委托时，需要实例化从 <xref:System.Delegate?displayProperty=nameWithType> 派生的类型并对其 `Invoke` 方法进行虚拟方法调用。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-222">Invoking a delegate involves instantiating a type derived from <xref:System.Delegate?displayProperty=nameWithType> and making a virtual method call to its `Invoke` method.</span></span> <span data-ttu-id="0f0fc-223">该虚拟调用使用 IL 指令 `callvirt`。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-223">This virtual call uses the `callvirt` IL instruction.</span></span> <span data-ttu-id="0f0fc-224">在性能关键的代码路径中，使用 IL 指令 `calli` 效率更高。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-224">In performance critical code paths, using the `calli` IL instruction is more efficient.</span></span>

<span data-ttu-id="0f0fc-225">可以使用 `delegate*` 语法定义函数指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-225">You can define a function pointer using the `delegate*` syntax.</span></span> <span data-ttu-id="0f0fc-226">编译器将使用 `calli` 指令来调用函数，而不是实例化 `delegate` 对象并调用 `Invoke`。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-226">The compiler will call the function using the `calli` instruction rather than instantiating a `delegate` object and calling `Invoke`.</span></span> <span data-ttu-id="0f0fc-227">以下代码声明了两种方法，它们使用 `delegate` 或 `delegate*` 来组合两个类型相同的对象。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-227">The following code declares two methods that use a `delegate` or a `delegate*` to combine two objects of the same type.</span></span> <span data-ttu-id="0f0fc-228">第一种方法使用 <xref:System.Func%603?displayProperty=nameWithType> 委托类型。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-228">The first method uses a <xref:System.Func%603?displayProperty=nameWithType> delegate type.</span></span> <span data-ttu-id="0f0fc-229">第二种方法使用具有相同参数和返回类型的 `delegate*` 声明：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-229">The second method uses a `delegate*` declaration with the same parameters and return type:</span></span>

:::code language="csharp" source="snippets/unsafe-code/FunctionPointers.cs" ID="UseDelegateOrPointer":::

<span data-ttu-id="0f0fc-230">以下代码显示如何声明静态本地函数并使用指向该本地函数的指针调用 `UnsafeCombine` 方法：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-230">The following code shows how you would declare a static local function and invoke the `UnsafeCombine` method using a pointer to that local function:</span></span>

:::code language="csharp" source="snippets/unsafe-code/FunctionPointers.cs" ID="InvokeViaFunctionPointer":::

<span data-ttu-id="0f0fc-231">前面的代码说明了有关作为函数指针使用的函数的几个规则：</span><span class="sxs-lookup"><span data-stu-id="0f0fc-231">The preceding code illustrates several of the rules on the function accessed as a function pointer:</span></span>

- <span data-ttu-id="0f0fc-232">函数指针只能在 `unsafe` 上下文中声明。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-232">Function pointers can only be declared in an `unsafe` context.</span></span>
- <span data-ttu-id="0f0fc-233">只能在 `unsafe` 上下文中调用采用 `delegate*`（或返回 `delegate*`）的方法。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-233">Methods that take a `delegate*` (or return a `delegate*`) can only be called in an `unsafe` context.</span></span>
- <span data-ttu-id="0f0fc-234">只可在 `static` 函数上使用 `&` 运算符获取函数的地址。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-234">The `&` operator to obtain the address of a function is allowed only on `static` functions.</span></span> <span data-ttu-id="0f0fc-235">（此规则适用于成员函数和本地函数）。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-235">(This rule applies to both member functions and local functions).</span></span>

<span data-ttu-id="0f0fc-236">此语法与声明 `delegate` 类型和使用指针具有相似之处。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-236">The syntax has parallels with declaring `delegate` types and using pointers.</span></span> <span data-ttu-id="0f0fc-237">`delegate` 上的后缀 `*` 表示声明是函数指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-237">The `*` suffix on `delegate` indicates the declaration is a *function pointer*.</span></span> <span data-ttu-id="0f0fc-238">将方法组分配给函数指针时，`&` 表示操作采用方法的地址。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-238">The `&` when assigning a method group to a function pointer indicates the operation takes the address of the method.</span></span>

<span data-ttu-id="0f0fc-239">可以使用关键字 `managed` 和 `unmanaged` 为 `delegate*` 指定调用约定。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-239">You can specify the calling convention for a `delegate*` using the keywords `managed` and `unmanaged`.</span></span> <span data-ttu-id="0f0fc-240">另外，对于 `unmanaged` 函数指针，可以指定调用约定。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-240">In addition, for `unmanaged` function pointers, you can specify the calling convention.</span></span> <span data-ttu-id="0f0fc-241">下面的声明显示每个示例。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-241">The following declarations show examples of each.</span></span> <span data-ttu-id="0f0fc-242">第一个声明使用 `managed` 调用约定，这是默认值。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-242">The first declaration uses the `managed` calling convention, which is the default.</span></span> <span data-ttu-id="0f0fc-243">后面三个使用 `unmanaged` 调用约定。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-243">The next three use an `unmanaged` calling convention.</span></span> <span data-ttu-id="0f0fc-244">每个声明都指定以下某个 ECMA 335 调用约定：`Cdecl`、`Stdcall`、`Fastcall` 或 `Thiscall`。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-244">Each specifies one of the ECMA 335 calling conventions: `Cdecl`, `Stdcall`, `Fastcall`, or `Thiscall`.</span></span> <span data-ttu-id="0f0fc-245">最后的声明使用 `unmanaged` 调用约定，指示 CLR 选择平台的默认调用约定。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-245">The last declarations uses the `unmanaged` calling convention, instructing the CLR to pick the default calling convention for the platform.</span></span> <span data-ttu-id="0f0fc-246">CLR 将在运行时选择调用约定。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-246">The CLR will choose the calling convention at run time.</span></span>

:::code language="csharp" source="snippets/unsafe-code/FunctionPointers.cs" ID="UnmanagedFunctionPointers":::

<span data-ttu-id="0f0fc-247">可以在 C# 9.0 的[函数指针](~/_csharplang/proposals/csharp-9.0/function-pointers.md)建议中详细了解函数指针。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-247">You can learn more about function pointers in the [Function pointer](~/_csharplang/proposals/csharp-9.0/function-pointers.md) proposal for C# 9.0.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="0f0fc-248">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="0f0fc-248">C# language specification</span></span>

<span data-ttu-id="0f0fc-249">有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[不安全代码](~/_csharplang/spec/unsafe-code.md)部分。</span><span class="sxs-lookup"><span data-stu-id="0f0fc-249">For more information, see the [Unsafe code](~/_csharplang/spec/unsafe-code.md) chapter of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>
