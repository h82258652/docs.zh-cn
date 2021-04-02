---
title: EShop 到 ASP.NET Core 的迁移示例
description: 使用示例在线商店应用作为参考，将现有的 ASP.NET MVC 应用迁移到 ASP.NET Core 的演练。
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 119ba64134813fa17848cf9f5fe02cb1a14f8a5d
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122971"
---
# <a name="example-migration-of-eshop-to-aspnet-core"></a><span data-ttu-id="5359d-103">EShop 到 ASP.NET Core 的迁移示例</span><span class="sxs-lookup"><span data-stu-id="5359d-103">Example migration of eShop to ASP.NET Core</span></span>

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

<span data-ttu-id="5359d-104">在本章中，你将了解如何将 .NET Framework 应用迁移到 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="5359d-104">In this chapter, you'll see how to migrate a .NET Framework app to .NET Core.</span></span> <span data-ttu-id="5359d-105">本章介绍为 ASP.NET 5.0 编写的在线应用商店应用的示例。</span><span class="sxs-lookup"><span data-stu-id="5359d-105">The chapter examines a sample online store app written for ASP.NET 5.0.</span></span> <span data-ttu-id="5359d-106">此应用将使用本书前面介绍的许多概念和工具。</span><span class="sxs-lookup"><span data-stu-id="5359d-106">The app will use many of the concepts and tools described earlier in this book.</span></span> <span data-ttu-id="5359d-107">你将在 [ *eShopModernizing* GitHub 存储库](https://github.com/dotnet-architecture/eShopModernizing)中找到开始点应用。</span><span class="sxs-lookup"><span data-stu-id="5359d-107">You'll find the starting point app in the [*eShopModernizing* GitHub repository](https://github.com/dotnet-architecture/eShopModernizing).</span></span> <span data-ttu-id="5359d-108">有几个不同的起点应用。</span><span class="sxs-lookup"><span data-stu-id="5359d-108">There are several different starting point apps.</span></span> <span data-ttu-id="5359d-109">本章重点介绍 *eShopLegacyMVCSolution*。</span><span class="sxs-lookup"><span data-stu-id="5359d-109">This chapter focuses on the *eShopLegacyMVCSolution*.</span></span>

<span data-ttu-id="5359d-110">项目的初始版本如图4-1 所示。</span><span class="sxs-lookup"><span data-stu-id="5359d-110">The initial version of the project is shown in Figure 4-1.</span></span> <span data-ttu-id="5359d-111">它是一个相当标准的 ASP.NET MVC 5 应用程序。</span><span class="sxs-lookup"><span data-stu-id="5359d-111">It's a fairly standard ASP.NET MVC 5 app.</span></span>

![图4-1](media/Figure4-1.png)

<span data-ttu-id="5359d-113">**图 4-1**。</span><span class="sxs-lookup"><span data-stu-id="5359d-113">**Figure 4-1.**</span></span> <span data-ttu-id="5359d-114">*EShopModernizing* MVC 示例项目结构。</span><span class="sxs-lookup"><span data-stu-id="5359d-114">The *eShopModernizing* MVC sample project structure.</span></span>

<span data-ttu-id="5359d-115">本章演示了如何手动执行许多升级步骤。</span><span class="sxs-lookup"><span data-stu-id="5359d-115">This chapter demonstrates how to perform many of the upgrade steps by hand.</span></span> <span data-ttu-id="5359d-116">或者，你可以使用 [.Net 升级助手工具](https://aka.ms/dotnet-upgrade-assistant) 来执行许多初始步骤，例如，转换项目文件、更改目标框架和更新 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="5359d-116">Alternatively, you can use the [.NET Upgrade Assistant tool](https://aka.ms/dotnet-upgrade-assistant) to perform many of the initial steps, like converting the project file, changing the target framework, and updating NuGet packages.</span></span>

## <a name="run-apiport-to-identify-problematic-apis"></a><span data-ttu-id="5359d-117">运行 *ApiPort* 以识别有问题的 api</span><span class="sxs-lookup"><span data-stu-id="5359d-117">Run *ApiPort* to identify problematic APIs</span></span>

<span data-ttu-id="5359d-118">准备迁移的第一步是运行 *ApiPort* 工具。</span><span class="sxs-lookup"><span data-stu-id="5359d-118">The first step in preparing to migrate is to run the *ApiPort* tool.</span></span> <span data-ttu-id="5359d-119">该工具识别应用程序调用的 .NET Framework Api 的数量，其中有多少 .NET Standard 或 .NET Core 等效项。</span><span class="sxs-lookup"><span data-stu-id="5359d-119">The tool identifies how many .NET Framework APIs the app calls and how many of these have .NET Standard or .NET Core equivalents.</span></span> <span data-ttu-id="5359d-120">主要关注自己的应用程序逻辑，而不是第三方依赖项，并注意 `System.Web` 需要移植的依赖项。</span><span class="sxs-lookup"><span data-stu-id="5359d-120">Focus primarily on your own app's logic, not third-party dependencies, and pay attention to `System.Web` dependencies that will need to be ported.</span></span> <span data-ttu-id="5359d-121">ApiPort 工具是在上一章 [了解和更新依赖项](understand-update-dependencies.md)中引入的。</span><span class="sxs-lookup"><span data-stu-id="5359d-121">The ApiPort tool was introduced in the last chapter on [understanding and updating dependencies](understand-update-dependencies.md).</span></span>

<span data-ttu-id="5359d-122">[安装和配置 *ApiPort* 工具](../../standard/analyzers/portability-analyzer.md)后，从 Visual Studio 中运行分析，如图4-2 所示。</span><span class="sxs-lookup"><span data-stu-id="5359d-122">After [installing and configuring the *ApiPort* tool](../../standard/analyzers/portability-analyzer.md), run the analysis from within Visual Studio, as shown in Figure 4-2.</span></span>

![图4-2](media/Figure4-2.png)

<span data-ttu-id="5359d-124">**图 4-2**。</span><span class="sxs-lookup"><span data-stu-id="5359d-124">**Figure 4-2.**</span></span> <span data-ttu-id="5359d-125">分析 Visual Studio 中的程序集可移植性。</span><span class="sxs-lookup"><span data-stu-id="5359d-125">Analyze assembly portability in Visual Studio.</span></span>

<span data-ttu-id="5359d-126">从项目的 *bin* 文件夹中选择 web 项目的程序集，如图4-3 所示。</span><span class="sxs-lookup"><span data-stu-id="5359d-126">Choose the web project's assembly from the project's *bin* folder, as shown in Figure 4-3.</span></span>

![图4-3](media/Figure4-3.png)

<span data-ttu-id="5359d-128">**图 4-3.**</span><span class="sxs-lookup"><span data-stu-id="5359d-128">**Figure 4-3.**</span></span> <span data-ttu-id="5359d-129">选择项目的 web 程序集。</span><span class="sxs-lookup"><span data-stu-id="5359d-129">Choose the project's web assembly.</span></span>

<span data-ttu-id="5359d-130">如果你的解决方案包含多个项目，则可以选择所有项目。</span><span class="sxs-lookup"><span data-stu-id="5359d-130">If your solution includes several projects, you can choose all of them.</span></span> <span data-ttu-id="5359d-131">*EShop* 示例只包含单个 MVC 项目。</span><span class="sxs-lookup"><span data-stu-id="5359d-131">The *eShop* sample includes just a single MVC project.</span></span>

<span data-ttu-id="5359d-132">生成报告后，打开文件并查看结果。</span><span class="sxs-lookup"><span data-stu-id="5359d-132">Once the report is generated, open the file and review the results.</span></span> <span data-ttu-id="5359d-133">摘要提供对应用正在进行的 .NET Framework 调用的百分比具有兼容版本的高级视图。</span><span class="sxs-lookup"><span data-stu-id="5359d-133">The summary provides a high-level view of what percentage of .NET Framework calls your app is making have compatible versions.</span></span> <span data-ttu-id="5359d-134">图4-4 显示了 *eShop* MVC 项目的摘要。</span><span class="sxs-lookup"><span data-stu-id="5359d-134">Figure 4-4 shows the summary for the *eShop* MVC project.</span></span>

![图4-4](media/Figure4-4.png)

<span data-ttu-id="5359d-136">**图 4-4.**</span><span class="sxs-lookup"><span data-stu-id="5359d-136">**Figure 4-4.**</span></span> <span data-ttu-id="5359d-137">ApiPort 摘要。</span><span class="sxs-lookup"><span data-stu-id="5359d-137">ApiPort summary.</span></span>

<span data-ttu-id="5359d-138">对于此应用程序，大约80% 的 .NET Framework 调用是兼容的。</span><span class="sxs-lookup"><span data-stu-id="5359d-138">For this app, about 80 percent of the .NET Framework calls are compatible.</span></span> <span data-ttu-id="5359d-139">在迁移过程中需要解决20% 的调用。</span><span class="sxs-lookup"><span data-stu-id="5359d-139">20 percent of the calls need to be addressed during the porting process.</span></span> <span data-ttu-id="5359d-140">查看详细信息会发现所有不兼容的调用都是的一部分 `System.Web` ，这是预期的不兼容性。</span><span class="sxs-lookup"><span data-stu-id="5359d-140">Viewing the details reveals that all of the incompatible calls are part of `System.Web`, which is an expected incompatibility.</span></span> <span data-ttu-id="5359d-141">`System.Web`当在稍后的步骤中迁移应用的控制器和相关类时，将解决调用上的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="5359d-141">The dependencies on `System.Web` calls will be addressed when the app's controllers and related classes are migrated in a later step.</span></span> <span data-ttu-id="5359d-142">图4-5 列出了工具发现的一些特定类型：</span><span class="sxs-lookup"><span data-stu-id="5359d-142">Figure 4-5 lists some of the specific types found by the tool:</span></span>

![图4-5](media/Figure4-5.png)

<span data-ttu-id="5359d-144">**图4-5。**</span><span class="sxs-lookup"><span data-stu-id="5359d-144">**Figure 4-5.**</span></span> <span data-ttu-id="5359d-145">*ApiPort* 不兼容的类型详细信息。</span><span class="sxs-lookup"><span data-stu-id="5359d-145">*ApiPort* incompatible type details.</span></span>

<span data-ttu-id="5359d-146">大多数不兼容的类型 `Controller` 在 ASP.NET Core 中引用和具有等效项的各种相关属性。</span><span class="sxs-lookup"><span data-stu-id="5359d-146">Most of the incompatible types refer to `Controller` and various related attributes that have equivalents in ASP.NET Core.</span></span>

## <a name="update-project-files-and-nuget-reference-syntax"></a><span data-ttu-id="5359d-147">更新项目文件和 NuGet 引用语法</span><span class="sxs-lookup"><span data-stu-id="5359d-147">Update project files and NuGet reference syntax</span></span>

<span data-ttu-id="5359d-148">接下来，从较旧的 *.csproj* 文件结构迁移到 .net Core 引入的较新的更简单的结构。</span><span class="sxs-lookup"><span data-stu-id="5359d-148">Next, migrate from the older *.csproj* file structure to the newer, simpler structure introduced with .NET Core.</span></span> <span data-ttu-id="5359d-149">在此过程中，还将从使用 *packages.config* 文件进行 NuGet 引用，以使用 `<PackageReference>` 项目文件中的元素。</span><span class="sxs-lookup"><span data-stu-id="5359d-149">In doing so, you'll also migrate from using a *packages.config* file for NuGet references to using `<PackageReference>` elements in the project file.</span></span> <span data-ttu-id="5359d-150">旧样式项目文件也可以使用 `<PackageReference>` 元素，因此，在升级到新的项目文件格式之前，最好先将所有 NuGet 包引用迁移为此格式。</span><span class="sxs-lookup"><span data-stu-id="5359d-150">Old-style project files may also use `<PackageReference>` elements, so it usually makes sense to migrate all NuGet package references to this format first, before upgrading to the new project file format.</span></span>

<span data-ttu-id="5359d-151">原始项目的 *eShopLegacyMVC* 文件长度为418行。</span><span class="sxs-lookup"><span data-stu-id="5359d-151">The original project's *eShopLegacyMVC.csproj* file is 418 lines long.</span></span> <span data-ttu-id="5359d-152">图4-6 显示了项目文件的示例。</span><span class="sxs-lookup"><span data-stu-id="5359d-152">A sample of the project file is shown in Figure 4-6.</span></span> <span data-ttu-id="5359d-153">为了提供总体大小和复杂性，映像的右边包含了整个文件的缩图视图。</span><span class="sxs-lookup"><span data-stu-id="5359d-153">To offer a sense of its overall size and complexity, the right side of the image contains a miniature view of the entire file.</span></span>

![图4-6](media/Figure4-6.png)

<span data-ttu-id="5359d-155">**图 4-6.**</span><span class="sxs-lookup"><span data-stu-id="5359d-155">**Figure 4-6.**</span></span> <span data-ttu-id="5359d-156">*EShopLegacyMVC* 文件结构。</span><span class="sxs-lookup"><span data-stu-id="5359d-156">The *eShopLegacyMVC.csproj* file structure.</span></span>

<span data-ttu-id="5359d-157">为现有 ASP.NET 项目创建新项目文件的常见方法是 `dotnet new`   >    >  在 Visual Studio 中使用或 "新建 **项目**" 创建新的 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="5359d-157">A common way to create a new project file for an existing ASP.NET project is to create a new ASP.NET Core app using `dotnet new` or **File** > **New** > **Project** in Visual Studio.</span></span> <span data-ttu-id="5359d-158">然后，可以将文件从旧项目复制到新项目，以完成迁移。</span><span class="sxs-lookup"><span data-stu-id="5359d-158">Then files can be copied from the old project to the new one to complete the migration.</span></span>

<span data-ttu-id="5359d-159">除了 c # 项目文件，NuGet 依赖项存储在单独的45行 *packages.config* 文件中，如图4-7 中所示。</span><span class="sxs-lookup"><span data-stu-id="5359d-159">In addition to the C# project file, NuGet dependencies are stored in a separate 45-line *packages.config* file, as shown in Figure 4-7.</span></span>

![图4-7](media/Figure4-7.png)

<span data-ttu-id="5359d-161">**图 4-7.**</span><span class="sxs-lookup"><span data-stu-id="5359d-161">**Figure 4-7.**</span></span> <span data-ttu-id="5359d-162">*packages.config* 的文件。</span><span class="sxs-lookup"><span data-stu-id="5359d-162">The *packages.config* file.</span></span>

<span data-ttu-id="5359d-163">你可以使用 Visual Studio 将 *packages.config* 迁移到类库项目中。</span><span class="sxs-lookup"><span data-stu-id="5359d-163">You can migrate *packages.config* in class library projects using Visual Studio.</span></span> <span data-ttu-id="5359d-164">不过，此功能不适用于 ASP.NET 项目。</span><span class="sxs-lookup"><span data-stu-id="5359d-164">This functionality doesn't work with ASP.NET projects, however.</span></span> <span data-ttu-id="5359d-165">[详细了解如何将 *packages.config* 迁移到 `<PackageReference>` Visual Studio 中](/nuget/consume-packages/migrate-packages-config-to-package-reference)。</span><span class="sxs-lookup"><span data-stu-id="5359d-165">[Learn more about migrating *packages.config* to `<PackageReference>` in Visual Studio](/nuget/consume-packages/migrate-packages-config-to-package-reference).</span></span> <span data-ttu-id="5359d-166">如果要迁移大量项目， [此社区工具可能会有所帮助](https://github.com/MarkKharitonov/NuGetPCToPRMigrator)。</span><span class="sxs-lookup"><span data-stu-id="5359d-166">If you have a large number of projects to migrate, [this community tool may help](https://github.com/MarkKharitonov/NuGetPCToPRMigrator).</span></span> <span data-ttu-id="5359d-167">如果要使用工具将项目文件迁移到新格式，则在完成迁移所有 NuGet 引用后，应执行此操作 `<PackageReverence>` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-167">If you're using a tool to migrate the project file to the new format, you should do that after you've finished migrating all NuGet references to use `<PackageReverence>`.</span></span>

## <a name="create-new-aspnet-core-project"></a><span data-ttu-id="5359d-168">创建新 ASP.NET Core 项目</span><span class="sxs-lookup"><span data-stu-id="5359d-168">Create new ASP.NET Core project</span></span>

<span data-ttu-id="5359d-169">向现有应用程序的解决方案中添加一个新的 ASP.NET Core 项目，以便更轻松地移动文件，因为大部分工作可从 Visual Studio 的 **解决方案资源管理器** 中完成。</span><span class="sxs-lookup"><span data-stu-id="5359d-169">Add a new ASP.NET Core project to the existing app's solution to make moving files easier, as most of the work can be done from within Visual Studio's **Solution Explorer**.</span></span> <span data-ttu-id="5359d-170">在 Visual Studio 中，右键单击应用的解决方案，然后选择 " **添加新项目**"。</span><span class="sxs-lookup"><span data-stu-id="5359d-170">In Visual Studio, right-click on your app's solution and choose **Add New Project**.</span></span> <span data-ttu-id="5359d-171">选择 **ASP.NET Core web 应用程序**"，并为新项目指定一个名称，如图4-8 所示。</span><span class="sxs-lookup"><span data-stu-id="5359d-171">Choose **ASP.NET Core web application**, and give the new project a name as shown in Figure 4-8.</span></span>

![图4-8](media/Figure4-8.png)

<span data-ttu-id="5359d-173">**图4-8。**</span><span class="sxs-lookup"><span data-stu-id="5359d-173">**Figure 4-8.**</span></span> <span data-ttu-id="5359d-174">添加新的 ASP.NET Core web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="5359d-174">Add new ASP.NET Core web application.</span></span>

<span data-ttu-id="5359d-175">下一个对话框将要求您选择要使用的模板。</span><span class="sxs-lookup"><span data-stu-id="5359d-175">The next dialog will ask you to choose which template to use.</span></span> <span data-ttu-id="5359d-176">选择“空”模板。</span><span class="sxs-lookup"><span data-stu-id="5359d-176">Select the **Empty** template.</span></span> <span data-ttu-id="5359d-177">还要确保将下拉列表从 **.Net Core** 更改为 **.NET Framework**。</span><span class="sxs-lookup"><span data-stu-id="5359d-177">Be sure to also change the dropdown from **.NET Core** to **.NET Framework**.</span></span> <span data-ttu-id="5359d-178">选择 **ASP.NET Core 2.2**，如图4-9 中所示。</span><span class="sxs-lookup"><span data-stu-id="5359d-178">Select **ASP.NET Core 2.2**, as shown in Figure 4-9.</span></span>

![图4-9](media/Figure4-9.png)

<span data-ttu-id="5359d-180">**图 4-9.** 。</span><span class="sxs-lookup"><span data-stu-id="5359d-180">**Figure 4-9.**</span></span> <span data-ttu-id="5359d-181">选择针对 .NET Framework ASP.NET Core 2.2 的空项目模板。</span><span class="sxs-lookup"><span data-stu-id="5359d-181">Choose an Empty project template targeting .NET Framework with ASP.NET Core 2.2.</span></span>

### <a name="migrating-nuget-packages"></a><span data-ttu-id="5359d-182">迁移 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="5359d-182">Migrating NuGet Packages</span></span>

<span data-ttu-id="5359d-183">由于用于迁移 *packages.config* 的内置迁移工具 `<PackageReference>` 不适用于 ASP.NET 项目，因此可以改为使用社区工具或手动迁移。</span><span class="sxs-lookup"><span data-stu-id="5359d-183">Since the built-in migration tool for migrating *packages.config* to `<PackageReference>` doesn't work on ASP.NET projects, you can use a community tool instead, or migrate by hand.</span></span> <span data-ttu-id="5359d-184">[我使用的社区工具](https://gist.github.com/tomkuijsten/2d75074d9a3c19c04ee8ea19a6289ddf)使用 XSL 文件从一种格式转换为另一种格式。</span><span class="sxs-lookup"><span data-stu-id="5359d-184">A [community tool I've used](https://gist.github.com/tomkuijsten/2d75074d9a3c19c04ee8ea19a6289ddf) uses an XSL file to transform from one format to the other.</span></span> <span data-ttu-id="5359d-185">若要使用它，请先将 *packages.config* 文件复制到新创建的 ASP.NET Core 项目文件夹中。</span><span class="sxs-lookup"><span data-stu-id="5359d-185">To use it, first copy the *packages.config* file to the newly created ASP.NET Core project folder.</span></span> <span data-ttu-id="5359d-186">对文件进行备份，因为此脚本将从运行脚本的下的所有文件夹中删除 *packages.config* 文件。</span><span class="sxs-lookup"><span data-stu-id="5359d-186">Make a backup of your files, as this script removes the *packages.config* file from all folders under where you run the script.</span></span> <span data-ttu-id="5359d-187">然后，在项目文件夹中运行以下命令 (对于整个解决方案，请按需) ：</span><span class="sxs-lookup"><span data-stu-id="5359d-187">Then run these commands from the project folder (or for the entire solution if you prefer):</span></span>

```powershell
iwr https://git.io/vdKaV -OutFile Convert-ToPackageReference.ps1
iwr https://git.io/vdKar -OutFile  Convert-ToPackageReference.xsl
./Convert-ToPackageReference.ps1 | Out-Null
```

<span data-ttu-id="5359d-188">前两个命令下载文件，使文件在本地存在。</span><span class="sxs-lookup"><span data-stu-id="5359d-188">The first two commands download files so that they exist locally.</span></span> <span data-ttu-id="5359d-189">最后一行运行脚本。</span><span class="sxs-lookup"><span data-stu-id="5359d-189">The last line runs the script.</span></span> <span data-ttu-id="5359d-190">运行后，尝试生成新的项目。</span><span class="sxs-lookup"><span data-stu-id="5359d-190">After running it, try to build the new project.</span></span> <span data-ttu-id="5359d-191">您很可能会遇到一些错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-191">You'll most likely get some errors.</span></span> <span data-ttu-id="5359d-192">若要解决这些问题，你需要消除某些引用 (如大多数 `Microsoft.AspNet` 和 `System` 包) ，你可能需要删除一些 `xmlns` 属性。</span><span class="sxs-lookup"><span data-stu-id="5359d-192">To resolve them, you'll want to eliminate some references (like most of the `Microsoft.AspNet` and `System` packages), and you may need to remove some `xmlns` attributes.</span></span>

<span data-ttu-id="5359d-193">在大多数 ASP.NET MVC 应用程序中，许多客户端依赖项（如启动和 jQuery）都是使用 NuGet 包部署的。</span><span class="sxs-lookup"><span data-stu-id="5359d-193">In most ASP.NET MVC apps, many client-side dependencies like Bootstrap and jQuery were deployed using NuGet packages.</span></span> <span data-ttu-id="5359d-194">在 ASP.NET Core 中，NuGet 包仅用于服务器端功能。</span><span class="sxs-lookup"><span data-stu-id="5359d-194">In ASP.NET Core, NuGet packages are only used for server-side functionality.</span></span> <span data-ttu-id="5359d-195">应通过其他方式管理客户端文件。</span><span class="sxs-lookup"><span data-stu-id="5359d-195">Client files should be managed through other means.</span></span> <span data-ttu-id="5359d-196">查看 `<PackageReference>` 添加和删除的元素列表，并记下客户端库的任何内容，包括：</span><span class="sxs-lookup"><span data-stu-id="5359d-196">Review the list of `<PackageReference>` elements added and remove and make note of any that are for client libraries, including:</span></span>

- <span data-ttu-id="5359d-197">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="5359d-197">Bootstrap</span></span>
- <span data-ttu-id="5359d-198">jQuery</span><span class="sxs-lookup"><span data-stu-id="5359d-198">jQuery</span></span>
- <span data-ttu-id="5359d-199">jQuery. 验证</span><span class="sxs-lookup"><span data-stu-id="5359d-199">jQuery.Validation</span></span>
- <span data-ttu-id="5359d-200">Modernizr</span><span class="sxs-lookup"><span data-stu-id="5359d-200">Modernizr</span></span>
- <span data-ttu-id="5359d-201">popper.js</span><span class="sxs-lookup"><span data-stu-id="5359d-201">popper.js</span></span>
- <span data-ttu-id="5359d-202">响应</span><span class="sxs-lookup"><span data-stu-id="5359d-202">Respond</span></span>

<span data-ttu-id="5359d-203">NuGet 为这些包安装的静态客户端文件将被复制到新项目的 *wwwroot* 文件夹，并从该文件夹中承载。</span><span class="sxs-lookup"><span data-stu-id="5359d-203">The static client files installed by NuGet for these packages will be copied over to the new project's *wwwroot* folder and hosted from there.</span></span> <span data-ttu-id="5359d-204">值得考虑的是，应用程序是否仍需要这些文件，以及是否有必要继续承载它们或改用 (CDN) 的内容交付网络。</span><span class="sxs-lookup"><span data-stu-id="5359d-204">It's worth considering whether these files are still needed by the app, and whether it makes sense to continue hosting them or to use a content delivery network (CDN) instead.</span></span> <span data-ttu-id="5359d-205">这些库版本可在生成时使用 [LibMan](/aspnet/core/client-side/libman/) 或 [npm](https://www.npmjs.com/)等工具进行管理。</span><span class="sxs-lookup"><span data-stu-id="5359d-205">These library versions can be managed at build time using tools like [LibMan](/aspnet/core/client-side/libman/) or [npm](https://www.npmjs.com/).</span></span> <span data-ttu-id="5359d-206">图4-10 显示了使用转换工具（显示和删除不必要的包）迁移包引用后的完整 *eShopPorted* 文件。</span><span class="sxs-lookup"><span data-stu-id="5359d-206">Figure 4-10 shows the full *eShopPorted.csproj* file after migrating package references using the conversion tool shown and removing unnecessary packages.</span></span>

![图4-10](media/Figure4-10.png)

<span data-ttu-id="5359d-208">**图 4-10.**</span><span class="sxs-lookup"><span data-stu-id="5359d-208">**Figure 4-10.**</span></span> <span data-ttu-id="5359d-209">*EShopPorted* 文件中的包引用。</span><span class="sxs-lookup"><span data-stu-id="5359d-209">Package references in the *eShopPorted.csproj* file.</span></span>

<span data-ttu-id="5359d-210">通过使元素成为特性，可以进一步压缩包引用 `<Version>1.0.0.0</Version>` `Version=1.0.0.0` `<PackageReference>` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-210">The package references can be further compacted by making the `<Version>1.0.0.0</Version>` element a `Version=1.0.0.0` attribute on `<PackageReference>`.</span></span>

### <a name="migrate-static-files"></a><span data-ttu-id="5359d-211">迁移静态文件</span><span class="sxs-lookup"><span data-stu-id="5359d-211">Migrate static files</span></span>

<span data-ttu-id="5359d-212">应用使用的任何静态文件（包括第三方脚本和框架，以及自定义图像和样式表）都必须从旧项目复制到新项目。</span><span class="sxs-lookup"><span data-stu-id="5359d-212">Any static files the app uses, including third-party scripts and frameworks but also custom images and stylesheets, must be copied from the old project to the new one.</span></span> <span data-ttu-id="5359d-213">在 ASP.NET MVC 应用中，通常会根据文件在项目文件夹中的位置对其进行访问。</span><span class="sxs-lookup"><span data-stu-id="5359d-213">In ASP.NET MVC apps, files were typically accessed based on their location within the project folder.</span></span> <span data-ttu-id="5359d-214">在 ASP.NET Core 应用中，将根据其在 *wwwroot* 文件夹中的位置访问这些静态文件。</span><span class="sxs-lookup"><span data-stu-id="5359d-214">In ASP.NET Core apps, these static files will be accessed based on their location within the *wwwroot* folder.</span></span> <span data-ttu-id="5359d-215">对于 *eShop* 项目，下列文件夹中有静态文件：</span><span class="sxs-lookup"><span data-stu-id="5359d-215">For the *eShop* project, there are static files in the following folders:</span></span>

* <span data-ttu-id="5359d-216">*内容*</span><span class="sxs-lookup"><span data-stu-id="5359d-216">*Content*</span></span>
* <span data-ttu-id="5359d-217">*字体*</span><span class="sxs-lookup"><span data-stu-id="5359d-217">*fonts*</span></span>
* <span data-ttu-id="5359d-218">*映像*</span><span class="sxs-lookup"><span data-stu-id="5359d-218">*Images*</span></span>
* <span data-ttu-id="5359d-219">*Pics*</span><span class="sxs-lookup"><span data-stu-id="5359d-219">*Pics*</span></span>
* <span data-ttu-id="5359d-220">*脚本*</span><span class="sxs-lookup"><span data-stu-id="5359d-220">*Scripts*</span></span>

<span data-ttu-id="5359d-221">在上一步中使用的 **空** 项目模板在默认情况下不包括此文件夹，或在运行时所需的中间件。</span><span class="sxs-lookup"><span data-stu-id="5359d-221">The **Empty** project template used in the previous step doesn't include this folder by default, or the middleware needed for it to work.</span></span> <span data-ttu-id="5359d-222">需要添加它们。</span><span class="sxs-lookup"><span data-stu-id="5359d-222">You'll need to add them.</span></span>

<span data-ttu-id="5359d-223">将 *wwwroot* 文件夹添加到项目的根目录。</span><span class="sxs-lookup"><span data-stu-id="5359d-223">Add a *wwwroot* folder to the root of the project.</span></span>

<span data-ttu-id="5359d-224">添加 NuGet 包的版本 2.2.0 `Microsoft.AspNetCore.StaticFiles` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-224">Add version 2.2.0 of the `Microsoft.AspNetCore.StaticFiles` NuGet package.</span></span>

<span data-ttu-id="5359d-225">在 *Startup* 中，在方法中添加对的调用 `app.UseStaticFiles()` `Configure` ：</span><span class="sxs-lookup"><span data-stu-id="5359d-225">In *Startup.cs*, add a call to `app.UseStaticFiles()` in the `Configure` method:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    // ...
}
```

<span data-ttu-id="5359d-226">将 *Content* 文件夹从 ASP.NET MVC 应用复制到新项目的 *wwwroot* 文件夹。</span><span class="sxs-lookup"><span data-stu-id="5359d-226">Copy the *Content* folder from the ASP.NET MVC app to the new project's *wwwroot* folder.</span></span>

<span data-ttu-id="5359d-227">运行应用并导航到其 */Content/base.css* 文件夹，以验证是否已正确从其预期路径提供静态文件。</span><span class="sxs-lookup"><span data-stu-id="5359d-227">Run the app and navigate to its */Content/base.css* folder to verify that the static file is served correctly from its expected path.</span></span> <span data-ttu-id="5359d-228">继续将包含静态文件的其余文件夹复制到新项目中。</span><span class="sxs-lookup"><span data-stu-id="5359d-228">Continue copying the rest of the folders containing static files to the new project.</span></span> <span data-ttu-id="5359d-229">还需要将 *favicon* 文件从项目的根复制到 *wwwroot* 文件夹。</span><span class="sxs-lookup"><span data-stu-id="5359d-229">You'll also want to copy the *favicon.ico* file from the project's root to the *wwwroot* folder.</span></span> <span data-ttu-id="5359d-230">图4-11 显示了这些文件及其文件夹全部复制后的结果。</span><span class="sxs-lookup"><span data-stu-id="5359d-230">Figure 4-11 shows the results after these files and their folders have all been copied.</span></span>

![图4-11](media/Figure4-11.png)

<span data-ttu-id="5359d-232">**图 4-11.**</span><span class="sxs-lookup"><span data-stu-id="5359d-232">**Figure 4-11.**</span></span> <span data-ttu-id="5359d-233">静态文件夹复制到 *wwwroot* 文件夹。</span><span class="sxs-lookup"><span data-stu-id="5359d-233">Static folders copied over to *wwwroot* folder.</span></span>

### <a name="migrate-c-files"></a><span data-ttu-id="5359d-234">迁移 c # 文件</span><span class="sxs-lookup"><span data-stu-id="5359d-234">Migrate C# files</span></span>

<span data-ttu-id="5359d-235">接下来，复制应用使用的 c # 文件，包括标准 MVC 文件夹及其内容，如 *控制器*、 *模型*、 *ViewModel* 和 *服务*。</span><span class="sxs-lookup"><span data-stu-id="5359d-235">Next, copy over the C# files used by the app, including standard MVC folders and their contents like *Controllers*, *Models*, *ViewModel*, and *Services*.</span></span> <span data-ttu-id="5359d-236">在这些文件中很可能需要进行一些更改。</span><span class="sxs-lookup"><span data-stu-id="5359d-236">There will most likely be some changes needed in these files.</span></span> <span data-ttu-id="5359d-237">最好一次复制一个文件夹 (或子文件夹) 并进行编译，以查看需要解决的错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-237">It's best to copy one folder (or subfolder) at a time and compile to see what errors need to be addressed as you go.</span></span>

<span data-ttu-id="5359d-238">对于 *eShop* 示例，我选择迁移的第一个文件夹是 " *模型* " 文件夹，其中包括 c # 实体和实体框架类。</span><span class="sxs-lookup"><span data-stu-id="5359d-238">For the *eShop* sample, the first folder I choose to migrate is the *Models* folder, which includes C# entities and Entity Framework classes.</span></span> <span data-ttu-id="5359d-239">此文件夹的类由其他大多数类使用，因此，在复制这些类之前，这些类将不起作用。</span><span class="sxs-lookup"><span data-stu-id="5359d-239">This folder's classes are used by most of the others, so they won't work until these classes have been copied.</span></span> <span data-ttu-id="5359d-240">复制文件夹和生成后，编译器会显示与缺少命名空间相关的错误、对的 `System.Web.Hosting` 访问 `HostingEnvironment` ，以及对的引用 `ConfigurationManager.AppSettings` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-240">After copying the folder and building, the compiler revealed errors related to missing namespace `System.Web.Hosting`, related access to `HostingEnvironment`, and a reference to `ConfigurationManager.AppSettings`.</span></span> <span data-ttu-id="5359d-241">这些问题的解决方案将传入必要的路径数据;现在，间断线会被注释掉，并向 `TODO:` 每个线添加注释进行跟踪。</span><span class="sxs-lookup"><span data-stu-id="5359d-241">The solution to these issues will be to pass in the necessary path data; for now the breaking lines are commented out and a `TODO:` comment is added to each one to track it.</span></span> <span data-ttu-id="5359d-242">更改5行后， **任务列表** 显示5个项，项目将生成。</span><span class="sxs-lookup"><span data-stu-id="5359d-242">After changing five lines, the **Task List** shows five items and the project builds.</span></span>

<span data-ttu-id="5359d-243">接下来，复制 *ViewModel* 文件夹及其一个类。</span><span class="sxs-lookup"><span data-stu-id="5359d-243">Next, the *ViewModel* folder, with its one class, is copied over.</span></span> <span data-ttu-id="5359d-244">这是一个简单的过程，可立即生成。</span><span class="sxs-lookup"><span data-stu-id="5359d-244">It's an easy one, and builds immediately.</span></span>

<span data-ttu-id="5359d-245">将复制 *服务* 文件夹。</span><span class="sxs-lookup"><span data-stu-id="5359d-245">The *Services* folder is copied over.</span></span> <span data-ttu-id="5359d-246">此文件夹的类依赖于 *模型* 文件夹中实体框架类，这就是需要在该文件夹后复制它的原因。</span><span class="sxs-lookup"><span data-stu-id="5359d-246">This folder's classes depend on Entity Framework classes from the *Models* folder, which is why it needed to be copied after that folder.</span></span> <span data-ttu-id="5359d-247">幸运的是，它也不会发生错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-247">Fortunately, it too builds without errors.</span></span>

<span data-ttu-id="5359d-248">这会离开 *控制器* 文件夹及其两个 `Controller` 类。</span><span class="sxs-lookup"><span data-stu-id="5359d-248">That leaves the *Controllers* folder and its two `Controller` classes.</span></span> <span data-ttu-id="5359d-249">将文件夹复制到新项目并生成后，出现7个生成错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-249">After copying the folder to the new project and building, there are seven build errors.</span></span> <span data-ttu-id="5359d-250">其中四个与 access 相关 `ViewBag` ，并报告错误：</span><span class="sxs-lookup"><span data-stu-id="5359d-250">Four of them are related to `ViewBag` access and report an error of:</span></span>

> `Missing compiler required member 'Microsoft.CSharp.RuntimeBinder.CSharpArgumentInfo.Create'`

<span data-ttu-id="5359d-251">若要解决此错误，请添加对 c # 的 NuGet 包引用：</span><span class="sxs-lookup"><span data-stu-id="5359d-251">To resolve this error add a NuGet package reference to C#:</span></span>

```xml
<PackageReference Include="Microsoft.CSharp" Version="4.7.0" />
```

<span data-ttu-id="5359d-252">其余三个错误指定在未引用的程序集中定义的类型。</span><span class="sxs-lookup"><span data-stu-id="5359d-252">The remaining three errors specify types that are defined in an assembly that isn't referenced.</span></span> <span data-ttu-id="5359d-253">具体来说，这些类型：</span><span class="sxs-lookup"><span data-stu-id="5359d-253">Specifically these types:</span></span>

- `HttpServerUtilityBase`
- `RouteValueDictionary`
- `HttpRequestBase`

<span data-ttu-id="5359d-254">让我们逐个查看每个错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-254">Let's look at each error one by one.</span></span> <span data-ttu-id="5359d-255">当尝试引用不再存在的属性时，将发生第一个错误 `Server` `Controller` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-255">The first error occurs while trying to reference the `Server` property of `Controller`, which no longer exists.</span></span> <span data-ttu-id="5359d-256">操作的目标是获取应用中映像文件的路径：</span><span class="sxs-lookup"><span data-stu-id="5359d-256">The goal of the operation is to get the path to an image file in the app:</span></span>

```csharp
if (item != null)
{
    var webRoot = Server.MapPath("~/Pics"); // compiler error on this line
    var path = Path.Combine(webRoot, item.PictureFileName);

    string imageFileExtension = Path.GetExtension(item.PictureFileName);
    string mimetype = GetImageMimeTypeFromImageFileExtension(imageFileExtension);

    var buffer = System.IO.File.ReadAllBytes(path);

    return File(buffer, mimetype);
}
```

<span data-ttu-id="5359d-257">此问题有两种可能的解决方法。</span><span class="sxs-lookup"><span data-stu-id="5359d-257">There are two possible solutions to this problem.</span></span> <span data-ttu-id="5359d-258">第一种方法是使功能保持原样。</span><span class="sxs-lookup"><span data-stu-id="5359d-258">The first is to keep the functionality as it is.</span></span> <span data-ttu-id="5359d-259">在这种情况下， `Server.MapPath` 应使用在 *wwwroot* 中引用图像文件位置的固定路径，而不是使用。</span><span class="sxs-lookup"><span data-stu-id="5359d-259">In this case, rather than using `Server.MapPath`, a fixed path referencing the image files' location in *wwwroot* should be used.</span></span> <span data-ttu-id="5359d-260">另外，由于此操作方法的唯一目的是返回一个静态图像文件，因此可以更新视图文件中对此操作的引用以直接引用静态文件，从而提高运行时性能。</span><span class="sxs-lookup"><span data-stu-id="5359d-260">Alternately, since the only purpose of this action method is to return a static image file, the references to this action in view files can be updated to reference the static files directly, which improves runtime performance.</span></span> <span data-ttu-id="5359d-261">由于此操作不会进行任何处理，因此没有理由直接为文件提供服务。</span><span class="sxs-lookup"><span data-stu-id="5359d-261">Since no processing is being done as part of this action, there's no reason not to just serve the files directly.</span></span> <span data-ttu-id="5359d-262">如果不 tenable 更新对此操作的所有引用，则可以重写此操作，以生成到静态文件位置的重定向。</span><span class="sxs-lookup"><span data-stu-id="5359d-262">If it's not tenable to update all references to this action, the action could be rewritten to produce a redirect to the static file's location.</span></span>

<span data-ttu-id="5359d-263">下面两个错误在同一代码行的同一私有方法中发生：</span><span class="sxs-lookup"><span data-stu-id="5359d-263">The next two errors both occur in the same private method in the same line of code:</span></span>

```csharp
private void AddUriPlaceHolder(CatalogItem item)
{
    item.PictureUri = this.Url.RouteUrl(PicController.GetPicRouteName, new { catalogItemId = item.Id }, this.Request.Url.Scheme);
}
```

<span data-ttu-id="5359d-264">和都将 `this.Url` `this.Request` 导致编译器错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-264">Both `this.Url` and `this.Request` cause compiler errors.</span></span> <span data-ttu-id="5359d-265">了解如何使用此代码，其目的是构建指向 `PicController` 呈现图像文件的操作的链接。</span><span class="sxs-lookup"><span data-stu-id="5359d-265">Looking at how this code is used, its purpose is to build a link to the `PicController` action that renders image files.</span></span> <span data-ttu-id="5359d-266">我们刚刚发现的相同的是，可能会被替换为 *wwwroot* 中静态文件的直接链接。</span><span class="sxs-lookup"><span data-stu-id="5359d-266">The same one we just discovered could probably be replaced with direct links to the static files located in *wwwroot*.</span></span> <span data-ttu-id="5359d-267">目前，有必要注释掉此代码，并添加 `TODO:` 注释以另一种方式引用图片。</span><span class="sxs-lookup"><span data-stu-id="5359d-267">For now, it's worth commenting out this code and adding a `TODO:` comment to reference the pics another way.</span></span>

<span data-ttu-id="5359d-268">值得注意的 `Controller` 是，此代码出现的类所使用的基类 `CatalogController` 仍引用 `System.Web.Mvc.Controller` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-268">It's worth noting that the base `Controller` class, used by the `CatalogController` class in which this code appears, is still referring to `System.Web.Mvc.Controller`.</span></span> <span data-ttu-id="5359d-269">在我们更新此更新以使用 ASP.NET Core 后，可能会有更多的修复错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-269">There will undoubtedly be more errors to fix once we update this to use ASP.NET Core.</span></span> <span data-ttu-id="5359d-270">首先，删除 `using System.Web.Mvc;` 中的语句列表中的行 `using` `CatalogController` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-270">First, remove the `using System.Web.Mvc;` line from the list of `using` statements in `CatalogController`.</span></span> <span data-ttu-id="5359d-271">接下来，添加 NuGet 包 `Microsoft.AspNetCore.Mvc` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-271">Next, add the NuGet package `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="5359d-272">最后，添加一个 `using Microsoft.AspNetCore.Mvc;` 语句，然后重新生成该应用程序。</span><span class="sxs-lookup"><span data-stu-id="5359d-272">Finally, add a `using Microsoft.AspNetCore.Mvc;` statement, and build the app again.</span></span>

<span data-ttu-id="5359d-273">这次，有16个错误：</span><span class="sxs-lookup"><span data-stu-id="5359d-273">This time, there are 16 errors:</span></span>

- <span data-ttu-id="5359d-274">`Include` 不是有效的命名特性参数 (2) </span><span class="sxs-lookup"><span data-stu-id="5359d-274">`Include` is not a valid named attribute argument (2)</span></span>
- <span data-ttu-id="5359d-275">`HttpStatusCodeResult` 找不到 (3) </span><span class="sxs-lookup"><span data-stu-id="5359d-275">`HttpStatusCodeResult` not found (3)</span></span>
- <span data-ttu-id="5359d-276">`HttpNotFound` 不存在 (3) </span><span class="sxs-lookup"><span data-stu-id="5359d-276">`HttpNotFound` does not exist (3)</span></span>
- <span data-ttu-id="5359d-277">`SelectList` 找不到 (8) </span><span class="sxs-lookup"><span data-stu-id="5359d-277">`SelectList` not found (8)</span></span>

<span data-ttu-id="5359d-278">接下来，让我们逐个查看这些错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-278">Once more, let's review these errors one by one.</span></span> <span data-ttu-id="5359d-279">首先， `SelectList` 可以通过添加来修复 `using Microsoft.AspNetCore.Mvc.Rendering;` ，这会消除一半错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-279">First, `SelectList` can be fixed by adding `using Microsoft.AspNetCore.Mvc.Rendering;`, which eliminates half of the errors.</span></span>

<span data-ttu-id="5359d-280">所有对的引用 `return HttpNotFound();` 应替换为 `return NotFound();` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-280">All references to `return HttpNotFound();` should be replaced with `return NotFound();`.</span></span>

<span data-ttu-id="5359d-281">所有对的引用 `return new HttpStatusCodeResult(HttpStatusCode.BadRequest);` 应替换为 `return BadRequest();` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-281">All references to `return new HttpStatusCodeResult(HttpStatusCode.BadRequest);` should be replaced with `return BadRequest();`.</span></span>

<span data-ttu-id="5359d-282">这只是在 `Include` `[Bind]` 一些操作方法上使用属性，这些方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="5359d-282">That just leaves the use of `Include` with a `[Bind]` attribute on a couple of action methods that look like this:</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult Create([Bind(Include = "Id,Name,Description,Price,PictureFileName,CatalogTypeId,CatalogBrandId,AvailableStock,RestockThreshold,MaxStockThreshold,OnReorder")] CatalogItem catalogItem)
{
```

<span data-ttu-id="5359d-283">前面的代码将模型绑定限制为字符串中列出的属性 `Include` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-283">The preceding code restricts model binding to the properties listed in the `Include` string.</span></span> <span data-ttu-id="5359d-284">在 ASP.NET Core MVC 中， `[Bind]` 特性仍然存在，但不再需要该 `Include =` 参数。</span><span class="sxs-lookup"><span data-stu-id="5359d-284">In ASP.NET Core MVC, the `[Bind]` attribute still exists, but no longer needs the `Include =` argument.</span></span> <span data-ttu-id="5359d-285">直接将属性列表传递给 `[Bind]` 特性：</span><span class="sxs-lookup"><span data-stu-id="5359d-285">Pass the list of properties directly to the `[Bind]` attribute:</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult Create([Bind("Id,Name,Description,Price,PictureFileName,CatalogTypeId,CatalogBrandId,AvailableStock,RestockThreshold,MaxStockThreshold,OnReorder")] CatalogItem catalogItem)
{
```

<span data-ttu-id="5359d-286">进行这些更改后，项目将再编译一次。</span><span class="sxs-lookup"><span data-stu-id="5359d-286">With these changes, the project compiles once more.</span></span> <span data-ttu-id="5359d-287">通常，更好的做法是对控制器输入使用单独的模型类型，而不是直接使用模型绑定到域模型或数据模型类型。</span><span class="sxs-lookup"><span data-stu-id="5359d-287">It's generally a better practice to use separate model types for controller inputs, rather than using model binding directly to your domain model or data model types.</span></span>

## <a name="migrate-views"></a><span data-ttu-id="5359d-288">迁移视图</span><span class="sxs-lookup"><span data-stu-id="5359d-288">Migrate views</span></span>

<span data-ttu-id="5359d-289">与视图相关的两个最重要的 ASP.NET Core MVC 功能都是 [Razor Pages](/aspnet/core/razor-pages/) 和 [标记帮助](/aspnet/core/mvc/views/tag-helpers/built-in/)程序。</span><span class="sxs-lookup"><span data-stu-id="5359d-289">The two biggest ASP.NET Core MVC features related to views are [Razor Pages](/aspnet/core/razor-pages/) and [Tag Helpers](/aspnet/core/mvc/views/tag-helpers/built-in/).</span></span> <span data-ttu-id="5359d-290">对于初始迁移，不使用这两个功能。</span><span class="sxs-lookup"><span data-stu-id="5359d-290">For the initial migration, we won't use either feature.</span></span> <span data-ttu-id="5359d-291">但是，如果在迁移后继续支持应用，则应记住这些功能。</span><span class="sxs-lookup"><span data-stu-id="5359d-291">You should, however, keep the features in mind if you continue supporting the app once it's been migrated.</span></span> <span data-ttu-id="5359d-292">下一步是将 *Views* 文件夹从原始项目复制到新项目中。</span><span class="sxs-lookup"><span data-stu-id="5359d-292">The next step is to copy the *Views* folder from the original project into the new one.</span></span> <span data-ttu-id="5359d-293">构建后，有九个错误：</span><span class="sxs-lookup"><span data-stu-id="5359d-293">After building, there are nine errors:</span></span>

- <span data-ttu-id="5359d-294">HttpContext 不存在 (2) </span><span class="sxs-lookup"><span data-stu-id="5359d-294">HttpContext does not exist (2)</span></span>
- <span data-ttu-id="5359d-295">脚本不存在 (5) </span><span class="sxs-lookup"><span data-stu-id="5359d-295">Scripts does not exist (5)</span></span>
- <span data-ttu-id="5359d-296">样式不存在 (1) </span><span class="sxs-lookup"><span data-stu-id="5359d-296">Styles does not exist (1)</span></span>
- <span data-ttu-id="5359d-297"> (1) 找不到 HtmlString</span><span class="sxs-lookup"><span data-stu-id="5359d-297">HtmlString could not be found(1)</span></span>

<span data-ttu-id="5359d-298">调查这些错误会发现其中的大多数都位于主 *_Layout cshtml* 中，其中有多个与呈现脚本和样式标记相关，或在上一次重新启动了承载应用程序的服务器时显示。</span><span class="sxs-lookup"><span data-stu-id="5359d-298">Investigating these errors finds that most of them are in the main *_Layout.cshtml*, with several related to rendering script and style tags, or displaying when the server hosting the app was last restarted.</span></span> <span data-ttu-id="5359d-299">以下代码列表显示 *_Layout 的 cshtml* 文件中的问题区域：</span><span class="sxs-lookup"><span data-stu-id="5359d-299">The following code listing shows problem areas in the *_Layout.cshtml* file:</span></span>

```razor
// other lines omitted; only errors shown
@Styles.Render("~/Content/css")
@Scripts.Render("~/bundles/modernizr")

@{ var sessionInfo = new HtmlString($"{HttpContext.Current.Session["MachineName"]}, {HttpContext.Current.Session["SessionStartTime"]}");}

@Scripts.Render("~/bundles/jquery")
@Scripts.Render("~/bundles/bootstrap")
```

<span data-ttu-id="5359d-300">对 Modernizr 的引用可以删除。</span><span class="sxs-lookup"><span data-stu-id="5359d-300">The reference to Modernizr can be removed.</span></span> <span data-ttu-id="5359d-301">可以将 CDN 链接替换为适当版本的启动和 jQuery 引用。</span><span class="sxs-lookup"><span data-stu-id="5359d-301">The references to Bootstrap and jQuery can be replaced with CDN links to the appropriate version.</span></span>

<span data-ttu-id="5359d-302">`@Styles.Render`行替换为：</span><span class="sxs-lookup"><span data-stu-id="5359d-302">Replace `@Styles.Render` line with:</span></span>

```html
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
```

<span data-ttu-id="5359d-303">将最后两 `Scripts.Render` 行替换为：</span><span class="sxs-lookup"><span data-stu-id="5359d-303">Replace the last two `Scripts.Render` lines with:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
```

<span data-ttu-id="5359d-304">最后，在引导后 `<link>` ， `<link>` 为应用使用的本地样式添加其他元素。</span><span class="sxs-lookup"><span data-stu-id="5359d-304">Finally, after the Bootstrap `<link>`, add additional `<link>` elements for local styles your app uses.</span></span> <span data-ttu-id="5359d-305">对于 *eShop*，结果如下所示：</span><span class="sxs-lookup"><span data-stu-id="5359d-305">For *eShop*, the result is shown here:</span></span>

```html
<link rel="stylesheet" href="~/Content/custom.css" />
<link rel="stylesheet" href="~/Content/base.css" />
<link rel="stylesheet" href="~/Content/Site.css" />
```

<span data-ttu-id="5359d-306">若要确定元素的显示顺序 `<link>` ，请查看原始应用程序的呈现的 HTML。</span><span class="sxs-lookup"><span data-stu-id="5359d-306">To determine the order in which the `<link>` elements should appear, look at your original app's rendered HTML.</span></span> <span data-ttu-id="5359d-307">另外，请查看 *BundleConfig*，其中 *eShop* 示例包含以下代码来指示适当的顺序：</span><span class="sxs-lookup"><span data-stu-id="5359d-307">Alternatively, review *BundleConfig.cs*, which for the *eShop* sample includes this code indicating the appropriate sequence:</span></span>

```csharp
bundles.Add(new StyleBundle("~/Content/css").Include(
          "~/Content/bootstrap.css",
          "~/Content/custom.css",
          "~/Content/base.css",
          "~/Content/site.css"));
```

<span data-ttu-id="5359d-308">再次生成会发现在 " *创建* 和 *编辑* " 视图上加载 jQuery 验证会出现一个错误。</span><span class="sxs-lookup"><span data-stu-id="5359d-308">Building again reveals one more error loading jQuery Validation on the *Create* and *Edit* views.</span></span> <span data-ttu-id="5359d-309">将其替换为以下脚本：</span><span class="sxs-lookup"><span data-stu-id="5359d-309">Replace it with this script:</span></span>

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.17.0/jquery.validate.min.js" integrity="sha512-O/nUTF5mdFkhEoQHFn9N5wmgYyW323JO6v8kr6ltSRKriZyTr/8417taVWeabVS4iONGk2V444QD0P2cwhuTkg==" crossorigin="anonymous"></script>
```

<span data-ttu-id="5359d-310">视图中要修复的最后一件事是对进行引用以 `Session` 显示应用已运行的时间，以及在哪个计算机上运行的时间。</span><span class="sxs-lookup"><span data-stu-id="5359d-310">The last thing to fix in the views is the reference to `Session` to display how long the app has been running, and on which machine.</span></span> <span data-ttu-id="5359d-311">通过使用和，我们可以直接在站点的 *_Layout* 中显示此数据 `System.Environment.MachineName` `System.Diagnostics.Process.GetCurrentProcess().StartTime` ：</span><span class="sxs-lookup"><span data-stu-id="5359d-311">We can display this data directly in the site's *_Layout.cshtml* by using `System.Environment.MachineName` and `System.Diagnostics.Process.GetCurrentProcess().StartTime`:</span></span>

```razor
<section class="col-sm-6">
    <img class="esh-app-footer-text hidden-xs" src="~/images/main_footer_text.png" width="335" height="26" alt="footer text image" />
    <br />
<small>@Environment.MachineName - @System.Diagnostics.Process.GetCurrentProcess().StartTime.ToString() UTC</small>
</section>
```

<span data-ttu-id="5359d-312">此时，已成功生成应用程序。</span><span class="sxs-lookup"><span data-stu-id="5359d-312">At this point, the app once more builds successfully.</span></span> <span data-ttu-id="5359d-313">不过，尝试运行它只会产生 *Hello World！*</span><span class="sxs-lookup"><span data-stu-id="5359d-313">However, trying to run it just yields *Hello World!*</span></span> <span data-ttu-id="5359d-314">因为 **空** 的 ASP.NET Core 模板仅配置为响应任何请求而显示。</span><span class="sxs-lookup"><span data-stu-id="5359d-314">because the **Empty** ASP.NET Core template is only configured to display that in response to any request.</span></span> <span data-ttu-id="5359d-315">在下一部分中，我通过将应用程序配置为使用 ASP.NET Core MVC 完成迁移，包括依赖关系注入和配置。</span><span class="sxs-lookup"><span data-stu-id="5359d-315">In the next section, I complete the migration by configuring the app to use ASP.NET Core MVC, including dependency injection and configuration.</span></span> <span data-ttu-id="5359d-316">一旦准备就绪，应用就应该运行。</span><span class="sxs-lookup"><span data-stu-id="5359d-316">Once that's in place, the app should run.</span></span> <span data-ttu-id="5359d-317">然后，可以修复 `TODO:` 之前创建的任务。</span><span class="sxs-lookup"><span data-stu-id="5359d-317">Then it will be time to fix the `TODO:` tasks that were created earlier.</span></span>

## <a name="migrate-app-startup-components"></a><span data-ttu-id="5359d-318">迁移应用启动组件</span><span class="sxs-lookup"><span data-stu-id="5359d-318">Migrate app startup components</span></span>

<span data-ttu-id="5359d-319">最后一个迁移步骤是从 *global.asax* 和它调用的类中获取应用启动任务，并将这些任务迁移到 ASP.NET Core 等效项。</span><span class="sxs-lookup"><span data-stu-id="5359d-319">The last migration step is to take the app startup tasks from *Global.asax*, and the classes it calls, and migrate these to their ASP.NET Core equivalents.</span></span> <span data-ttu-id="5359d-320">这些任务包括 MVC 本身的配置，设置依赖关系注入，并使用新的配置系统。</span><span class="sxs-lookup"><span data-stu-id="5359d-320">These tasks include configuration of MVC itself, setting up dependency injection, and working with the new configuration system.</span></span> <span data-ttu-id="5359d-321">在 ASP.NET Core 中，这些任务在 *启动 .cs* 文件中进行处理。</span><span class="sxs-lookup"><span data-stu-id="5359d-321">In ASP.NET Core, these tasks are handled in the *Startup.cs* file.</span></span>

### <a name="configure-mvc"></a><span data-ttu-id="5359d-322">配置 MVC</span><span class="sxs-lookup"><span data-stu-id="5359d-322">Configure MVC</span></span>

<span data-ttu-id="5359d-323">原始 ASP.NET MVC 应用程序在 global.asax 中具有以下代码 `Application_Start` ， 该应用程序在启动时运行：</span><span class="sxs-lookup"><span data-stu-id="5359d-323">The original ASP.NET MVC app has the following code in its `Application_Start` in *Global.asax*, which runs when the app starts up:</span></span>

```csharp
protected void Application_Start()
{
    container = RegisterContainer();
    AreaRegistration.RegisterAllAreas();
    FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
    RouteConfig.RegisterRoutes(RouteTable.Routes);
    BundleConfig.RegisterBundles(BundleTable.Bundles);
    ConfigDataBase();
}
```

<span data-ttu-id="5359d-324">逐个查看这些行， `RegisterContainer` 方法会设置依赖关系注入，这将在下面进行移植。</span><span class="sxs-lookup"><span data-stu-id="5359d-324">Looking at these lines one by one, the `RegisterContainer` method sets up dependency injection, which will be ported below.</span></span> <span data-ttu-id="5359d-325">接下来的三行配置 MVC 的不同部分：区域、筛选器和路由。</span><span class="sxs-lookup"><span data-stu-id="5359d-325">The next three lines configure different parts of MVC: areas, filters, and routes.</span></span> <span data-ttu-id="5359d-326">绑定由移植应用中的静态文件替换。</span><span class="sxs-lookup"><span data-stu-id="5359d-326">Bundles are replaced by static files in the ported app.</span></span> <span data-ttu-id="5359d-327">最后一行为应用设置数据访问，将在后面的部分中显示。</span><span class="sxs-lookup"><span data-stu-id="5359d-327">The last line sets up data access for the app, which will be shown in a later section.</span></span>

<span data-ttu-id="5359d-328">由于此应用程序实际上不使用区域，因此无需执行任何操作来迁移区域注册呼叫。</span><span class="sxs-lookup"><span data-stu-id="5359d-328">Since this app isn't actually using areas, there's nothing that needs to be done to migrate the area registration call.</span></span> <span data-ttu-id="5359d-329">如果你的应用程序需要迁移区域，则 [文档将指定如何在 ASP.NET Core 中配置区域](/aspnet/core/mvc/controllers/areas)。</span><span class="sxs-lookup"><span data-stu-id="5359d-329">If your app does need to migrate areas, the [docs specify how to configure areas in ASP.NET Core](/aspnet/core/mvc/controllers/areas).</span></span>

<span data-ttu-id="5359d-330">注册全局筛选器的调用会在 `FilterConfig` 应用的 *App_Start* 文件夹中调用类的帮助程序：</span><span class="sxs-lookup"><span data-stu-id="5359d-330">The call to register global filters invokes a helper on the `FilterConfig` class in the app's *App_Start* folder:</span></span>

```csharp
public static void RegisterGlobalFilters(GlobalFilterCollection filters)
{
    filters.Add(new HandleErrorAttribute());
}
```

<span data-ttu-id="5359d-331">添加到应用的唯一属性是 ASP.NET MVC 筛选器 `HandleErrorAttribute` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-331">The only attribute added to the app is the ASP.NET MVC filter, `HandleErrorAttribute`.</span></span> <span data-ttu-id="5359d-332">此筛选器可确保当发生异常作为请求的一部分时，将显示默认操作和视图，而不是显示异常详细信息。</span><span class="sxs-lookup"><span data-stu-id="5359d-332">This filter ensures that when an exception occurs as part of a request, a default action and view are displayed, rather than the exception details.</span></span> <span data-ttu-id="5359d-333">在 ASP.NET Core 中，中间件将执行相同的功能 `UseExceptionHandler` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-333">In ASP.NET Core, this same functionality is performed by the `UseExceptionHandler` middleware.</span></span> <span data-ttu-id="5359d-334">默认情况下不启用详细的错误消息。</span><span class="sxs-lookup"><span data-stu-id="5359d-334">The detailed error messages aren't enabled by default.</span></span> <span data-ttu-id="5359d-335">必须使用中间件来配置这些设置 `UseDeveloperExceptionPage` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-335">They must be configured using the `UseDeveloperExceptionPage` middleware.</span></span> <span data-ttu-id="5359d-336">若要配置此行为以匹配原始应用程序，必须将以下代码添加到 `Configure` *启动* 中方法的开头：</span><span class="sxs-lookup"><span data-stu-id="5359d-336">To configure this behavior to match the original app, the following code must be added to the start of the `Configure` method in *Startup.cs*:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
    }
    // ...
}
```

<span data-ttu-id="5359d-337">这将负责 eShop 应用使用的唯一筛选器，在这种情况下，可以使用内置中间件来完成此操作。</span><span class="sxs-lookup"><span data-stu-id="5359d-337">This takes care of the only filter used by the eShop app, and in this case it was done by using built-in middleware.</span></span> <span data-ttu-id="5359d-338">如果你有必须在应用程序中配置的全局筛选器，则当在方法中添加 MVC 时，将会执行此操作 `ConfigureServices` ，本章稍后将会显示。</span><span class="sxs-lookup"><span data-stu-id="5359d-338">If you have global filters that must be configured in your app, this is done when MVC is added in the `ConfigureServices` method, which is shown later in this chapter.</span></span>

<span data-ttu-id="5359d-339">需要迁移的与 MVC 相关的最后一部分是应用的默认路由。</span><span class="sxs-lookup"><span data-stu-id="5359d-339">The last piece of MVC-related logic that needs to be migrated are the app's default routes.</span></span> <span data-ttu-id="5359d-340">对的调用 `RouteConfig.RegisterRoutes(RouteTable.Routes)` 会将 MVC 路由表传递给 `RegisterRoutes` helper 方法，在此方法中，应用启动时将执行以下代码：</span><span class="sxs-lookup"><span data-stu-id="5359d-340">The call to `RouteConfig.RegisterRoutes(RouteTable.Routes)` passes the MVC route table to the `RegisterRoutes` helper method, where the following code is executed when the app starts up:</span></span>

```csharp
public static void RegisterRoutes(RouteCollection routes)
{
    routes.MapMvcAttributeRoutes();
    routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Catalog", action = "Index", id = UrlParameter.Optional }
    );
}
```

<span data-ttu-id="5359d-341">如果逐行执行此代码，则第一行会设置对属性路由的支持。</span><span class="sxs-lookup"><span data-stu-id="5359d-341">Taking this code line-by-line, the first line sets up support for attribute routes.</span></span> <span data-ttu-id="5359d-342">这内置于 ASP.NET Core 中，因此不需要单独配置。</span><span class="sxs-lookup"><span data-stu-id="5359d-342">This is built into ASP.NET Core, so it's unnecessary to configure it separately.</span></span> <span data-ttu-id="5359d-343">同样， *{resource} axd* 文件不与 ASP.NET Core 一起使用，因此无需忽略此类路由。</span><span class="sxs-lookup"><span data-stu-id="5359d-343">Likewise, *{resource}.axd* files aren't used with ASP.NET Core, so there's no need to ignore such routes.</span></span> <span data-ttu-id="5359d-344">`MapRoute`方法为使用典型路由模板的 MVC 配置默认值 `{controller}/{action}/{id}` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-344">The `MapRoute` method configures the default for MVC, which uses the typical `{controller}/{action}/{id}` route template.</span></span> <span data-ttu-id="5359d-345">它还指定此模板的默认值，以使 `CatalogController` 成为使用的默认控制器并且 `Index` 方法是默认操作。</span><span class="sxs-lookup"><span data-stu-id="5359d-345">It also specifies the defaults for this template, such that the `CatalogController` is the default controller used and the `Index` method is the default action.</span></span> <span data-ttu-id="5359d-346">较大的应用通常会包含多个对的调用，以 `MapRoute` 设置其他路由。</span><span class="sxs-lookup"><span data-stu-id="5359d-346">Larger apps will frequently include more calls to `MapRoute` to set up additional routes.</span></span>

<span data-ttu-id="5359d-347">ASP.NET Core MVC 支持 [传统路由和属性路由](/aspnet/core/mvc/controllers/routing?preserve-view=true&view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="5359d-347">ASP.NET Core MVC supports [conventional routing and attribute routing](/aspnet/core/mvc/controllers/routing?preserve-view=true&view=aspnetcore-2.2).</span></span> <span data-ttu-id="5359d-348">传统路由类似于前面列出的方法中的路由表的配置方式 `RegisterRoutes` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-348">Conventional routing is analogous to how the route table is configured in the `RegisterRoutes` method listed previously.</span></span> <span data-ttu-id="5359d-349">若要使用默认路由（如 *eShop* 应用程序中使用的路由）设置传统路由，请将以下代码添加到 `Configure` *Startup* 中方法的底部：</span><span class="sxs-lookup"><span data-stu-id="5359d-349">To set up conventional routing with a default route like the one used in the *eShop* app, add the following code to the bottom of the `Configure` method in *Startup.cs*:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Catalog}/{action=Index}/{id?}");
});
```

