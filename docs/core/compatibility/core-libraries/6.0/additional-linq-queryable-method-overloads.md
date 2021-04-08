---
title: .NET 6 中断性变更：LINQ 可查询方法的其他重载
description: 了解核心 .NET 库中的 .NET 6 中断性变更，其中将其他方法重载添加到了 System.Linq.Queryable 类型。
ms.date: 03/31/2021
ms.openlocfilehash: 15a4e480f7d1b4e110084e736fc920a522439e69
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106123557"
---
# <a name="new-systemlinqqueryable-method-overloads"></a>新的 System.Linq.Queryable 方法重载

已将其他公共方法重载添加到 <xref:System.Linq.Queryable?displayProperty=fullName>，作为在 <https://github.com/dotnet/runtime/pull/47231> 中实现的新功能的一部分。 如果查找方法时的反射代码不够可靠，这些新增功能可以中断查询提供程序的实现。

## <a name="change-description"></a>更改说明

在 .NET 6 中，已将新的重载添加到[受影响的 API](#affected-apis) 部分列出的方法中。 反射代码可能会因这些新增功能而出现中断，如以下示例中所示：

```csharp
typeof(System.Linq.Queryable)
    .GetMethods(BindingFlags.Public | BindingFlags.Static)
    .Where(m => m.Name == "ElementAt")
    .Single();
```

此时，该反射代码将引发 <xref:System.InvalidOperationException>，其中包含一条类似于以下内容的消息：**序列包含多个元素**。

## <a name="version-introduced"></a>引入的版本

6.0 预览版 3 和预览版 4

## <a name="reason-for-change"></a>更改原因

添加了新的重载来扩展 LINQ `Queryable` API。

## <a name="recommended-action"></a>建议操作

如果你是查询提供程序库作者，请确保反射代码具有方法重载添加功能。 例如，使用显式接受该方法的参数类型的 <xref:System.Type.GetMethod%2A?displayProperty=nameWithType> 重载。

## <a name="affected-apis"></a>受影响的 API

为以下 <xref:System.Linq.Queryable> 扩展方法添加了新重载：

- <xref:System.Linq.Queryable.ElementAt%2A?displayProperty=fullName>
- <xref:System.Linq.Queryable.ElementAtOrDefault%2A?displayProperty=fullName>
- <xref:System.Linq.Queryable.Take%2A?displayProperty=fullName>
- <xref:System.Linq.Queryable.Min%2A?displayProperty=fullName>
- <xref:System.Linq.Queryable.Max%2A?displayProperty=fullName>
- <xref:System.Linq.Queryable.FirstOrDefault%2A?displayProperty=fullName>
- <xref:System.Linq.Queryable.LastOrDefault%2A?displayProperty=fullName>
- <xref:System.Linq.Queryable.SingleOrDefault%2A?displayProperty=fullName>
- <xref:System.Linq.Queryable.Zip%2A?displayProperty=fullName>

<!--

### Category

- Core .NET libraries
- LINQ

### Affected APIs

- `Overload:System.Linq.Queryable.ElementAt`
- `Overload:System.Linq.Queryable.ElementAtOrDefault`
- `Overload:System.Linq.Queryable.Take`
- `Overload:System.Linq.Queryable.Min`
- `Overload:System.Linq.Queryable.Max`
- `Overload:System.Linq.Queryable.FirstOrDefault`
- `Overload:System.Linq.Queryable.LastOrDefault`
- `Overload:System.Linq.Queryable.SingleOrDefault`
- `Overload:System.Linq.Queryable.Zip`

-->
