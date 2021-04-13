---
title: 中断性变更：已删除 Pubternal API
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：已删除部分 pubternal 本地化 API
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 35b6f80569945e54367117446ea96107d6c5199c
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497656"
---
# <a name="localization-pubternal-apis-removed"></a>本地化：已删除“Pubternal”API

为更好地维护 ASP.NET Core 的 public API 面，已删除部分 :::no-loc text="\"pubternal\""::: 本地化 API。 :::no-loc text="\"pubternal\""::: API 具有 `public` 访问修饰符，在指示 [internal](../../../../csharp/language-reference/keywords/internal.md) 意向的命名空间中定义。

有关讨论，请参阅 [dotnet/aspnetcore#22291](https://github.com/dotnet/aspnetcore/issues/22291)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 6

## <a name="old-behavior"></a>旧行为

以下 API 是 `public`：

- `Microsoft.Extensions.Localization.Internal.AssemblyWrapper`
- `Microsoft.Extensions.Localization.Internal.IResourceStringProvider`
- `Microsoft.Extensions.Localization.ResourceManagerStringLocalizer` 构造函数重载，接受以下参数类型之一：
  - `AssemblyWrapper`
  - `IResourceStringProvider`

## <a name="new-behavior"></a>新行为

以下列表概述了更改：

- `Microsoft.Extensions.Localization.Internal.AssemblyWrapper` 变成了 `Microsoft.Extensions.Localization.AssemblyWrapper`，现在是 `internal`。
- `Microsoft.Extensions.Localization.Internal.IResourceStringProvider` 变成了 `Microsoft.Extensions.Localization.Internal.IResourceStringProvider`，现在是 `internal`。
- `Microsoft.Extensions.Localization.ResourceManagerStringLocalizer` 构造函数重载，接受以下参数类型之一，现在是 `internal`：
  - `AssemblyWrapper`
  - `IResourceStringProvider`

## <a name="reason-for-change"></a>更改原因

[aspnet/Announcements#377](https://github.com/aspnet/Announcements/issues/377#issue-473651882) 中有更多全面的解释，从 `public` API 面中删除了 :::no-loc text="\"pubternal\""::: 类型。 这些更改可使更多类适应该设计决策。 上述类用作了团队内部测试的扩展点。

## <a name="recommended-action"></a>建议操作

尽管不太可能，但某些应用可能有意或无意地依赖 :::no-loc text="\"pubternal\""::: 类型。 请参阅[新行为](#new-behavior)部分，确定如何从类型中进行迁移。

如果你已确定方案，且该方案在此更改之前允许公共 API 但现在不允许，则请在 [dotnet/aspnetcore](https://github.com/dotnet/aspnetcore/issues) 上提交问题。

## <a name="affected-apis"></a>受影响的 API

- `Microsoft.Extensions.Localization.Internal.AssemblyWrapper`
- `Microsoft.Extensions.Localization.Internal.IResourceStringProvider`
- <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer.%23ctor%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

- `T:Microsoft.Extensions.Localization.Internal.AssemblyWrapper`
- `T:Microsoft.Extensions.Localization.Internal.IResourceStringProvider`
- `Overload:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer.#ctor`

-->