> [!NOTE]
> <span data-ttu-id="5359d-350">对于 ASP.NET Core 3.0 及更高版本，此更改为使用终结点。</span><span class="sxs-lookup"><span data-stu-id="5359d-350">With ASP.NET Core 3.0 and later, this is changed to use endpoints.</span></span> <span data-ttu-id="5359d-351">对于要 ASP.NET Core 2.2 的初始端口，这是用于映射传统路由的正确语法。</span><span class="sxs-lookup"><span data-stu-id="5359d-351">For the initial port to ASP.NET Core 2.2, this is the proper syntax for mapping conventional routes.</span></span>

<span data-ttu-id="5359d-352">进行这些更改 `Configure` 后，就几乎完成了方法。</span><span class="sxs-lookup"><span data-stu-id="5359d-352">With these changes in place, the `Configure` method is almost done.</span></span> <span data-ttu-id="5359d-353">打印 Hello World 的原始模板 `app.Run` 方法 *！*</span><span class="sxs-lookup"><span data-stu-id="5359d-353">The original template's `app.Run` method that prints *Hello World!*</span></span> <span data-ttu-id="5359d-354">应删除。</span><span class="sxs-lookup"><span data-stu-id="5359d-354">should be deleted.</span></span> <span data-ttu-id="5359d-355">此时，该方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="5359d-355">At this point, the method is as shown here:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute("default", "{controller=Catalog}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="5359d-356">现在是时候配置 MVC 服务，接下来是应用程序对依赖关系注入的支持 (DI) 。</span><span class="sxs-lookup"><span data-stu-id="5359d-356">Now it's time to configure MVC services, followed by the rest of the app's support for dependency injection (DI).</span></span> <span data-ttu-id="5359d-357">到目前为止， *eShopPorted* 项目的 `ConfigureServices` 方法保持为空。</span><span class="sxs-lookup"><span data-stu-id="5359d-357">So far, the *eShopPorted* project's `ConfigureServices` method has remained empty.</span></span> <span data-ttu-id="5359d-358">现在是时候开始填充它了。</span><span class="sxs-lookup"><span data-stu-id="5359d-358">Now it's time to start populating it.</span></span>

