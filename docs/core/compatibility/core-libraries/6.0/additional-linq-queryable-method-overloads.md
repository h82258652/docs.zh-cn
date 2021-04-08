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
# <a name="new-systemlinqqueryable-method-overloads"></a><span data-ttu-id="829b5-103">新的 System.Linq.Queryable 方法重载</span><span class="sxs-lookup"><span data-stu-id="829b5-103">New System.Linq.Queryable method overloads</span></span>

<span data-ttu-id="829b5-104">已将其他公共方法重载添加到 <xref:System.Linq.Queryable?displayProperty=fullName>，作为在 <https://github.com/dotnet/runtime/pull/47231> 中实现的新功能的一部分。</span><span class="sxs-lookup"><span data-stu-id="829b5-104">Additional public method overloads have been added to <xref:System.Linq.Queryable?displayProperty=fullName> as part of the new features implemented in <https://github.com/dotnet/runtime/pull/47231>.</span></span> <span data-ttu-id="829b5-105">如果查找方法时的反射代码不够可靠，这些新增功能可以中断查询提供程序的实现。</span><span class="sxs-lookup"><span data-stu-id="829b5-105">If your reflection code isn't sufficiently robust when looking up methods, these additions can break your query provider implementations.</span></span>

## <a name="change-description"></a><span data-ttu-id="829b5-106">更改说明</span><span class="sxs-lookup"><span data-stu-id="829b5-106">Change description</span></span>

<span data-ttu-id="829b5-107">在 .NET 6 中，已将新的重载添加到[受影响的 API](#affected-apis) 部分列出的方法中。</span><span class="sxs-lookup"><span data-stu-id="829b5-107">In .NET 6, new overloads were added to the methods listed in the [Affected APIs](#affected-apis) section.</span></span> <span data-ttu-id="829b5-108">反射代码可能会因这些新增功能而出现中断，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="829b5-108">Reflection code, such as that shown in the following example, may break as a result of these additions:</span></span>

```csharp
typeof(System.Linq.Queryable)
    .GetMethods(BindingFlags.Public | BindingFlags.Static)
    .Where(m => m.Name == "ElementAt")
    .Single();
```

<span data-ttu-id="829b5-109">此时，该反射代码将引发 <xref:System.InvalidOperationException>，其中包含一条类似于以下内容的消息：**序列包含多个元素**。</span><span class="sxs-lookup"><span data-stu-id="829b5-109">This reflection code will now throw an <xref:System.InvalidOperationException> with a message similar to: **Sequence contains more than one element**.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="829b5-110">引入的版本</span><span class="sxs-lookup"><span data-stu-id="829b5-110">Version introduced</span></span>

<span data-ttu-id="829b5-111">6.0 预览版 3 和预览版 4</span><span class="sxs-lookup"><span data-stu-id="829b5-111">6.0 Preview 3 and Preview 4</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="829b5-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="829b5-112">Reason for change</span></span>

<span data-ttu-id="829b5-113">添加了新的重载来扩展 LINQ `Queryable` API。</span><span class="sxs-lookup"><span data-stu-id="829b5-113">The new overloads were added to extend the LINQ `Queryable` API.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="829b5-114">建议操作</span><span class="sxs-lookup"><span data-stu-id="829b5-114">Recommended action</span></span>

<span data-ttu-id="829b5-115">如果你是查询提供程序库作者，请确保反射代码具有方法重载添加功能。</span><span class="sxs-lookup"><span data-stu-id="829b5-115">If you're a query-provider library author, ensure that your reflection code is tolerant of method overload additions.</span></span> <span data-ttu-id="829b5-116">例如，使用显式接受该方法的参数类型的 <xref:System.Type.GetMethod%2A?displayProperty=nameWithType> 重载。</span><span class="sxs-lookup"><span data-stu-id="829b5-116">For example, use a <xref:System.Type.GetMethod%2A?displayProperty=nameWithType> overload that explicitly accepts the method's parameter types.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="829b5-117">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="829b5-117">Affected APIs</span></span>

<span data-ttu-id="829b5-118">为以下 <xref:System.Linq.Queryable> 扩展方法添加了新重载：</span><span class="sxs-lookup"><span data-stu-id="829b5-118">New overloads were added for the following <xref:System.Linq.Queryable> extension methods:</span></span>

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
