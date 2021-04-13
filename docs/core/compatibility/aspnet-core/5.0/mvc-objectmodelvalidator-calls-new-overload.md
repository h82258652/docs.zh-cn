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
# <a name="mvc-objectmodelvalidator-calls-a-new-overload-of-validationvisitorvalidate"></a>MVC：ObjectModelValidator 调用 ValidationVisitor.Validate 的新重载

在 ASP.NET Core 5.0 中，添加了 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor.Validate%2A?displayProperty=nameWithType> 的重载。 新重载接受包含以下属性的顶级模型实例：

```diff
  bool Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel);
+ bool Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel, object container);
```

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.ObjectModelValidator> 调用 `ValidationVisitor` 的这一新重载来执行验证。 如果你的验证库与 ASP.NET Core MVC 的模型验证系统集成，请关注此新重载。

有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#26020](https://github.com/dotnet/aspnetcore/issues/26020)。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="old-behavior"></a>旧行为

`ObjectModelValidator` 在模型验证过程中调用以下重载：

```csharp
ValidationVisitor.Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel)
```

## <a name="new-behavior"></a>新行为

`ObjectModelValidator` 在模型验证过程中调用以下重载：

```csharp
ValidationVisitor.Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel, object container)
```

## <a name="reason-for-change"></a>更改原因

引入此更改是为了支持依赖于检查其他属性的验证程序，例如 <xref:System.ComponentModel.DataAnnotations.CompareAttribute>。

## <a name="recommended-action"></a>建议的操作

依赖于 `ObjectModelValidator` 调用 `ValidationVisitor` 的现有重载的验证框架在面向 .NET 5.0 或更高版本时必须重写新方法：

```csharp
public class MyCustomValidationVisitor : ValidationVisitor
{
+  public override bool Validate(ModelMetadata metadata, string key, object model, bool alwaysValidateAtTopLevel, object container)
+  {
+    ...
}
```

## <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor.Validate%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`Overload:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor.Validate`

-->