<span data-ttu-id="5359d-359">首先，若要使 ASP.NET Core MVC 正常工作，需要添加：</span><span class="sxs-lookup"><span data-stu-id="5359d-359">First, to get ASP.NET Core MVC to work properly, it needs to be added:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
}
```

<span data-ttu-id="5359d-360">前面的代码是使 MVC 功能正常工作所需的最低配置。</span><span class="sxs-lookup"><span data-stu-id="5359d-360">The preceding code is the minimal configuration required to get MVC features working.</span></span> <span data-ttu-id="5359d-361">此调用可以配置许多其他功能 (其中一些功能在本章后面的部分进行了详细介绍) ，但是现在这就足以构建应用了。</span><span class="sxs-lookup"><span data-stu-id="5359d-361">There are many additional features that can be configured from this call (some of which are detailed later in this chapter), but for now this will suffice to build the app.</span></span> <span data-ttu-id="5359d-362">现在运行它会正确路由默认请求，但由于尚未配置 DI，因此在激活时出现错误， `CatalogController` 因为尚未提供类型的实现 `ICatalogService` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-362">Running it now routes the default request properly, but since we've not yet configured DI, an error occurs while activating `CatalogController`, because no implementation of type `ICatalogService` has been provided yet.</span></span> <span data-ttu-id="5359d-363">稍后我们将再次配置 MVC。</span><span class="sxs-lookup"><span data-stu-id="5359d-363">We'll return to configure MVC further in a moment.</span></span> <span data-ttu-id="5359d-364">现在，让我们迁移应用程序的依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="5359d-364">For now, let's migrate the app's dependency injection.</span></span>

#### <a name="migrate-dependency-injection-configuration"></a><span data-ttu-id="5359d-365">迁移依赖关系注入配置</span><span class="sxs-lookup"><span data-stu-id="5359d-365">Migrate dependency injection configuration</span></span>

<span data-ttu-id="5359d-366">原始应用程序的 *global.asax* 文件定义以下方法，在应用启动时调用该方法：</span><span class="sxs-lookup"><span data-stu-id="5359d-366">The original app's *Global.asax* file defines the following method, called when the app starts up:</span></span>

```csharp
protected IContainer RegisterContainer()
{
  var builder = new ContainerBuilder();

  builder.RegisterControllers(typeof(MvcApplication).Assembly);

  var mockData = bool.Parse(ConfigurationManager.AppSettings["UseMockData"]);
  builder.RegisterModule(new ApplicationModule(mockData));

  var container = builder.Build();
  DependencyResolver.SetResolver(new AutofacDependencyResolver(container));

  return container;
}
```

<span data-ttu-id="5359d-367">此代码配置一个 [Autofac](https://autofac.org/) 容器，读取一个配置设置以确定是否应使用真实数据或模拟数据，并将此设置传递到 Autofac 模块 (在应用的 */Modules* 目录) 中找到。</span><span class="sxs-lookup"><span data-stu-id="5359d-367">This code configures an [Autofac](https://autofac.org/) container, reads a config setting to determine whether real or mock data should be used, and passes this setting into an Autofac module (found in the app's */Modules* directory).</span></span> <span data-ttu-id="5359d-368">幸运的是，Autofac 支持 .NET Core，因此模块可以直接迁移。</span><span class="sxs-lookup"><span data-stu-id="5359d-368">Fortunately, Autofac supports .NET Core, so the module can be migrated directly.</span></span> <span data-ttu-id="5359d-369">将文件夹复制到新项目并更新类的命名空间，并进行编译。</span><span class="sxs-lookup"><span data-stu-id="5359d-369">Copy the folder into the new project and updates the class's namespace and it should compile.</span></span>

<span data-ttu-id="5359d-370">ASP.NET Core 提供对依赖关系注入的内置支持，但是，如果需要，可以轻松地连接到 Autofac 等第三方容器。</span><span class="sxs-lookup"><span data-stu-id="5359d-370">ASP.NET Core has built-in support for dependency injection, but you can wire up a third-party container such as Autofac easily if necessary.</span></span> <span data-ttu-id="5359d-371">在这种情况下，因为已将应用程序配置为使用 Autofac，所以最简单的解决方案是保持其使用。</span><span class="sxs-lookup"><span data-stu-id="5359d-371">In this case, since the app is already configured to use Autofac, the simplest solution is to maintain its usage.</span></span> <span data-ttu-id="5359d-372">为此， `ConfigureServices` 必须修改方法签名以返回 `IServiceProvider` ，并从方法配置和返回 Autofac 容器实例。</span><span class="sxs-lookup"><span data-stu-id="5359d-372">To do so, the `ConfigureServices` method signature must be modified to return an `IServiceProvider`, and the Autofac container instance must be configured and returned from the method.</span></span>

<span data-ttu-id="5359d-373">**注意：** 在 .NET Core 3.0 及更高版本中，集成第三方 DI 容器的过程已更改。</span><span class="sxs-lookup"><span data-stu-id="5359d-373">**Note:** In .NET Core 3.0 and later, the process for integrating a third-party DI container has changed.</span></span>

<span data-ttu-id="5359d-374">配置 Autofac 的一部分需要调用 `builder.Populate(services)` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-374">Part of configuring Autofac requires a call to `builder.Populate(services)`.</span></span> <span data-ttu-id="5359d-375">此扩展在 `Autofac.Extensions.DependencyInjection` NuGet 包中找到，在编译代码之前，必须先安装此扩展。</span><span class="sxs-lookup"><span data-stu-id="5359d-375">This extension is found in the `Autofac.Extensions.DependencyInjection` NuGet package, which must be installed before the code will compile.</span></span>

<span data-ttu-id="5359d-376">在修改 `ConfigureServices` 以配置 Autofac 容器后，新的方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="5359d-376">After modifying `ConfigureServices` to configure an Autofac container, the new method is as shown here:</span></span>

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    // Create Autofac container builder
    var builder = new ContainerBuilder();
    builder.Populate(services);
    bool useMockData = true; // TODO: read from config
    builder.RegisterModule(new ApplicationModule(useMockData));

    ILifetimeScope container = builder.Build();

    return new AutofacServiceProvider(container);
}
```

