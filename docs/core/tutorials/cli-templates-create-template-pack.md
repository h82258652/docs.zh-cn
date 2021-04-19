---
title: 为 dotnet new 创建模板包
description: 了解如何创建一个 csproj 文件，该文件将为 dotnet new 命令生成模板包。
author: adegeo
ms.date: 03/26/2021
ms.topic: tutorial
ms.author: adegeo
ms.openlocfilehash: 343104f9609c59e7da24f857de6a7fc29803e2df
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255384"
---
# <a name="tutorial-create-a-template-pack"></a><span data-ttu-id="e9ae4-103">教程：创建模板包</span><span class="sxs-lookup"><span data-stu-id="e9ae4-103">Tutorial: Create a template pack</span></span>

<span data-ttu-id="e9ae4-104">使用 .NET，可以创建和部署可生成项目、文件甚至资源的模板。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-104">With .NET, you can create and deploy templates that generate projects, files, even resources.</span></span> <span data-ttu-id="e9ae4-105">本教程是系列教程的第三部分，介绍如何创建、安装和卸载用于 `dotnet new` 命令的模板。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-105">This tutorial is part three of a series that teaches you how to create, install, and uninstall templates for use with the `dotnet new` command.</span></span>

<span data-ttu-id="e9ae4-106">在本系列的这一部分中，你将了解如何：</span><span class="sxs-lookup"><span data-stu-id="e9ae4-106">In this part of the series you'll learn how to:</span></span>

> [!div class="checklist"]
>
> * <span data-ttu-id="e9ae4-107">创建一个 \*.csproj 项目以生成模板包</span><span class="sxs-lookup"><span data-stu-id="e9ae4-107">Create a \*.csproj project to build a template pack</span></span>
> * <span data-ttu-id="e9ae4-108">配置项目文件以进行打包</span><span class="sxs-lookup"><span data-stu-id="e9ae4-108">Configure the project file for packing</span></span>
> * <span data-ttu-id="e9ae4-109">从 NuGet 包文件安装模板</span><span class="sxs-lookup"><span data-stu-id="e9ae4-109">Install a template from a NuGet package file</span></span>
> * <span data-ttu-id="e9ae4-110">按包 ID 卸载模板</span><span class="sxs-lookup"><span data-stu-id="e9ae4-110">Uninstall a template by package ID</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9ae4-111">先决条件</span><span class="sxs-lookup"><span data-stu-id="e9ae4-111">Prerequisites</span></span>

* <span data-ttu-id="e9ae4-112">完成本系列教程的[第 1 部分](cli-templates-create-item-template.md)和[第 2 部分](cli-templates-create-project-template.md)。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-112">Complete [part 1](cli-templates-create-item-template.md) and [part 2](cli-templates-create-project-template.md) of this tutorial series.</span></span>

  <span data-ttu-id="e9ae4-113">本教程使用本教程前两部分中创建的两个模板。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-113">This tutorial uses the two templates created in the first two parts of this tutorial.</span></span> <span data-ttu-id="e9ae4-114">只要将不同的模板作为文件夹复制到 working\templates\\  文件夹中，就可以使用该模板。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-114">You can use a different template as long as you copy the template, as a folder, into the _working\templates\\_ folder.</span></span>

* <span data-ttu-id="e9ae4-115">打开终端并导航到 working\\  文件夹。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-115">Open a terminal and navigate to the _working\\_ folder.</span></span>

## <a name="create-a-template-pack-project"></a><span data-ttu-id="e9ae4-116">创建模板包项目</span><span class="sxs-lookup"><span data-stu-id="e9ae4-116">Create a template pack project</span></span>

<span data-ttu-id="e9ae4-117">模板包是打包到文件中的一个或多个模板。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-117">A template pack is one or more templates packaged into a file.</span></span> <span data-ttu-id="e9ae4-118">安装或卸载程序包时，将分别添加或删除包中包含的所有模板。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-118">When you install or uninstall a pack, all templates contained in the pack are added or removed, respectively.</span></span> <span data-ttu-id="e9ae4-119">本系列教程的前几部分仅适用于各自的模板。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-119">The previous parts of this tutorial series only worked with individual templates.</span></span> <span data-ttu-id="e9ae4-120">若要共享非打包模板，必须复制模板文件夹并通过该文件夹进行安装。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-120">To share a non-packed template, you have to copy the template folder and install via that folder.</span></span> <span data-ttu-id="e9ae4-121">由于模板包中可以包含多个模板，并且是单个文件，因此共享更容易。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-121">Because a template pack can have more than one template in it, and is a single file, sharing is easier.</span></span>

