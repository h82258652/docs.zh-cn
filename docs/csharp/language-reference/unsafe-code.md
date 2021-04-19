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
# <a name="unsafe-code-and-pointer-types"></a>不安全代码和指针类型

你编写的大多数 C# 代码都是“可验证的安全代码”。 可验证的安全代码表示 .NET 工具可以验证代码是否安全。 通常，安全代码不会直接使用指针访问内存， 也不会分配原始内存， 而是创建托管对象。

C# 支持 [`unsafe`](keywords/unsafe.md) 上下文，你可在其中编写不可验证的代码。 在 `unsafe` 上下文中，代码可使用指针、分配和释放内存块，以及使用函数指针调用方法。 C# 中的不安全代码不一定是危险的，它只是其安全性不可验证的代码。

不安全代码具有以下属性：

- 可将方法、类型和代码块定义为不安全。
- 在某些情况下，通过移除数组绑定检查，不安全代码可提高应用程序的性能。
- 调用需要指针的本机函数时，需使用不安全代码。
- 使用不安全代码将引发安全风险和稳定性风险。
- 必须使用 [AllowUnsafeBlocks](compiler-options/language.md#allowunsafeblocks) 编译器选项来编译包含不安全块的代码。

## <a name="pointer-types"></a>指针类型

在不安全的上下文中，类型除了是值类型或引用类型外，还可以是指针类型。 指针类型声明采用下列形式之一：

``` csharp
type* identifier;
void* identifier; //allowed but not recommended
```

在指针类型中的 `*` 之前指定的类型被称为“referent 类型”  。 只有[非托管类型](builtin-types/unmanaged-types.md)可为引用类型。

指针类型不从[对象](builtin-types/reference-types.md)继承，并且指针类型与 `object` 之间不存在转换。 此外，装箱和取消装箱不支持指针。 但是，你可在不同的指针类型之间以及指针类型和整型之间进行转换。

当在同一声明中声明多个指针时，星号 (`*`) 仅与基础类型一起写入， 而不是用作每个指针名称的前缀。 例如：

```csharp
int* p1, p2, p3;   // Ok
int *p1, *p2, *p3;   // Invalid in C#
```

指针不能指向引用或包含引用的[结构](builtin-types/struct.md)，因为无法对对象引用进行垃圾回收，即使有指针指向它也是如此。 垃圾回收器并不跟踪是否有任何类型的指针指向对象。

`MyType*` 类型的指针变量的值为 `MyType` 类型的变量的地址。 下面是指针类型声明的示例：

- `int* p`：`p` 是指向整数的指针。
- `int** p`：`p` 是指向整数的指针的指针。
- `int*[] p`：`p` 是指向整数的指针的一维数组。
- `char* p`：`p` 是指向字符的指针。
- `void* p`：`p` 是指向未知类型的指针。

指针间接寻址运算符 `*` 可用于访问位于指针变量所指向的位置的内容。 例如，请考虑以下声明：

```csharp
int* myVariable;
```

表达式 `*myVariable` 表示在 `int` 中包含的地址处找到的 `myVariable` 变量。

关于 [`fixed` 语句](keywords/fixed-statement.md) 的文章中有几个指针示例。 下面的示例使用 `unsafe` 关键字和 `fixed` 语句，并显示如何递增内部指针。  你可将此代码粘贴到控制台应用程序的 Main 函数中来运行它。 这些示例必须使用 [AllowUnsafeBlocks](compiler-options/language.md#allowunsafeblocks) 编译器选项集进行编译。

:::code language="csharp" source="snippets/unsafe-code/FixedKeywordExamples.cs" ID="5":::

你无法对 `void*` 类型的指针应用间接寻址运算符。 但是，你可以使用强制转换将 void 指针转换为任何其他指针类型，反之亦然。

指针可以为 `null`。 将间接寻址运算符应用于 null 指针将导致由实现定义的行为。

在方法之间传递指针会导致未定义的行为。 考虑这种方法，该方法通过 `in`、`out` 或 `ref` 参数或作为函数结果返回一个指向局部变量的指针。 如果已在固定块中设置指针，则它指向的变量不再是固定的。

下表列出了可在不安全的上下文中对指针执行的运算符和语句：

|运算符/语句|使用|
|-------------------------|---------|
|`*`|执行指针间接寻址。|
|`->`|通过指针访问结构的成员。|
|`[]`|为指针建立索引。|
|`&`|获取变量的地址。|
|`++` 和 `--`|递增和递减指针。|
|`+` 和 `-`|执行指针算法。|
|`==`、`!=`、`<`、`>`、`<=` 和 `>=`|比较指针。|
|[`stackalloc`](operators/stackalloc.md)|在堆栈上分配内存。|
|[`fixed` 语句](keywords/fixed-statement.md)|临时固定变量以便找到其地址。|

要详细了解与指针相关的运算符，请参阅[与指针相关的运算符](operators/pointer-related-operators.md)。

任何指针类型都可以隐式转换为 `void*` 类型。 可以为任何指针类型分配值 `null`。 可以使用强制转换表达式将任何指针类型显式转换为任何其他指针类型。 也可以将任何整数类型转换为指针类型，或将任何指针类型转换为整数类型。 这些转换需要显式转换。

以下示例将 `int*` 转换为 `byte*`。 请注意，指针指向变量的最低寻址字节。 如果结果连续递增，直到达到 `int` 的大小（4 字节），可显示变量的其余字节。

:::code language="csharp" source="snippets/unsafe-code/Conversions.cs" ID="Conversion":::

## <a name="fixed-size-buffers"></a>固定大小的缓冲区

在 C# 中，可以使用 [fixed](keywords/fixed-statement.md) 语句来创建在数据结构中具有固定大小的数组的缓冲区。 当编写与其他语言或平台的数据源进行互操作的方法时，固定大小的缓冲区很有用。 固定的数组可以采用允许用于常规结构成员的任何属性或修饰符。 唯一的限制是数组类型必须为 `bool`、`byte`、`char`、`short`、`int`, `long`、`sbyte`、`ushort`、`uint`、`ulong`、`float` 或 `double`。

```csharp
private fixed char name[30];
```

在安全代码中，包含数组的 C# 结构不包含该数组的元素， 而该结构包含对这些元素的引用。 当在[不安全的](keywords/unsafe.md)代码块中使用数组时，可以在[结构](builtin-types/struct.md)中嵌入固定大小的数组。

以下 `struct` 的大小不依赖于数组中的元素数，因为 `pathName` 是一个引用：

:::code language="csharp" source="snippets/unsafe-code/FixedKeywordExamples.cs" ID="6":::

`struct` 可以在不安全代码中包含嵌入的数组。 在下面的示例中，`fixedBuffer` 数组具有固定的大小。 使用 `fixed` 语句建立指向第一个元素的指针。 通过此指针访问数组的元素。 `fixed` 语句将 `fixedBuffer` 实例字段固定到内存中的特定位置。

:::code language="csharp" source="snippets/unsafe-code/FixedKeywordExamples.cs" ID="7":::

包含 128 个元素的 `char` 数组的大小为 256 个字节。 在固定大小的 [char](builtin-types/char.md) 缓冲区中，每个字符总是占用 2 个字节，不考虑编码。 甚至在使用 `CharSet = CharSet.Auto` 或 `CharSet = CharSet.Ansi` 将 char 缓冲区封送到 API 方法或结构时，此数组大小也是相同的。 有关更多信息，请参见<xref:System.Runtime.InteropServices.CharSet>。

前面的示例演示访问未固定的 `fixed` 字段，此功能从 C# 7.3 开始提供。

另一常见的固定大小的数组是 [bool](builtin-types/bool.md) 数组。 `bool` 数组中的元素大小始终为 1 个字节。 `bool` 数组不适用于创建位数组或缓冲区。

固定大小的缓冲区使用 <xref:System.Runtime.CompilerServices.UnsafeValueTypeAttribute?displayProperty=nameWithType> 进行编译，它指示公共语言运行时 (CLR) 某个类型包含可能溢出的非托管数组。 使用 [stackalloc](operators/stackalloc.md) 分配的内存还会在 CLR 中自动启用缓冲区溢出检测功能。 前面的示例演示如何在 `unsafe struct` 中存在固定大小的缓冲区。

```csharp
internal unsafe struct Buffer
{
    public fixed char fixedBuffer[128];
}
```

为 `Buffer` 生成 C# 的编译器的特性如下：

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

固定大小的缓冲区与常规数组的区别体现在以下方面：

- 只能在 `unsafe` 上下文中使用。
- 只能是结构的实例字段。
- 它们始终是矢量或一维数组。
- 声明应包括长度，如 `fixed char id[8]`。 不能使用 `fixed char id[]`。

## <a name="how-to-use-pointers-to-copy-an-array-of-bytes"></a>如何使用指针来复制字节数组

下面的示例使用指针将字节从一个数组复制到另一个数组。

此示例使用 [unsafe](keywords/unsafe.md) 关键字，使你可以在 `Copy` 方法中使用指针。 [fixed](keywords/fixed-statement.md) 语句用于声明指向源数组和目标数组的指针。 `fixed` 语句将源数组和目标数组的位置固定在内存中，以便它们不会被垃圾回收所移动。 当完成 `fixed` 块后，将取消固定数组的内存块。 因为此示例中的 `Copy` 方法使用 `unsafe` 关键字，所以必须使用 [AllowUnsafeBlocks](compiler-options/language.md#allowunsafeblocks) 编译器选项对其进行编译。

此示例使用索引而非第二个非托管的指针访问这两个数组的元素。 `pSource` 和 `pTarget` 指针的声明固定数组。 从 C# 7.3 开始可以使用此功能。

:::code language="csharp" source="snippets/unsafe-code/FixedKeywordExamples.cs" ID="8":::

## <a name="function-pointers"></a>函数指针

C# 提供 [`delegate`](builtin-types/reference-types.md#the-delegate-type) 类型来定义安全函数指针对象。 调用委托时，需要实例化从 <xref:System.Delegate?displayProperty=nameWithType> 派生的类型并对其 `Invoke` 方法进行虚拟方法调用。 该虚拟调用使用 IL 指令 `callvirt`。 在性能关键的代码路径中，使用 IL 指令 `calli` 效率更高。

可以使用 `delegate*` 语法定义函数指针。 编译器将使用 `calli` 指令来调用函数，而不是实例化 `delegate` 对象并调用 `Invoke`。 以下代码声明了两种方法，它们使用 `delegate` 或 `delegate*` 来组合两个类型相同的对象。 第一种方法使用 <xref:System.Func%603?displayProperty=nameWithType> 委托类型。 第二种方法使用具有相同参数和返回类型的 `delegate*` 声明：

:::code language="csharp" source="snippets/unsafe-code/FunctionPointers.cs" ID="UseDelegateOrPointer":::

以下代码显示如何声明静态本地函数并使用指向该本地函数的指针调用 `UnsafeCombine` 方法：

:::code language="csharp" source="snippets/unsafe-code/FunctionPointers.cs" ID="InvokeViaFunctionPointer":::

前面的代码说明了有关作为函数指针使用的函数的几个规则：

- 函数指针只能在 `unsafe` 上下文中声明。
- 只能在 `unsafe` 上下文中调用采用 `delegate*`（或返回 `delegate*`）的方法。
- 只可在 `static` 函数上使用 `&` 运算符获取函数的地址。 （此规则适用于成员函数和本地函数）。

此语法与声明 `delegate` 类型和使用指针具有相似之处。 `delegate` 上的后缀 `*` 表示声明是函数指针。 将方法组分配给函数指针时，`&` 表示操作采用方法的地址。

可以使用关键字 `managed` 和 `unmanaged` 为 `delegate*` 指定调用约定。 另外，对于 `unmanaged` 函数指针，可以指定调用约定。 下面的声明显示每个示例。 第一个声明使用 `managed` 调用约定，这是默认值。 后面三个使用 `unmanaged` 调用约定。 每个声明都指定以下某个 ECMA 335 调用约定：`Cdecl`、`Stdcall`、`Fastcall` 或 `Thiscall`。 最后的声明使用 `unmanaged` 调用约定，指示 CLR 选择平台的默认调用约定。 CLR 将在运行时选择调用约定。

:::code language="csharp" source="snippets/unsafe-code/FunctionPointers.cs" ID="UnmanagedFunctionPointers":::

可以在 C# 9.0 的[函数指针](~/_csharplang/proposals/csharp-9.0/function-pointers.md)建议中详细了解函数指针。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[不安全代码](~/_csharplang/spec/unsafe-code.md)部分。
