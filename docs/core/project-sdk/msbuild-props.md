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
# <a name="msbuild-reference-for-net-sdk-projects"></a><span data-ttu-id="04b33-103">.NET SDK 项目的 MSBuild 引用</span><span class="sxs-lookup"><span data-stu-id="04b33-103">MSBuild reference for .NET SDK projects</span></span>

<span data-ttu-id="04b33-104">此页是对可用于配置 .NET 项目的 MSBuild 属性和项的引用。</span><span class="sxs-lookup"><span data-stu-id="04b33-104">This page is a reference for the MSBuild properties and items that you can use to configure .NET projects.</span></span>

> [!NOTE]
> <span data-ttu-id="04b33-105">此页面正在运行中，未列出 .NET SDK 的所有有用的 MSBuild 属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-105">This page is a work in progress and does not list all of the useful MSBuild properties for the .NET SDK.</span></span> <span data-ttu-id="04b33-106">有关通用 MSBuild 属性的列表，请参阅[通用 MSBuild 属性](/visualstudio/msbuild/common-msbuild-project-properties)。</span><span class="sxs-lookup"><span data-stu-id="04b33-106">For a list of common MSBuild properties, see [Common MSBuild properties](/visualstudio/msbuild/common-msbuild-project-properties).</span></span>

## <a name="framework-properties"></a><span data-ttu-id="04b33-107">框架属性</span><span class="sxs-lookup"><span data-stu-id="04b33-107">Framework properties</span></span>

<span data-ttu-id="04b33-108">本节收录了以下 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="04b33-108">The following MSBuild properties are documented in this section:</span></span>