<span data-ttu-id="e9ae4-122">模板包由 NuGet 包 (.nupkg  ) 文件表示。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-122">Template packs are represented by a NuGet package (_.nupkg_) file.</span></span> <span data-ttu-id="e9ae4-123">并且，与任何 NuGet 包一样，可以将模板包上传到 NuGet 源。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-123">And, like any NuGet package, you can upload the template pack to a NuGet feed.</span></span> <span data-ttu-id="e9ae4-124">`dotnet new -i` 命令支持从 NuGet 包源安装模板包。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-124">The `dotnet new -i` command supports installing template pack from a NuGet package feed.</span></span> <span data-ttu-id="e9ae4-125">此外，可以直接从 .nupkg  文件安装模板包。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-125">Additionally, you can install a template pack from a _.nupkg_ file directly.</span></span>

<span data-ttu-id="e9ae4-126">通常情况下，使用 C# 项目文件来编译代码并生成二进制文件。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-126">Normally you use a C# project file to compile code and produce a binary.</span></span> <span data-ttu-id="e9ae4-127">但是，该项目也可用于生成模板包。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-127">However, the project can also be used to generate a template pack.</span></span> <span data-ttu-id="e9ae4-128">通过更改 .csproj  的设置，可以阻止它编译任何代码，而是将模板的所有资产都包含在内作为资源。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-128">By changing the settings of the _.csproj_, you can prevent it from compiling any code and instead include all the assets of your templates as resources.</span></span> <span data-ttu-id="e9ae4-129">生成此项目后，它会生成模板包 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-129">When this project is built, it produces a template pack NuGet package.</span></span>

<span data-ttu-id="e9ae4-130">将要创建的包将包含先前创建的[项模板](cli-templates-create-item-template.md)和[包模板](cli-templates-create-project-template.md)。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-130">The pack you'll create will include the [item template](cli-templates-create-item-template.md) and [package template](cli-templates-create-project-template.md) previously created.</span></span> <span data-ttu-id="e9ae4-131">由于我们将两个模板分组到 working\templates\\  文件夹中，因此可以使用 .csproj  文件的 working  文件夹。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-131">Because we grouped the two templates into the _working\templates\\_ folder, we can use the _working_ folder for the _.csproj_ file.</span></span>

<span data-ttu-id="e9ae4-132">在终端中，导航到 working  文件夹。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-132">In your terminal, navigate to the _working_ folder.</span></span> <span data-ttu-id="e9ae4-133">创建一个新项目，将名称设置为 `templatepack`，并将输出文件夹设置为当前文件夹。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-133">Create a new project and set the name to `templatepack` and the output folder to the current folder.</span></span>

```dotnetcli
dotnet new console -n templatepack -o .
```

<span data-ttu-id="e9ae4-134">`-n` 参数将 .csproj  文件名设置为 templatepack.csproj  。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-134">The `-n` parameter sets the _.csproj_ filename to _templatepack.csproj_.</span></span> <span data-ttu-id="e9ae4-135">`-o` 参数将在当前目录中创建文件。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-135">The `-o` parameter creates the files in the current directory.</span></span> <span data-ttu-id="e9ae4-136">应看到类似于以下输出的结果。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-136">You should see a result similar to the following output.</span></span>

```console
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on .\templatepack.csproj...
  Restore completed in 52.38 ms for C:\working\templatepack.csproj.

Restore succeeded.
```