<span data-ttu-id="5359d-377">目前，的设置 `useMockData` 设置为 `true` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-377">For now, the setting for `useMockData` is set to `true`.</span></span> <span data-ttu-id="5359d-378">此设置将在一段时间内从配置中读取。</span><span class="sxs-lookup"><span data-stu-id="5359d-378">This setting will be read from configuration in a moment.</span></span> <span data-ttu-id="5359d-379">此时，应用会在运行时编译并成功加载，如图4-12 所示。</span><span class="sxs-lookup"><span data-stu-id="5359d-379">At this point, the app compiles and should load successfully when run, as shown in Figure 4-12.</span></span>

![图4-12](media/Figure4-12.png)

<span data-ttu-id="5359d-381">**图 4-12.**</span><span class="sxs-lookup"><span data-stu-id="5359d-381">**Figure 4-12.**</span></span> <span data-ttu-id="5359d-382">在本地运行的、带有模拟数据的 *eShop* 应用。</span><span class="sxs-lookup"><span data-stu-id="5359d-382">Ported *eShop* app running locally with mock data.</span></span>

#### <a name="migrate-app-settings"></a><span data-ttu-id="5359d-383">迁移应用设置</span><span class="sxs-lookup"><span data-stu-id="5359d-383">Migrate app settings</span></span>