- [<span data-ttu-id="04b33-109">TargetFramework</span><span class="sxs-lookup"><span data-stu-id="04b33-109">TargetFramework</span></span>](#targetframework)
- [<span data-ttu-id="04b33-110">TargetFrameworks</span><span class="sxs-lookup"><span data-stu-id="04b33-110">TargetFrameworks</span></span>](#targetframeworks)
- [<span data-ttu-id="04b33-111">NetStandardImplicitPackageVersion</span><span class="sxs-lookup"><span data-stu-id="04b33-111">NetStandardImplicitPackageVersion</span></span>](#netstandardimplicitpackageversion)

### <a name="targetframework"></a><span data-ttu-id="04b33-112">TargetFramework</span><span class="sxs-lookup"><span data-stu-id="04b33-112">TargetFramework</span></span>

<span data-ttu-id="04b33-113">`TargetFramework` 属性指定应用的目标框架版本。</span><span class="sxs-lookup"><span data-stu-id="04b33-113">The `TargetFramework` property specifies the target framework version for the app.</span></span> <span data-ttu-id="04b33-114">有关有效的目标框架名字对象的列表，请参阅 [SDK 样式项目中的目标框架](../../standard/frameworks.md#supported-target-frameworks)。</span><span class="sxs-lookup"><span data-stu-id="04b33-114">For a list of valid target framework monikers, see [Target frameworks in SDK-style projects](../../standard/frameworks.md#supported-target-frameworks).</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp3.1</TargetFramework>
</PropertyGroup>
```

<span data-ttu-id="04b33-115">有关详细信息，请参阅 [SDK 样式项目中的目标框架](../../standard/frameworks.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-115">For more information, see [Target frameworks in SDK-style projects](../../standard/frameworks.md).</span></span>

### <a name="targetframeworks"></a><span data-ttu-id="04b33-116">TargetFrameworks</span><span class="sxs-lookup"><span data-stu-id="04b33-116">TargetFrameworks</span></span>

<span data-ttu-id="04b33-117">如果希望应用面向多个平台，请使用 `TargetFrameworks` 属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-117">Use the `TargetFrameworks` property when you want your app to target multiple platforms.</span></span> <span data-ttu-id="04b33-118">有关有效的目标框架名字对象的列表，请参阅 [SDK 样式项目中的目标框架](../../standard/frameworks.md#supported-target-frameworks)。</span><span class="sxs-lookup"><span data-stu-id="04b33-118">For a list of valid target framework monikers, see [Target frameworks in SDK-style projects](../../standard/frameworks.md#supported-target-frameworks).</span></span>

> [!NOTE]
> <span data-ttu-id="04b33-119">如果指定了 `TargetFramework`（单数），则忽略此属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-119">This property is ignored if `TargetFramework` (singular) is specified.</span></span>

```xml
<PropertyGroup>
  <TargetFrameworks>netcoreapp3.1;net462</TargetFrameworks>
</PropertyGroup>
```

<span data-ttu-id="04b33-120">有关详细信息，请参阅 [SDK 样式项目中的目标框架](../../standard/frameworks.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-120">For more information, see [Target frameworks in SDK-style projects](../../standard/frameworks.md).</span></span>

### <a name="netstandardimplicitpackageversion"></a><span data-ttu-id="04b33-121">NetStandardImplicitPackageVersion</span><span class="sxs-lookup"><span data-stu-id="04b33-121">NetStandardImplicitPackageVersion</span></span>

> [!NOTE]
> <span data-ttu-id="04b33-122">此属性仅适用于使用 `netstandard1.x` 的项目。</span><span class="sxs-lookup"><span data-stu-id="04b33-122">This property only applies to projects using `netstandard1.x`.</span></span> <span data-ttu-id="04b33-123">它不适用于使用 `netstandard2.x` 的项目。</span><span class="sxs-lookup"><span data-stu-id="04b33-123">It doesn't apply to projects that use `netstandard2.x`.</span></span>

<span data-ttu-id="04b33-124">如果要指定低于元包版本的框架版本，请使用 `NetStandardImplicitPackageVersion` 属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-124">Use the `NetStandardImplicitPackageVersion` property when you want to specify a framework version that's lower than the metapackage version.</span></span> <span data-ttu-id="04b33-125">以下示例中的项目文件以 `netstandard1.3` 为目标，但使用 `NETStandard.Library` 的 1.6.0 版本。</span><span class="sxs-lookup"><span data-stu-id="04b33-125">The project file in the following example targets `netstandard1.3` but uses the 1.6.0 version of `NETStandard.Library`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netstandard1.3</TargetFramework>
  <NetStandardImplicitPackageVersion>1.6.0</NetStandardImplicitPackageVersion>
</PropertyGroup>
```

## <a name="assembly-info-generation-properties"></a><span data-ttu-id="04b33-126">程序集信息生成属性</span><span class="sxs-lookup"><span data-stu-id="04b33-126">Assembly info generation properties</span></span>

- [<span data-ttu-id="04b33-127">GenerateAssemblyCompanyAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-127">GenerateAssemblyCompanyAttribute</span></span>](#generateassemblycompanyattribute)
- [<span data-ttu-id="04b33-128">GenerateAssemblyConfigurationAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-128">GenerateAssemblyConfigurationAttribute</span></span>](#generateassemblyconfigurationattribute)
- [<span data-ttu-id="04b33-129">GenerateAssemblyCopyrightAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-129">GenerateAssemblyCopyrightAttribute</span></span>](#generateassemblycopyrightattribute)
- [<span data-ttu-id="04b33-130">GenerateAssemblyDescriptionAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-130">GenerateAssemblyDescriptionAttribute</span></span>](#generateassemblydescriptionattribute)
- [<span data-ttu-id="04b33-131">GenerateAssemblyFileVersionAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-131">GenerateAssemblyFileVersionAttribute</span></span>](#generateassemblyfileversionattribute)
- [<span data-ttu-id="04b33-132">GenerateAssemblyInfo</span><span class="sxs-lookup"><span data-stu-id="04b33-132">GenerateAssemblyInfo</span></span>](#generateassemblyinfo)
- [<span data-ttu-id="04b33-133">GenerateAssemblyInformationalVersionAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-133">GenerateAssemblyInformationalVersionAttribute</span></span>](#generateassemblyinformationalversionattribute)
- [<span data-ttu-id="04b33-134">GenerateAssemblyProductAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-134">GenerateAssemblyProductAttribute</span></span>](#generateassemblyproductattribute)
- [<span data-ttu-id="04b33-135">GenerateAssemblyTitleAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-135">GenerateAssemblyTitleAttribute</span></span>](#generateassemblytitleattribute)
- [<span data-ttu-id="04b33-136">GenerateAssemblyVersionAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-136">GenerateAssemblyVersionAttribute</span></span>](#generateassemblyversionattribute)
- [<span data-ttu-id="04b33-137">GeneratedAssemblyInfoFile</span><span class="sxs-lookup"><span data-stu-id="04b33-137">GeneratedAssemblyInfoFile</span></span>](#generatedassemblyinfofile)
- [<span data-ttu-id="04b33-138">GenerateNeutralResourcesLanguageAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-138">GenerateNeutralResourcesLanguageAttribute</span></span>](#generateneutralresourceslanguageattribute)

### <a name="generateassemblycompanyattribute"></a><span data-ttu-id="04b33-139">GenerateAssemblyCompanyAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-139">GenerateAssemblyCompanyAttribute</span></span>

<span data-ttu-id="04b33-140">此属性控制 `Company` 属性是否为程序集生成 <xref:System.Reflection.AssemblyCompanyAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-140">This property controls whether or not the `Company` property generates the <xref:System.Reflection.AssemblyCompanyAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-141">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-141">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyCompanyAttribute>false</GenerateAssemblyCompanyAttribute>
</PropertyGroup>
```

### <a name="generateassemblyconfigurationattribute"></a><span data-ttu-id="04b33-142">GenerateAssemblyConfigurationAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-142">GenerateAssemblyConfigurationAttribute</span></span>

<span data-ttu-id="04b33-143">此属性控制 `Configuration` 属性是否为程序集生成 <xref:System.Reflection.AssemblyConfigurationAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-143">This property controls whether or not the `Configuration` property generates the <xref:System.Reflection.AssemblyConfigurationAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-144">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-144">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyConfigurationAttribute>false</GenerateAssemblyConfigurationAttribute>
</PropertyGroup>
```

### <a name="generateassemblycopyrightattribute"></a><span data-ttu-id="04b33-145">GenerateAssemblyCopyrightAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-145">GenerateAssemblyCopyrightAttribute</span></span>

<span data-ttu-id="04b33-146">此属性控制 `Copyright` 属性是否为程序集生成 <xref:System.Reflection.AssemblyCopyrightAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-146">This property controls whether or not the `Copyright` property generates the <xref:System.Reflection.AssemblyCopyrightAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-147">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-147">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyCopyrightAttribute>false</GenerateAssemblyCopyrightAttribute>
</PropertyGroup>
```

### <a name="generateassemblydescriptionattribute"></a><span data-ttu-id="04b33-148">GenerateAssemblyDescriptionAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-148">GenerateAssemblyDescriptionAttribute</span></span>

<span data-ttu-id="04b33-149">此属性控制 `Description` 属性是否为程序集生成 <xref:System.Reflection.AssemblyDescriptionAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-149">This property controls whether or not the `Description` property generates the <xref:System.Reflection.AssemblyDescriptionAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-150">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-150">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyDescriptionAttribute>false</GenerateAssemblyDescriptionAttribute>
</PropertyGroup>
```

### <a name="generateassemblyfileversionattribute"></a><span data-ttu-id="04b33-151">GenerateAssemblyFileVersionAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-151">GenerateAssemblyFileVersionAttribute</span></span>

<span data-ttu-id="04b33-152">此属性控制 `FileVersion` 属性是否为程序集生成 <xref:System.Reflection.AssemblyFileVersionAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-152">This property controls whether or not the `FileVersion` property generates the <xref:System.Reflection.AssemblyFileVersionAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-153">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-153">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyFileVersionAttribute>false</GenerateAssemblyFileVersionAttribute>
</PropertyGroup>
```

### <a name="generateassemblyinfo"></a><span data-ttu-id="04b33-154">GenerateAssemblyInfo</span><span class="sxs-lookup"><span data-stu-id="04b33-154">GenerateAssemblyInfo</span></span>

<span data-ttu-id="04b33-155">控制项目的 `AssemblyInfo` 属性生成。</span><span class="sxs-lookup"><span data-stu-id="04b33-155">Controls `AssemblyInfo` attribute generation for the project.</span></span> <span data-ttu-id="04b33-156">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-156">The default value is `true`.</span></span> <span data-ttu-id="04b33-157">使用 `false` 禁用文件生成：</span><span class="sxs-lookup"><span data-stu-id="04b33-157">Use `false` to disable generation of the file:</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
</PropertyGroup>
```

<span data-ttu-id="04b33-158">[GeneratedAssemblyInfoFile](#generatedassemblyinfofile) 设置控制生成的文件的名称。</span><span class="sxs-lookup"><span data-stu-id="04b33-158">The [GeneratedAssemblyInfoFile](#generatedassemblyinfofile) setting controls the name of the generated file.</span></span>

<span data-ttu-id="04b33-159">当 `GenerateAssemblyInfo` 值为 `true` 时，项目属性将转换为 `AssemblyInfo` 属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-159">When the `GenerateAssemblyInfo` value is `true`, project properties are transformed into `AssemblyInfo` attributes.</span></span> <span data-ttu-id="04b33-160">下表列出了用于生成特性的项目属性，以及可禁用该生成的属性：</span><span class="sxs-lookup"><span data-stu-id="04b33-160">The following table lists the project properties that generate the attributes, and the properties that can disable that generation:</span></span>

| <span data-ttu-id="04b33-161">属性</span><span class="sxs-lookup"><span data-stu-id="04b33-161">Property</span></span>               | <span data-ttu-id="04b33-162">属性</span><span class="sxs-lookup"><span data-stu-id="04b33-162">Attribute</span></span>                                                      | <span data-ttu-id="04b33-163">要禁用的属性</span><span class="sxs-lookup"><span data-stu-id="04b33-163">Property to disable</span></span>                                                                               |
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

<span data-ttu-id="04b33-164">有关这些设置的说明：</span><span class="sxs-lookup"><span data-stu-id="04b33-164">Notes about these settings:</span></span>

- <span data-ttu-id="04b33-165">`AssemblyVersion` 和 `FileVersion` 默认采用 `$(Version)` 的值而不带后缀。</span><span class="sxs-lookup"><span data-stu-id="04b33-165">`AssemblyVersion` and `FileVersion` default to the value of `$(Version)` without the suffix.</span></span> <span data-ttu-id="04b33-166">例如，如果 `$(Version)` 为 `1.2.3-beta.4`，则值将为 `1.2.3`。</span><span class="sxs-lookup"><span data-stu-id="04b33-166">For example, if `$(Version)` is `1.2.3-beta.4`, then the value would be `1.2.3`.</span></span>
- <span data-ttu-id="04b33-167">`InformationalVersion` 默认是 `$(Version)` 的值。</span><span class="sxs-lookup"><span data-stu-id="04b33-167">`InformationalVersion` defaults to the value of `$(Version)`.</span></span>
- <span data-ttu-id="04b33-168">如果存在 `$(SourceRevisionId)` 属性，则该属性会附加到 `InformationalVersion`。</span><span class="sxs-lookup"><span data-stu-id="04b33-168">If the `$(SourceRevisionId)` property is present, it's appended to `InformationalVersion`.</span></span> <span data-ttu-id="04b33-169">可以使用 `IncludeSourceRevisionInInformationalVersion` 禁用此行为。</span><span class="sxs-lookup"><span data-stu-id="04b33-169">You can disable this behavior using `IncludeSourceRevisionInInformationalVersion`.</span></span>
- <span data-ttu-id="04b33-170">`Copyright` 和 `Description` 属性也可用于 NuGet 元数据。</span><span class="sxs-lookup"><span data-stu-id="04b33-170">`Copyright` and `Description` properties are also used for NuGet metadata.</span></span>
- <span data-ttu-id="04b33-171">`Configuration`（默认值为 `Debug`）由所有 MSBuild 目标共享。</span><span class="sxs-lookup"><span data-stu-id="04b33-171">`Configuration`, which defaults to `Debug`, is shared with all MSBuild targets.</span></span> <span data-ttu-id="04b33-172">可以通过 `dotnet` 命令的 `--configuration` 选项对其进行设置，例如 [dotnet pack](../tools/dotnet-pack.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-172">You can set it via the `--configuration` option of `dotnet` commands, for example, [dotnet pack](../tools/dotnet-pack.md).</span></span>
- <span data-ttu-id="04b33-173">创建 NuGet 包时会使用某些属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-173">Some of the properties are used when creating a NuGet package.</span></span> <span data-ttu-id="04b33-174">有关详细信息，请参阅[包属性](#package-properties)。</span><span class="sxs-lookup"><span data-stu-id="04b33-174">For more information, see [Package properties](#package-properties).</span></span>

#### <a name="migrating-from-net-framework"></a><span data-ttu-id="04b33-175">从 .NET Framework 迁移</span><span class="sxs-lookup"><span data-stu-id="04b33-175">Migrating from .NET Framework</span></span>

<span data-ttu-id="04b33-176">.NET Framework 项目模板会创建一个包含这些程序集信息属性集的代码文件。</span><span class="sxs-lookup"><span data-stu-id="04b33-176">.NET Framework project templates create a code file with these assembly info attributes set.</span></span> <span data-ttu-id="04b33-177">该文件通常位于 .\Properties\AssemblyInfo.cs 或 .\Properties\AssemblyInfo.vb 。</span><span class="sxs-lookup"><span data-stu-id="04b33-177">The file is typically located at *.\Properties\AssemblyInfo.cs* or *.\Properties\AssemblyInfo.vb*.</span></span> <span data-ttu-id="04b33-178">SDK 样式项目基于项目设置为你生成此文件。</span><span class="sxs-lookup"><span data-stu-id="04b33-178">SDK-style projects generate this file for you based on the project settings.</span></span> <span data-ttu-id="04b33-179">你不能同时具有这两者。</span><span class="sxs-lookup"><span data-stu-id="04b33-179">**You can't have both.**</span></span> <span data-ttu-id="04b33-180">将代码移植到 .NET 5（和 .NET Core 3.1）或更高版本时，请执行以下操作之一：</span><span class="sxs-lookup"><span data-stu-id="04b33-180">When porting your code to .NET 5 (and .NET Core 3.1) or later, do one of the following:</span></span>

- <span data-ttu-id="04b33-181">通过将 `GenerateAssemblyInfo` 设置为 `false` 来禁用包含程序集信息特性的临时代码文件的生成。</span><span class="sxs-lookup"><span data-stu-id="04b33-181">Disable the generation of the temporary code file that contains the assembly info attributes by setting `GenerateAssemblyInfo` to `false`.</span></span> <span data-ttu-id="04b33-182">这使你可以保留 AssemblyInfo 文件。</span><span class="sxs-lookup"><span data-stu-id="04b33-182">This enables you to keep your *AssemblyInfo* file.</span></span>
- <span data-ttu-id="04b33-183">将 `AssemblyInfo` 文件中的设置迁移到项目文件，并删除该 `AssemblyInfo` 文件。</span><span class="sxs-lookup"><span data-stu-id="04b33-183">Migrate the settings in the `AssemblyInfo` file to the project file, and delete the `AssemblyInfo` file.</span></span>

### <a name="generateassemblyinformationalversionattribute"></a><span data-ttu-id="04b33-184">GenerateAssemblyInformationalVersionAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-184">GenerateAssemblyInformationalVersionAttribute</span></span>

<span data-ttu-id="04b33-185">此属性控制 `InformationalVersion` 属性是否为程序集生成 <xref:System.Reflection.AssemblyInformationalVersionAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-185">This property controls whether or not the `InformationalVersion` property generates the <xref:System.Reflection.AssemblyInformationalVersionAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-186">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-186">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyInformationalVersionAttribute>false</GenerateAssemblyInformationalVersionAttribute>
</PropertyGroup>
```

### <a name="generateassemblyproductattribute"></a><span data-ttu-id="04b33-187">GenerateAssemblyProductAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-187">GenerateAssemblyProductAttribute</span></span>

<span data-ttu-id="04b33-188">此属性控制 `Product` 属性是否为程序集生成 <xref:System.Reflection.AssemblyProductAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-188">This property controls whether or not the `Product` property generates the <xref:System.Reflection.AssemblyProductAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-189">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-189">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyProductAttribute>false</GenerateAssemblyProductAttribute>
</PropertyGroup>
```

### <a name="generateassemblytitleattribute"></a><span data-ttu-id="04b33-190">GenerateAssemblyTitleAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-190">GenerateAssemblyTitleAttribute</span></span>

<span data-ttu-id="04b33-191">此属性控制 `AssemblyTitle` 属性是否为程序集生成 <xref:System.Reflection.AssemblyTitleAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-191">This property controls whether or not the `AssemblyTitle` property generates the <xref:System.Reflection.AssemblyTitleAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-192">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-192">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyTitleAttribute>false</GenerateAssemblyTitleAttribute>
</PropertyGroup>
```

### <a name="generateassemblyversionattribute"></a><span data-ttu-id="04b33-193">GenerateAssemblyVersionAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-193">GenerateAssemblyVersionAttribute</span></span>

<span data-ttu-id="04b33-194">此属性控制 `AssemblyVersion` 属性是否为程序集生成 <xref:System.Reflection.AssemblyVersionAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-194">This property controls whether or not the `AssemblyVersion` property generates the <xref:System.Reflection.AssemblyVersionAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-195">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-195">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateAssemblyVersionAttribute>false</GenerateAssemblyVersionAttribute>
</PropertyGroup>
```

### <a name="generatedassemblyinfofile"></a><span data-ttu-id="04b33-196">GeneratedAssemblyInfoFile</span><span class="sxs-lookup"><span data-stu-id="04b33-196">GeneratedAssemblyInfoFile</span></span>

<span data-ttu-id="04b33-197">此属性定义生成的程序集信息文件的相对或绝对路径。</span><span class="sxs-lookup"><span data-stu-id="04b33-197">The property defines the relative or absolute path of the generated assembly info file.</span></span> <span data-ttu-id="04b33-198">默认为 `$(IntermediateOutputPath)`（通常为 obj）目录中名为 [project-name].AssemblyInfo.[cs|vb] 的文件 。</span><span class="sxs-lookup"><span data-stu-id="04b33-198">Defaults to a file named *[project-name].AssemblyInfo.[cs|vb]* in the `$(IntermediateOutputPath)` (usually the *obj*) directory.</span></span>

```xml
<PropertyGroup>
  <GeneratedAssemblyInfoFile>assemblyinfo.cs</GeneratedAssemblyInfoFile>
</PropertyGroup>
```

### <a name="generateneutralresourceslanguageattribute"></a><span data-ttu-id="04b33-199">GenerateNeutralResourcesLanguageAttribute</span><span class="sxs-lookup"><span data-stu-id="04b33-199">GenerateNeutralResourcesLanguageAttribute</span></span>

<span data-ttu-id="04b33-200">此属性控制 `NeutralLanguage` 属性是否为程序集生成 <xref:System.Resources.NeutralResourcesLanguageAttribute>。</span><span class="sxs-lookup"><span data-stu-id="04b33-200">This property controls whether or not the `NeutralLanguage` property generates the <xref:System.Resources.NeutralResourcesLanguageAttribute> for the assembly.</span></span> <span data-ttu-id="04b33-201">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-201">The default value is `true`.</span></span>

```xml
<PropertyGroup>
  <GenerateNeutralResourcesLanguageAttribute>false</GenerateNeutralResourcesLanguageAttribute>
</PropertyGroup>
```

## <a name="package-properties"></a><span data-ttu-id="04b33-202">包属性</span><span class="sxs-lookup"><span data-stu-id="04b33-202">Package properties</span></span>

<span data-ttu-id="04b33-203">可以指定 `PackageId`、`PackageVersion`、`PackageIcon`、`Title` 和 `Description` 等属性来描述通过项目创建的包。</span><span class="sxs-lookup"><span data-stu-id="04b33-203">You can specify properties such as `PackageId`, `PackageVersion`, `PackageIcon`, `Title`, and `Description` to describe the package that gets created from your project.</span></span> <span data-ttu-id="04b33-204">若要了解这些属性和其他属性，请参阅[包目标](/nuget/reference/msbuild-targets#pack-target)。</span><span class="sxs-lookup"><span data-stu-id="04b33-204">For information about these and other properties, see [pack target](/nuget/reference/msbuild-targets#pack-target).</span></span>

```xml
<PropertyGroup>
  ...
  <PackageId>ClassLibDotNetStandard</PackageId>
  <Version>1.0.0</Version>
  <Authors>John Doe</Authors>
  <Company>Contoso</Company>
</PropertyGroup>
```

## <a name="publish-related-properties"></a><span data-ttu-id="04b33-205">与发布相关的属性</span><span class="sxs-lookup"><span data-stu-id="04b33-205">Publish-related properties</span></span>

<span data-ttu-id="04b33-206">本节收录了以下 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="04b33-206">The following MSBuild properties are documented in this section:</span></span>

- [<span data-ttu-id="04b33-207">AppendRuntimeIdentifierToOutputPath</span><span class="sxs-lookup"><span data-stu-id="04b33-207">AppendRuntimeIdentifierToOutputPath</span></span>](#appendruntimeidentifiertooutputpath)
- [<span data-ttu-id="04b33-208">AppendTargetFrameworkToOutputPath</span><span class="sxs-lookup"><span data-stu-id="04b33-208">AppendTargetFrameworkToOutputPath</span></span>](#appendtargetframeworktooutputpath)
- [<span data-ttu-id="04b33-209">CopyLocalLockFileAssemblies</span><span class="sxs-lookup"><span data-stu-id="04b33-209">CopyLocalLockFileAssemblies</span></span>](#copylocallockfileassemblies)
- [<span data-ttu-id="04b33-210">PreserveCompilationContext</span><span class="sxs-lookup"><span data-stu-id="04b33-210">PreserveCompilationContext</span></span>](#preservecompilationcontext)
- [<span data-ttu-id="04b33-211">PreserveCompilationReferences</span><span class="sxs-lookup"><span data-stu-id="04b33-211">PreserveCompilationReferences</span></span>](#preservecompilationreferences)
- [<span data-ttu-id="04b33-212">RuntimeIdentifier</span><span class="sxs-lookup"><span data-stu-id="04b33-212">RuntimeIdentifier</span></span>](#runtimeidentifier)
- [<span data-ttu-id="04b33-213">RuntimeIdentifiers</span><span class="sxs-lookup"><span data-stu-id="04b33-213">RuntimeIdentifiers</span></span>](#runtimeidentifiers)
- [<span data-ttu-id="04b33-214">UseAppHost</span><span class="sxs-lookup"><span data-stu-id="04b33-214">UseAppHost</span></span>](#useapphost)

### <a name="appendtargetframeworktooutputpath"></a><span data-ttu-id="04b33-215">AppendTargetFrameworkToOutputPath</span><span class="sxs-lookup"><span data-stu-id="04b33-215">AppendTargetFrameworkToOutputPath</span></span>

<span data-ttu-id="04b33-216">`AppendTargetFrameworkToOutputPath` 属性控制是否将[目标框架名字对象 (TFM)](../../standard/frameworks.md) 追加到输出路径（由 [OutputPath](/visualstudio/msbuild/common-msbuild-project-properties#list-of-common-properties-and-parameters) 定义）。</span><span class="sxs-lookup"><span data-stu-id="04b33-216">The `AppendTargetFrameworkToOutputPath` property controls whether the [target framework moniker (TFM)](../../standard/frameworks.md) is appended to the output path (which is defined by [OutputPath](/visualstudio/msbuild/common-msbuild-project-properties#list-of-common-properties-and-parameters)).</span></span> <span data-ttu-id="04b33-217">.NET SDK 会自动将目标框架以及运行时标识符（如果有）追加到输出路径。</span><span class="sxs-lookup"><span data-stu-id="04b33-217">The .NET SDK automatically appends the target framework and, if present, the runtime identifier to the output path.</span></span> <span data-ttu-id="04b33-218">将 `AppendTargetFrameworkToOutputPath` 设置为 `false` 可防止将 TFM 追加到输出路径。</span><span class="sxs-lookup"><span data-stu-id="04b33-218">Setting `AppendTargetFrameworkToOutputPath` to `false` prevents the TFM from being appended to the output path.</span></span> <span data-ttu-id="04b33-219">但是，如果输出路径中没有 TFM，则可能会发生多个生成项目相互覆盖的情况。</span><span class="sxs-lookup"><span data-stu-id="04b33-219">However, without the TFM in the output path, multiple build artifacts may overwrite each other.</span></span>

<span data-ttu-id="04b33-220">例如，对于 .NET 5.0 应用，输出路径将从 `bin\Debug\net5.0` 更改为 `bin\Debug`，并具有以下设置：</span><span class="sxs-lookup"><span data-stu-id="04b33-220">For example, for a .NET 5.0 app, the output path changes from `bin\Debug\net5.0` to `bin\Debug` with the following setting:</span></span>

```xml
<PropertyGroup>
  <AppendTargetFrameworkToOutputPath>false</AppendTargetFrameworkToOutputPath>
</PropertyGroup>
```

### <a name="appendruntimeidentifiertooutputpath"></a><span data-ttu-id="04b33-221">AppendRuntimeIdentifierToOutputPath</span><span class="sxs-lookup"><span data-stu-id="04b33-221">AppendRuntimeIdentifierToOutputPath</span></span>

<span data-ttu-id="04b33-222">`AppendRuntimeIdentifierToOutputPath` 属性控制是否将[运行时标识符 (RID)](../rid-catalog.md) 追加到输出路径。</span><span class="sxs-lookup"><span data-stu-id="04b33-222">The `AppendRuntimeIdentifierToOutputPath` property controls whether the [runtime identifier (RID)](../rid-catalog.md) is appended to the output path.</span></span> <span data-ttu-id="04b33-223">.NET SDK 会自动将目标框架以及运行时标识符（如果有）追加到输出路径。</span><span class="sxs-lookup"><span data-stu-id="04b33-223">The .NET SDK automatically appends the target framework and, if present, the runtime identifier to the output path.</span></span> <span data-ttu-id="04b33-224">将 `AppendRuntimeIdentifierToOutputPath` 设置为 `false` 可防止将 RID 追加到输出路径。</span><span class="sxs-lookup"><span data-stu-id="04b33-224">Setting `AppendRuntimeIdentifierToOutputPath` to `false` prevents the RID from being appended to the output path.</span></span>

<span data-ttu-id="04b33-225">例如，对于 .NET 5.0 应用和 RID `win10-x64`，输出路径将从 `bin\Debug\net5.0\win10-x64` 更改为 `bin\Debug\net5.0`，并具有以下设置：</span><span class="sxs-lookup"><span data-stu-id="04b33-225">For example, for a .NET 5.0 app and an RID of `win10-x64`, the output path changes from `bin\Debug\net5.0\win10-x64` to `bin\Debug\net5.0` with the following setting:</span></span>

```xml
<PropertyGroup>
  <AppendRuntimeIdentifierToOutputPath>false</AppendRuntimeIdentifierToOutputPath>
</PropertyGroup>
```

### <a name="copylocallockfileassemblies"></a><span data-ttu-id="04b33-226">CopyLocalLockFileAssemblies</span><span class="sxs-lookup"><span data-stu-id="04b33-226">CopyLocalLockFileAssemblies</span></span>

<span data-ttu-id="04b33-227">对于依赖于其他库的插件项目，`CopyLocalLockFileAssemblies` 属性非常有用。</span><span class="sxs-lookup"><span data-stu-id="04b33-227">The `CopyLocalLockFileAssemblies` property is useful for plugin projects that have dependencies on other libraries.</span></span> <span data-ttu-id="04b33-228">如果将此属性设置为 `true`，系统会将所有 NuGet 包依赖项复制到输出目录。</span><span class="sxs-lookup"><span data-stu-id="04b33-228">If you set this property to `true`, any NuGet package dependencies are copied to the output directory.</span></span> <span data-ttu-id="04b33-229">这意味着，可以使用 `dotnet build` 的输出在任何计算机上运行插件。</span><span class="sxs-lookup"><span data-stu-id="04b33-229">That means you can use the output of `dotnet build` to run your plugin on any machine.</span></span>

```xml
<PropertyGroup>
  <CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>
</PropertyGroup>
```

> [!TIP]
> <span data-ttu-id="04b33-230">或者，可以使用 `dotnet publish` 发布类库。</span><span class="sxs-lookup"><span data-stu-id="04b33-230">Alternatively, you can use `dotnet publish` to publish the class library.</span></span> <span data-ttu-id="04b33-231">有关详细信息，请查看 [dotnet publish](../tools/dotnet-publish.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-231">For more information, see [dotnet publish](../tools/dotnet-publish.md).</span></span>

### <a name="preservecompilationcontext"></a><span data-ttu-id="04b33-232">PreserveCompilationContext</span><span class="sxs-lookup"><span data-stu-id="04b33-232">PreserveCompilationContext</span></span>

<span data-ttu-id="04b33-233">`PreserveCompilationContext` 属性允许已构建或已发布的应用程序在运行时使用与构建时使用的相同设置来编译更多代码。</span><span class="sxs-lookup"><span data-stu-id="04b33-233">The `PreserveCompilationContext` property allows a built or published application to compile more code at run time using the same settings that were used at build time.</span></span> <span data-ttu-id="04b33-234">在构建时引用的程序集将被复制到输出目录的 ref 子目录中。</span><span class="sxs-lookup"><span data-stu-id="04b33-234">The assemblies referenced at build time will be copied into the *ref* subdirectory of the output directory.</span></span> <span data-ttu-id="04b33-235">引用程序集的名称与传递给编译器的选项一起存储在应用程序的 .deps.json 文件中。</span><span class="sxs-lookup"><span data-stu-id="04b33-235">The names of the reference assemblies are stored in the application's *.deps.json* file along with the options passed to the compiler.</span></span> <span data-ttu-id="04b33-236">可使用 <xref:Microsoft.Extensions.DependencyModel.DependencyContext.CompileLibraries?displayProperty=nameWithType> 和 <xref:Microsoft.Extensions.DependencyModel.DependencyContext.CompilationOptions?displayProperty=nameWithType> 属性检索此信息。</span><span class="sxs-lookup"><span data-stu-id="04b33-236">You can retrieve this information using the <xref:Microsoft.Extensions.DependencyModel.DependencyContext.CompileLibraries?displayProperty=nameWithType> and <xref:Microsoft.Extensions.DependencyModel.DependencyContext.CompilationOptions?displayProperty=nameWithType> properties.</span></span>

<span data-ttu-id="04b33-237">此功能主要由 ASP.NET Core MVC 和 Razor Pages 在内部使用，以支持 Razor文件的运行时编译。</span><span class="sxs-lookup"><span data-stu-id="04b33-237">This functionality is mostly used internally by ASP.NET Core MVC and Razor pages to support run-time compilation of Razor files.</span></span>

```xml
<PropertyGroup>
  <PreserveCompilationContext>true</PreserveCompilationContext>
</PropertyGroup>
```

### <a name="preservecompilationreferences"></a><span data-ttu-id="04b33-238">PreserveCompilationReferences</span><span class="sxs-lookup"><span data-stu-id="04b33-238">PreserveCompilationReferences</span></span>

<span data-ttu-id="04b33-239">`PreserveCompilationReferences` 属性与 [PreserveCompilationContext](#preservecompilationcontext) 属性类似，只不过它仅将引用的程序集复制到发布目录，而不是 .deps.json 文件。</span><span class="sxs-lookup"><span data-stu-id="04b33-239">The `PreserveCompilationReferences` property is similar to the [PreserveCompilationContext](#preservecompilationcontext) property, except that it only copies the referenced assemblies to the publish directory, and not the *.deps.json* file.</span></span>

```xml
<PropertyGroup>
  <PreserveCompilationReferences>true</PreserveCompilationReferences>
</PropertyGroup>
```

<span data-ttu-id="04b33-240">有关详细信息，请参阅 [Razor SDK 属性](/aspnet/core/razor-pages/sdk#properties)。</span><span class="sxs-lookup"><span data-stu-id="04b33-240">For more information, see [Razor SDK properties](/aspnet/core/razor-pages/sdk#properties).</span></span>

### <a name="runtimeidentifier"></a><span data-ttu-id="04b33-241">RuntimeIdentifier</span><span class="sxs-lookup"><span data-stu-id="04b33-241">RuntimeIdentifier</span></span>

<span data-ttu-id="04b33-242">`RuntimeIdentifier` 属性可用于指定项目的单个[运行时标识符 (RID)](../rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-242">The `RuntimeIdentifier` property lets you specify a single [runtime identifier (RID)](../rid-catalog.md) for the project.</span></span> <span data-ttu-id="04b33-243">RID 支持发布独立部署。</span><span class="sxs-lookup"><span data-stu-id="04b33-243">The RID enables publishing a self-contained deployment.</span></span>

```xml
<PropertyGroup>
  <RuntimeIdentifier>ubuntu.16.04-x64</RuntimeIdentifier>
</PropertyGroup>
```

### <a name="runtimeidentifiers"></a><span data-ttu-id="04b33-244">RuntimeIdentifiers</span><span class="sxs-lookup"><span data-stu-id="04b33-244">RuntimeIdentifiers</span></span>

<span data-ttu-id="04b33-245">`RuntimeIdentifiers` 属性可用于指定项目的[运行时标识符 (RID)](../rid-catalog.md) 的列表（以分号分隔）。</span><span class="sxs-lookup"><span data-stu-id="04b33-245">The `RuntimeIdentifiers` property lets you specify a semicolon-delimited list of [runtime identifiers (RIDs)](../rid-catalog.md) for the project.</span></span> <span data-ttu-id="04b33-246">如果需要为多个运行时发布，请使用此属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-246">Use this property if you need to publish for multiple runtimes.</span></span> <span data-ttu-id="04b33-247">`RuntimeIdentifiers` 在还原时使用，以确保图中有适当的资产。</span><span class="sxs-lookup"><span data-stu-id="04b33-247">`RuntimeIdentifiers` is used at restore time to ensure the right assets are in the graph.</span></span>

> [!TIP]
> <span data-ttu-id="04b33-248">`RuntimeIdentifier`（单数）可以在只需要一个运行时时提供更快的生成。</span><span class="sxs-lookup"><span data-stu-id="04b33-248">`RuntimeIdentifier` (singular) can provide faster builds when only a single runtime is required.</span></span>

```xml
<PropertyGroup>
  <RuntimeIdentifiers>win10-x64;osx.10.11-x64;ubuntu.16.04-x64</RuntimeIdentifiers>
</PropertyGroup>
```

### <a name="useapphost"></a><span data-ttu-id="04b33-249">UseAppHost</span><span class="sxs-lookup"><span data-stu-id="04b33-249">UseAppHost</span></span>

<span data-ttu-id="04b33-250">`UseAppHost` 属性控制是否为部署创建本机可执行文件。</span><span class="sxs-lookup"><span data-stu-id="04b33-250">The `UseAppHost` property controls whether or not a native executable is created for a deployment.</span></span> <span data-ttu-id="04b33-251">自包含部署需要本机可执行文件。</span><span class="sxs-lookup"><span data-stu-id="04b33-251">A native executable is required for self-contained deployments.</span></span>

<span data-ttu-id="04b33-252">在 .NET Core3.0 及更高版本中，默认情况下会创建依赖于框架的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="04b33-252">In .NET Core 3.0 and later versions, a framework-dependent executable is created by default.</span></span> <span data-ttu-id="04b33-253">将 `UseAppHost` 属性设置为 `false` 可禁用可执行文件的生成。</span><span class="sxs-lookup"><span data-stu-id="04b33-253">Set the `UseAppHost` property to `false` to disable generation of the executable.</span></span>

```xml
<PropertyGroup>
  <UseAppHost>false</UseAppHost>
</PropertyGroup>
```

<span data-ttu-id="04b33-254">有关部署的详细信息，请参阅 [.NET 应用程序部署](../deploying/index.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-254">For more information about deployment, see [.NET application deployment](../deploying/index.md).</span></span>

## <a name="compilation-related-properties"></a><span data-ttu-id="04b33-255">与编译相关的属性</span><span class="sxs-lookup"><span data-stu-id="04b33-255">Compilation-related properties</span></span>

<span data-ttu-id="04b33-256">本节收录了以下 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="04b33-256">The following MSBuild properties are documented in this section:</span></span>

- [<span data-ttu-id="04b33-257">EmbeddedResourceUseDependentUponConvention</span><span class="sxs-lookup"><span data-stu-id="04b33-257">EmbeddedResourceUseDependentUponConvention</span></span>](#embeddedresourceusedependentuponconvention)
- [<span data-ttu-id="04b33-258">LangVersion</span><span class="sxs-lookup"><span data-stu-id="04b33-258">LangVersion</span></span>](#langversion)

### <a name="embeddedresourceusedependentuponconvention"></a><span data-ttu-id="04b33-259">EmbeddedResourceUseDependentUponConvention</span><span class="sxs-lookup"><span data-stu-id="04b33-259">EmbeddedResourceUseDependentUponConvention</span></span>

<span data-ttu-id="04b33-260">`EmbeddedResourceUseDependentUponConvention` 属性定义了是否从与资源文件并置的源文件中的类型信息生成资源清单文件名。</span><span class="sxs-lookup"><span data-stu-id="04b33-260">The `EmbeddedResourceUseDependentUponConvention` property defines whether resource manifest file names are generated from type information in source files that are colocated with resource files.</span></span> <span data-ttu-id="04b33-261">例如，如果 Form1.resx 与 Form1.cs 位于同一文件夹中，并且 `EmbeddedResourceUseDependentUponConvention` 设置为 `true`，则生成的 .resources 文件将采用 Form1.cs 中定义的第一个类型的名称作为其文件名。</span><span class="sxs-lookup"><span data-stu-id="04b33-261">For example, if *Form1.resx* is in the same folder as *Form1.cs*, and `EmbeddedResourceUseDependentUponConvention` is set to `true`, the generated *.resources* file takes its name from the first type that's defined in *Form1.cs*.</span></span> <span data-ttu-id="04b33-262">例如，如果 Form1.cs 中定义的第一个类型为 `MyNamespace.Form1`，则生成的文件名为 MyNamespace.Form1.resources。</span><span class="sxs-lookup"><span data-stu-id="04b33-262">For example, if `MyNamespace.Form1` is the first type defined in *Form1.cs*, the generated file name is *MyNamespace.Form1.resources*.</span></span>

> [!NOTE]
> <span data-ttu-id="04b33-263">如果为 `EmbeddedResource` 项指定 `LogicalName`、`ManifestResourceName` 或 `DependentUpon` 元数据，则为该资源文件生成的清单文件名将改为基于该元数据。</span><span class="sxs-lookup"><span data-stu-id="04b33-263">If `LogicalName`, `ManifestResourceName`, or `DependentUpon` metadata is specified for an `EmbeddedResource` item, the generated manifest file name for that resource file is based on that metadata instead.</span></span>

<span data-ttu-id="04b33-264">默认情况下，在新的 .NET 项目中，此属性设置为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-264">By default, in a new .NET project, this property is set to `true`.</span></span> <span data-ttu-id="04b33-265">如果设置为 `false`，并且没有为项目文件中的 `EmbeddedResource` 项指定 `LogicalName`、`ManifestResourceName` 或 `DependentUpon` 元数据，则资源清单文件名将基于项目的根命名空间和 .resx 文件的相对文件路径。</span><span class="sxs-lookup"><span data-stu-id="04b33-265">If set to `false`, and no `LogicalName`, `ManifestResourceName`, or `DependentUpon` metadata is specified for the `EmbeddedResource` item in the project file, the resource manifest file name is based off the root namespace for the project and the relative file path to the *.resx* file.</span></span> <span data-ttu-id="04b33-266">有关详细信息，请参阅[资源清单文件的命名](../resources/manifest-file-names.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-266">For more information, see [How resource manifest files are named](../resources/manifest-file-names.md).</span></span>

```xml
<PropertyGroup>
  <EmbeddedResourceUseDependentUponConvention>true</EmbeddedResourceUseDependentUponConvention>
</PropertyGroup>
```

### <a name="langversion"></a><span data-ttu-id="04b33-267">LangVersion</span><span class="sxs-lookup"><span data-stu-id="04b33-267">LangVersion</span></span>

<span data-ttu-id="04b33-268">`LangVersion` 属性可用于指定特定的编程语言版本。</span><span class="sxs-lookup"><span data-stu-id="04b33-268">The `LangVersion` property lets you specify a specific programming language version.</span></span> <span data-ttu-id="04b33-269">例如，如果要访问 C# 预览功能，请将 `LangVersion` 设置为 `preview`。</span><span class="sxs-lookup"><span data-stu-id="04b33-269">For example, if you want access to C# preview features, set `LangVersion` to `preview`.</span></span>

```xml
<PropertyGroup>
  <LangVersion>preview</LangVersion>
</PropertyGroup>
```

<span data-ttu-id="04b33-270">有关详细信息，请参阅 [C# 语言版本控制](../../csharp/language-reference/configure-language-version.md#override-a-default)。</span><span class="sxs-lookup"><span data-stu-id="04b33-270">For more information, see [C# language versioning](../../csharp/language-reference/configure-language-version.md#override-a-default).</span></span>

## <a name="default-item-inclusion-properties"></a><span data-ttu-id="04b33-271">默认项包含属性</span><span class="sxs-lookup"><span data-stu-id="04b33-271">Default item inclusion properties</span></span>

<span data-ttu-id="04b33-272">本节收录了以下 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="04b33-272">The following MSBuild properties are documented in this section:</span></span>

- [<span data-ttu-id="04b33-273">DefaultExcludesInProjectFolder</span><span class="sxs-lookup"><span data-stu-id="04b33-273">DefaultExcludesInProjectFolder</span></span>](#defaultexcludesinprojectfolder)
- [<span data-ttu-id="04b33-274">DefaultItemExcludes</span><span class="sxs-lookup"><span data-stu-id="04b33-274">DefaultItemExcludes</span></span>](#defaultitemexcludes)
- [<span data-ttu-id="04b33-275">EnableDefaultCompileItems</span><span class="sxs-lookup"><span data-stu-id="04b33-275">EnableDefaultCompileItems</span></span>](#enabledefaultcompileitems)
- [<span data-ttu-id="04b33-276">EnableDefaultEmbeddedResourceItems</span><span class="sxs-lookup"><span data-stu-id="04b33-276">EnableDefaultEmbeddedResourceItems</span></span>](#enabledefaultembeddedresourceitems)
- [<span data-ttu-id="04b33-277">EnableDefaultItems</span><span class="sxs-lookup"><span data-stu-id="04b33-277">EnableDefaultItems</span></span>](#enabledefaultitems)
- [<span data-ttu-id="04b33-278">EnableDefaultNoneItems</span><span class="sxs-lookup"><span data-stu-id="04b33-278">EnableDefaultNoneItems</span></span>](#enabledefaultnoneitems)

<span data-ttu-id="04b33-279">有关详细信息，请参阅[默认的包括和排除](overview.md#default-includes-and-excludes)。</span><span class="sxs-lookup"><span data-stu-id="04b33-279">For more information, see [Default includes and excludes](overview.md#default-includes-and-excludes).</span></span>

### <a name="defaultitemexcludes"></a><span data-ttu-id="04b33-280">DefaultItemExcludes</span><span class="sxs-lookup"><span data-stu-id="04b33-280">DefaultItemExcludes</span></span>

<span data-ttu-id="04b33-281">使用 `DefaultItemExcludes` 属性定义需从“包括”、“排除”和“删除”glob 中排除的文件和文件夹的 glob 模式。</span><span class="sxs-lookup"><span data-stu-id="04b33-281">Use the `DefaultItemExcludes` property to define glob patterns for files and folders that should be excluded from the include, exclude, and remove globs.</span></span> <span data-ttu-id="04b33-282">默认情况下从 glob 模式中排除 ./bin 和 ./obj 文件夹 。</span><span class="sxs-lookup"><span data-stu-id="04b33-282">By default, the *./bin* and *./obj* folders are excluded from the glob patterns.</span></span>

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);**/*.myextension</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="defaultexcludesinprojectfolder"></a><span data-ttu-id="04b33-283">DefaultExcludesInProjectFolder</span><span class="sxs-lookup"><span data-stu-id="04b33-283">DefaultExcludesInProjectFolder</span></span>

<span data-ttu-id="04b33-284">使用 `DefaultExcludesInProjectFolder` 属性定义项目文件夹中需要从“包括”、“排除”和“删除”glob 中排除的文件和文件夹的 glob 模式。</span><span class="sxs-lookup"><span data-stu-id="04b33-284">Use the `DefaultExcludesInProjectFolder` property to define glob patterns for files and folders in the project folder that should be excluded from the include, exclude, and remove globs.</span></span> <span data-ttu-id="04b33-285">默认情况下，从 glob 模式中排除以句点 (`.`) 开头的文件夹，如 .git 和 .vs。</span><span class="sxs-lookup"><span data-stu-id="04b33-285">By default, folders that start with a period (`.`), such as *.git* and *.vs*, are excluded from the glob patterns.</span></span>

<span data-ttu-id="04b33-286">此属性与 `DefaultItemExcludes` 属性非常相似，不同之处在于它只涉及项目文件夹中的文件和文件夹。</span><span class="sxs-lookup"><span data-stu-id="04b33-286">This property is very similar to the `DefaultItemExcludes` property, except that it only considers files and folders in the project folder.</span></span> <span data-ttu-id="04b33-287">如果 glob 模式会无意中将项目文件夹外部的项与相对路径进行匹配，请使用 `DefaultExcludesInProjectFolder` 属性，而不是 `DefaultItemExcludes` 属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-287">When a glob pattern would unintentionally match items outside the project folder with a relative path, use the `DefaultExcludesInProjectFolder` property instead of the `DefaultItemExcludes` property.</span></span>

```xml
<PropertyGroup>
  <DefaultExcludesInProjectFolder>$(DefaultExcludesInProjectFolder);**/myprefix*/**</DefaultExcludesInProjectFolder>
</PropertyGroup>
```

### <a name="enabledefaultitems"></a><span data-ttu-id="04b33-288">EnableDefaultItems</span><span class="sxs-lookup"><span data-stu-id="04b33-288">EnableDefaultItems</span></span>

<span data-ttu-id="04b33-289">`EnableDefaultItems` 属性控制是否在项目中隐式包含编译项、嵌入的资源项和 `None` 项。</span><span class="sxs-lookup"><span data-stu-id="04b33-289">The `EnableDefaultItems` property controls whether compile items, embedded resource items, and `None` items are implicitly included in the project.</span></span> <span data-ttu-id="04b33-290">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-290">The default value is `true`.</span></span> <span data-ttu-id="04b33-291">若要禁用所有隐式文件包含，请将 `EnableDefaultItems` 属性设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="04b33-291">Set the `EnableDefaultItems` property to `false` to disable all implicit file inclusion.</span></span>

```xml
<PropertyGroup>
  <EnableDefaultItems>false</EnableDefaultItems>
</PropertyGroup>
```

### <a name="enabledefaultcompileitems"></a><span data-ttu-id="04b33-292">EnableDefaultCompileItems</span><span class="sxs-lookup"><span data-stu-id="04b33-292">EnableDefaultCompileItems</span></span>

<span data-ttu-id="04b33-293">`EnableDefaultCompileItems` 属性控制是否在项目中隐式包含编译项。</span><span class="sxs-lookup"><span data-stu-id="04b33-293">The `EnableDefaultCompileItems` property controls whether compile items are implicitly included in the project.</span></span> <span data-ttu-id="04b33-294">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-294">The default value is `true`.</span></span> <span data-ttu-id="04b33-295">将 `EnableDefaultCompileItems` 属性设置为 `false` 以禁用 \* .cs 和其他语言扩展文件的隐式包含。</span><span class="sxs-lookup"><span data-stu-id="04b33-295">Set the `EnableDefaultCompileItems` property to `false` to disable implicit inclusion of \*.cs and other language-extension files.</span></span>

```xml
<PropertyGroup>
  <EnableDefaultCompileItems>false</EnableDefaultCompileItems>
</PropertyGroup>
```

### <a name="enabledefaultembeddedresourceitems"></a><span data-ttu-id="04b33-296">EnableDefaultEmbeddedResourceItems</span><span class="sxs-lookup"><span data-stu-id="04b33-296">EnableDefaultEmbeddedResourceItems</span></span>

<span data-ttu-id="04b33-297">`EnableDefaultEmbeddedResourceItems` 属性控制是否在项目中隐式包含嵌入的资源项。</span><span class="sxs-lookup"><span data-stu-id="04b33-297">The `EnableDefaultEmbeddedResourceItems` property controls whether embedded resource items are implicitly included in the project.</span></span> <span data-ttu-id="04b33-298">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-298">The default value is `true`.</span></span> <span data-ttu-id="04b33-299">将 `EnableDefaultEmbeddedResourceItems` 属性设置为 `false` 以禁用嵌入的资源文件的隐式包含。</span><span class="sxs-lookup"><span data-stu-id="04b33-299">Set the `EnableDefaultEmbeddedResourceItems` property to `false` to disable implicit inclusion of embedded resource files.</span></span>

```xml
<PropertyGroup>
  <EnableDefaultEmbeddedResourceItems>false</EnableDefaultEmbeddedResourceItems>
</PropertyGroup>
```

### <a name="enabledefaultnoneitems"></a><span data-ttu-id="04b33-300">EnableDefaultNoneItems</span><span class="sxs-lookup"><span data-stu-id="04b33-300">EnableDefaultNoneItems</span></span>

<span data-ttu-id="04b33-301">`EnableDefaultNoneItems` 属性控制是否在项目中隐式包含 `None` 项（生成过程中未赋予角色的文件）。</span><span class="sxs-lookup"><span data-stu-id="04b33-301">The `EnableDefaultNoneItems` property controls whether `None` items (files that have no role in the build process) are implicitly included in the project.</span></span> <span data-ttu-id="04b33-302">默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-302">The default value is `true`.</span></span> <span data-ttu-id="04b33-303">将 `EnableDefaultNoneItems` 属性设置为 `false` 以禁用 `None` 项的隐式包含。</span><span class="sxs-lookup"><span data-stu-id="04b33-303">Set the `EnableDefaultNoneItems` property to `false` to disable implicit inclusion of `None` items.</span></span>

```xml
<PropertyGroup>
  <EnableDefaultNoneItems>false</EnableDefaultNoneItems>
</PropertyGroup>
```

## <a name="code-analysis-properties"></a><span data-ttu-id="04b33-304">代码分析属性</span><span class="sxs-lookup"><span data-stu-id="04b33-304">Code analysis properties</span></span>

<span data-ttu-id="04b33-305">本节收录了以下 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="04b33-305">The following MSBuild properties are documented in this section:</span></span>

- [<span data-ttu-id="04b33-306">AnalysisLevel</span><span class="sxs-lookup"><span data-stu-id="04b33-306">AnalysisLevel</span></span>](#analysislevel)
- [<span data-ttu-id="04b33-307">AnalysisMode</span><span class="sxs-lookup"><span data-stu-id="04b33-307">AnalysisMode</span></span>](#analysismode)
- [<span data-ttu-id="04b33-308">CodeAnalysisTreatWarningsAsErrors</span><span class="sxs-lookup"><span data-stu-id="04b33-308">CodeAnalysisTreatWarningsAsErrors</span></span>](#codeanalysistreatwarningsaserrors)
- [<span data-ttu-id="04b33-309">EnableNETAnalyzers</span><span class="sxs-lookup"><span data-stu-id="04b33-309">EnableNETAnalyzers</span></span>](#enablenetanalyzers)
- [<span data-ttu-id="04b33-310">EnforceCodeStyleInBuild</span><span class="sxs-lookup"><span data-stu-id="04b33-310">EnforceCodeStyleInBuild</span></span>](#enforcecodestyleinbuild)

### <a name="analysislevel"></a><span data-ttu-id="04b33-311">AnalysisLevel</span><span class="sxs-lookup"><span data-stu-id="04b33-311">AnalysisLevel</span></span>

<span data-ttu-id="04b33-312">`AnalysisLevel` 属性可指定代码分析级别。</span><span class="sxs-lookup"><span data-stu-id="04b33-312">The `AnalysisLevel` property lets you specify a code-analysis level.</span></span> <span data-ttu-id="04b33-313">例如，如果要访问预览代码分析器，请将 `AnalysisLevel` 设置为 `preview`。</span><span class="sxs-lookup"><span data-stu-id="04b33-313">For example, if you want access to preview code analyzers, set `AnalysisLevel` to `preview`.</span></span>

<span data-ttu-id="04b33-314">默认值：</span><span class="sxs-lookup"><span data-stu-id="04b33-314">Default value:</span></span>

- <span data-ttu-id="04b33-315">如果你的项目以 .NET 5.0 或更高版本为目标，或你添加了 [AnalysisMode](#analysismode) 属性，则默认值为 `latest`。</span><span class="sxs-lookup"><span data-stu-id="04b33-315">If your project targets .NET 5.0 or later, or if you've added the [AnalysisMode](#analysismode) property, the default value is `latest`.</span></span>
- <span data-ttu-id="04b33-316">否则，此属性被省略，除非你将它明确添加到项目文件中。</span><span class="sxs-lookup"><span data-stu-id="04b33-316">Otherwise, this property is omitted unless you explicitly add it to the project file.</span></span>

```xml
<PropertyGroup>
  <AnalysisLevel>preview</AnalysisLevel>
</PropertyGroup>
```

<span data-ttu-id="04b33-317">下表显示可用的选项。</span><span class="sxs-lookup"><span data-stu-id="04b33-317">The following table shows the available options.</span></span>

| <span data-ttu-id="04b33-318">值</span><span class="sxs-lookup"><span data-stu-id="04b33-318">Value</span></span> | <span data-ttu-id="04b33-319">含义</span><span class="sxs-lookup"><span data-stu-id="04b33-319">Meaning</span></span> |
|-|-|
| `latest` | <span data-ttu-id="04b33-320">使用已发布的最新版代码分析器。</span><span class="sxs-lookup"><span data-stu-id="04b33-320">The latest code analyzers that have been released are used.</span></span> <span data-ttu-id="04b33-321">这是默认值。</span><span class="sxs-lookup"><span data-stu-id="04b33-321">This is the default.</span></span> |
| `preview` | <span data-ttu-id="04b33-322">使用最新的代码分析器（即使它们处于预览状态）。</span><span class="sxs-lookup"><span data-stu-id="04b33-322">The latest code analyzers are used, even if they are in preview.</span></span> |
| `5.0` | <span data-ttu-id="04b33-323">即使有较新的规则可用，也会使用为 .NET 5.0 版本启用的规则集。</span><span class="sxs-lookup"><span data-stu-id="04b33-323">The set of rules that was enabled for the .NET 5.0 release is used, even if newer rules are available.</span></span> |
| `5` | <span data-ttu-id="04b33-324">即使有较新的规则可用，也会使用为 .NET 5.0 版本启用的规则集。</span><span class="sxs-lookup"><span data-stu-id="04b33-324">The set of rules that was enabled for the .NET 5.0 release is used, even if newer rules are available.</span></span> |

> [!NOTE]
> <span data-ttu-id="04b33-325">此属性对未引用[项目 SDK](overview.md) 的项目（例如，引用 Microsoft.CodeAnalysis.NetAnalyzers NuGet 包的旧版 .NET Framework 项目）中的代码分析没有影响。</span><span class="sxs-lookup"><span data-stu-id="04b33-325">This property has no effect on code analysis in projects that don't reference a [project SDK](overview.md), for example, legacy .NET Framework projects that reference the Microsoft.CodeAnalysis.NetAnalyzers NuGet package.</span></span>

### <a name="analysismode"></a><span data-ttu-id="04b33-326">AnalysisMode</span><span class="sxs-lookup"><span data-stu-id="04b33-326">AnalysisMode</span></span>

<span data-ttu-id="04b33-327">从 .NET 5.0 开始，.NET SDK 附带了所有[“CA”代码质量规则](../../fundamentals/code-analysis/quality-rules/index.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-327">Starting with .NET 5.0, the .NET SDK ships with all of the ["CA" code quality rules](../../fundamentals/code-analysis/quality-rules/index.md).</span></span> <span data-ttu-id="04b33-328">默认情况下，只有[一些规则作为生成警告启用](../../fundamentals/code-analysis/overview.md#enabled-rules)。</span><span class="sxs-lookup"><span data-stu-id="04b33-328">By default, only [some rules are enabled](../../fundamentals/code-analysis/overview.md#enabled-rules) as build warnings.</span></span> <span data-ttu-id="04b33-329">`AnalysisMode` 属性允许自定义默认启用的一组规则。</span><span class="sxs-lookup"><span data-stu-id="04b33-329">The `AnalysisMode` property lets you customize the set of rules that are enabled by default.</span></span> <span data-ttu-id="04b33-330">可以切换到更主动的（选择退出）分析模式，也可以切换到更保守的（选择加入）分析模式。</span><span class="sxs-lookup"><span data-stu-id="04b33-330">You can either switch to a more aggressive (opt-out) analysis mode or a more conservative (opt-in) analysis mode.</span></span> <span data-ttu-id="04b33-331">例如，如果要作为生成警告默认启用所有规则，请将值设置为 `AllEnabledByDefault`。</span><span class="sxs-lookup"><span data-stu-id="04b33-331">For example, if you want to enable all rules by default as build warnings, set the value to `AllEnabledByDefault`.</span></span>

```xml
<PropertyGroup>
  <AnalysisMode>AllEnabledByDefault</AnalysisMode>
</PropertyGroup>
```

<span data-ttu-id="04b33-332">下表显示可用的选项。</span><span class="sxs-lookup"><span data-stu-id="04b33-332">The following table shows the available options.</span></span>

| <span data-ttu-id="04b33-333">值</span><span class="sxs-lookup"><span data-stu-id="04b33-333">Value</span></span> | <span data-ttu-id="04b33-334">含义</span><span class="sxs-lookup"><span data-stu-id="04b33-334">Meaning</span></span> |
|-|-|
| `Default` | <span data-ttu-id="04b33-335">默认模式，其中某些规则作为生成警告启用，某些规则作为 Visual Studio IDE 建议启用，其余规则被禁用。</span><span class="sxs-lookup"><span data-stu-id="04b33-335">Default mode, where certain rules are enabled as build warnings, certain rules are enabled as Visual Studio IDE suggestions, and the remainder are disabled.</span></span> |
| `AllEnabledByDefault` | <span data-ttu-id="04b33-336">主动或选择退出模式，默认情况下所有规则都作为生成警告启用。</span><span class="sxs-lookup"><span data-stu-id="04b33-336">Aggressive or opt-out mode, where all rules are enabled by default as build warnings.</span></span> <span data-ttu-id="04b33-337">可以选择[选择退出](../../fundamentals/code-analysis/configuration-options.md)各条规则，以禁用它们。</span><span class="sxs-lookup"><span data-stu-id="04b33-337">You can selectively [opt out](../../fundamentals/code-analysis/configuration-options.md) of individual rules to disable them.</span></span> |
| `AllDisabledByDefault` | <span data-ttu-id="04b33-338">保守或选择加入模式，默认情况下所有规则都处于禁用状态。</span><span class="sxs-lookup"><span data-stu-id="04b33-338">Conservative or opt-in mode, where all rules are disabled by default.</span></span> <span data-ttu-id="04b33-339">可以选择[选择加入](../../fundamentals/code-analysis/configuration-options.md)各条规则，以启用它们。</span><span class="sxs-lookup"><span data-stu-id="04b33-339">You can selectively [opt into](../../fundamentals/code-analysis/configuration-options.md) individual rules to enable them.</span></span> |

> [!NOTE]
> <span data-ttu-id="04b33-340">此属性对未引用[项目 SDK](overview.md) 的项目（例如，引用 Microsoft.CodeAnalysis.NetAnalyzers NuGet 包的旧版 .NET Framework 项目）中的代码分析没有影响。</span><span class="sxs-lookup"><span data-stu-id="04b33-340">This property has no effect on code analysis in projects that don't reference a [project SDK](overview.md), for example, legacy .NET Framework projects that reference the Microsoft.CodeAnalysis.NetAnalyzers NuGet package.</span></span>

### <a name="codeanalysistreatwarningsaserrors"></a><span data-ttu-id="04b33-341">CodeAnalysisTreatWarningsAsErrors</span><span class="sxs-lookup"><span data-stu-id="04b33-341">CodeAnalysisTreatWarningsAsErrors</span></span>

<span data-ttu-id="04b33-342">`CodeAnalysisTreatWarningsAsErrors` 属性可配置是否应将代码质量分析警告 (CAxxxx) 视为警告并中断生成。</span><span class="sxs-lookup"><span data-stu-id="04b33-342">The `CodeAnalysisTreatWarningsAsErrors` property lets you configure whether code quality analysis warnings (CAxxxx) should be treated as warnings and break the build.</span></span> <span data-ttu-id="04b33-343">如果在生成项目时使用 `-warnaserror` 标志，则 [.NET 代码质量分析](../../fundamentals/code-analysis/overview.md#code-quality-analysis)警告也会被视为错误。</span><span class="sxs-lookup"><span data-stu-id="04b33-343">If you use the `-warnaserror` flag when you build your projects, [.NET code quality analysis](../../fundamentals/code-analysis/overview.md#code-quality-analysis) warnings are also treated as errors.</span></span> <span data-ttu-id="04b33-344">如果不希望将代码质量分析警告视为错误，可以在项目文件中将 `CodeAnalysisTreatWarningsAsErrors` MSBuild 属性设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="04b33-344">If you do not want code quality analysis warnings to be treated as errors, you can set the `CodeAnalysisTreatWarningsAsErrors` MSBuild property to `false` in your project file.</span></span>

```xml
<PropertyGroup>
  <CodeAnalysisTreatWarningsAsErrors>false</CodeAnalysisTreatWarningsAsErrors>
</PropertyGroup>
```

### <a name="enablenetanalyzers"></a><span data-ttu-id="04b33-345">EnableNETAnalyzers</span><span class="sxs-lookup"><span data-stu-id="04b33-345">EnableNETAnalyzers</span></span>

<span data-ttu-id="04b33-346">默认情况下，为面向 .NET 5.0 或更高版本的项目启用 [.NET 代码质量分析](../../fundamentals/code-analysis/overview.md#code-quality-analysis)。</span><span class="sxs-lookup"><span data-stu-id="04b33-346">[.NET code quality analysis](../../fundamentals/code-analysis/overview.md#code-quality-analysis) is enabled, by default, for projects that target .NET 5.0 or later.</span></span> <span data-ttu-id="04b33-347">可通过将 `EnableNETAnalyzers` 属性设置为 `true`，来为面向 .NET 早期版本的 SDK 样式项目启用 .NET 代码分析。</span><span class="sxs-lookup"><span data-stu-id="04b33-347">You can enable .NET code analysis for SDK-style projects that target earlier versions of .NET by setting the `EnableNETAnalyzers` property to `true`.</span></span> <span data-ttu-id="04b33-348">若要禁用任何项目中的代码分析，可将此属性设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="04b33-348">To disable code analysis in any project, set this property to `false`.</span></span>

```xml
<PropertyGroup>
  <EnableNETAnalyzers>true</EnableNETAnalyzers>
</PropertyGroup>
```

> [!NOTE]
> <span data-ttu-id="04b33-349">此属性专门用于 .NET 5 及更高版本 SDK 中的内置分析器。</span><span class="sxs-lookup"><span data-stu-id="04b33-349">This property applies specifically to the built-in analyzers in the .NET 5+ SDK.</span></span> <span data-ttu-id="04b33-350">安装 NuGet 代码分析包时不应使用此属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-350">It should not be used when you install a NuGet code analysis package.</span></span>

### <a name="enforcecodestyleinbuild"></a><span data-ttu-id="04b33-351">EnforceCodeStyleInBuild</span><span class="sxs-lookup"><span data-stu-id="04b33-351">EnforceCodeStyleInBuild</span></span>

> [!NOTE]
> <span data-ttu-id="04b33-352">此功能当前为实验性功能，可能会在 .NET 5 和 .NET 6 版本之间发生更改。</span><span class="sxs-lookup"><span data-stu-id="04b33-352">This feature is currently experimental and may change between the .NET 5 and .NET 6 releases.</span></span>

<span data-ttu-id="04b33-353">对于所有 .NET 项目的版本，[.NET 代码样式分析](../../fundamentals/code-analysis/overview.md#code-style-analysis)默认处于禁用状态。</span><span class="sxs-lookup"><span data-stu-id="04b33-353">[.NET code style analysis](../../fundamentals/code-analysis/overview.md#code-style-analysis) is disabled, by default, on build for all .NET projects.</span></span> <span data-ttu-id="04b33-354">通过将 `EnforceCodeStyleInBuild` 属性设置为 `true`，可以为 .NET 项目启用代码样式分析。</span><span class="sxs-lookup"><span data-stu-id="04b33-354">You can enable code style analysis for .NET projects by setting the `EnforceCodeStyleInBuild` property to `true`.</span></span>

```xml
<PropertyGroup>
  <EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>
</PropertyGroup>
```

<span data-ttu-id="04b33-355">生成和报告违规时将执行[配置](../../fundamentals/code-analysis/overview.md#code-style-analysis)为警告或错误的所有代码样式规则。</span><span class="sxs-lookup"><span data-stu-id="04b33-355">All code style rules that are [configured](../../fundamentals/code-analysis/overview.md#code-style-analysis) to be warnings or errors will execute on build and report violations.</span></span>

## <a name="run-time-configuration-properties"></a><span data-ttu-id="04b33-356">运行时配置属性</span><span class="sxs-lookup"><span data-stu-id="04b33-356">Run-time configuration properties</span></span>

<span data-ttu-id="04b33-357">可以通过在应用的项目文件中指定 MSBuild 属性来配置某些运行时行为。</span><span class="sxs-lookup"><span data-stu-id="04b33-357">You can configure some run-time behaviors by specifying MSBuild properties in the project file of the app.</span></span> <span data-ttu-id="04b33-358">有关配置运行时行为的其他方法的信息，请参阅[运行时配置设置](../run-time-config/index.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-358">For information about other ways of configuring run-time behavior, see [Run-time configuration settings](../run-time-config/index.md).</span></span>

- [<span data-ttu-id="04b33-359">ConcurrentGarbageCollection</span><span class="sxs-lookup"><span data-stu-id="04b33-359">ConcurrentGarbageCollection</span></span>](#concurrentgarbagecollection)
- [<span data-ttu-id="04b33-360">InvariantGlobalization</span><span class="sxs-lookup"><span data-stu-id="04b33-360">InvariantGlobalization</span></span>](#invariantglobalization)
- [<span data-ttu-id="04b33-361">RetainVMGarbageCollection</span><span class="sxs-lookup"><span data-stu-id="04b33-361">RetainVMGarbageCollection</span></span>](#retainvmgarbagecollection)
- [<span data-ttu-id="04b33-362">ServerGarbageCollection</span><span class="sxs-lookup"><span data-stu-id="04b33-362">ServerGarbageCollection</span></span>](#servergarbagecollection)
- [<span data-ttu-id="04b33-363">ThreadPoolMaxThreads</span><span class="sxs-lookup"><span data-stu-id="04b33-363">ThreadPoolMaxThreads</span></span>](#threadpoolmaxthreads)
- [<span data-ttu-id="04b33-364">ThreadPoolMinThreads</span><span class="sxs-lookup"><span data-stu-id="04b33-364">ThreadPoolMinThreads</span></span>](#threadpoolminthreads)
- [<span data-ttu-id="04b33-365">TieredCompilation</span><span class="sxs-lookup"><span data-stu-id="04b33-365">TieredCompilation</span></span>](#tieredcompilation)
- [<span data-ttu-id="04b33-366">TieredCompilationQuickJit</span><span class="sxs-lookup"><span data-stu-id="04b33-366">TieredCompilationQuickJit</span></span>](#tieredcompilationquickjit)
- [<span data-ttu-id="04b33-367">TieredCompilationQuickJitForLoops</span><span class="sxs-lookup"><span data-stu-id="04b33-367">TieredCompilationQuickJitForLoops</span></span>](#tieredcompilationquickjitforloops)

### <a name="concurrentgarbagecollection"></a><span data-ttu-id="04b33-368">ConcurrentGarbageCollection</span><span class="sxs-lookup"><span data-stu-id="04b33-368">ConcurrentGarbageCollection</span></span>

<span data-ttu-id="04b33-369">`ConcurrentGarbageCollection` 属性配置是否启用 [后台（并发）垃圾回收](../../standard/garbage-collection/background-gc.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-369">The `ConcurrentGarbageCollection` property configures whether [background (concurrent) garbage collection](../../standard/garbage-collection/background-gc.md) is enabled.</span></span> <span data-ttu-id="04b33-370">将值设置为 `false` 以禁用后台垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="04b33-370">Set the value to `false` to disable background garbage collection.</span></span> <span data-ttu-id="04b33-371">有关详细信息，请参阅[后台 GC](../run-time-config/garbage-collector.md#background-gc)。</span><span class="sxs-lookup"><span data-stu-id="04b33-371">For more information, see [Background GC](../run-time-config/garbage-collector.md#background-gc).</span></span>

```xml
<PropertyGroup>
  <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
</PropertyGroup>
```

### <a name="invariantglobalization"></a><span data-ttu-id="04b33-372">InvariantGlobalization</span><span class="sxs-lookup"><span data-stu-id="04b33-372">InvariantGlobalization</span></span>

<span data-ttu-id="04b33-373">`InvariantGlobalization` 属性配置应用是否在全球化固定模式下运行，这意味着它无权访问特定于区域性的数据。</span><span class="sxs-lookup"><span data-stu-id="04b33-373">The `InvariantGlobalization` property configures whether the app runs in *globalization-invariant* mode, which means it doesn't have access to culture-specific data.</span></span> <span data-ttu-id="04b33-374">将值设置为 `true` 以在全球化固定模式下运行。</span><span class="sxs-lookup"><span data-stu-id="04b33-374">Set the value to `true` to run in globalization-invariant mode.</span></span> <span data-ttu-id="04b33-375">有关详细信息，请参阅[固定模式](../run-time-config/globalization.md#invariant-mode)。</span><span class="sxs-lookup"><span data-stu-id="04b33-375">For more information, see [Invariant mode](../run-time-config/globalization.md#invariant-mode).</span></span>

```xml
<PropertyGroup>
  <InvariantGlobalization>true</InvariantGlobalization>
</PropertyGroup>
```

### <a name="retainvmgarbagecollection"></a><span data-ttu-id="04b33-376">RetainVMGarbageCollection</span><span class="sxs-lookup"><span data-stu-id="04b33-376">RetainVMGarbageCollection</span></span>

<span data-ttu-id="04b33-377">`RetainVMGarbageCollection` 属性配置垃圾回收器，以将已删除的内存段放置在备用列表上供将来使用或释放它们。</span><span class="sxs-lookup"><span data-stu-id="04b33-377">The `RetainVMGarbageCollection` property configures the garbage collector to put deleted memory segments on a standby list for future use or release them.</span></span> <span data-ttu-id="04b33-378">将值设置为 `true` 会告知垃圾回收器将段放在备用列表上。</span><span class="sxs-lookup"><span data-stu-id="04b33-378">Setting the value to `true` tells the garbage collector to put the segments on a standby list.</span></span> <span data-ttu-id="04b33-379">有关详细信息，请参阅[保留 VM](../run-time-config/garbage-collector.md#retain-vm)。</span><span class="sxs-lookup"><span data-stu-id="04b33-379">For more information, see [Retain VM](../run-time-config/garbage-collector.md#retain-vm).</span></span>

```xml
<PropertyGroup>
  <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
</PropertyGroup>
```

### <a name="servergarbagecollection"></a><span data-ttu-id="04b33-380">ServerGarbageCollection</span><span class="sxs-lookup"><span data-stu-id="04b33-380">ServerGarbageCollection</span></span>

<span data-ttu-id="04b33-381">`ServerGarbageCollection` 属性配置应用程序是使用[工作站垃圾回收还是服务器垃圾回收](../../standard/garbage-collection/workstation-server-gc.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-381">The `ServerGarbageCollection` property configures whether the application uses [workstation garbage collection or server garbage collection](../../standard/garbage-collection/workstation-server-gc.md).</span></span> <span data-ttu-id="04b33-382">将值设置为 `true` 以使用服务器垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="04b33-382">Set the value to `true` to use server garbage collection.</span></span> <span data-ttu-id="04b33-383">有关详细信息，请参阅[工作站与服务器](../run-time-config/garbage-collector.md#workstation-vs-server)。</span><span class="sxs-lookup"><span data-stu-id="04b33-383">For more information, see [Workstation vs. server](../run-time-config/garbage-collector.md#workstation-vs-server).</span></span>

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

### <a name="threadpoolmaxthreads"></a><span data-ttu-id="04b33-384">ThreadPoolMaxThreads</span><span class="sxs-lookup"><span data-stu-id="04b33-384">ThreadPoolMaxThreads</span></span>

<span data-ttu-id="04b33-385">`ThreadPoolMaxThreads` 属性配置工作线程池的最大线程数。</span><span class="sxs-lookup"><span data-stu-id="04b33-385">The `ThreadPoolMaxThreads` property configures the maximum number of threads for the worker thread pool.</span></span> <span data-ttu-id="04b33-386">有关详细信息，请参阅[最大线程数](../run-time-config/threading.md#maximum-threads)。</span><span class="sxs-lookup"><span data-stu-id="04b33-386">For more information, see [Maximum threads](../run-time-config/threading.md#maximum-threads).</span></span>

```xml
<PropertyGroup>
  <ThreadPoolMaxThreads>20</ThreadPoolMaxThreads>
</PropertyGroup>
```

### <a name="threadpoolminthreads"></a><span data-ttu-id="04b33-387">ThreadPoolMinThreads</span><span class="sxs-lookup"><span data-stu-id="04b33-387">ThreadPoolMinThreads</span></span>

<span data-ttu-id="04b33-388">`ThreadPoolMinThreads` 属性配置工作线程池的最小线程数。</span><span class="sxs-lookup"><span data-stu-id="04b33-388">The `ThreadPoolMinThreads` property configures the minimum number of threads for the worker thread pool.</span></span> <span data-ttu-id="04b33-389">有关详细信息，请参阅[最小线程数](../run-time-config/threading.md#minimum-threads)。</span><span class="sxs-lookup"><span data-stu-id="04b33-389">For more information, see [Minimum threads](../run-time-config/threading.md#minimum-threads).</span></span>

```xml
<PropertyGroup>
  <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
</PropertyGroup>
```

### <a name="tieredcompilation"></a><span data-ttu-id="04b33-390">TieredCompilation</span><span class="sxs-lookup"><span data-stu-id="04b33-390">TieredCompilation</span></span>

<span data-ttu-id="04b33-391">`TieredCompilation` 属性配置实时 (JIT) 编译器是否使用[分层编译](../whats-new/dotnet-core-3-0.md#tiered-compilation)。</span><span class="sxs-lookup"><span data-stu-id="04b33-391">The `TieredCompilation` property configures whether the just-in-time (JIT) compiler uses [tiered compilation](../whats-new/dotnet-core-3-0.md#tiered-compilation).</span></span> <span data-ttu-id="04b33-392">将值设置为 `false` 以禁用分层编译。</span><span class="sxs-lookup"><span data-stu-id="04b33-392">Set the value to `false` to disable tiered compilation.</span></span> <span data-ttu-id="04b33-393">有关详细信息，请参阅[分层编译](../run-time-config/compilation.md#tiered-compilation)。</span><span class="sxs-lookup"><span data-stu-id="04b33-393">For more information, see [Tiered compilation](../run-time-config/compilation.md#tiered-compilation).</span></span>

```xml
<PropertyGroup>
  <TieredCompilation>false</TieredCompilation>
</PropertyGroup>
```

### <a name="tieredcompilationquickjit"></a><span data-ttu-id="04b33-394">TieredCompilationQuickJit</span><span class="sxs-lookup"><span data-stu-id="04b33-394">TieredCompilationQuickJit</span></span>

<span data-ttu-id="04b33-395">`TieredCompilationQuickJit` 属性配置 JIT 编译器是否使用快速 JIT。</span><span class="sxs-lookup"><span data-stu-id="04b33-395">The `TieredCompilationQuickJit` property configures whether the JIT compiler uses quick JIT.</span></span> <span data-ttu-id="04b33-396">将值设置为 `false` 以禁用快速 JIT。</span><span class="sxs-lookup"><span data-stu-id="04b33-396">Set the value to `false` to disable quick JIT.</span></span> <span data-ttu-id="04b33-397">有关详细信息，请参阅[快速 JIT](../run-time-config/compilation.md#quick-jit)。</span><span class="sxs-lookup"><span data-stu-id="04b33-397">For more information, see [Quick JIT](../run-time-config/compilation.md#quick-jit).</span></span>

```xml
<PropertyGroup>
  <TieredCompilationQuickJit>false</TieredCompilationQuickJit>
</PropertyGroup>
```

### <a name="tieredcompilationquickjitforloops"></a><span data-ttu-id="04b33-398">TieredCompilationQuickJitForLoops</span><span class="sxs-lookup"><span data-stu-id="04b33-398">TieredCompilationQuickJitForLoops</span></span>

<span data-ttu-id="04b33-399">`TieredCompilationQuickJitForLoops` 配置 JIT 编译器是否对包含循环的方法使用快速 JIT。</span><span class="sxs-lookup"><span data-stu-id="04b33-399">The `TieredCompilationQuickJitForLoops` property configures whether the JIT compiler uses quick JIT on methods that contain loops.</span></span> <span data-ttu-id="04b33-400">将值设置为 `true` 以对包含循环的方法启用快速 JIT。</span><span class="sxs-lookup"><span data-stu-id="04b33-400">Set the value to `true` to enable quick JIT on methods that contain loops.</span></span> <span data-ttu-id="04b33-401">有关详细信息，请参阅[适用于循环的快速 JIT](../run-time-config/compilation.md#quick-jit-for-loops)。</span><span class="sxs-lookup"><span data-stu-id="04b33-401">For more information, see [Quick JIT for loops](../run-time-config/compilation.md#quick-jit-for-loops).</span></span>

```xml
<PropertyGroup>
  <TieredCompilationQuickJitForLoops>true</TieredCompilationQuickJitForLoops>
</PropertyGroup>
```

## <a name="reference-properties"></a><span data-ttu-id="04b33-402">引用属性</span><span class="sxs-lookup"><span data-stu-id="04b33-402">Reference properties</span></span>

<span data-ttu-id="04b33-403">本节收录了以下 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="04b33-403">The following MSBuild properties are documented in this section:</span></span>

- [<span data-ttu-id="04b33-404">AssetTargetFallback</span><span class="sxs-lookup"><span data-stu-id="04b33-404">AssetTargetFallback</span></span>](#assettargetfallback)
- [<span data-ttu-id="04b33-405">DisableImplicitFrameworkReferences</span><span class="sxs-lookup"><span data-stu-id="04b33-405">DisableImplicitFrameworkReferences</span></span>](#disableimplicitframeworkreferences)
- [<span data-ttu-id="04b33-406">与还原相关的属性</span><span class="sxs-lookup"><span data-stu-id="04b33-406">Restore-related properties</span></span>](#restore-related-properties)

### <a name="assettargetfallback"></a><span data-ttu-id="04b33-407">AssetTargetFallback</span><span class="sxs-lookup"><span data-stu-id="04b33-407">AssetTargetFallback</span></span>

<span data-ttu-id="04b33-408">使用 `AssetTargetFallback` 属性，可以为项目引用和 NuGet 包指定其他兼容的框架版本。</span><span class="sxs-lookup"><span data-stu-id="04b33-408">The `AssetTargetFallback` property lets you specify additional compatible framework versions for project references and NuGet packages.</span></span> <span data-ttu-id="04b33-409">例如，如果使用 `PackageReference` 指定包依赖项，但该包不包含与项目的 `TargetFramework` 兼容的资源，则可使用 `AssetTargetFallback` 属性。</span><span class="sxs-lookup"><span data-stu-id="04b33-409">For example, if you specify a package dependency using `PackageReference` but that package doesn't contain assets that are compatible with your projects's `TargetFramework`, the `AssetTargetFallback` property comes into play.</span></span> <span data-ttu-id="04b33-410">使用 `AssetTargetFallback` 中指定的每个目标框架重新检查引用包的兼容性。</span><span class="sxs-lookup"><span data-stu-id="04b33-410">The compatibility of the referenced package is rechecked using each target framework that's specified in `AssetTargetFallback`.</span></span> <span data-ttu-id="04b33-411">此属性替换已弃用的属性 `PackageTargetFallback`。</span><span class="sxs-lookup"><span data-stu-id="04b33-411">This property replaces the deprecated property `PackageTargetFallback`.</span></span>

<span data-ttu-id="04b33-412">可以将 `AssetTargetFallback` 属性设置为一个或多个[目标框架版本](../../standard/frameworks.md#supported-target-frameworks)。</span><span class="sxs-lookup"><span data-stu-id="04b33-412">You can set the `AssetTargetFallback` property to one or more [target framework versions](../../standard/frameworks.md#supported-target-frameworks).</span></span>

```xml
<PropertyGroup>
  <AssetTargetFallback>net461</AssetTargetFallback>
</PropertyGroup>
```

### <a name="disableimplicitframeworkreferences"></a><span data-ttu-id="04b33-413">DisableImplicitFrameworkReferences</span><span class="sxs-lookup"><span data-stu-id="04b33-413">DisableImplicitFrameworkReferences</span></span>

<span data-ttu-id="04b33-414">面向 .NET Core 3.0 及更高版本时，`DisableImplicitFrameworkReferences` 属性会控制隐式 `FrameworkReference` 项。</span><span class="sxs-lookup"><span data-stu-id="04b33-414">The `DisableImplicitFrameworkReferences` property controls implicit `FrameworkReference` items when targeting .NET Core 3.0 and later versions.</span></span> <span data-ttu-id="04b33-415">面向 .NET Core 2.1 或 .NET Standard 2.0 及早期版本时，该属性会控制元包中包的隐式 [PackageReference](#packagereference) 项。</span><span class="sxs-lookup"><span data-stu-id="04b33-415">When targeting .NET Core 2.1 or .NET Standard 2.0 and earlier versions, it controls implicit [PackageReference](#packagereference) items to packages in a metapackage.</span></span> <span data-ttu-id="04b33-416">（元包是一种基于框架的包，其中仅包含对其他包的依赖项。）在面向 .NET Framework 时，此属性还控制隐式引用，如 `System` 和 `System.Core`。</span><span class="sxs-lookup"><span data-stu-id="04b33-416">(A metapackage is a framework-based package that consist only of dependencies on other packages.) This property also controls implicit references such as `System` and `System.Core` when targeting .NET Framework.</span></span>

<span data-ttu-id="04b33-417">将此属性设置为 `true` 以禁用隐式 `FrameworkReference` 或 [PackageReference](#packagereference) 项。</span><span class="sxs-lookup"><span data-stu-id="04b33-417">Set this property to `true` to disable implicit `FrameworkReference` or [PackageReference](#packagereference) items.</span></span> <span data-ttu-id="04b33-418">如果将此属性设置为 `true`，则可以仅添加对所需框架或包的显式引用。</span><span class="sxs-lookup"><span data-stu-id="04b33-418">If you set this property to `true`, you can add explicit references to just the frameworks or packages you need.</span></span>

```xml
<PropertyGroup>
  <DisableImplicitFrameworkReferences>true</DisableImplicitFrameworkReferences>
</PropertyGroup>
```

### <a name="restore-related-properties"></a><span data-ttu-id="04b33-419">与还原相关的属性</span><span class="sxs-lookup"><span data-stu-id="04b33-419">Restore-related properties</span></span>

<span data-ttu-id="04b33-420">还原被引用的包会安装它的所有直接依赖项，以及这些依赖项的全部依赖项。</span><span class="sxs-lookup"><span data-stu-id="04b33-420">Restoring a referenced package installs all of its direct dependencies and all the dependencies of those dependencies.</span></span> <span data-ttu-id="04b33-421">可以通过指定 `RestorePackagesPath` 和 `RestoreIgnoreFailedSources` 等属性来自定义包还原。</span><span class="sxs-lookup"><span data-stu-id="04b33-421">You can customize package restoration by specifying properties such as `RestorePackagesPath` and `RestoreIgnoreFailedSources`.</span></span> <span data-ttu-id="04b33-422">若要详细了解这些属性和其他属性，请参阅[还原目标](/nuget/reference/msbuild-targets#restore-target)。</span><span class="sxs-lookup"><span data-stu-id="04b33-422">For more information about these and other properties, see [restore target](/nuget/reference/msbuild-targets#restore-target).</span></span>

```xml
<PropertyGroup>
  <RestoreIgnoreFailedSource>true</RestoreIgnoreFailedSource>
</PropertyGroup>
```

## <a name="run-related-properties"></a><span data-ttu-id="04b33-423">与运行相关的属性</span><span class="sxs-lookup"><span data-stu-id="04b33-423">Run-related properties</span></span>

<span data-ttu-id="04b33-424">以下属性用于使用 [`dotnet run`](../tools/dotnet-run.md) 命令启动应用：</span><span class="sxs-lookup"><span data-stu-id="04b33-424">The following properties are used for launching an app with the [`dotnet run`](../tools/dotnet-run.md) command:</span></span>

- [<span data-ttu-id="04b33-425">RunArguments</span><span class="sxs-lookup"><span data-stu-id="04b33-425">RunArguments</span></span>](#runarguments)
- [<span data-ttu-id="04b33-426">RunWorkingDirectory</span><span class="sxs-lookup"><span data-stu-id="04b33-426">RunWorkingDirectory</span></span>](#runworkingdirectory)

### <a name="runarguments"></a><span data-ttu-id="04b33-427">RunArguments</span><span class="sxs-lookup"><span data-stu-id="04b33-427">RunArguments</span></span>

<span data-ttu-id="04b33-428">`RunArguments` 属性定义了在应用运行时向其传递的参数。</span><span class="sxs-lookup"><span data-stu-id="04b33-428">The `RunArguments` property defines the arguments that are passed to the app when it is run.</span></span>

```xml
<PropertyGroup>
  <RunArguments>-mode dryrun</RunArguments>
</PropertyGroup>
```

> [!TIP]
> <span data-ttu-id="04b33-429">可以使用 [`dotnet run` 的 `--` 选项](../tools/dotnet-run.md#options)来指定要传递到应用的其他参数。</span><span class="sxs-lookup"><span data-stu-id="04b33-429">You can specify additional arguments to be passed to the app by using the [`--` option for `dotnet run`](../tools/dotnet-run.md#options).</span></span>

### <a name="runworkingdirectory"></a><span data-ttu-id="04b33-430">RunWorkingDirectory</span><span class="sxs-lookup"><span data-stu-id="04b33-430">RunWorkingDirectory</span></span>

<span data-ttu-id="04b33-431">`RunWorkingDirectory` 属性定义要用于启动应用程序进程的工作目录。</span><span class="sxs-lookup"><span data-stu-id="04b33-431">The `RunWorkingDirectory` property defines the working directory for the application process to be started in.</span></span> <span data-ttu-id="04b33-432">它可以是绝对路径，也可以是相对于项目目录的路径。</span><span class="sxs-lookup"><span data-stu-id="04b33-432">It can be an absolute path or a path that's relative to the project directory.</span></span> <span data-ttu-id="04b33-433">如果未指定目录，`OutDir` 将用作工作目录。</span><span class="sxs-lookup"><span data-stu-id="04b33-433">If you don't specify a directory, `OutDir` is used as the working directory.</span></span>

```xml
<PropertyGroup>
  <RunWorkingDirectory>c:\temp</RunWorkingDirectory>
</PropertyGroup>
```

## <a name="hosting-related-properties"></a><span data-ttu-id="04b33-434">与托管相关的属性</span><span class="sxs-lookup"><span data-stu-id="04b33-434">Hosting-related properties</span></span>

<span data-ttu-id="04b33-435">本节收录了以下 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="04b33-435">The following MSBuild properties are documented in this section:</span></span>

- [<span data-ttu-id="04b33-436">EnableComHosting</span><span class="sxs-lookup"><span data-stu-id="04b33-436">EnableComHosting</span></span>](#enablecomhosting)
- [<span data-ttu-id="04b33-437">EnableDynamicLoading</span><span class="sxs-lookup"><span data-stu-id="04b33-437">EnableDynamicLoading</span></span>](#enabledynamicloading)

### <a name="enablecomhosting"></a><span data-ttu-id="04b33-438">EnableComHosting</span><span class="sxs-lookup"><span data-stu-id="04b33-438">EnableComHosting</span></span>

<span data-ttu-id="04b33-439">`EnableComHosting` 属性表示程序集提供了 COM 服务器。</span><span class="sxs-lookup"><span data-stu-id="04b33-439">The `EnableComHosting` property indicates that an assembly provides a COM server.</span></span> <span data-ttu-id="04b33-440">将 `EnableComHosting` 设置为 `true` 也表明 [EnableDynamicLoading](#enabledynamicloading) 为 `true`。</span><span class="sxs-lookup"><span data-stu-id="04b33-440">Setting the `EnableComHosting` to `true` also implies that [EnableDynamicLoading](#enabledynamicloading) is `true`.</span></span>

```xml
<PropertyGroup>
  <EnableComHosting>True</EnableComHosting>
</PropertyGroup>
```

<span data-ttu-id="04b33-441">有关详细信息，请参阅[向 COM 公开 .NET 组件](../native-interop/expose-components-to-com.md)。</span><span class="sxs-lookup"><span data-stu-id="04b33-441">For more information, see [Expose .NET components to COM](../native-interop/expose-components-to-com.md).</span></span>

### <a name="enabledynamicloading"></a><span data-ttu-id="04b33-442">EnableDynamicLoading</span><span class="sxs-lookup"><span data-stu-id="04b33-442">EnableDynamicLoading</span></span>

<span data-ttu-id="04b33-443">`EnableDynamicLoading` 属性指示程序集是动态加载的组件。</span><span class="sxs-lookup"><span data-stu-id="04b33-443">The `EnableDynamicLoading` property indicates that an assembly is a dynamically loaded component.</span></span> <span data-ttu-id="04b33-444">组件可以是 [COM 库](/windows/win32/com/the-component-object-model)，也可以是[在本机主机中使用的](../tutorials/netcore-hosting.md)非 COM 库。</span><span class="sxs-lookup"><span data-stu-id="04b33-444">The component could be a [COM library](/windows/win32/com/the-component-object-model) or a non-COM library that can be [used from a native host](../tutorials/netcore-hosting.md).</span></span> <span data-ttu-id="04b33-445">将此属性设置为 `true` 会产生以下结果：</span><span class="sxs-lookup"><span data-stu-id="04b33-445">Setting this property to `true` has the following effects:</span></span>

- <span data-ttu-id="04b33-446">生成 runtimeconfig.json 文件。</span><span class="sxs-lookup"><span data-stu-id="04b33-446">A *.runtimeconfig.json* file is generated.</span></span>
- <span data-ttu-id="04b33-447">[前滚](../whats-new/dotnet-core-3-0.md#major-version-runtime-roll-forward)设置为 `LatestMinor`。</span><span class="sxs-lookup"><span data-stu-id="04b33-447">[Roll forward](../whats-new/dotnet-core-3-0.md#major-version-runtime-roll-forward) is set to `LatestMinor`.</span></span>
- <span data-ttu-id="04b33-448">在本地复制 NuGet 引用。</span><span class="sxs-lookup"><span data-stu-id="04b33-448">NuGet references are copied locally.</span></span>

```xml
<PropertyGroup>
  <EnableDynamicLoading>true</EnableDynamicLoading>
</PropertyGroup>
```

## <a name="items"></a><span data-ttu-id="04b33-449">项</span><span class="sxs-lookup"><span data-stu-id="04b33-449">Items</span></span>

<span data-ttu-id="04b33-450">[MSBuild 项](/visualstudio/msbuild/msbuild-items)是生成系统的输入。</span><span class="sxs-lookup"><span data-stu-id="04b33-450">[MSBuild items](/visualstudio/msbuild/msbuild-items) are inputs into the build system.</span></span> <span data-ttu-id="04b33-451">根据项的类型（即元素名称）指定项。</span><span class="sxs-lookup"><span data-stu-id="04b33-451">Items are specified according to their type, which is the element name.</span></span> <span data-ttu-id="04b33-452">例如，`Compile` 和 `Reference` 是两个[常见项类型](/visualstudio/msbuild/common-msbuild-project-items)。</span><span class="sxs-lookup"><span data-stu-id="04b33-452">For example, `Compile` and `Reference` are two [common item types](/visualstudio/msbuild/common-msbuild-project-items).</span></span> <span data-ttu-id="04b33-453">.NET SDK 提供了以下附加项类型：</span><span class="sxs-lookup"><span data-stu-id="04b33-453">The following additional item types are made available by the .NET SDK:</span></span>

- [<span data-ttu-id="04b33-454">PackageReference</span><span class="sxs-lookup"><span data-stu-id="04b33-454">PackageReference</span></span>](#packagereference)
- [<span data-ttu-id="04b33-455">TrimmerRootAssembly</span><span class="sxs-lookup"><span data-stu-id="04b33-455">TrimmerRootAssembly</span></span>](#trimmerrootassembly)

<span data-ttu-id="04b33-456">你可以在这些项上使用任何标准的[项目属性](/visualstudio/msbuild/item-element-msbuild#attributes-and-elements)，例如 `Include` 和 `Update`。</span><span class="sxs-lookup"><span data-stu-id="04b33-456">You can use any of the standard [item attributes](/visualstudio/msbuild/item-element-msbuild#attributes-and-elements), for example, `Include` and `Update`, on these items.</span></span> <span data-ttu-id="04b33-457">使用 `Include` 添加新项，使用 `Update` 修改现有项。</span><span class="sxs-lookup"><span data-stu-id="04b33-457">Use `Include` to add a new item, and use `Update` to modify an existing item.</span></span> <span data-ttu-id="04b33-458">例如，`Update` 通常用于修改由 .NET SDK 隐式添加的项。</span><span class="sxs-lookup"><span data-stu-id="04b33-458">For example, `Update` is often used to modify an item that has implicitly been added by the .NET SDK.</span></span>

### <a name="packagereference"></a><span data-ttu-id="04b33-459">PackageReference</span><span class="sxs-lookup"><span data-stu-id="04b33-459">PackageReference</span></span>

<span data-ttu-id="04b33-460">`PackageReference` 项定义了对 NuGet 包的引用。</span><span class="sxs-lookup"><span data-stu-id="04b33-460">The `PackageReference` item defines a reference to a NuGet package.</span></span>

<span data-ttu-id="04b33-461">`Include` 属性指定包 ID。</span><span class="sxs-lookup"><span data-stu-id="04b33-461">The `Include` attribute specifies the package ID.</span></span> <span data-ttu-id="04b33-462">`Version` 特性指定版本或版本范围。</span><span class="sxs-lookup"><span data-stu-id="04b33-462">The `Version` attribute specifies the version or version range.</span></span> <span data-ttu-id="04b33-463">若要了解如何指定最低版本、最高版本、范围或完全匹配，请参阅[版本范围](/nuget/concepts/package-versioning#version-ranges)。</span><span class="sxs-lookup"><span data-stu-id="04b33-463">For information about how to specify a minimum version, maximum version, range, or exact match, see [Version ranges](/nuget/concepts/package-versioning#version-ranges).</span></span>

<span data-ttu-id="04b33-464">以下示例中的项目文件片段引用 [System.Runtime](https://www.nuget.org/packages/System.Runtime/) 包。</span><span class="sxs-lookup"><span data-stu-id="04b33-464">The project file snippet in the following example references the [System.Runtime](https://www.nuget.org/packages/System.Runtime/) package.</span></span>

```xml
<ItemGroup>
  <PackageReference Include="System.Runtime" Version="4.3.0" />
</ItemGroup>
```

<span data-ttu-id="04b33-465">你还可以使用元数据（例如 `PrivateAssets`）来[控制依赖项资产](/nuget/consume-packages/package-references-in-project-files#controlling-dependency-assets)。</span><span class="sxs-lookup"><span data-stu-id="04b33-465">You can also [control dependency assets](/nuget/consume-packages/package-references-in-project-files#controlling-dependency-assets) using metadata such as `PrivateAssets`.</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0">
    <PrivateAssets>all</PrivateAssets>
  </PackageReference>
</ItemGroup>
```

<span data-ttu-id="04b33-466">有关详细信息，请参阅[项目文件中的包引用](/nuget/consume-packages/package-references-in-project-files)。</span><span class="sxs-lookup"><span data-stu-id="04b33-466">For more information, see [Package references in project files](/nuget/consume-packages/package-references-in-project-files).</span></span>

### <a name="trimmerrootassembly"></a><span data-ttu-id="04b33-467">TrimmerRootAssembly</span><span class="sxs-lookup"><span data-stu-id="04b33-467">TrimmerRootAssembly</span></span>

<span data-ttu-id="04b33-468">`TrimmerRootAssembly` 项允许通过[修整](../deploying/trim-self-contained.md)排除程序集。</span><span class="sxs-lookup"><span data-stu-id="04b33-468">The `TrimmerRootAssembly` item lets you exclude an assembly from [*trimming*](../deploying/trim-self-contained.md).</span></span> <span data-ttu-id="04b33-469">修整是从打包的应用程序中删除运行时未使用部分的过程。</span><span class="sxs-lookup"><span data-stu-id="04b33-469">Trimming is the process of removing unused parts of the runtime from a packaged application.</span></span> <span data-ttu-id="04b33-470">在某些情况下，修整可能会错误地删除所需的引用。</span><span class="sxs-lookup"><span data-stu-id="04b33-470">In some cases, trimming might incorrectly remove required references.</span></span>

<span data-ttu-id="04b33-471">以下 XML 通过修整排除 `System.Security` 程序集。</span><span class="sxs-lookup"><span data-stu-id="04b33-471">The following XML excludes the `System.Security` assembly from trimming.</span></span>

```xml
<ItemGroup>
  <TrimmerRootAssembly Include="System.Security" />
</ItemGroup>
```

## <a name="item-metadata"></a><span data-ttu-id="04b33-472">项元数据</span><span class="sxs-lookup"><span data-stu-id="04b33-472">Item metadata</span></span>

<span data-ttu-id="04b33-473">除了标准的 [MSBuild 项目属性](/visualstudio/msbuild/item-element-msbuild#attributes-and-elements)之外，.net SDK 还提供以下项元数据标记：</span><span class="sxs-lookup"><span data-stu-id="04b33-473">In addition to the standard [MSBuild item attributes](/visualstudio/msbuild/item-element-msbuild#attributes-and-elements), the following item metadata tags are made available by the .NET SDK:</span></span>

- [<span data-ttu-id="04b33-474">CopyToPublishDirectory</span><span class="sxs-lookup"><span data-stu-id="04b33-474">CopyToPublishDirectory</span></span>](#copytopublishdirectory)
- [<span data-ttu-id="04b33-475">LinkBase</span><span class="sxs-lookup"><span data-stu-id="04b33-475">LinkBase</span></span>](#linkbase)

### <a name="copytopublishdirectory"></a><span data-ttu-id="04b33-476">CopyToPublishDirectory</span><span class="sxs-lookup"><span data-stu-id="04b33-476">CopyToPublishDirectory</span></span>

<span data-ttu-id="04b33-477">MSBuild 项上的 `CopyToPublishDirectory` 元数据控制何时将项复制到发布目录。</span><span class="sxs-lookup"><span data-stu-id="04b33-477">The `CopyToPublishDirectory` metadata on an MSBuild item controls when the item is copied to the publish directory.</span></span> <span data-ttu-id="04b33-478">允许的值为 `PreserveNewest`（仅在项已更改时复制项）、`Always`（始终复制项）和 `Never`（从不复制项）。</span><span class="sxs-lookup"><span data-stu-id="04b33-478">Allowable values are `PreserveNewest`, which only copies the item if it has changed, `Always`, which always copies the item, and `Never`, which never copies the item.</span></span> <span data-ttu-id="04b33-479">从性能角度来看，`PreserveNewest` 更为可取，因为它可实现增量生成。</span><span class="sxs-lookup"><span data-stu-id="04b33-479">From a performance standpoint, `PreserveNewest` is preferable because it enables an incremental build.</span></span>

```xml
<ItemGroup>
  <None Update="appsettings.Development.json" CopyToOutputDirectory="PreserveNewest" CopyToPublishDirectory="PreserveNewest" />
</ItemGroup>
```

### <a name="linkbase"></a><span data-ttu-id="04b33-480">LinkBase</span><span class="sxs-lookup"><span data-stu-id="04b33-480">LinkBase</span></span>

<span data-ttu-id="04b33-481">对于项目目录及其子目录之外的项，发布目标使用项的[链接元数据](/visualstudio/msbuild/common-msbuild-item-metadata)来确定要将项复制到的位置。</span><span class="sxs-lookup"><span data-stu-id="04b33-481">For an item that's outside of the project directory and its subdirectories, the publish target uses the item's [Link metadata](/visualstudio/msbuild/common-msbuild-item-metadata) to determine where to copy the item to.</span></span> <span data-ttu-id="04b33-482">`Link` 还将确定项目树外的项在 Visual Studio 的“解决方案资源管理器”窗口中的显示方式。</span><span class="sxs-lookup"><span data-stu-id="04b33-482">`Link` also determines how items outside of the project tree are displayed in the Solution Explorer window of Visual Studio.</span></span>

<span data-ttu-id="04b33-483">如果没有为项目圆锥之外的项指定 `Link`，则默认为 `%(LinkBase)\%(RecursiveDir)%(Filename)%(Extension)`。</span><span class="sxs-lookup"><span data-stu-id="04b33-483">If `Link` is not specified for an item that's outside of the project cone, it defaults to `%(LinkBase)\%(RecursiveDir)%(Filename)%(Extension)`.</span></span> <span data-ttu-id="04b33-484">通过 `LinkBase`，可以为项目圆锥之外的项指定一个合理的基础文件夹。</span><span class="sxs-lookup"><span data-stu-id="04b33-484">`LinkBase` lets you specify a sensible base folder for items outside of the project cone.</span></span> <span data-ttu-id="04b33-485">基础文件夹下的文件夹层次结构通过 `RecursiveDir` 保留。</span><span class="sxs-lookup"><span data-stu-id="04b33-485">The folder hierarchy under the base folder is preserved via `RecursiveDir`.</span></span> <span data-ttu-id="04b33-486">如果未指定 `LinkBase`，则将从 `Link` 路径中省略它。</span><span class="sxs-lookup"><span data-stu-id="04b33-486">If `LinkBase` is not specified, it's omitted from the `Link` path.</span></span>

```xml
<ItemGroup>
  <Content Include="..\Extras\**\*.cs" LinkBase="Shared"/>
</ItemGroup>
```

<span data-ttu-id="04b33-487">下图显示通过上一个项 `Include` glob 包含的文件在解决方案资源管理器中的显示方式。</span><span class="sxs-lookup"><span data-stu-id="04b33-487">The following image shows how a file that's included via the previous item `Include` glob displays in Solution Explorer.</span></span>

:::image type="content" source="media/solution-explorer-linkbase.png" alt-text="解决方案资源管理器，显示具有 LinkBase 元数据的项。":::

## <a name="see-also"></a><span data-ttu-id="04b33-489">请参阅</span><span class="sxs-lookup"><span data-stu-id="04b33-489">See also</span></span>

- [<span data-ttu-id="04b33-490">MSBuild 架构引用</span><span class="sxs-lookup"><span data-stu-id="04b33-490">MSBuild schema reference</span></span>](/visualstudio/msbuild/msbuild-project-file-schema-reference)
- [<span data-ttu-id="04b33-491">通用 MSBuild 属性</span><span class="sxs-lookup"><span data-stu-id="04b33-491">Common MSBuild properties</span></span>](/visualstudio/msbuild/common-msbuild-project-properties)
- [<span data-ttu-id="04b33-492">NuGet 包的 MSBuild 属性</span><span class="sxs-lookup"><span data-stu-id="04b33-492">MSBuild properties for NuGet pack</span></span>](/nuget/reference/msbuild-targets#pack-target)
- [<span data-ttu-id="04b33-493">NuGet 还原的 MSBuild 属性</span><span class="sxs-lookup"><span data-stu-id="04b33-493">MSBuild properties for NuGet restore</span></span>](/nuget/reference/msbuild-targets#restore-properties)
- [<span data-ttu-id="04b33-494">自定义生成</span><span class="sxs-lookup"><span data-stu-id="04b33-494">Customize a build</span></span>](/visualstudio/msbuild/customize-your-build)