<span data-ttu-id="e9ae4-137">接下来，在你喜爱的编辑器中打开 templatepack.csproj  文件，并将内容替换为以下 XML：</span><span class="sxs-lookup"><span data-stu-id="e9ae4-137">Next, open the _templatepack.csproj_ file in your favorite editor and replace the content with the following XML:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <PackageType>Template</PackageType>
    <PackageVersion>1.0</PackageVersion>
    <PackageId>AdatumCorporation.Utility.Templates</PackageId>
    <Title>AdatumCorporation Templates</Title>
    <Authors>Me</Authors>
    <Description>Templates to use when creating an application for Adatum Corporation.</Description>
    <PackageTags>dotnet-new;templates;contoso</PackageTags>

    <TargetFramework>netstandard2.0</TargetFramework>

    <IncludeContentInPack>true</IncludeContentInPack>
    <IncludeBuildOutput>false</IncludeBuildOutput>
    <ContentTargetFolders>content</ContentTargetFolders>
    <NoWarn>$(NoWarn);NU5128</NoWarn>
  </PropertyGroup>

  <ItemGroup>
    <Content Include="templates\**\*" Exclude="templates\**\bin\**;templates\**\obj\**" />
    <Compile Remove="**\*" />
  </ItemGroup>

</Project>
```

<span data-ttu-id="e9ae4-138">上面的 XML 中的 `<PropertyGroup>` 设置分为三组。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-138">The `<PropertyGroup>` settings in the XML above is broken into three groups.</span></span> <span data-ttu-id="e9ae4-139">第一组处理 NuGet 包所需的属性。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-139">The first group deals with properties required for a NuGet package.</span></span> <span data-ttu-id="e9ae4-140">三个 `<Package*>` 设置与 NuGet 包属性相关，用于在 NuGet 源上标识你的包。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-140">The three `<Package*>` settings have to do with the NuGet package properties to identify your package on a NuGet feed.</span></span> <span data-ttu-id="e9ae4-141">具体来说，`<PackageId>` 值用于通过单个名称而不是目录路径卸载模板包。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-141">Specifically the `<PackageId>` value is used to uninstall the template pack with a single name instead of a directory path.</span></span> <span data-ttu-id="e9ae4-142">它还可用于从 NuGet 源安装模板包。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-142">It can also be used to install the template pack from a NuGet feed.</span></span> <span data-ttu-id="e9ae4-143">其余的设置，如 `<Title>` 和 `<PackageTags>`，它们与 NuGet 源上显示的元数据相关。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-143">The remaining settings such as `<Title>` and `<PackageTags>` have to do with metadata displayed on the NuGet feed.</span></span> <span data-ttu-id="e9ae4-144">有关 NuGet 设置的详细信息，请参阅 [NuGet 和 MSBuild 属性](/nuget/reference/msbuild-targets)。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-144">For more information about NuGet settings, see [NuGet and MSBuild properties](/nuget/reference/msbuild-targets).</span></span>

<span data-ttu-id="e9ae4-145">必须设置 `<TargetFramework>` 设置，以便在运行 pack 命令编译和打包项目时 MSBuild 正常运行。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-145">The `<TargetFramework>` setting must be set so that MSBuild will run properly when you run the pack command to compile and pack the project.</span></span>

<span data-ttu-id="e9ae4-146">接下来的三项设置与正确配置项目有关，以便在创建 NuGet 包时将模板包含在该包内的相应文件夹中。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-146">The next three settings have to do with configuring the project correctly to include the templates in the appropriate folder in the NuGet pack when it's created.</span></span>

<span data-ttu-id="e9ae4-147">最后一项设置可用于取消不适用于模板包项目的警告消息。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-147">The last setting suppresses a warning message that doesn't apply to template pack projects.</span></span>

<span data-ttu-id="e9ae4-148">`<ItemGroup>` 包含两个设置。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-148">The `<ItemGroup>` contains two settings.</span></span> <span data-ttu-id="e9ae4-149">首先，`<Content>` 设置的内容包含 templates 文件夹中的所有内容。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-149">First, the `<Content>` setting includes everything in the _templates_ folder as content.</span></span> <span data-ttu-id="e9ae4-150">它还设置为排除任何 bin 文件夹或 obj 文件夹，以防止包含任何已编译的代码（如果已测试和编译模板）。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-150">It's also set to exclude any _bin_ folder or _obj_ folder to prevent any compiled code (if you tested and compiled your templates) from being included.</span></span> <span data-ttu-id="e9ae4-151">其次，`<Compile>` 设置将所有代码文件排除在编译范围之外，无论它们位于何处都是如此。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-151">Second, the `<Compile>` setting excludes all code files from compiling no matter where they're located.</span></span> <span data-ttu-id="e9ae4-152">此设置可阻止用于创建模板包的项目尝试编译 templates 文件夹层次结构中的代码。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-152">This setting prevents the project being used to create a template pack from trying to compile the code in the _templates_ folder hierarchy.</span></span>

## <a name="build-and-install"></a><span data-ttu-id="e9ae4-153">生成和安装</span><span class="sxs-lookup"><span data-stu-id="e9ae4-153">Build and install</span></span>

<span data-ttu-id="e9ae4-154">保存项目文件。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-154">Save the project file.</span></span> <span data-ttu-id="e9ae4-155">在生成模板包之前，请验证文件夹结构是否正确。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-155">Before building the template pack, verify that your folder structure is correct.</span></span> <span data-ttu-id="e9ae4-156">要打包的任何模板都应放置在自己的文件夹中的 templates 文件夹中。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-156">Any template you want to pack should be placed in the _templates_ folder, in its own folder.</span></span> <span data-ttu-id="e9ae4-157">文件夹结构应如下所示：</span><span class="sxs-lookup"><span data-stu-id="e9ae4-157">The folder structure should look similar to the following:</span></span>

```console
working
│   templatepack.csproj
└───templates
    ├───extensions
    │   └───.template.config
    │           template.json
    └───consoleasync
        └───.template.config
                template.json
