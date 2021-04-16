---
title: 性能规则（代码分析）
description: 了解代码分析性能规则。
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.performancerules
helpviewer_keywords:
- rules, performance
- performance rules
- performance, rules
- managed code analysis rules, performance rules
author: gewarren
ms.author: gewarren
ms.openlocfilehash: 4409cc46eb73f13f8e59d7a51899da27035bb6af
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590170"
---
# <a name="performance-rules"></a>性能规则

性能规则支持高性能库和应用程序。

## <a name="in-this-section"></a>在本节中

| 规则 | 描述 |
| - | - |
| [CA1802:在合适的位置使用文本](ca1802.md) | 某个字段被声明为 static 和 read-only（在 Visual Basic 中为 Shared 和 ReadOnly），并使用可在编译时计算的值初始化。 因为赋给目标字段的值可在编译时计算，因此请将声明更改为 const（在 Visual Basic 中为 Const）字段，以便在编译时而非运行时计算值。 |
| [CA1805：避免进行不必要的初始化](ca1805.md) | 在运行构造函数之前，.NET 运行时将引用类型的所有字段初始化为其默认值。 在大多数情况下，将字段显式初始化为其默认值是多余的，这会增加维护成本，并可能会降低性能（例如随着程序集大小的增加）。 |
| [CA1806:不要忽略方法结果](ca1806.md) | 创建一个新对象，但从不使用该对象；或者调用会创建并返回一个新字符串的方法，但从不使用这个新字符串；或者组件对象模型 (COM) 或 P/Invoke 方法返回一个从不使用的 HRESULT 或错误代码。 |
| [CA1810:以内联方式初始化引用类型的静态字段](ca1810.md) | 当一个类型声明显式静态构造函数时，实时 (JIT) 编译器会向该类型的每个静态方法和实例构造函数中添加一项检查，以确保之前已调用该静态构造函数。 静态构造函数检查会降低性能。 |
| [CA1812:避免未实例化的内部类](ca1812.md) | 程序集级别类型的实例不是由程序集中的代码创建的。 |
| [CA1813:避免使用非密封特性](ca1813.md) | .NET 提供用于检索自定义属性的方法。 默认情况下，这些方法搜索特性继承层次结构。 通过密封特性，将无需搜索继承层次结构，且能够提高性能。 |
| [CA1814:与多维数组相比，首选使用交错数组](ca1814.md) | 交错数组是元素为数组的数组。 构成元素的数组可采用不同的大小，使某些数据集浪费的空间减少。 |
| [CA1815:重写值类型上的 Equals 和相等运算符](ca1815.md) | 对于值类型，Equals 的继承的实现使用反射库，并比较所有字段的内容。 反射需要消耗大量计算资源，可能没有必要比较每一个字段是否相等。 如果希望用户对实例进行比较或排序，或者希望用户将实例用作哈希表键，则值类型应实现 Equals。 |
| [CA1819:属性不应返回数组](ca1819.md) | 即使属性是只读的，该属性返回的数组也不受写入保护。 若要使数组不会被更改，属性必须返回数组的副本。 通常，用户不能理解调用这种属性的负面性能影响。 |
| [CA1820:使用字符串长度测试是否有空字符串](ca1820.md) | 使用 String.Length 属性或 String.IsNullOrEmpty 方法比较字符串要比使用 Equals 的速度快得多。 |
| [CA1821:移除空终结器](ca1821.md) | 应尽可能避免终结器，因为跟踪对象生存期会产生额外的性能系统开销。 空的终结器只会徒增开销，没有一点好处。 |
| [CA1822:将成员标记为 static](ca1822.md) | 可以将不访问实例数据或不调用实例方法的成员标记为 static（在 Visual Basic 中为 Shared）。 在将这些方法标记为 static 之后，编译器将向这些成员发出非虚拟调用站点。 这会使性能敏感的代码的性能得到显著提高。 |
| [CA1823:避免未使用的私有字段](ca1823.md) | 检测到程序集内有似乎未访问过的私有字段。 |
| [CA1824:用 NeutralResourcesLanguageAttribute 标记程序集](ca1824.md) | NeutralResourcesLanguage 属性通知资源管理器用于显示程序集的非特定区域性资源的语言。 这将改进所加载的第一个资源的查找性能，并缩小工作集。 |
| [CA1825:避免数组分配长度为零](ca1825.md) | 初始化长度为零的数组将导致不必要的内存分配。 相反，请通过调用 <xref:System.Array.Empty%2A?displayProperty=nameWithType> 来使用静态分配的空数组实例。 内存分配在此方法的所有调用之间共享。 |
| [CA1826:使用属性，而不是 Linq Enumerable 方法](ca1826.md) | 对支持等效且更有效的属性的类型使用了 <xref:System.Linq.Enumerable> LINQ 方法。 |
| [CA1827:如果可以使用 Any，请勿使用 Count/LongCount](ca1827.md) | 在使用 <xref:System.Linq.Enumerable.Any%2A> 方法会更有效的情况下使用了 <xref:System.Linq.Enumerable.Count%2A> 或 <xref:System.Linq.Enumerable.LongCount%2A> 方法。 |
| [CA1828:如果可以使用 AnyAsync，请勿使用 CountAsync/LongCountAsync](ca1828.md) | 在使用 <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.AnyAsync%2A> 方法会更有效的情况下使用了 <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.CountAsync%2A> 或 <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.LongCountAsync%2A> 方法。 |
| [CA1829:使用 Length/Count 属性，而不是 Enumerable.Count 方法](ca1829.md) | 对支持等效且更有效的 `Length` 或 `Count` 属性的类型使用了 <xref:System.Linq.Enumerable.Count%2A> LINQ 方法。 |
| [CA1830:在 StringBuilder 上优先使用强类型“追加和插入”方法重载](ca1830.md) | <xref:System.Text.StringBuilder.Append%2A> 和 <xref:System.Text.StringBuilder.Insert%2A> 为除 System.String 之外的多种类型提供重载。  如果可能，首选强类型重载，而非 ToString () 和基于字符串的重载。 |
| [CA1831:在合适的情况下，为字符串使用 AsSpan 而不是基于范围的索引器](ca1831.md) | 对字符串使用范围索引器并向 ReadOnlySpan&lt;char&gt; 类型隐式赋值时，将使用方法 <xref:System.String.Substring%2A?#System_String_Substring_System_Int32_System_Int32_> 而非 <xref:System.Span%601.Slice%2A?#System_Span_1_Slice_System_Int32_System_Int32_>，这会生成字符串请求部分的副本。 |
| [CA1832:使用 AsSpan 或 AsMemory 而不是基于范围的索引器来获取数组的 ReadOnlySpan 或 ReadOnlyMemory 部分](ca1832.md) | 对字符串使用范围索引器并向 <xref:System.ReadOnlySpan%601> 或 <xref:System.ReadOnlyMemory%601> 类型隐式赋值时，将使用方法 <xref:System.Runtime.CompilerServices.RuntimeHelpers.GetSubArray%2A> 而非 <xref:System.Span%601.Slice%2A?#System_Span_1_Slice_System_Int32_System_Int32_>，这会生成数组请求部分的副本。 |
| [CA1833:使用 AsSpan 或 AsMemory 而不是基于范围的索引器来获取数组的 Span 或 Memory 部分](ca1833.md) | 对字符串使用范围索引器并向 <xref:System.Span%601> 或 <xref:System.Memory%601> 类型隐式赋值时，将使用方法 <xref:System.Runtime.CompilerServices.RuntimeHelpers.GetSubArray%2A> 而非 <xref:System.Span%601.Slice%2A?#System_Span_1_Slice_System_Int32_System_Int32_>，这会生成数组请求部分的副本。 |
| [CA1834:对单字符字符串使用 StringBuilder.Append(char)](ca1834.md) | <xref:System.Text.StringBuilder> 具有将 `char` 用作其参数的 `Append` 重载。 优先选择调用 `char` 重载以提高性能。 |
| [CA1835：对于“ReadAsync”和“WriteAsync”，首选基于“Memory”的重载](ca1835.md) | “Stream”有一个将“Memory&lt;byte&gt;”用作第一个参数的“ReadAsync”重载和一个将“ReadOnlyMemory&lt;Byte&gt;”用作第一个参数的“WriteAsync”重载。 优先选择调用基于内存的重载，它们更有效。 |
| [CA1836：如可用，首选 `IsEmpty` 而不是 `Count`](ca1836.md) | 首选比 `Count`、`Length`、<xref:System.Linq.Enumerable.Count%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29> 或 <xref:System.Linq.Enumerable.LongCount%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29> 更有效的 `IsEmpty` 属性，以确定对象是否包含任何项目。 |
| [CA1837：使用 `Environment.ProcessId` 而不是 `Process.GetCurrentProcess().Id`](ca1837.md) | `Environment.ProcessId` 比 `Process.GetCurrentProcess().Id` 更简单、更快速。 |
| [CA1838：避免对 P/Invokes 使用 `StringBuilder` 参数](ca1838.md) | `StringBuilder` 的封送处理总是会创建一个本机缓冲区副本，这会导致一个封送处理操作出现多次分配。 |