<span data-ttu-id="5359d-384">ASP.NET Core 使用一个新的 [配置系统](/aspnet/core/fundamentals/configuration/?preserve-view=true&view=aspnetcore-2.2)，默认情况下，该系统利用文件 *上的appsettings.js* 。</span><span class="sxs-lookup"><span data-stu-id="5359d-384">ASP.NET Core uses a new [configuration system](/aspnet/core/fundamentals/configuration/?preserve-view=true&view=aspnetcore-2.2), which by default leverages an *appsettings.json* file.</span></span> <span data-ttu-id="5359d-385">通过 `CreateDefaultBuilder` 在 *.cs* 中使用，已在应用程序中设置了默认配置。</span><span class="sxs-lookup"><span data-stu-id="5359d-385">By using `CreateDefaultBuilder` in *Program.cs*, the default configuration is already set up in the app.</span></span> <span data-ttu-id="5359d-386">若要访问配置，类只需在其构造函数中请求它即可。</span><span class="sxs-lookup"><span data-stu-id="5359d-386">To access configuration, classes just need to request it in their constructor.</span></span> <span data-ttu-id="5359d-387">`Startup`类不是异常。</span><span class="sxs-lookup"><span data-stu-id="5359d-387">The `Startup` class is no exception.</span></span> <span data-ttu-id="5359d-388">若要开始访问中的配置 `Startup` 和应用程序的其余部分，请 `IConfiguration` 从其构造函数中请求的实例：</span><span class="sxs-lookup"><span data-stu-id="5359d-388">To start accessing configuration in `Startup` and the rest of the app, request an instance of `IConfiguration` from its constructor:</span></span>