```

<span data-ttu-id="e9ae4-158">templates 文件夹中有两个文件夹：extensions 和 consoleasync  。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-158">The _templates_ folder has two folders: _extensions_ and _consoleasync_.</span></span>

<span data-ttu-id="e9ae4-159">在终端中的 working 文件夹中，运行 `dotnet pack` 命令。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-159">In your terminal, from the _working_ folder, run the `dotnet pack` command.</span></span> <span data-ttu-id="e9ae4-160">此命令会生成项目，并在 working\bin\Debug 文件夹中创建一个 NuGet 包，如以下输出所示：</span><span class="sxs-lookup"><span data-stu-id="e9ae4-160">This command builds your project and creates a NuGet package in the _working\bin\Debug_ folder, as indicated by the following output:</span></span>

```console
C:\working> dotnet pack

Microsoft (R) Build Engine version 16.8.0+126527ff1 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 123.86 ms for C:\working\templatepack.csproj.

  templatepack -> C:\working\bin\Debug\netstandard2.0\templatepack.dll
  Successfully created package 'C:\working\bin\Debug\AdatumCorporation.Utility.Templates.1.0.0.nupkg'.
```

<span data-ttu-id="e9ae4-161">接下来，使用 `dotnet new -i PATH_TO_NUPKG_FILE` 命令安装模板包文件。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-161">Next, install the template pack file with the `dotnet new -i PATH_TO_NUPKG_FILE` command.</span></span>

```console
C:\working> dotnet new -i C:\working\bin\Debug\AdatumCorporation.Utility.Templates.1.0.0.nupkg
Usage: new [options]

Options:
  -h, --help          Displays help for this command.
  -l, --list          Lists templates containing the specified name. If no name is specified, lists all templates.

... cut to save space ...

