---
title: 内置类型 - C# 参考
description: 了解 C# 内置值类型和引用类型
ms.date: 03/15/2021
helpviewer_keywords:
- types [C#], built-in
- built-in C# types
ms.openlocfilehash: 4b92add8189c6205408ec78c281eaacf04173047
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637060"
---
# <a name="built-in-types-c-reference"></a>内置类型（C# 参考）

下表列出了 C# 内置[值](value-types.md)类型：

|C# 类型关键字|.NET 类型|
|--------------|-------------------------|
|[`bool`](bool.md)|<xref:System.Boolean?displayProperty=nameWithType>|
|[`byte`](integral-numeric-types.md)|<xref:System.Byte?displayProperty=nameWithType>|
|[`sbyte`](integral-numeric-types.md)|<xref:System.SByte?displayProperty=nameWithType>|
|[`char`](char.md)|<xref:System.Char?displayProperty=nameWithType>|
|[`decimal`](floating-point-numeric-types.md)|<xref:System.Decimal?displayProperty=nameWithType>|
|[`double`](floating-point-numeric-types.md)|<xref:System.Double?displayProperty=nameWithType>|
|[`float`](floating-point-numeric-types.md)|<xref:System.Single?displayProperty=nameWithType>|
|[`int`](integral-numeric-types.md)|<xref:System.Int32?displayProperty=nameWithType>|
|[`uint`](integral-numeric-types.md)|<xref:System.UInt32?displayProperty=nameWithType>|
|[`nint`](nint-nuint.md)|<xref:System.IntPtr?displayProperty=nameWithType>|
|[`nuint`](nint-nuint.md)|<xref:System.UIntPtr?displayProperty=nameWithType>|
|[`long`](integral-numeric-types.md)|<xref:System.Int64?displayProperty=nameWithType>|
|[`ulong`](integral-numeric-types.md)|<xref:System.UInt64?displayProperty=nameWithType>|
|[`short`](integral-numeric-types.md)|<xref:System.Int16?displayProperty=nameWithType>|
|[`ushort`](integral-numeric-types.md)|<xref:System.UInt16?displayProperty=nameWithType>|

下表列出了 C# 内置[引用](../keywords/reference-types.md)类型：

|C# 类型关键字|.NET 类型|
|--------------|-------------------------|
|[`object`](reference-types.md#the-object-type)|<xref:System.Object?displayProperty=nameWithType>|
|[`string`](reference-types.md#the-string-type)|<xref:System.String?displayProperty=nameWithType>|
|[`dynamic`](reference-types.md#the-dynamic-type)|<xref:System.Object?displayProperty=nameWithType>|

在上表中，左侧列中的每个 C# 类型关键字（[nint、nuint](nint-nuint.md) 和 [dynamic](reference-types.md#the-dynamic-type) 除外）都是相应 .NET 类型的别名。 它们是可互换的。 例如，以下声明声明了相同类型的变量：

```csharp
int a = 123;
System.Int32 b = 123;
```

第一个表的最后两行中的 `nint` 和 `nuint` 类型是本机大小的整数。 在内部它们由所指示的 .NET 类型表示，但在任意情况下关键字和 .NET 类型都是不可互换的。 编译器为 `nint` 和 `nuint` 的整数类型提供操作和转换，而不为指针类型 `System.IntPtr` 和 `System.UIntPtr` 提供。 有关详细信息，请参阅 [`nint` 和 `nuint` 类型](nint-nuint.md)。

[`void`](void.md) 关键字表示缺少类型。 将其用作不返回值的方法的返回类型。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 类型的默认值](default-values.md)
