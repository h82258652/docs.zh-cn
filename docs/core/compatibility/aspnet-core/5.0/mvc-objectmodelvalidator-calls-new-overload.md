---
title: 中断性变更：MVC：ObjectModelValidator 调用 ValidationVisitor.Validate 的新重载
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：MVC：ObjectModelValidator 调用 ValidationVisitor.Validate 的新重载
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 7bf07360f5d5e93641b7fbc6634fda2e9f3e649f
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497588"
---
# <a name="mvc-objectmodelvalidator-calls-a-new-overload-of-validationvisitorvalidate"></a><span data-ttu-id="52766-103">MVC：ObjectModelValidator 调用 ValidationVisitor.Validate 的新重载</span><span class="sxs-lookup"><span data-stu-id="52766-103">MVC: ObjectModelValidator calls a new overload of ValidationVisitor.Validate</span></span>

<span data-ttu-id="52766-104">在 ASP.NET Core 5.0 中，添加了 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor.Validate%2A?displayProperty=nameWithType> 的重载。</span><span class="sxs-lookup"><span data-stu-id="52766-104">In ASP.NET Core 5.0, an overload of the <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor.Validate%2A?displayProperty=nameWithType> was added.</span></span> <span data-ttu-id="52766-105">新重载接受包含以下属性的顶级模型实例：</span><span class="sxs-lookup"><span data-stu-id="52766-105">The new overload accepts the top-level model instance that contains properties:</span></span>

```diff
  bool Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel);
+ bool Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel, object container);
```

<span data-ttu-id="52766-106"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.ObjectModelValidator> 调用 `ValidationVisitor` 的这一新重载来执行验证。</span><span class="sxs-lookup"><span data-stu-id="52766-106"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.ObjectModelValidator> invokes this new overload of `ValidationVisitor` to perform validation.</span></span> <span data-ttu-id="52766-107">如果你的验证库与 ASP.NET Core MVC 的模型验证系统集成，请关注此新重载。</span><span class="sxs-lookup"><span data-stu-id="52766-107">This new overload is pertinent if your validation library integrates with ASP.NET Core MVC's model validation system.</span></span>

<span data-ttu-id="52766-108">有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#26020](https://github.com/dotnet/aspnetcore/issues/26020)。</span><span class="sxs-lookup"><span data-stu-id="52766-108">For discussion, see GitHub issue [dotnet/aspnetcore#26020](https://github.com/dotnet/aspnetcore/issues/26020).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="52766-109">引入的版本</span><span class="sxs-lookup"><span data-stu-id="52766-109">Version introduced</span></span>

<span data-ttu-id="52766-110">5.0</span><span class="sxs-lookup"><span data-stu-id="52766-110">5.0</span></span>

## <a name="old-behavior"></a><span data-ttu-id="52766-111">旧行为</span><span class="sxs-lookup"><span data-stu-id="52766-111">Old behavior</span></span>

<span data-ttu-id="52766-112">`ObjectModelValidator` 在模型验证过程中调用以下重载：</span><span class="sxs-lookup"><span data-stu-id="52766-112">`ObjectModelValidator` invokes the following overload during model validation:</span></span>

```csharp
ValidationVisitor.Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel)
```

## <a name="new-behavior"></a><span data-ttu-id="52766-113">新行为</span><span class="sxs-lookup"><span data-stu-id="52766-113">New behavior</span></span>

<span data-ttu-id="52766-114">`ObjectModelValidator` 在模型验证过程中调用以下重载：</span><span class="sxs-lookup"><span data-stu-id="52766-114">`ObjectModelValidator` invokes the following overload during model validation:</span></span>

```csharp
ValidationVisitor.Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel, object container)
```

## <a name="reason-for-change"></a><span data-ttu-id="52766-115">更改原因</span><span class="sxs-lookup"><span data-stu-id="52766-115">Reason for change</span></span>

<span data-ttu-id="52766-116">引入此更改是为了支持依赖于检查其他属性的验证程序，例如 <xref:System.ComponentModel.DataAnnotations.CompareAttribute>。</span><span class="sxs-lookup"><span data-stu-id="52766-116">This change was introduced to support validators, such as <xref:System.ComponentModel.DataAnnotations.CompareAttribute>, that rely on inspection of other properties.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="52766-117">建议的操作</span><span class="sxs-lookup"><span data-stu-id="52766-117">Recommended action</span></span>

<span data-ttu-id="52766-118">依赖于 `ObjectModelValidator` 调用 `ValidationVisitor` 的现有重载的验证框架在面向 .NET 5.0 或更高版本时必须重写新方法：</span><span class="sxs-lookup"><span data-stu-id="52766-118">Validation frameworks that rely on `ObjectModelValidator` to invoke the existing overload of `ValidationVisitor` must override the new method when targeting .NET 5.0 or later:</span></span>

```csharp
public class MyCustomValidationVisitor : ValidationVisitor
{
+  public override bool Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel, object container)
+  {
+    ...
}
```

## <a name="affected-apis"></a><span data-ttu-id="52766-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="52766-119">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor.Validate%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`Overload:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor.Validate`

-->