Templates                                         Short Name               Language          Tags
--------------------------------------------      -------------------      ------------      ----------------------
Example templates: string extensions              stringext                [C#]              Common/Code
Console Application                               console                  [C#], F#, VB      Common/Console
Example templates: async project                  consoleasync             [C#]              Common/Console/C#9
Class library                                     classlib                 [C#], F#, VB      Common/Library
```

<span data-ttu-id="e9ae4-162">如果将 NuGet 包上传到 NuGet 源，可以使用 `dotnet new -i PACKAGEID` 命令，其中，`PACKAGEID` 与 .csproj  文件中的 `<PackageId>` 设置相同。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-162">If you uploaded the NuGet package to a NuGet feed, you can use the `dotnet new -i PACKAGEID` command where `PACKAGEID` is the same as the `<PackageId>` setting from the _.csproj_ file.</span></span> <span data-ttu-id="e9ae4-163">此包 ID 与 NuGet 包标识符相同。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-163">This package ID is the same as the NuGet package identifier.</span></span>

## <a name="uninstall-the-template-pack"></a><span data-ttu-id="e9ae4-164">卸载模板包</span><span class="sxs-lookup"><span data-stu-id="e9ae4-164">Uninstall the template pack</span></span>

<span data-ttu-id="e9ae4-165">无论如何安装模板包，即无论是直接使用 .nupkg 文件还是通过 NuGet 源安装，删除模板包的操作都是一样的。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-165">No matter how you installed the template pack, either with the _.nupkg_ file directly or by NuGet feed, removing a template pack is the same.</span></span> <span data-ttu-id="e9ae4-166">使用要卸载的模板的 `<PackageId>`。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-166">Use the `<PackageId>` of the template you want to uninstall.</span></span> <span data-ttu-id="e9ae4-167">可以通过运行 `dotnet new -u` 命令获取已安装的模板列表。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-167">You can get a list of templates that are installed by running the `dotnet new -u` command.</span></span>

```dotnetcli
C:\working> dotnet new -u

Template Instantiation Commands for .NET Core CLI

Currently installed items:
  Microsoft.DotNet.Common.ProjectTemplates.2.2
    Details:
      NuGetPackageId: Microsoft.DotNet.Common.ProjectTemplates.2.2
      Version: 1.0.2-beta4
      Author: Microsoft
    Templates:
      Class library (classlib) C#
      Class library (classlib) F#
      Class library (classlib) VB
      Console Application (console) C#
      Console Application (console) F#
      Console Application (console) VB
    Uninstall Command:
      dotnet new -u Microsoft.DotNet.Common.ProjectTemplates.2.2

... cut to save space ...

  AdatumCorporation.Utility.Templates
    Details:
      NuGetPackageId: AdatumCorporation.Utility.Templates
      Version: 1.0.0
      Author: Me
    Templates:
      Example templates: async project (consoleasync) C#
      Example templates: string extensions (stringext) C#
    Uninstall Command:
      dotnet new -u AdatumCorporation.Utility.Templates
```

<span data-ttu-id="e9ae4-168">运行 `dotnet new -u AdatumCorporation.Utility.Templates` 以卸载模板。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-168">Run `dotnet new -u AdatumCorporation.Utility.Templates` to uninstall the template.</span></span> <span data-ttu-id="e9ae4-169">`dotnet new` 命令将输出应忽略先前安装的模板的帮助信息。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-169">The `dotnet new` command will output help information that should omit the templates you previously installed.</span></span>

<span data-ttu-id="e9ae4-170">恭喜！</span><span class="sxs-lookup"><span data-stu-id="e9ae4-170">Congratulations!</span></span> <span data-ttu-id="e9ae4-171">你已安装，并卸载了模板包。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-171">you've installed and uninstalled a template pack.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9ae4-172">后续步骤</span><span class="sxs-lookup"><span data-stu-id="e9ae4-172">Next steps</span></span>

<span data-ttu-id="e9ae4-173">若要了解有关模板的详细信息（你已经了解了大部分相关信息），请参阅[为 dotnet new 自定义模板](../tools/custom-templates.md)一文。</span><span class="sxs-lookup"><span data-stu-id="e9ae4-173">To learn more about templates, most of which you've already learned, see the [Custom templates for dotnet new](../tools/custom-templates.md) article.</span></span>

* [<span data-ttu-id="e9ae4-174">dotnet/templating GitHub 存储库 Wiki</span><span class="sxs-lookup"><span data-stu-id="e9ae4-174">dotnet/templating GitHub repo Wiki</span></span>](https://github.com/dotnet/templating/wiki)
* [<span data-ttu-id="e9ae4-175">dotnet/dotnet-template-samples GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="e9ae4-175">dotnet/dotnet-template-samples GitHub repo</span></span>](https://github.com/dotnet/dotnet-template-samples)
* [<span data-ttu-id="e9ae4-176">JSON 架构存储中的 template.json 架构</span><span class="sxs-lookup"><span data-stu-id="e9ae4-176">*template.json* schema at the JSON Schema Store</span></span>](http://json.schemastore.org/template)