```csharp
public Startup(IConfiguration configuration)
{
    Configuration = configuration;
}

public IConfiguration Configuration { get; }
```

<span data-ttu-id="5359d-389">原始应用使用引用其设置 `ConfigurationManager.AppSettings` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-389">The original app referenced its settings using `ConfigurationManager.AppSettings`.</span></span> <span data-ttu-id="5359d-390">此术语的所有引用的快速搜索将生成新应用所需的一组设置。</span><span class="sxs-lookup"><span data-stu-id="5359d-390">A quick search for all references of this term yields the set of settings the new app needs.</span></span> <span data-ttu-id="5359d-391">只有两个：</span><span class="sxs-lookup"><span data-stu-id="5359d-391">There are only two:</span></span>

- `UseMockData`
- `UseCustomizationData`

<span data-ttu-id="5359d-392">如果应用具有更复杂的配置（尤其是在使用自定义配置节时），则可能需要创建对象并将其绑定到应用配置的不同部分。</span><span class="sxs-lookup"><span data-stu-id="5359d-392">If your app has more complex configuration, especially if it's using custom configuration sections, you'll probably want to create and bind objects to different parts of your app's configuration.</span></span> <span data-ttu-id="5359d-393">然后，可以使用 [options 模式](../../core/extensions/options.md)访问这些类型。</span><span class="sxs-lookup"><span data-stu-id="5359d-393">These types can then be accessed using the [options pattern](../../core/extensions/options.md).</span></span> <span data-ttu-id="5359d-394">但是，如引用的文档中所述，此模式不应在中使用 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-394">However, as noted in the referenced doc, this pattern shouldn't be used in `ConfigureServices`.</span></span> <span data-ttu-id="5359d-395">相反，移植应用会直接引用 `UseMockData` 配置值。</span><span class="sxs-lookup"><span data-stu-id="5359d-395">Instead the ported app will reference the `UseMockData` configuration value directly.</span></span>

<span data-ttu-id="5359d-396">首先，修改端口上的应用 `appsettings.json` 文件，并在根中添加这两个设置：</span><span class="sxs-lookup"><span data-stu-id="5359d-396">First, modify the ported app's `appsettings.json` file and add the two settings in the root:</span></span>

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*",
  "UseMockData": "true",
  "UseCustomizationData" :  "true"
}
```

<span data-ttu-id="5359d-397">现在，请修改 `ConfigureServices` 以 `UseMockData` 从属性 (访问设置， `Configuration` 先前我们将值设置为 `true`) ：</span><span class="sxs-lookup"><span data-stu-id="5359d-397">Now, modify `ConfigureServices` to access the `UseMockData` setting from the `Configuration` property (where previously we set the value to `true`):</span></span>

```csharp
  bool useMockData = Configuration.GetValue<bool>("UseMockData");
```

<span data-ttu-id="5359d-398">此时，该设置是从配置中提取的。</span><span class="sxs-lookup"><span data-stu-id="5359d-398">At this point, the setting is pulled from configuration.</span></span> <span data-ttu-id="5359d-399">其他设置 `UseCustomizationData` 由 `CatalogDBInitializer` 类使用。</span><span class="sxs-lookup"><span data-stu-id="5359d-399">The other setting, `UseCustomizationData`, is used by the `CatalogDBInitializer` class.</span></span> <span data-ttu-id="5359d-400">首次移植此类时，已注释掉对的访问 `ConfigurationManager.AppSettings["UseCustomizationData"]` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-400">When you first ported this class, you commented out the access to `ConfigurationManager.AppSettings["UseCustomizationData"]`.</span></span> <span data-ttu-id="5359d-401">现在可以将其修改为使用 ASP.NET Core 配置。</span><span class="sxs-lookup"><span data-stu-id="5359d-401">Now it's time to modify it to use ASP.NET Core configuration.</span></span> <span data-ttu-id="5359d-402">修改的构造函数 `CatalogDBInitializer` ，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5359d-402">Modify the constructor of `CatalogDBInitializer` as follows:</span></span>

```csharp
  // add using Microsoft.Extensions.Configuration
  public CatalogDBInitializer(CatalogItemHiLoGenerator indexGenerator,
      IConfiguration configuration)
  {
      this.indexGenerator = indexGenerator;
      useCustomizationData = configuration.GetValue<bool>("UseCustomizationData");
  }
```

<span data-ttu-id="5359d-403">应以这种方式修改对 web 应用中的配置的所有访问权限，以使用新 `IConfiguration` 类型。</span><span class="sxs-lookup"><span data-stu-id="5359d-403">All access to configuration within the web app should be modified in this manner to use the new `IConfiguration` type.</span></span> <span data-ttu-id="5359d-404">需要访问 .NET Framework 配置的依赖项可能会在添加到 web 项目的 *app.config* 文件中包含此类设置。</span><span class="sxs-lookup"><span data-stu-id="5359d-404">Dependencies that require access to .NET Framework configuration can include such settings in an *app.config* file added to the web project.</span></span> <span data-ttu-id="5359d-405">依赖项目可用于 `ConfigurationManager` 访问设置，如果已使用此方法，则不应要求进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="5359d-405">The dependent projects can work with `ConfigurationManager` to access settings, and shouldn't require any changes if they already use this approach.</span></span> <span data-ttu-id="5359d-406">不过，由于 ASP.NET Core 应用作为其自己的可执行文件运行，因此它们不会引用 *web.config* ，而是 *app.config*。通过将旧应用程序 *web.config* 文件中的设置迁移到 ASP.NET Core 应用程序中的新 *app.config* 文件，用于 `ConfigurationManager` 访问其设置的组件将继续正常工作。</span><span class="sxs-lookup"><span data-stu-id="5359d-406">However, since ASP.NET Core apps run as their own executable, they don't reference *web.config* but rather *app.config*. By migrating settings from the legacy app's *web.config* file to a new *app.config* file in the ASP.NET Core app, components that use `ConfigurationManager` to access their settings will continue to function properly.</span></span>

<span data-ttu-id="5359d-407">应用的迁移即将完成。</span><span class="sxs-lookup"><span data-stu-id="5359d-407">The app's migration is nearly complete.</span></span> <span data-ttu-id="5359d-408">唯一剩余的任务是数据访问配置。</span><span class="sxs-lookup"><span data-stu-id="5359d-408">The only remaining task is data access configuration.</span></span>
  
## <a name="data-access-considerations"></a><span data-ttu-id="5359d-409">数据访问注意事项</span><span class="sxs-lookup"><span data-stu-id="5359d-409">Data access considerations</span></span>

<span data-ttu-id="5359d-410">.NET Framework 上运行的 ASP.NET Core 应用可以继续利用 (EF) 的实体框架。</span><span class="sxs-lookup"><span data-stu-id="5359d-410">ASP.NET Core apps running on .NET Framework can continue to leverage Entity Framework (EF).</span></span> <span data-ttu-id="5359d-411">如果执行增量迁移，则在尝试将应用程序的数据访问移植到使用 EF Core 时，应用程序将使用 EF 6。</span><span class="sxs-lookup"><span data-stu-id="5359d-411">If performing an incremental migration, getting the app working with EF 6 before trying to port its data access to use EF Core may be worthwhile.</span></span> <span data-ttu-id="5359d-412">通过这种方式，可以在另一个迁移工作块开始之前确定和解决应用的任何迁移问题。</span><span class="sxs-lookup"><span data-stu-id="5359d-412">In this way, any problems with the app's migration can be identified and addressed before another block of migration effort is begun.</span></span>

<span data-ttu-id="5359d-413">在这种情况下，在 eShop 示例迁移中配置 EF 6 并不需要任何特殊工作，因为此操作是在 Autofac 中执行的 `ApplicationModule` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-413">As it happens, configuring EF 6 in the eShop sample migration doesn't require any special work, since this work was performed in the Autofac `ApplicationModule`.</span></span> <span data-ttu-id="5359d-414">唯一的问题是，类当前 `CatalogDBContext` 尝试从 *web.config* 读取其连接字符串。若要解决此情况，需要将连接详细信息添加到 *appsettings.js*。</span><span class="sxs-lookup"><span data-stu-id="5359d-414">The only problem is that currently the `CatalogDBContext` class tries to read its connection string from *web.config*. To address this, the connection details need to be added to *appsettings.json*.</span></span> <span data-ttu-id="5359d-415">然后，在创建连接字符串时必须将其传递到 `CatalogDBContext` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-415">Then the connection string must be passed into `CatalogDBContext` when it's created.</span></span>

<span data-ttu-id="5359d-416">更新 *appsettings.js* 以包含连接字符串。</span><span class="sxs-lookup"><span data-stu-id="5359d-416">Update the *appsettings.json* to include the connection string.</span></span> <span data-ttu-id="5359d-417">下面列出了完整的文件：</span><span class="sxs-lookup"><span data-stu-id="5359d-417">The full file is listed here:</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=eShopPorted;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*",
  "UseMockData": "false",
  "UseCustomizationData": "true"
}
```

