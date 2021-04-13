---
title: 中断性变更：Blazor：RenderTreeFrame readonly public 字段已变为属性
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Blazor：RenderTreeFrame readonly public 字段已变为属性
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: c4d70279b4280b079570099b82b056299405508b
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498160"
---
# <a name="blazor-rendertreeframe-readonly-public-fields-have-become-properties"></a>Blazor：RenderTreeFrame readonly public 字段已变为属性

在 ASP.NET Core 3.0 和 3.1 中，<xref:Microsoft.AspNetCore.Components.RenderTree.RenderTreeFrame> 结构公开了各种 `readonly public` 字段，包括 <xref:Microsoft.AspNetCore.Components.RenderTree.RenderTreeFrame.FrameType>、<xref:Microsoft.AspNetCore.Components.RenderTree.RenderTreeFrame.Sequence> 等。 在 ASP.NET Core 5.0 RC1 及更高版本中，所有 `readonly public` 字段都更改为 `readonly public` 属性。

此更改不会影响很多开发人员，原因如下：

* 只使用 `.razor` 文件（或甚至手动 <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> 调用）来定义其组件的任何应用或库都不会直接引用此类型。
* `RenderTreeFrame` 类型本身被视为实现详细信息，不用于框架外部。 ASP.NET Core 3.0 及更高版本包括一个分析器，如果直接使用该类型，则该分析器会发出编译器警告。
* 即使直接引用 `RenderTreeFrame`，此更改仍将破坏二进制文件，而不会破坏源。 也就是说，现有的源代码将正常编译并运行。 仅当针对 .NET Core 3.x 框架进行编译，然后针对 .NET 5.0 RC1 及更高版本的框架运行这些二进制文件时，才会遇到问题。

有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#25727](https://github.com/dotnet/aspnetcore/issues/25727)。

## <a name="version-introduced"></a>引入的版本

5.0 RC1

## <a name="old-behavior"></a>旧行为

`RenderTreeFrame` 上的公共成员被定义为字段。 例如，`renderTreeFrame.Sequence` 和 `renderTreeFrame.ElementName`。

## <a name="new-behavior"></a>新行为

`RenderTreeFrame` 上的公共成员被定义名称与以前相同的属性。 例如，`renderTreeFrame.Sequence` 和 `renderTreeFrame.ElementName`。

如果在此更改后，尚未重新编译旧的预编译代码，它可能会引发与此类似的异常：*MissingFieldException:找不到字段:"Microsoft.AspNetCore.Components.RenderTree.RenderTreeFrame.FrameType"* 。

## <a name="reason-for-change"></a>更改原因

若要在 ASP.NET Core 5.0 中的 Blazor 组件呈现方面实现强影响力的性能改进，此更改是必需的。 保持相同级别的安全和封装。

## <a name="recommended-action"></a>建议操作

大多数 Blazor 开发人员不受此更改的影响。 此更改更有可能影响库和包作者，但仅在极少数情况下会造成影响。 具体来说，如果你要执行以下操作：

* 开发一款应用，并使用 ASP.NET Core 3.x 或者升级到 5.0 RC1 或更高版本，则无需更改自己的代码。 但是，如果依赖于已升级的库来进行此更改，则需要更新到该库的更新版本。
* 开发一个库，并希望仅支持 ASP.NET Core 5.0 RC1 或更高版本，则无需执行任何操作。 只需确保项目文件声明 `net5.0` 或更高版本的 `<TargetFramework>` 值。
* 开发一个库，并希望支持 ASP.NET Core 3.x 和 5.0，则需要确定代码是否读取任何 `RenderTreeFrame` 成员。 例如，计算 `someRenderTreeFrame.FrameType`。
  * 大多数库不会读取 `RenderTreeFrame` 成员，包括包含 `.razor` 组件的库。 在这种情况下，无需执行任何操作。
  * 但是，如果你的库执行此操作，则你需要进行多目标操作以同时支持 `netstandard2.1` 和 `net5.0`。 在项目文件中应用以下更改：
    * 将现有 `<TargetFramework>` 元素替换为 `<TargetFrameworks>netstandard2.0;net5.0</TargetFrameworks>`。
    * 使用条件 `Microsoft.AspNetCore.Components` 包引用来说明希望支持的两个版本。 例如：

        ```xml
        <PackageReference Include="Microsoft.AspNetCore.Components" Version="3.0.0" Condition="'$(TargetFramework)' == 'netstandard2.0'" />
        <PackageReference Include="Microsoft.AspNetCore.Components" Version="5.0.0-rc.1.*" Condition="'$(TargetFramework)' != 'netstandard2.0'" />
        ```

有关进一步说明，请参阅此[显示 @jsakamoto 如何已升级 `Toolbelt.Blazor.HeadElement` 库的差异](https://github.com/jsakamoto/Toolbelt.Blazor.HeadElement/commit/090df430ba725f9420d412753db8104e8c32bf51)。

## <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Components.RenderTree.RenderTreeFrame?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`T:Microsoft.AspNetCore.Components.RenderTree.RenderTreeFrame`

-->
