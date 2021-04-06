---
title: 中断性变更：WinForms 和 WPF 应用使用 Microsoft.NET.Sdk
description: 了解 .NET 5 中的中断性变更：Windows 窗体和 Windows Presentation Framework 应用现使用 .NET SDK，而不使用 .NET Core WinForms 和 WPF SDK。
ms.date: 09/18/2020
ms.openlocfilehash: ebd4f51d5436cec2f0463558e99a85cb261ae682
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079487"
---
# <a name="winforms-and-wpf-apps-use-microsoftnetsdk"></a>WinForms 和 WPF 应用使用 Microsoft.NET.Sdk

Windows 窗体和 Windows Presentation Framework (WPF) 应用现在使用 .NET SDK (`Microsoft.NET.Sdk`)，而不使用 .NET Core WinForms 和 WPF SDK (`Microsoft.NET.Sdk.WindowsDesktop`)。

## <a name="change-description"></a>更改说明

在以前的 .NET Core 版本中，WinForms 和 WPF 应用使用单独的[项目 SDK](../../../project-sdk/overview.md) (`Microsoft.NET.Sdk.WindowsDesktop`)。 从 .NET 5 开始，WinForms 和 WPF SDK 已与 .NET SDK (`Microsoft.NET.Sdk`) 统一。 此外，新的[目标框架名字对象 (TFM)](../../../../standard/frameworks.md) 替换 .NET 5 中的 `netcoreapp` 和 `netstandard`。 下面的示例显示了在重新面向 .NET 5 或更高版本时，需要对 WPF 项目文件进行的更改。

在以前的 .NET Core 版本中：

```xml
<Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>netcoreapp3.1</TargetFramework>
    <UseWPF>true</UseWPF>
  </PropertyGroup>

</Project>
```

在 .NET 5 及更高版本中：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net5.0-windows</TargetFramework>
    <UseWPF>true</UseWPF>
  </PropertyGroup>

</Project>
```

## <a name="version-introduced"></a>引入的版本

.NET SDK 5.0.100

## <a name="recommended-action"></a>建议操作

在 WPF 或 Windows 窗体项目文件中：

- 将 `Sdk` 属性更新为 `Microsoft.NET.Sdk`。
- 将 `TargetFramework` 属性更新为 `net5.0-windows`。

## <a name="affected-apis"></a>受影响的 API

无。

<!--

### Affected APIs

Not detectable via API analysis.

### Category

- Windows Forms
- Windows Presentation Framework (WPF)

-->