<span data-ttu-id="5359d-418">创建时，必须将连接字符串传递到构造函数中 `DbContext` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-418">The connection string must be passed into the constructor when the `DbContext` is created.</span></span> <span data-ttu-id="5359d-419">由于实例是由 Autofac 创建的，因此需要在中进行更改 `ApplicationModule` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-419">Since the instances are created by Autofac, the change needs to be made in `ApplicationModule`.</span></span> <span data-ttu-id="5359d-420">修改模块以在 `connectionString` 其构造函数中采用，并将其分配给字段。</span><span class="sxs-lookup"><span data-stu-id="5359d-420">Modify the module to take in a `connectionString` in its constructor and assign it to a field.</span></span> <span data-ttu-id="5359d-421">然后修改的注册 `CatalogDBContext` ，将连接字符串作为参数添加：</span><span class="sxs-lookup"><span data-stu-id="5359d-421">Then modify the registration for `CatalogDBContext` to add connection string as a parameter:</span></span>

```csharp
builder.RegisterType<CatalogDBContext>()
  .WithParameter("connectionString", _connectionString)
  .InstancePerLifetimeScope();
```

<span data-ttu-id="5359d-422">还必须将参数添加到新的构造函数重载 `CatalogDBContext` 本身中：</span><span class="sxs-lookup"><span data-stu-id="5359d-422">The parameter must also be added to a new constructor overload in `CatalogDBContext` itself:</span></span>

```csharp
public CatalogDBContext(string connectionString) : base(connectionString)
{
}
```

<span data-ttu-id="5359d-423">最后， `ConfigureServices` 必须从中读取连接字符串 `Config` ，并在 `ApplicationModule` 其实例化时将其传递到：</span><span class="sxs-lookup"><span data-stu-id="5359d-423">Finally, `ConfigureServices` must read the connection string from `Config` and pass it into the `ApplicationModule` when it instantiates it:</span></span>

```csharp
bool useMockData = Configuration.GetValue<bool>("UseMockData");
string connectionString = Configuration.GetConnectionString("DefaultConnection");
builder.RegisterModule(new ApplicationModule(useMockData, connectionString));
```

<span data-ttu-id="5359d-424">使用此代码后，应用程序将像以前一样运行，在为时连接到 SQL Server 的 `UseMockData` 数据库 `false` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-424">With this code in place, the app runs as it did before, connecting to a SQL Server database when `UseMockData` is `false`.</span></span>

<span data-ttu-id="5359d-425">此时，可以在生产环境中部署和运行该应用，转换为 ASP.NET Core 但仍在 .NET Framework 和 EF 6 上运行。</span><span class="sxs-lookup"><span data-stu-id="5359d-425">The app can be deployed and run in production at this point, converted to ASP.NET Core but still running on .NET Framework and EF 6.</span></span> <span data-ttu-id="5359d-426">如果需要，可以将应用迁移到 .NET Core 和 Entity Framework Core 上运行，这会带来前面几章所述的其他优势。</span><span class="sxs-lookup"><span data-stu-id="5359d-426">If desired, the app can be migrated to run on .NET Core and Entity Framework Core, which will bring additional advantages described in earlier chapters.</span></span> <span data-ttu-id="5359d-427">此文档特定于实体框架， [它将 EF Core 和 EF 6 进行比较](/ef/efcore-and-ef6/) ，并包含一个网格，其中显示了哪些库支持每个单个功能。</span><span class="sxs-lookup"><span data-stu-id="5359d-427">Specific to Entity Framework, [this documentation compares EF Core and EF 6](/ef/efcore-and-ef6/) and includes a grid showing which library supports each of dozens of individual features.</span></span>

### <a name="migrate-to-entity-framework-core"></a><span data-ttu-id="5359d-428">迁移到 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5359d-428">Migrate to Entity Framework Core</span></span>

<span data-ttu-id="5359d-429">假设决定迁移到 EF Core，则步骤可能相当简单，特别是在原始应用使用基于代码的模型方法时。</span><span class="sxs-lookup"><span data-stu-id="5359d-429">Assuming a decision is made to migrate to EF Core, the steps can be fairly straightforward, especially if the original app used a code-based model approach.</span></span> <span data-ttu-id="5359d-430">[准备从 EF 6 到 EF Core 的端口](/ef/efcore-and-ef6/porting/)时，请查看要使用 EF Core 的目标版本中的功能可用性。</span><span class="sxs-lookup"><span data-stu-id="5359d-430">When [preparing to port from EF 6 to EF Core](/ef/efcore-and-ef6/porting/), review the availability of features in the destination version of EF Core you'll be using.</span></span> <span data-ttu-id="5359d-431">查看有关[从和基于 EDMX 的模型的迁移以及](/ef/efcore-and-ef6/porting/port-edmx)[从基于代码的模型移植](/ef/efcore-and-ef6/porting/port-code)的文档。</span><span class="sxs-lookup"><span data-stu-id="5359d-431">Review the documentation on [porting from and EDMX-based model](/ef/efcore-and-ef6/porting/port-edmx) versus [porting from a code-based model](/ef/efcore-and-ef6/porting/port-code).</span></span>

<span data-ttu-id="5359d-432">若要升级到 EF Core 2.2，所涉及的基本步骤是添加适当的 NuGet 包 (s) 和更新命名空间。</span><span class="sxs-lookup"><span data-stu-id="5359d-432">To upgrade to EF Core 2.2, the basic steps involved are to add the appropriate NuGet package(s) and update namespaces.</span></span> <span data-ttu-id="5359d-433">然后调整如何将连接字符串传递到 `DbContext` 类型，以及如何将其连接到依赖关系注入。</span><span class="sxs-lookup"><span data-stu-id="5359d-433">Then adjust how the connection string is passed to the `DbContext` type and how they're wired up for dependency injection.</span></span>

<span data-ttu-id="5359d-434">EF Core 作为包引用添加到项目中：</span><span class="sxs-lookup"><span data-stu-id="5359d-434">EF Core is added as a package reference to the project:</span></span>

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore" Version="2.2.6" />
```

<span data-ttu-id="5359d-435">移除了对 EF 6 的引用：</span><span class="sxs-lookup"><span data-stu-id="5359d-435">The reference to EF 6 is removed:</span></span>

```xml
<PackageReference Include="EntityFramework" Version="6.2.0" />
```

<span data-ttu-id="5359d-436">编译器将在和中报告 `CatalogDBContext` 错误 `CatalogDBInitializer` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-436">The compiler will report errors in `CatalogDBContext` and `CatalogDBInitializer`.</span></span> <span data-ttu-id="5359d-437">`CatalogDbContext` 需要删除旧的命名空间，并将其替换为 `Microsoft.EntityFrameworkCore` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-437">`CatalogDbContext` needs to have the old namespaces removed and replaced with `Microsoft.EntityFrameworkCore`.</span></span> <span data-ttu-id="5359d-438">可删除其构造函数。</span><span class="sxs-lookup"><span data-stu-id="5359d-438">Its constructors can be removed.</span></span> <span data-ttu-id="5359d-439">`DbModelBuilder` 应该替换为 `ModelBuilder` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-439">`DbModelBuilder` should be replaced with `ModelBuilder`.</span></span> <span data-ttu-id="5359d-440">用于配置类型的帮助器方法将移动到实现的单独类中 `IEntityTypeConfiguration<T>` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-440">The helper methods for configuring types are moved to separate classes implementing `IEntityTypeConfiguration<T>`.</span></span> <span data-ttu-id="5359d-441">然后， `CatalogDBContext` 类的 `OnModelCreating` 方法只是：</span><span class="sxs-lookup"><span data-stu-id="5359d-441">Then the `CatalogDBContext` class's `OnModelCreating` method simply becomes:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    builder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());

    base.OnModelCreating(builder);
}
```

<span data-ttu-id="5359d-442">涉及的其他更改包括：</span><span class="sxs-lookup"><span data-stu-id="5359d-442">Other changes involved include:</span></span>

- <span data-ttu-id="5359d-443">`HasDatabaseGeneratedOption(DatabaseGeneratedOption.None)` 替换为 `ValueGeneratedNever()`</span><span class="sxs-lookup"><span data-stu-id="5359d-443">`HasDatabaseGeneratedOption(DatabaseGeneratedOption.None)` replaced with `ValueGeneratedNever()`</span></span>
- <span data-ttu-id="5359d-444">`HasRequired<T>` 替换为 `HasOne<T>`</span><span class="sxs-lookup"><span data-stu-id="5359d-444">`HasRequired<T>` replaced with `HasOne<T>`</span></span>
- <span data-ttu-id="5359d-445">已安装 `Microsoft.EntityFrameworkCore.Relational` 包</span><span class="sxs-lookup"><span data-stu-id="5359d-445">Installed `Microsoft.EntityFrameworkCore.Relational` package</span></span>
- <span data-ttu-id="5359d-446">添加构造函数以 `CatalogDBContext` 采用 `DbContextOptions` 并将其传递到基构造函数</span><span class="sxs-lookup"><span data-stu-id="5359d-446">Add a constructor to `CatalogDBContext` taking `DbContextOptions` and passing it to the base constructor</span></span>

<span data-ttu-id="5359d-447">的示例配置类如下 `CatalogType` 所示：</span><span class="sxs-lookup"><span data-stu-id="5359d-447">An example configuration class for `CatalogType` is shown here:</span></span>

```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata.Builders;

namespace eShopPorted.Models.Config
{
    public class CatalogTypeConfig : IEntityTypeConfiguration<CatalogType>
    {
        public void Configure(EntityTypeBuilder<CatalogType> builder)
        {
            builder.ToTable(nameof(CatalogType));

            builder.HasKey(ci => ci.Id);

            builder.Property(ci => ci.Id)
               .IsRequired();

            builder.Property(cb => cb.Type)
                .IsRequired()
                .HasMaxLength(100);
        }
    }
}
```

<span data-ttu-id="5359d-448">`CatalogDBInitializer`及其基类 `CreateDatabaseIfNotExists<T>` 与 EF Core 不兼容。</span><span class="sxs-lookup"><span data-stu-id="5359d-448">The `CatalogDBInitializer` and its base class, `CreateDatabaseIfNotExists<T>`, are incompatible with EF Core.</span></span> <span data-ttu-id="5359d-449">此类的目的是创建数据库并为数据库创建种子。</span><span class="sxs-lookup"><span data-stu-id="5359d-449">The purpose of this class is to create and seed the database.</span></span> <span data-ttu-id="5359d-450">使用 EF Core 将使用以下方法[创建并删除关联的 `DbContext` 数据库](/ef/core/managing-schemas/ensure-created)：</span><span class="sxs-lookup"><span data-stu-id="5359d-450">Using EF Core will [create and drop the associated database for a `DbContext`](/ef/core/managing-schemas/ensure-created) using these methods:</span></span>

```csharp
dbContext.Database.EnsureDeleted();
dbContext.Database.EnsureCreated();
```

<span data-ttu-id="5359d-451">可以通过手动脚本或作为类型配置的一部分来实现 EF Core 中的数据种子设定。</span><span class="sxs-lookup"><span data-stu-id="5359d-451">Seeding data in EF Core can be done with manual scripts, or as part of the type configuration.</span></span> <span data-ttu-id="5359d-452">除了其他实体属性，还可以使用在类中配置种子数据 `IEntityTypeConfiguration` `builder.HasData()` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-452">Along with other entity properties, seed data can be configured in `IEntityTypeConfiguration` classes by using `builder.HasData()`.</span></span> <span data-ttu-id="5359d-453">原始应用已从 *安装* 目录中的 CSV 文件加载种子数据。</span><span class="sxs-lookup"><span data-stu-id="5359d-453">The original app loaded seed data from CSV files in the *Setup* directory.</span></span> <span data-ttu-id="5359d-454">假设只有少量项，则可以将这些数据记录作为实体配置的一部分添加。</span><span class="sxs-lookup"><span data-stu-id="5359d-454">Given that there are only a handful of items, these data records can instead be added as part of the entity configuration.</span></span> <span data-ttu-id="5359d-455">此方法非常适用于不经常更改的表中的查找数据。</span><span class="sxs-lookup"><span data-stu-id="5359d-455">This approach works well for lookup data in tables that change infrequently.</span></span> <span data-ttu-id="5359d-456">将以下方法添加到 `CatalogTypeConfig` 的 `Configure` 方法可确保在创建数据库时存在相关行：</span><span class="sxs-lookup"><span data-stu-id="5359d-456">Adding the following to `CatalogTypeConfig`'s `Configure` method ensures the associated rows are present when the database is created:</span></span>

```csharp
builder.HasData(
    new CatalogType { Id = 1, Type = "Mug" },
    new CatalogType { Id = 2, Type = "T-Shirt" },
    new CatalogType { Id = 3, Type = "Sheet" },
    new CatalogType { Id = 4, Type = "USB Memory Stick" }
);
```

<span data-ttu-id="5359d-457">初始应用包含一个 `PreconfiguredData` 类，该类包含和的数据 `CatalogBrand` `CatalogType` ，因此使用此方法时， `HasData` 调用将减少到：</span><span class="sxs-lookup"><span data-stu-id="5359d-457">The initial app includes a `PreconfiguredData` class, which includes data for `CatalogBrand` and `CatalogType`, so using this method the `HasData` call reduces to:</span></span>

```csharp
builder.HasData(
    PreconfiguredData.GetPreconfiguredCatalogBrands()
);
```

