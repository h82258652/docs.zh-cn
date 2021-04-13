---
title: Microsoft.NET.Sdk 的 MSBuild 属性
description: .NET SDK 可以理解的 MSBuild 属性和项的引用。
ms.date: 02/14/2020
ms.topic: reference
ms.custom: updateeachrelease
ms.openlocfilehash: effcb704056f553b2986ee4a61f73c0dc58af599
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105636761"
---
# <a name="msbuild-reference-for-net-sdk-projects"></a>.NET SDK 项目的 MSBuild 引用

此页是对可用于配置 .NET 项目的 MSBuild 属性和项的引用。

> [!NOTE]
> 此页面正在运行中，未列出 .NET SDK 的所有有用的 MSBuild 属性。 有关通用 MSBuild 属性的列表，请参阅[通用 MSBuild 属性](/visualstudio/msbuild/common-msbuild-project-properties)。

## <a name="framework-properties"></a>框架属性

本节收录了以下 MSBuild 属性：

- [TargetFramework](#targetframework)
- [TargetFrameworks](#targetframeworks)
- [NetStandardImplicitPackageVersion](#netstandardimplicitpackageversion)

### <a name="targetframework"></a>TargetFramework

`TargetFramework` 属性指定应用的目标框架版本。 有关有效的目标框架名字对象的列表，请参阅 [SDK 样式项目中的目标框架](../../standard/frameworks.md#supported-target-frameworks)。

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.1</TargetFramework>
</PropertyGroup>
```

有关详细信息，请参阅 [SDK 样式项目中的目标框架](../../standard/frameworks.md)。

### <a name="targetframeworks"></a>TargetFrameworks

如果希望应用面向多个平台，请使用 `TargetFrameworks` 属性。 有关有效的目标框架名字对象的列表，请参阅 [SDK 样式项目中的目标框架](../../standard/frameworks.md#supported-target-frameworks)。

> [!NOTE]
> 如果指定了 `TargetFramework`（单数），则忽略此属性。

```xml
<PropertyGroup>
  <TargetFrameworks>netcoreapp3.1;net462</TargetFrameworks>
</PropertyGroup>
```

有关详细信息，请参阅 [SDK 样式项目中的目标框架](../../standard/frameworks.md)。

### <a name="netstandardimplicitpackageversion"></a>NetStandardImplicitPackageVersion

> [!NOTE]
> 此属性仅适用于使用 `netstandard1.x` 的项目。 它不适用于使用 `netstandard2.x` 的项目。

如果要指定低于元包版本的框架版本，请使用 `NetStandardImplicitPackageVersion` 属性。 以下示例中的项目文件以 `netstandard1.3` 为目标，但使用 `NETStandard.Library` 的 1.6.0 版本。

```xml
<PropertyGroup>
  <TargetFramework>netstandard1.3</TargetFramework>
  <NetStandardImplicitPackageVersion>1.6.0</NetStandardImplicitPackageVersion>
</PropertyGroup>
```

## <a name="assembly-info-generation-properties"></a>程序集信息生成属性

- [GenerateAssemblyCompanyAttribute](#generateassemblycompanyattribute)
- [GenerateAssemblyConfigurationAttribute](#generateassemblyconfigurationattribute)
- [GenerateAssemblyCopyrightAttribute](#generateassemblycopyrightattribute)
- [GenerateAssemblyDescriptionAttribute](#generateassemblydescriptionattribute)
- [GenerateAssemblyFileVersionAttribute](#generateassemblyfileversionattribute)
- [GenerateAssemblyInfo](#generateassemblyinfo)
- [GenerateAssemblyInformationalVersionAttribute](#generateassemblyinformationalversionattribute)
- [GenerateAssemblyProductAttribute](#generateassemblyproductattribute)
- [GenerateAssemblyTitleAttribute](#generateassemblytitleattribute)
- [GenerateAssemblyVersionAttribute](#generateassemblyversionattribute)
- [GeneratedAssemblyInfoFile](#generatedassemblyinfofile)
- [GenerateNeutralResourcesLanguageAttribute](#generateneutralresourceslanguageattribute)

### <a name="generateassemblycompanyattribute"></a>GenerateAssemblyCompanyAttribute

此属性控制 `Company` 属性是否为程序集生成 <xref:System.Reflection.AssemblyCompanyAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateAssemblyCompanyAttribute>false</GenerateAssemblyCompanyAttribute>
</PropertyGroup>
```

### <a name="generateassemblyconfigurationattribute"></a>GenerateAssemblyConfigurationAttribute

此属性控制 `Configuration` 属性是否为程序集生成 <xref:System.Reflection.AssemblyConfigurationAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateAssemblyConfigurationAttribute>false</GenerateAssemblyConfigurationAttribute>
</PropertyGroup>
```

### <a name="generateassemblycopyrightattribute"></a>GenerateAssemblyCopyrightAttribute

此属性控制 `Copyright` 属性是否为程序集生成 <xref:System.Reflection.AssemblyCopyrightAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateAssemblyCopyrightAttribute>false</GenerateAssemblyCopyrightAttribute>
</PropertyGroup>
```

### <a name="generateassemblydescriptionattribute"></a>GenerateAssemblyDescriptionAttribute

此属性控制 `Description` 属性是否为程序集生成 <xref:System.Reflection.AssemblyDescriptionAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateAssemblyDescriptionAttribute>false</GenerateAssemblyDescriptionAttribute>
</PropertyGroup>
```

### <a name="generateassemblyfileversionattribute"></a>GenerateAssemblyFileVersionAttribute

此属性控制 `FileVersion` 属性是否为程序集生成 <xref:System.Reflection.AssemblyFileVersionAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateAssemblyFileVersionAttribute>false</GenerateAssemblyFileVersionAttribute>
</PropertyGroup>
```

### <a name="generateassemblyinfo"></a>GenerateAssemblyInfo

控制项目的 `AssemblyInfo` 属性生成。 默认值为 `true`。 使用 `false` 禁用文件生成：

```xml
<PropertyGroup>
  <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
</PropertyGroup>
```

[GeneratedAssemblyInfoFile](#generatedassemblyinfofile) 设置控制生成的文件的名称。

当 `GenerateAssemblyInfo` 值为 `true` 时，项目属性将转换为 `AssemblyInfo` 属性。 下表列出了用于生成特性的项目属性，以及可禁用该生成的属性：

| 属性               | 属性                                                      | 要禁用的属性                                                                               |
|------------------------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `Company`              | <xref:System.Reflection.AssemblyCompanyAttribute>              | [`GenerateAssemblyCompanyAttribute`](#generateassemblycompanyattribute)                           |
| `Configuration`        | <xref:System.Reflection.AssemblyConfigurationAttribute>        | [`GenerateAssemblyConfigurationAttribute`](#generateassemblyconfigurationattribute)               |
| `Copyright`            | <xref:System.Reflection.AssemblyCopyrightAttribute>            | [`GenerateAssemblyCopyrightAttribute`](#generateassemblycopyrightattribute)                       |
| `Description`          | <xref:System.Reflection.AssemblyDescriptionAttribute>          | [`GenerateAssemblyDescriptionAttribute`](#generateassemblydescriptionattribute)                   |
| `FileVersion`          | <xref:System.Reflection.AssemblyFileVersionAttribute>          | [`GenerateAssemblyFileVersionAttribute`](#generateassemblyfileversionattribute)                   |
| `InformationalVersion` | <xref:System.Reflection.AssemblyInformationalVersionAttribute> | [`GenerateAssemblyInformationalVersionAttribute`](#generateassemblyinformationalversionattribute) |
| `Product`              | <xref:System.Reflection.AssemblyProductAttribute>              | [`GenerateAssemblyProductAttribute`](#generateassemblyproductattribute)                           |
| `AssemblyTitle`        | <xref:System.Reflection.AssemblyTitleAttribute>                | [`GenerateAssemblyTitleAttribute`](#generateassemblytitleattribute)                               |
| `AssemblyVersion`      | <xref:System.Reflection.AssemblyVersionAttribute>              | [`GenerateAssemblyVersionAttribute`](#generateassemblyversionattribute)                           |
| `NeutralLanguage`      | <xref:System.Resources.NeutralResourcesLanguageAttribute>      | [`GenerateNeutralResourcesLanguageAttribute`](#generateneutralresourceslanguageattribute)         |

有关这些设置的说明：

- `AssemblyVersion` 和 `FileVersion` 默认采用 `$(Version)` 的值而不带后缀。 例如，如果 `$(Version)` 为 `1.2.3-beta.4`，则值将为 `1.2.3`。
- `InformationalVersion` 默认是 `$(Version)` 的值。
- 如果存在 `$(SourceRevisionId)` 属性，则该属性会附加到 `InformationalVersion`。 可以使用 `IncludeSourceRevisionInInformationalVersion` 禁用此行为。
- `Copyright` 和 `Description` 属性也可用于 NuGet 元数据。
- `Configuration`（默认值为 `Debug`）由所有 MSBuild 目标共享。 可以通过 `dotnet` 命令的 `--configuration` 选项对其进行设置，例如 [dotnet pack](../tools/dotnet-pack.md)。
- 创建 NuGet 包时会使用某些属性。 有关详细信息，请参阅[包属性](#package-properties)。

#### <a name="migrating-from-net-framework"></a>从 .NET Framework 迁移

.NET Framework 项目模板会创建一个包含这些程序集信息属性集的代码文件。 该文件通常位于 .\Properties\AssemblyInfo.cs 或 .\Properties\AssemblyInfo.vb 。 SDK 样式项目基于项目设置为你生成此文件。 你不能同时具有这两者。 将代码移植到 .NET 5（和 .NET Core 3.1）或更高版本时，请执行以下操作之一：

- 通过将 `GenerateAssemblyInfo` 设置为 `false` 来禁用包含程序集信息特性的临时代码文件的生成。 这使你可以保留 AssemblyInfo 文件。
- 将 `AssemblyInfo` 文件中的设置迁移到项目文件，并删除该 `AssemblyInfo` 文件。

### <a name="generateassemblyinformationalversionattribute"></a>GenerateAssemblyInformationalVersionAttribute

此属性控制 `InformationalVersion` 属性是否为程序集生成 <xref:System.Reflection.AssemblyInformationalVersionAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateAssemblyInformationalVersionAttribute>false</GenerateAssemblyInformationalVersionAttribute>
</PropertyGroup>
```

### <a name="generateassemblyproductattribute"></a>GenerateAssemblyProductAttribute

此属性控制 `Product` 属性是否为程序集生成 <xref:System.Reflection.AssemblyProductAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateAssemblyProductAttribute>false</GenerateAssemblyProductAttribute>
</PropertyGroup>
```

### <a name="generateassemblytitleattribute"></a>GenerateAssemblyTitleAttribute

此属性控制 `AssemblyTitle` 属性是否为程序集生成 <xref:System.Reflection.AssemblyTitleAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateAssemblyTitleAttribute>false</GenerateAssemblyTitleAttribute>
</PropertyGroup>
```

### <a name="generateassemblyversionattribute"></a>GenerateAssemblyVersionAttribute

此属性控制 `AssemblyVersion` 属性是否为程序集生成 <xref:System.Reflection.AssemblyVersionAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateAssemblyVersionAttribute>false</GenerateAssemblyVersionAttribute>
</PropertyGroup>
```

### <a name="generatedassemblyinfofile"></a>GeneratedAssemblyInfoFile

此属性定义生成的程序集信息文件的相对或绝对路径。 默认为 `$(IntermediateOutputPath)`（通常为 obj）目录中名为 [project-name].AssemblyInfo.[cs|vb] 的文件 。

```xml
<PropertyGroup>
  <GeneratedAssemblyInfoFile>assemblyinfo.cs</GeneratedAssemblyInfoFile>
</PropertyGroup>
```

### <a name="generateneutralresourceslanguageattribute"></a>GenerateNeutralResourcesLanguageAttribute

此属性控制 `NeutralLanguage` 属性是否为程序集生成 <xref:System.Resources.NeutralResourcesLanguageAttribute>。 默认值为 `true`。

```xml
<PropertyGroup>
  <GenerateNeutralResourcesLanguageAttribute>false</GenerateNeutralResourcesLanguageAttribute>
</PropertyGroup>
```

## <a name="package-properties"></a>包属性

可以指定 `PackageId`、`PackageVersion`、`PackageIcon`、`Title` 和 `Description` 等属性来描述通过项目创建的包。 若要了解这些属性和其他属性，请参阅[包目标](/nuget/reference/msbuild-targets#pack-target)。

```xml
<PropertyGroup>
  ...
  <PackageId>ClassLibDotNetStandard</PackageId>
  <Version>1.0.0</Version>
  <Authors>John Doe</Authors>
  <Company>Contoso</Company>
</PropertyGroup>
```

## <a name="publish-related-properties"></a>与发布相关的属性

本节收录了以下 MSBuild 属性：

- [AppendRuntimeIdentifierToOutputPath](#appendruntimeidentifiertooutputpath)
- [AppendTargetFrameworkToOutputPath](#appendtargetframeworktooutputpath)
- [CopyLocalLockFileAssemblies](#copylocallockfileassemblies)
- [PreserveCompilationContext](#preservecompilationcontext)
- [PreserveCompilationReferences](#preservecompilationreferences)
- [RuntimeIdentifier](#runtimeidentifier)
- [RuntimeIdentifiers](#runtimeidentifiers)
- [UseAppHost](#useapphost)

### <a name="appendtargetframeworktooutputpath"></a>AppendTargetFrameworkToOutputPath

`AppendTargetFrameworkToOutputPath` 属性控制是否将[目标框架名字对象 (TFM)](../../standard/frameworks.md) 追加到输出路径（由 [OutputPath](/visualstudio/msbuild/common-msbuild-project-properties#list-of-common-properties-and-parameters) 定义）。 .NET SDK 会自动将目标框架以及运行时标识符（如果有）追加到输出路径。 将 `AppendTargetFrameworkToOutputPath` 设置为 `false` 可防止将 TFM 追加到输出路径。 但是，如果输出路径中没有 TFM，则可能会发生多个生成项目相互覆盖的情况。

例如，对于 .NET 5.0 应用，输出路径将从 `bin\Debug\net5.0` 更改为 `bin\Debug`，并具有以下设置：

```xml
<PropertyGroup>
  <AppendTargetFrameworkToOutputPath>false</AppendTargetFrameworkToOutputPath>
</PropertyGroup>
```

### <a name="appendruntimeidentifiertooutputpath"></a>AppendRuntimeIdentifierToOutputPath

`AppendRuntimeIdentifierToOutputPath` 属性控制是否将[运行时标识符 (RID)](../rid-catalog.md) 追加到输出路径。 .NET SDK 会自动将目标框架以及运行时标识符（如果有）追加到输出路径。 将 `AppendRuntimeIdentifierToOutputPath` 设置为 `false` 可防止将 RID 追加到输出路径。

例如，对于 .NET 5.0 应用和 RID `win10-x64`，输出路径将从 `bin\Debug\net5.0\win10-x64` 更改为 `bin\Debug\net5.0`，并具有以下设置：

```xml
<PropertyGroup>
  <AppendRuntimeIdentifierToOutputPath>false</AppendRuntimeIdentifierToOutputPath>
</PropertyGroup>
```

### <a name="copylocallockfileassemblies"></a>CopyLocalLockFileAssemblies

对于依赖于其他库的插件项目，`CopyLocalLockFileAssemblies` 属性非常有用。 如果将此属性设置为 `true`，系统会将所有 NuGet 包依赖项复制到输出目录。 这意味着，可以使用 `dotnet build` 的输出在任何计算机上运行插件。

```xml
<PropertyGroup>
  <CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>
</PropertyGroup>
```

> [!TIP]
> 或者，可以使用 `dotnet publish` 发布类库。 有关详细信息，请查看 [dotnet publish](../tools/dotnet-publish.md)。

### <a name="preservecompilationcontext"></a>PreserveCompilationContext

`PreserveCompilationContext` 属性允许已构建或已发布的应用程序在运行时使用与构建时使用的相同设置来编译更多代码。 在构建时引用的程序集将被复制到输出目录的 ref 子目录中。 引用程序集的名称与传递给编译器的选项一起存储在应用程序的 .deps.json 文件中。 可使用 <xref:Microsoft.Extensions.DependencyModel.DependencyContext.CompileLibraries?displayProperty=nameWithType> 和 <xref:Microsoft.Extensions.DependencyModel.DependencyContext.CompilationOptions?displayProperty=nameWithType> 属性检索此信息。

此功能主要由 ASP.NET Core MVC 和 Razor Pages 在内部使用，以支持 Razor文件的运行时编译。

```xml
<PropertyGroup>
  <PreserveCompilationContext>true</PreserveCompilationContext>
</PropertyGroup>
```

### <a name="preservecompilationreferences"></a>PreserveCompilationReferences

`PreserveCompilationReferences` 属性与 [PreserveCompilationContext](#preservecompilationcontext) 属性类似，只不过它仅将引用的程序集复制到发布目录，而不是 .deps.json 文件。

```xml
<PropertyGroup>
  <PreserveCompilationReferences>true</PreserveCompilationReferences>
</PropertyGroup>
```

有关详细信息，请参阅 [Razor SDK 属性](/aspnet/core/razor-pages/sdk#properties)。

### <a name="runtimeidentifier"></a>RuntimeIdentifier

`RuntimeIdentifier` 属性可用于指定项目的单个[运行时标识符 (RID)](../rid-catalog.md)。 RID 支持发布独立部署。

```xml
<PropertyGroup>
  <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
</PropertyGroup>
```

### <a name="runtimeidentifiers"></a>RuntimeIdentifiers

`RuntimeIdentifiers` 属性可用于指定项目的[运行时标识符 (RID)](../rid-catalog.md) 的列表（以分号分隔）。 如果需要为多个运行时发布，请使用此属性。 `RuntimeIdentifiers` 在还原时使用，以确保图中有适当的资产。

> [!TIP]
> `RuntimeIdentifier`（单数）可以在只需要一个运行时时提供更快的生成。

```xml
<PropertyGroup>
  <RuntimeIdentifiers>win10-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
</PropertyGroup>
```

### <a name="useapphost"></a>UseAppHost

`UseAppHost` 属性控制是否为部署创建本机可执行文件。 自包含部署需要本机可执行文件。

在 .NET Core3.0 及更高版本中，默认情况下会创建依赖于框架的可执行文件。 将 `UseAppHost` 属性设置为 `false` 可禁用可执行文件的生成。

```xml
<PropertyGroup>
  <UseAppHost>false</UseAppHost>
</PropertyGroup>
```

有关部署的详细信息，请参阅 [.NET 应用程序部署](../deploying/index.md)。

## <a name="compilation-related-properties"></a>与编译相关的属性

本节收录了以下 MSBuild 属性：

- [EmbeddedResourceUseDependentUponConvention](#embeddedresourceusedependentuponconvention)
- [LangVersion](#langversion)

### <a name="embeddedresourceusedependentuponconvention"></a>EmbeddedResourceUseDependentUponConvention

`EmbeddedResourceUseDependentUponConvention` 属性定义了是否从与资源文件并置的源文件中的类型信息生成资源清单文件名。 例如，如果 Form1.resx 与 Form1.cs 位于同一文件夹中，并且 `EmbeddedResourceUseDependentUponConvention` 设置为 `true`，则生成的 .resources 文件将采用 Form1.cs 中定义的第一个类型的名称作为其文件名。 例如，如果 Form1.cs 中定义的第一个类型为 `MyNamespace.Form1`，则生成的文件名为 MyNamespace.Form1.resources。

> [!NOTE]
> 如果为 `EmbeddedResource` 项指定 `LogicalName`、`ManifestResourceName` 或 `DependentUpon` 元数据，则为该资源文件生成的清单文件名将改为基于该元数据。

默认情况下，在新的 .NET 项目中，此属性设置为 `true`。 如果设置为 `false`，并且没有为项目文件中的 `EmbeddedResource` 项指定 `LogicalName`、`ManifestResourceName` 或 `DependentUpon` 元数据，则资源清单文件名将基于项目的根命名空间和 .resx 文件的相对文件路径。 有关详细信息，请参阅[资源清单文件的命名](../resources/manifest-file-names.md)。

```xml
<PropertyGroup>
  <EmbeddedResourceUseDependentUponConvention>true</EmbeddedResourceUseDependentUponConvention>
</PropertyGroup>
```

### <a name="langversion"></a>LangVersion

`LangVersion` 属性可用于指定特定的编程语言版本。 例如，如果要访问 C# 预览功能，请将 `LangVersion` 设置为 `preview`。

```xml
<PropertyGroup>
  <LangVersion>preview</LangVersion>
</PropertyGroup>
```

有关详细信息，请参阅 [C# 语言版本控制](../../csharp/language-reference/configure-language-version.md#override-a-default)。

## <a name="default-item-inclusion-properties"></a>默认项包含属性

本节收录了以下 MSBuild 属性：

- [DefaultExcludesInProjectFolder](#defaultexcludesinprojectfolder)
- [DefaultItemExcludes](#defaultitemexcludes)
- [EnableDefaultCompileItems](#enabledefaultcompileitems)
- [EnableDefaultEmbeddedResourceItems](#enabledefaultembeddedresourceitems)
- [EnableDefaultItems](#enabledefaultitems)
- [EnableDefaultNoneItems](#enabledefaultnoneitems)

有关详细信息，请参阅[默认的包括和排除](overview.md#default-includes-and-excludes)。

### <a name="defaultitemexcludes"></a>DefaultItemExcludes

使用 `DefaultItemExcludes` 属性定义需从“包括”、“排除”和“删除”glob 中排除的文件和文件夹的 glob 模式。 默认情况下从 glob 模式中排除 ./bin 和 ./obj 文件夹 。

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);**/*.myextension</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="defaultexcludesinprojectfolder"></a>DefaultExcludesInProjectFolder

使用 `DefaultExcludesInProjectFolder` 属性定义项目文件夹中需要从“包括”、“排除”和“删除”glob 中排除的文件和文件夹的 glob 模式。 默认情况下，从 glob 模式中排除以句点 (`.`) 开头的文件夹，如 .git 和 .vs。

此属性与 `DefaultItemExcludes` 属性非常相似，不同之处在于它只涉及项目文件夹中的文件和文件夹。 如果 glob 模式会无意中将项目文件夹外部的项与相对路径进行匹配，请使用 `DefaultExcludesInProjectFolder` 属性，而不是 `DefaultItemExcludes` 属性。

```xml
<PropertyGroup>
  <DefaultExcludesInProjectFolder>$(DefaultExcludesInProjectFolder);**/myprefix*/**</DefaultExcludesInProjectFolder>
</PropertyGroup>
```

### <a name="enabledefaultitems"></a>EnableDefaultItems

`EnableDefaultItems` 属性控制是否在项目中隐式包含编译项、嵌入的资源项和 `None` 项。 默认值为 `true`。 若要禁用所有隐式文件包含，请将 `EnableDefaultItems` 属性设置为 `false`。

```xml
<PropertyGroup>
  <EnableDefaultItems>false</EnableDefaultItems>
</PropertyGroup>
```

### <a name="enabledefaultcompileitems"></a>EnableDefaultCompileItems

`EnableDefaultCompileItems` 属性控制是否在项目中隐式包含编译项。 默认值为 `true`。 将 `EnableDefaultCompileItems` 属性设置为 `false` 以禁用 * .cs 和其他语言扩展文件的隐式包含。

```xml
<PropertyGroup>
  <EnableDefaultCompileItems>false</EnableDefaultCompileItems>
</PropertyGroup>
```

### <a name="enabledefaultembeddedresourceitems"></a>EnableDefaultEmbeddedResourceItems

`EnableDefaultEmbeddedResourceItems` 属性控制是否在项目中隐式包含嵌入的资源项。 默认值为 `true`。 将 `EnableDefaultEmbeddedResourceItems` 属性设置为 `false` 以禁用嵌入的资源文件的隐式包含。

```xml
<PropertyGroup>
  <EnableDefaultEmbeddedResourceItems>false</EnableDefaultEmbeddedResourceItems>
</PropertyGroup>
```

### <a name="enabledefaultnoneitems"></a>EnableDefaultNoneItems

`EnableDefaultNoneItems` 属性控制是否在项目中隐式包含 `None` 项（生成过程中未赋予角色的文件）。 默认值为 `true`。 将 `EnableDefaultNoneItems` 属性设置为 `false` 以禁用 `None` 项的隐式包含。

```xml
<PropertyGroup>
  <EnableDefaultNoneItems>false</EnableDefaultNoneItems>
</PropertyGroup>
```

## <a name="code-analysis-properties"></a>代码分析属性

本节收录了以下 MSBuild 属性：

- [AnalysisLevel](#analysislevel)
- [AnalysisMode](#analysismode)
- [CodeAnalysisTreatWarningsAsErrors](#codeanalysistreatwarningsaserrors)
- [EnableNETAnalyzers](#enablenetanalyzers)
- [EnforceCodeStyleInBuild](#enforcecodestyleinbuild)

### <a name="analysislevel"></a>AnalysisLevel

`AnalysisLevel` 属性可指定代码分析级别。 例如，如果要访问预览代码分析器，请将 `AnalysisLevel` 设置为 `preview`。

默认值：

- 如果你的项目以 .NET 5.0 或更高版本为目标，或你添加了 [AnalysisMode](#analysismode) 属性，则默认值为 `latest`。
- 否则，此属性被省略，除非你将它明确添加到项目文件中。

```xml
<PropertyGroup>
  <AnalysisLevel>preview</AnalysisLevel>
</PropertyGroup>
```

下表显示可用的选项。

| 值 | 含义 |
|-|-|
| `latest` | 使用已发布的最新版代码分析器。 这是默认值。 |
| `preview` | 使用最新的代码分析器（即使它们处于预览状态）。 |
| `5.0` | 即使有较新的规则可用，也会使用为 .NET 5.0 版本启用的规则集。 |
| `5` | 即使有较新的规则可用，也会使用为 .NET 5.0 版本启用的规则集。 |

> [!NOTE]
> 此属性对未引用[项目 SDK](overview.md) 的项目（例如，引用 Microsoft.CodeAnalysis.NetAnalyzers NuGet 包的旧版 .NET Framework 项目）中的代码分析没有影响。

### <a name="analysismode"></a>AnalysisMode

从 .NET 5.0 开始，.NET SDK 附带了所有[“CA”代码质量规则](../../fundamentals/code-analysis/quality-rules/index.md)。 默认情况下，只有[一些规则作为生成警告启用](../../fundamentals/code-analysis/overview.md#enabled-rules)。 `AnalysisMode` 属性允许自定义默认启用的一组规则。 可以切换到更主动的（选择退出）分析模式，也可以切换到更保守的（选择加入）分析模式。 例如，如果要作为生成警告默认启用所有规则，请将值设置为 `AllEnabledByDefault`。

```xml
<PropertyGroup>
  <AnalysisMode>AllEnabledByDefault</AnalysisMode>
</PropertyGroup>
```

下表显示可用的选项。

| 值 | 含义 |
|-|-|
| `Default` | 默认模式，其中某些规则作为生成警告启用，某些规则作为 Visual Studio IDE 建议启用，其余规则被禁用。 |
| `AllEnabledByDefault` | 主动或选择退出模式，默认情况下所有规则都作为生成警告启用。 可以选择[选择退出](../../fundamentals/code-analysis/configuration-options.md)各条规则，以禁用它们。 |
| `AllDisabledByDefault` | 保守或选择加入模式，默认情况下所有规则都处于禁用状态。 可以选择[选择加入](../../fundamentals/code-analysis/configuration-options.md)各条规则，以启用它们。 |

> [!NOTE]
> 此属性对未引用[项目 SDK](overview.md) 的项目（例如，引用 Microsoft.CodeAnalysis.NetAnalyzers NuGet 包的旧版 .NET Framework 项目）中的代码分析没有影响。

### <a name="codeanalysistreatwarningsaserrors"></a>CodeAnalysisTreatWarningsAsErrors

`CodeAnalysisTreatWarningsAsErrors` 属性可配置是否应将代码质量分析警告 (CAxxxx) 视为警告并中断生成。 如果在生成项目时使用 `-warnaserror` 标志，则 [.NET 代码质量分析](../../fundamentals/code-analysis/overview.md#code-quality-analysis)警告也会被视为错误。 如果不希望将代码质量分析警告视为错误，可以在项目文件中将 `CodeAnalysisTreatWarningsAsErrors` MSBuild 属性设置为 `false`。

```xml
<PropertyGroup>
  <CodeAnalysisTreatWarningsAsErrors>false</CodeAnalysisTreatWarningsAsErrors>
</PropertyGroup>
```

### <a name="enablenetanalyzers"></a>EnableNETAnalyzers

默认情况下，为面向 .NET 5.0 或更高版本的项目启用 [.NET 代码质量分析](../../fundamentals/code-analysis/overview.md#code-quality-analysis)。 可通过将 `EnableNETAnalyzers` 属性设置为 `true`，来为面向 .NET 早期版本的 SDK 样式项目启用 .NET 代码分析。 若要禁用任何项目中的代码分析，可将此属性设置为 `false`。

```xml
<PropertyGroup>
  <EnableNETAnalyzers>true</EnableNETAnalyzers>
</PropertyGroup>
```

> [!NOTE]
> 此属性专门用于 .NET 5 及更高版本 SDK 中的内置分析器。 安装 NuGet 代码分析包时不应使用此属性。

### <a name="enforcecodestyleinbuild"></a>EnforceCodeStyleInBuild

> [!NOTE]
> 此功能当前为实验性功能，可能会在 .NET 5 和 .NET 6 版本之间发生更改。

对于所有 .NET 项目的版本，[.NET 代码样式分析](../../fundamentals/code-analysis/overview.md#code-style-analysis)默认处于禁用状态。 通过将 `EnforceCodeStyleInBuild` 属性设置为 `true`，可以为 .NET 项目启用代码样式分析。

```xml
<PropertyGroup>
  <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
</PropertyGroup>
```

生成和报告违规时将执行[配置](../../fundamentals/code-analysis/overview.md#code-style-analysis)为警告或错误的所有代码样式规则。

## <a name="run-time-configuration-properties"></a>运行时配置属性

可以通过在应用的项目文件中指定 MSBuild 属性来配置某些运行时行为。 有关配置运行时行为的其他方法的信息，请参阅[运行时配置设置](../run-time-config/index.md)。

- [ConcurrentGarbageCollection](#concurrentgarbagecollection)
- [InvariantGlobalization](#invariantglobalization)
- [RetainVMGarbageCollection](#retainvmgarbagecollection)
- [ServerGarbageCollection](#servergarbagecollection)
- [ThreadPoolMaxThreads](#threadpoolmaxthreads)
- [ThreadPoolMinThreads](#threadpoolminthreads)
- [TieredCompilation](#tieredcompilation)
- [TieredCompilationQuickJit](#tieredcompilationquickjit)
- [TieredCompilationQuickJitForLoops](#tieredcompilationquickjitforloops)

### <a name="concurrentgarbagecollection"></a>ConcurrentGarbageCollection

`ConcurrentGarbageCollection` 属性配置是否启用 [后台（并发）垃圾回收](../../standard/garbage-collection/background-gc.md)。 将值设置为 `false` 以禁用后台垃圾回收。 有关详细信息，请参阅[后台 GC](../run-time-config/garbage-collector.md#background-gc)。

```xml
<PropertyGroup>
  <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
</PropertyGroup>
```

### <a name="invariantglobalization"></a>InvariantGlobalization

`InvariantGlobalization` 属性配置应用是否在全球化固定模式下运行，这意味着它无权访问特定于区域性的数据。 将值设置为 `true` 以在全球化固定模式下运行。 有关详细信息，请参阅[固定模式](../run-time-config/globalization.md#invariant-mode)。

```xml
<PropertyGroup>
  <InvariantGlobalization>true</InvariantGlobalization>
</PropertyGroup>
```

### <a name="retainvmgarbagecollection"></a>RetainVMGarbageCollection

`RetainVMGarbageCollection` 属性配置垃圾回收器，以将已删除的内存段放置在备用列表上供将来使用或释放它们。 将值设置为 `true` 会告知垃圾回收器将段放在备用列表上。 有关详细信息，请参阅[保留 VM](../run-time-config/garbage-collector.md#retain-vm)。

```xml
<PropertyGroup>
  <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
</PropertyGroup>
```

### <a name="servergarbagecollection"></a>ServerGarbageCollection

`ServerGarbageCollection` 属性配置应用程序是使用[工作站垃圾回收还是服务器垃圾回收](../../standard/garbage-collection/workstation-server-gc.md)。 将值设置为 `true` 以使用服务器垃圾回收。 有关详细信息，请参阅[工作站与服务器](../run-time-config/garbage-collector.md#workstation-vs-server)。

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

### <a name="threadpoolmaxthreads"></a>ThreadPoolMaxThreads

`ThreadPoolMaxThreads` 属性配置工作线程池的最大线程数。 有关详细信息，请参阅[最大线程数](../run-time-config/threading.md#maximum-threads)。

```xml
<PropertyGroup>
  <ThreadPoolMaxThreads>20</ThreadPoolMaxThreads>
</PropertyGroup>
```

### <a name="threadpoolminthreads"></a>ThreadPoolMinThreads

`ThreadPoolMinThreads` 属性配置工作线程池的最小线程数。 有关详细信息，请参阅[最小线程数](../run-time-config/threading.md#minimum-threads)。

```xml
<PropertyGroup>
  <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
</PropertyGroup>
```

### <a name="tieredcompilation"></a>TieredCompilation

`TieredCompilation` 属性配置实时 (JIT) 编译器是否使用[分层编译](../whats-new/dotnet-core-3-0.md#tiered-compilation)。 将值设置为 `false` 以禁用分层编译。 有关详细信息，请参阅[分层编译](../run-time-config/compilation.md#tiered-compilation)。

```xml
<PropertyGroup>
  <TieredCompilation>false</TieredCompilation>
</PropertyGroup>
```

### <a name="tieredcompilationquickjit"></a>TieredCompilationQuickJit

`TieredCompilationQuickJit` 属性配置 JIT 编译器是否使用快速 JIT。 将值设置为 `false` 以禁用快速 JIT。 有关详细信息，请参阅[快速 JIT](../run-time-config/compilation.md#quick-jit)。

```xml
<PropertyGroup>
  <TieredCompilationQuickJit>false</TieredCompilationQuickJit>
</PropertyGroup>
```

### <a name="tieredcompilationquickjitforloops"></a>TieredCompilationQuickJitForLoops

`TieredCompilationQuickJitForLoops` 配置 JIT 编译器是否对包含循环的方法使用快速 JIT。 将值设置为 `true` 以对包含循环的方法启用快速 JIT。 有关详细信息，请参阅[适用于循环的快速 JIT](../run-time-config/compilation.md#quick-jit-for-loops)。

```xml
<PropertyGroup>
  <TieredCompilationQuickJitForLoops>true</TieredCompilationQuickJitForLoops>
</PropertyGroup>
```

## <a name="reference-properties"></a>引用属性

本节收录了以下 MSBuild 属性：

- [AssetTargetFallback](#assettargetfallback)
- [DisableImplicitFrameworkReferences](#disableimplicitframeworkreferences)
- [与还原相关的属性](#restore-related-properties)

### <a name="assettargetfallback"></a>AssetTargetFallback

使用 `AssetTargetFallback` 属性，可以为项目引用和 NuGet 包指定其他兼容的框架版本。 例如，如果使用 `PackageReference` 指定包依赖项，但该包不包含与项目的 `TargetFramework` 兼容的资源，则可使用 `AssetTargetFallback` 属性。 使用 `AssetTargetFallback` 中指定的每个目标框架重新检查引用包的兼容性。 此属性替换已弃用的属性 `PackageTargetFallback`。

可以将 `AssetTargetFallback` 属性设置为一个或多个[目标框架版本](../../standard/frameworks.md#supported-target-frameworks)。

```xml
<PropertyGroup>
  <AssetTargetFallback>net461</AssetTargetFallback>
</PropertyGroup>
```

### <a name="disableimplicitframeworkreferences"></a>DisableImplicitFrameworkReferences

面向 .NET Core 3.0 及更高版本时，`DisableImplicitFrameworkReferences` 属性会控制隐式 `FrameworkReference` 项。 面向 .NET Core 2.1 或 .NET Standard 2.0 及早期版本时，该属性会控制元包中包的隐式 [PackageReference](#packagereference) 项。 （元包是一种基于框架的包，其中仅包含对其他包的依赖项。）在面向 .NET Framework 时，此属性还控制隐式引用，如 `System` 和 `System.Core`。

将此属性设置为 `true` 以禁用隐式 `FrameworkReference` 或 [PackageReference](#packagereference) 项。 如果将此属性设置为 `true`，则可以仅添加对所需框架或包的显式引用。

```xml
<PropertyGroup>
  <DisableImplicitFrameworkReferences>true</DisableImplicitFrameworkReferences>
</PropertyGroup>
```

### <a name="restore-related-properties"></a>与还原相关的属性

还原被引用的包会安装它的所有直接依赖项，以及这些依赖项的全部依赖项。 可以通过指定 `RestorePackagesPath` 和 `RestoreIgnoreFailedSources` 等属性来自定义包还原。 若要详细了解这些属性和其他属性，请参阅[还原目标](/nuget/reference/msbuild-targets#restore-target)。

```xml
<PropertyGroup>
  <RestoreIgnoreFailedSource>true</RestoreIgnoreFailedSource>
</PropertyGroup>
```

## <a name="run-related-properties"></a>与运行相关的属性

以下属性用于使用 [`dotnet run`](../tools/dotnet-run.md) 命令启动应用：

- [RunArguments](#runarguments)
- [RunWorkingDirectory](#runworkingdirectory)

### <a name="runarguments"></a>RunArguments

`RunArguments` 属性定义了在应用运行时向其传递的参数。

```xml
<PropertyGroup>
  <RunArguments>-mode dryrun</RunArguments>
</PropertyGroup>
```

> [!TIP]
> 可以使用 [`dotnet run` 的 `--` 选项](../tools/dotnet-run.md#options)来指定要传递到应用的其他参数。

### <a name="runworkingdirectory"></a>RunWorkingDirectory

`RunWorkingDirectory` 属性定义要用于启动应用程序进程的工作目录。 它可以是绝对路径，也可以是相对于项目目录的路径。 如果未指定目录，`OutDir` 将用作工作目录。

```xml
<PropertyGroup>
  <RunWorkingDirectory>c:\temp</RunWorkingDirectory>
</PropertyGroup>
```

## <a name="hosting-related-properties"></a>与托管相关的属性

本节收录了以下 MSBuild 属性：

- [EnableComHosting](#enablecomhosting)
- [EnableDynamicLoading](#enabledynamicloading)

### <a name="enablecomhosting"></a>EnableComHosting

`EnableComHosting` 属性表示程序集提供了 COM 服务器。 将 `EnableComHosting` 设置为 `true` 也表明 [EnableDynamicLoading](#enabledynamicloading) 为 `true`。

```xml
<PropertyGroup>
  <EnableComHosting>True</EnableComHosting>
</PropertyGroup>
```

有关详细信息，请参阅[向 COM 公开 .NET 组件](../native-interop/expose-components-to-com.md)。

### <a name="enabledynamicloading"></a>EnableDynamicLoading

`EnableDynamicLoading` 属性指示程序集是动态加载的组件。 组件可以是 [COM 库](/windows/win32/com/the-component-object-model)，也可以是[在本机主机中使用的](../tutorials/netcore-hosting.md)非 COM 库。 将此属性设置为 `true` 会产生以下结果：

- 生成 runtimeconfig.json 文件。
- [前滚](../whats-new/dotnet-core-3-0.md#major-version-runtime-roll-forward)设置为 `LatestMinor`。
- 在本地复制 NuGet 引用。

```xml
<PropertyGroup>
  <EnableDynamicLoading>true</EnableDynamicLoading>
</PropertyGroup>
```

## <a name="items"></a>项

[MSBuild 项](/visualstudio/msbuild/msbuild-items)是生成系统的输入。 根据项的类型（即元素名称）指定项。 例如，`Compile` 和 `Reference` 是两个[常见项类型](/visualstudio/msbuild/common-msbuild-project-items)。 .NET SDK 提供了以下附加项类型：

- [PackageReference](#packagereference)
- [TrimmerRootAssembly](#trimmerrootassembly)

你可以在这些项上使用任何标准的[项目属性](/visualstudio/msbuild/item-element-msbuild#attributes-and-elements)，例如 `Include` 和 `Update`。 使用 `Include` 添加新项，使用 `Update` 修改现有项。 例如，`Update` 通常用于修改由 .NET SDK 隐式添加的项。

### <a name="packagereference"></a>PackageReference

`PackageReference` 项定义了对 NuGet 包的引用。

`Include` 属性指定包 ID。 `Version` 特性指定版本或版本范围。 若要了解如何指定最低版本、最高版本、范围或完全匹配，请参阅[版本范围](/nuget/concepts/package-versioning#version-ranges)。

以下示例中的项目文件片段引用 [System.Runtime](https://www.nuget.org/packages/System.Runtime/) 包。

```xml
<ItemGroup>
  <PackageReference Include="System.Runtime" Version="4.3.0" />
</ItemGroup>
```

你还可以使用元数据（例如 `PrivateAssets`）来[控制依赖项资产](/nuget/consume-packages/package-references-in-project-files#controlling-dependency-assets)。

```xml
<ItemGroup>
  <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">
    <PrivateAssets>all</PrivateAssets>
  </PackageReference>
</ItemGroup>
```

有关详细信息，请参阅[项目文件中的包引用](/nuget/consume-packages/package-references-in-project-files)。

### <a name="trimmerrootassembly"></a>TrimmerRootAssembly

`TrimmerRootAssembly` 项允许通过[修整](../deploying/trim-self-contained.md)排除程序集。 修整是从打包的应用程序中删除运行时未使用部分的过程。 在某些情况下，修整可能会错误地删除所需的引用。

以下 XML 通过修整排除 `System.Security` 程序集。

```xml
<ItemGroup>
  <TrimmerRootAssembly Include="System.Security" />
</ItemGroup>
```

## <a name="item-metadata"></a>项元数据

除了标准的 [MSBuild 项目属性](/visualstudio/msbuild/item-element-msbuild#attributes-and-elements)之外，.net SDK 还提供以下项元数据标记：

- [CopyToPublishDirectory](#copytopublishdirectory)
- [LinkBase](#linkbase)

### <a name="copytopublishdirectory"></a>CopyToPublishDirectory

MSBuild 项上的 `CopyToPublishDirectory` 元数据控制何时将项复制到发布目录。 允许的值为 `PreserveNewest`（仅在项已更改时复制项）、`Always`（始终复制项）和 `Never`（从不复制项）。 从性能角度来看，`PreserveNewest` 更为可取，因为它可实现增量生成。

```xml
<ItemGroup>
  <None Update="appsettings.Development.json" CopyToOutputDirectory="PreserveNewest" CopyToPublishDirectory="PreserveNewest" />
</ItemGroup>
```

### <a name="linkbase"></a>LinkBase

对于项目目录及其子目录之外的项，发布目标使用项的[链接元数据](/visualstudio/msbuild/common-msbuild-item-metadata)来确定要将项复制到的位置。 `Link` 还将确定项目树外的项在 Visual Studio 的“解决方案资源管理器”窗口中的显示方式。

如果没有为项目圆锥之外的项指定 `Link`，则默认为 `%(LinkBase)\%(RecursiveDir)%(Filename)%(Extension)`。 通过 `LinkBase`，可以为项目圆锥之外的项指定一个合理的基础文件夹。 基础文件夹下的文件夹层次结构通过 `RecursiveDir` 保留。 如果未指定 `LinkBase`，则将从 `Link` 路径中省略它。

```xml
<ItemGroup>
  <Content Include="..\Extras\**\*.cs" LinkBase="Shared"/>
</ItemGroup>
```

下图显示通过上一个项 `Include` glob 包含的文件在解决方案资源管理器中的显示方式。

:::image type="content" source="media/solution-explorer-linkbase.png" alt-text="解决方案资源管理器，显示具有 LinkBase 元数据的项。":::

## <a name="see-also"></a>请参阅

- [MSBuild 架构引用](/visualstudio/msbuild/msbuild-project-file-schema-reference)
- [通用 MSBuild 属性](/visualstudio/msbuild/common-msbuild-project-properties)
- [NuGet 包的 MSBuild 属性](/nuget/reference/msbuild-targets#pack-target)
- [NuGet 还原的 MSBuild 属性](/nuget/reference/msbuild-targets#restore-properties)
- [自定义生成](/visualstudio/msbuild/customize-your-build)