<span data-ttu-id="5359d-458">`CatalogItem`还可以从中提取数据 `PreconfiguredData` ，假定关联的图像保留在源代码管理中，这是应用程序正常运行所需的最后一个表。</span><span class="sxs-lookup"><span data-stu-id="5359d-458">The `CatalogItem` data can also be pulled from `PreconfiguredData`, and assuming the associated images are kept in source control, that is the last table needed for the app to function.</span></span> <span data-ttu-id="5359d-459">`CatalogDBInitializer`类可以与任何对它的引用一起被移除。</span><span class="sxs-lookup"><span data-stu-id="5359d-459">The `CatalogDBInitializer` class can be removed, along with any references to it.</span></span> <span data-ttu-id="5359d-460">`CatalogItemHiLoGenerator`还将删除目录中的类和 SQL 文件，并在 `Infrastructure`) 中删除对这些文件的任何引用 (`CatalogService` `ApplicationModule` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-460">The `CatalogItemHiLoGenerator` class and the SQL files in the `Infrastructure` directory are also removed, along with any references to them (in `CatalogService`, `ApplicationModule`).</span></span>

<span data-ttu-id="5359d-461">由于消除了的特殊密钥生成器类 `CatalogItem` ，此代码现在将从 `CatalogItemConfig` 以下内容中删除：</span><span class="sxs-lookup"><span data-stu-id="5359d-461">With the elimination of the special key generator classes for `CatalogItem`, this code now is removed from `CatalogItemConfig`:</span></span>

```csharp
builder.Property(ci => ci.Id)
    .ValueGeneratedNever()
    .IsRequired();
```

<span data-ttu-id="5359d-462">进行这些修改后，ASP.NET Core 应用程序将生成，但它尚不能用于 EF Core，但仍必须配置为依赖项注入。</span><span class="sxs-lookup"><span data-stu-id="5359d-462">With these modifications, the ASP.NET Core app builds, but it doesn't yet work with EF Core, which must still be configured for dependency injection.</span></span> <span data-ttu-id="5359d-463">在 EF Core 中，对其进行配置的最简单方法是在中 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="5359d-463">With EF Core, the simplest way to configure it is in `ConfigureServices`:</span></span>

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    bool useMockData = Configuration.GetValue<bool>("UseMockData");
    if (!useMockData)
    {
        string connectionString = Configuration.GetConnectionString("DefaultConnection");

        services.AddDbContext<CatalogDBContext>(options =>
            options.UseSqlServer(connectionString)
        );
    }

    // Create Autofac container builder
    var builder = new ContainerBuilder();
    builder.Populate(services);
    builder.RegisterModule(new ApplicationModule(useMockData));

    ILifetimeScope container = builder.Build();

    return new AutofacServiceProvider(container);
}
```

<span data-ttu-id="5359d-464">Autofac 的最终版本 `ApplicationModule` 仅配置一种类型，具体取决于应用是否已配置为使用模拟数据：</span><span class="sxs-lookup"><span data-stu-id="5359d-464">The final version of Autofac's `ApplicationModule` only configures one type, depending on whether the app is configured to use mock data:</span></span>

```csharp
public class ApplicationModule : Module
{
    private bool _useMockData;

    public ApplicationModule(bool useMockData)
    {
        _useMockData = useMockData;
    }

    protected override void Load(ContainerBuilder builder)
    {
        if (_useMockData)
        {
            builder.RegisterType<CatalogServiceMock>()
                .As<ICatalogService>()
                .SingleInstance();
        }
        else
        {
            builder.RegisterType<CatalogService>()
                .As<ICatalogService>()
                .InstancePerLifetimeScope();
        }
    }
}
```

<span data-ttu-id="5359d-465">端口应用将运行，但如果配置为使用非模拟数据，则不会显示任何数据。</span><span class="sxs-lookup"><span data-stu-id="5359d-465">The ported app runs, but doesn't display any data if configured to use non-mock data.</span></span> <span data-ttu-id="5359d-466">`HasData`仅在应用迁移时插入通过添加的种子数据。</span><span class="sxs-lookup"><span data-stu-id="5359d-466">The seed data added through `HasData` is only inserted when migrations are applied.</span></span> <span data-ttu-id="5359d-467">源应用未使用迁移，如果存在，则不会按原样迁移。</span><span class="sxs-lookup"><span data-stu-id="5359d-467">The source app didn't use migrations, and if it had, they wouldn't migrate as-is.</span></span> <span data-ttu-id="5359d-468">最好的方法是从新的迁移脚本开始。</span><span class="sxs-lookup"><span data-stu-id="5359d-468">The best approach is to start with a new migration script.</span></span> <span data-ttu-id="5359d-469">为此，请添加的包引用， `Microsoft.EntityFrameworkCore.Design` 并在项目根目录中打开一个终端窗口。</span><span class="sxs-lookup"><span data-stu-id="5359d-469">To do this, add a package reference for `Microsoft.EntityFrameworkCore.Design` and open a terminal window in the project root.</span></span> <span data-ttu-id="5359d-470">然后运行：</span><span class="sxs-lookup"><span data-stu-id="5359d-470">Then run:</span></span>

```dotnetcli
dotnet ef migrations add Initial
```

<span data-ttu-id="5359d-471">删除现有的 *eShopPorted* 数据库（如果存在），然后运行：</span><span class="sxs-lookup"><span data-stu-id="5359d-471">Drop the existing *eShopPorted* database if it exists, then run:</span></span>

```dotnetcli
dotnet ef database update
```

<span data-ttu-id="5359d-472">这会创建数据库并将其作为种子。</span><span class="sxs-lookup"><span data-stu-id="5359d-472">This creates and seeds the database.</span></span> <span data-ttu-id="5359d-473">现已准备好运行，其中包含几个较小的小更新可解决。</span><span class="sxs-lookup"><span data-stu-id="5359d-473">It's now ready to run, with a few small updates left to address.</span></span>

## <a name="fix-all-todo-tasks"></a><span data-ttu-id="5359d-474">修复所有 TODO 任务</span><span class="sxs-lookup"><span data-stu-id="5359d-474">Fix all TODO tasks</span></span>

<span data-ttu-id="5359d-475">此时运行移植应用会显示页面上不显示图片。</span><span class="sxs-lookup"><span data-stu-id="5359d-475">Running the ported app at this point reveals that no pictures are shown on the page.</span></span> <span data-ttu-id="5359d-476">这是因为 `PictureUri` `CatalogItem` 从不设置的属性。</span><span class="sxs-lookup"><span data-stu-id="5359d-476">This is because the `PictureUri` property of `CatalogItem` is never set.</span></span> <span data-ttu-id="5359d-477">查看 `TODO` 我们使用 Visual Studio **任务列表** 创建的项的列表，只保留处于中的项 `CatalogController` ，并提供 "从 wwwroot 引用 pic" 的注释。</span><span class="sxs-lookup"><span data-stu-id="5359d-477">Looking at the list of `TODO` items we created using Visual Studio's **Task List**, the only one that remains is in `CatalogController`, with a note to "Reference pic from wwwroot."</span></span> <span data-ttu-id="5359d-478">相关代码是：</span><span class="sxs-lookup"><span data-stu-id="5359d-478">The code in question is:</span></span>

```csharp
private void AddUriPlaceHolder(CatalogItem item)
{
    //TODO: Reference pic from wwwroot
    //item.PictureUri = this.Url.RouteUrl(PicController.GetPicRouteName, new { catalogItemId = item.Id }, this.Request.Url.Scheme);
}
```

<span data-ttu-id="5359d-479">最简单的解决方法是在站点的公共 *wwwroot/图片* 目录中引用公共映像文件。</span><span class="sxs-lookup"><span data-stu-id="5359d-479">The simplest fix is to reference the public image files in the site's public *wwwroot/Pics* directory.</span></span> <span data-ttu-id="5359d-480">可以通过将方法替换为以下代码来完成此任务：</span><span class="sxs-lookup"><span data-stu-id="5359d-480">This task can be accomplished by replacing the method with the following code:</span></span>

```csharp
private void AddUriPlaceHolder(CatalogItem item)
{
    item.PictureUri = $"/Pics/{item.Id}.png";
}
```

<span data-ttu-id="5359d-481">进行此更改后，运行应用程序将使图像像以前一样工作。</span><span class="sxs-lookup"><span data-stu-id="5359d-481">With this change, running the app reveals the images work as before.</span></span>

## <a name="additional-mvc-customizations"></a><span data-ttu-id="5359d-482">其他 MVC 自定义</span><span class="sxs-lookup"><span data-stu-id="5359d-482">Additional MVC customizations</span></span>

<span data-ttu-id="5359d-483">*EShopLegacyMVC* 应用程序非常简单，因此在默认 MVC 行为方面没有太多的配置。</span><span class="sxs-lookup"><span data-stu-id="5359d-483">The *eShopLegacyMVC* app is fairly simple, so there isn't much to configure in terms of default MVC behavior.</span></span> <span data-ttu-id="5359d-484">但是，如果确实需要配置其他 MVC 组件（如 CORS、筛选器和路由约束），则通常会在中提供此信息 `Startup.ConfigureServices` ，其中 `UseMvc` 调用。</span><span class="sxs-lookup"><span data-stu-id="5359d-484">However, if you do need to configure additional MVC components, such as CORS, filters, and route constraints, you generally provide this information in `Startup.ConfigureServices`, where `UseMvc` is called.</span></span> <span data-ttu-id="5359d-485">例如，下面的代码列表配置 [CORS](/aspnet/core/security/cors?preserve-view=true&view=aspnetcore-2.2) 并设置全局操作筛选器：</span><span class="sxs-lookup"><span data-stu-id="5359d-485">For example, the following code listing configures [CORS](/aspnet/core/security/cors?preserve-view=true&view=aspnetcore-2.2) and sets up a global action filter:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy(MyAllowSpecificOrigins,
            builder =>
                builder.WithOrigins("http://example.com", "http://www.contoso.com")
                    .AllowAnyHeader()
                    .AllowAnyMethod());
    });

    services.AddMvc(options =>
    {
      options.Filters.Add(new SampleGlobalActionFilter());
    }).SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

> [!Note]
> <span data-ttu-id="5359d-486">若要完成 CORS 配置，还必须 `app.UseCors()` 在中调用 `Configure` 。</span><span class="sxs-lookup"><span data-stu-id="5359d-486">To finish configuring CORS, you must also call `app.UseCors()` in `Configure`.</span></span>

<span data-ttu-id="5359d-487">详细的 ASP.NET Core 文档中介绍了其他高级方案，如添加 [自定义模型联编](/aspnet/core/mvc/advanced/custom-model-binding?preserve-view=true&view=aspnetcore-2.2)程序、格式化程序等。通常，这些应用程序可以应用于单个控制器或操作基础上，也可以使用上述代码列表中所示的相同选项方法全局应用。</span><span class="sxs-lookup"><span data-stu-id="5359d-487">Other advanced scenarios, like adding [custom model binders](/aspnet/core/mvc/advanced/custom-model-binding?preserve-view=true&view=aspnetcore-2.2), formatters, and more are covered in the detailed ASP.NET Core docs. Generally these can be applied on an individual controller or action basis, or globally using the same options approach shown in the previous code listing.</span></span>

## <a name="other-dependencies"></a><span data-ttu-id="5359d-488">其他依赖项</span><span class="sxs-lookup"><span data-stu-id="5359d-488">Other dependencies</span></span>

<span data-ttu-id="5359d-489">使用依赖于旧配置模型的 .NET Framework 功能的依赖项（例如 WCF 客户端类型和跟踪代码）必须在移植时修改。</span><span class="sxs-lookup"><span data-stu-id="5359d-489">Dependencies that use .NET Framework features that had a dependency on the legacy configuration model, such as the WCF client type and tracing code, must be modified when ported.</span></span> <span data-ttu-id="5359d-490">它们不是直接请求配置信息，而是应在代码中进行配置。</span><span class="sxs-lookup"><span data-stu-id="5359d-490">Rather than having these types pull in their configuration information directly, they should be configured in code.</span></span> <span data-ttu-id="5359d-491">例如，与在 ASP.NET 应用程序的 *web.config* 中配置的 WCF 服务的连接，可以使用 `basicHttpBinding` 以下代码以编程方式进行配置：</span><span class="sxs-lookup"><span data-stu-id="5359d-491">For example, a connection to a WCF service that was configured in an ASP.NET app's *web.config* to use `basicHttpBinding` could instead be configured programmatically with the following code:</span></span>

```csharp
var binding = new BasicHttpBinding();
binding.MaxReceivedMessageSize = 2_000_000;

var endpointAddress = new EndpointAddress("http://localhost:9200/ExampleService");

var myClient = new MyServiceClient(binding, endpointAddress);
```

<span data-ttu-id="5359d-492">WCF 客户端和其他 .NET Framework 类型应在代码中指定其设置，而不是依赖于配置文件的设置。</span><span class="sxs-lookup"><span data-stu-id="5359d-492">Rather than relying on config files for its settings, WCF clients and other .NET Framework types should have their settings specified in code.</span></span> <span data-ttu-id="5359d-493">通过这种方式进行配置，这些类型可在 ASP.NET Core 2.2 应用中继续工作。</span><span class="sxs-lookup"><span data-stu-id="5359d-493">Configured in this manner, these types can continue to work in ASP.NET Core 2.2 apps.</span></span>

## <a name="references"></a><span data-ttu-id="5359d-494">参考</span><span class="sxs-lookup"><span data-stu-id="5359d-494">References</span></span>

- [<span data-ttu-id="5359d-495">eShopModernizing GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="5359d-495">eShopModernizing GitHub repository</span></span>](https://github.com/dotnet-architecture/eShopModernizing)
- [<span data-ttu-id="5359d-496">.NET 升级助手工具</span><span class="sxs-lookup"><span data-stu-id="5359d-496">.NET Upgrade Assistant tool</span></span>](https://aka.ms/dotnet-upgrade-assistant)
- [<span data-ttu-id="5359d-497">API 和 Viewmodel 不应引用域模型</span><span class="sxs-lookup"><span data-stu-id="5359d-497">Your API and ViewModels Should Not Reference Domain Models</span></span>](https://ardalis.com/your-api-and-view-models-should-not-reference-domain-models/)
- [<span data-ttu-id="5359d-498">开发人员异常页中间件</span><span class="sxs-lookup"><span data-stu-id="5359d-498">Developer Exception Page Middleware</span></span>](/aspnet/core/fundamentals/error-handling#developer-exception-page)
- [<span data-ttu-id="5359d-499">深入了解 EF Core HasData</span><span class="sxs-lookup"><span data-stu-id="5359d-499">Deep Dive into EF Core HasData</span></span>](/archive/msdn-magazine/2018/august/data-points-deep-dive-into-ef-core-hasdata-seeding)

>[!div class="step-by-step"]
><span data-ttu-id="5359d-500">[上一页](more-migration-scenarios.md)
>[下一页](deployment-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="5359d-500">[Previous](more-migration-scenarios.md)
[Next](deployment-scenarios.md)</span></span>
